---
title: Como adicionar métodos personalizados para consultas LINQ (C#)
description: Saiba como estender o conjunto de métodos que você pode usar para consultas LINQ adicionando métodos de extensão à <T> interface IEnumerable em C#.
ms.date: 07/20/2015
ms.assetid: 1a500f60-2e10-49fb-8b2a-d8d08e4817cb
ms.openlocfilehash: fac0eb4e14eb3bb36313232a7d7fa3060c0ac171
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87103603"
---
# <a name="how-to-add-custom-methods-for-linq-queries-c"></a>Como adicionar métodos personalizados para consultas LINQ (C#)

Você pode estender o conjunto de métodos que você pode usar para consultas LINQ, adicionando os métodos de extensão à interface <xref:System.Collections.Generic.IEnumerable%601>. Por exemplo, além do padrão médio ou máximo de operações, você pode criar um método de agregação personalizado para calcular um único valor de uma sequência de valores. Você também pode criar um método que funciona como um filtro personalizado ou como uma transformação de dados específicos para uma sequência de valores e que retorna uma nova sequência. Exemplos desses métodos são <xref:System.Linq.Enumerable.Distinct%2A>, <xref:System.Linq.Enumerable.Skip%2A> e <xref:System.Linq.Enumerable.Reverse%2A>.

Ao estender a interface <xref:System.Collections.Generic.IEnumerable%601>, você pode aplicar seus métodos personalizados para qualquer coleção enumerável. Para obter mais informações, consulte [Métodos de extensão](../../classes-and-structs/extension-methods.md).

## <a name="adding-an-aggregate-method"></a>Adicionando um método de agregação

Um método de agregação calcula um valor único de um conjunto de valores. O LINQ fornece vários métodos de agregação, incluindo <xref:System.Linq.Enumerable.Average%2A>, <xref:System.Linq.Enumerable.Min%2A> e <xref:System.Linq.Enumerable.Max%2A>. Você pode criar seu próprio método de agregação, adicionando um método de extensão à interface <xref:System.Collections.Generic.IEnumerable%601>.

O exemplo de código a seguir mostra como criar um método de extensão chamado `Median` para calcular uma mediana de uma sequência de números do tipo `double`.

```csharp
public static class LINQExtension
{
    public static double Median(this IEnumerable<double> source)
    {
        var countOfElementsInTheSet = source?.Count() ?? 0;

        if (countOfElementsInTheSet == 0)
        {
            throw new InvalidOperationException("Cannot compute median for a null or empty set.");
        }

        var sortedList = (from number in source
                         orderby number
                         select number).ToList();

        int itemIndex = countOfElementsInTheSet / 2;

        if (countOfElementsInTheSet % 2 == 0)
        {
            // Even number of items.
            return (sortedList[itemIndex] + sortedList[itemIndex - 1]) / 2;
        }
        else
        {
            // Odd number of items.
            return sortedList[itemIndex];
        }
    }
}
```

Você chama esse método de extensão para qualquer coleção enumerável da mesma maneira que chama outros métodos de agregação da interface <xref:System.Collections.Generic.IEnumerable%601>.

O exemplo de código a seguir mostra como usar o método `Median` para uma matriz do tipo `double`.

```csharp
double[] numbers1 = { 1.9, 2, 8, 4, 5.7, 6, 7.2, 0 };

var query1 = numbers1.Median();

Console.WriteLine("double: Median = " + query1);
```

```csharp
/*
 This code produces the following output:

 Double: Median = 4.85
*/
```

### <a name="overloading-an-aggregate-method-to-accept-various-types"></a>Sobrecarregando um método de agregação para aceitar vários tipos

Você pode sobrecarregar o método de agregação para que ele aceite sequências de vários tipos. A abordagem padrão é criar uma sobrecarga para cada tipo. Outra abordagem é criar uma sobrecarga que vai pegar um tipo genérico e convertê-lo em um tipo específico, usando um delegado. Você também pode combinar as duas abordagens.

#### <a name="to-create-an-overload-for-each-type"></a>Para criar uma sobrecarga para cada tipo

Você pode criar uma sobrecarga específica para cada tipo que deseja oferecer suporte. O exemplo de código a seguir mostra uma sobrecarga do método `Median` para o tipo `integer`.

```csharp
//int overload

public static double Median(this IEnumerable<int> source)
{
    return (from num in source select (double)num).Median();
}
```

Agora você pode chamar as sobrecargas `Median` para os tipos `integer` e `double`, conforme mostrado no código a seguir:

```csharp
double[] numbers1 = { 1.9, 2, 8, 4, 5.7, 6, 7.2, 0 };

var query1 = numbers1.Median();

Console.WriteLine("double: Median = " + query1);
```

```csharp
int[] numbers2 = { 1, 2, 3, 4, 5 };

var query2 = numbers2.Median();

Console.WriteLine("int: Median = " + query2);
```

```csharp
/*
 This code produces the following output:

 Double: Median = 4.85
 Integer: Median = 3
*/
```

#### <a name="to-create-a-generic-overload"></a>Para criar uma sobrecarga genérica

Você também pode criar uma sobrecarga que aceita uma sequência de objetos genéricos. Essa sobrecarga recebe um delegado como parâmetro e usa-o para converter uma sequência de objetos de um tipo genérico em um tipo específico.

O código a seguir mostra uma sobrecarga do método `Median` que recebe o delegado <xref:System.Func%602> como um parâmetro. Esse delegado recebe um objeto de tipo genérico T e retorna um objeto do tipo `double`.

```csharp
// Generic overload.

public static double Median<T>(this IEnumerable<T> numbers,
                       Func<T, double> selector)
{
    return (from num in numbers select selector(num)).Median();
}
```

Agora você pode chamar o método `Median` para uma sequência de objetos de qualquer tipo. Se o tipo não tem sua própria sobrecarga de método, você precisa passar um parâmetro delegado. No C# você pode usar uma expressão lambda para essa finalidade. Além disso, no Visual Basic, se você usar a cláusula `Aggregate` ou `Group By` em vez da chamada de método, você pode passar qualquer valor ou expressão que estiver no escopo dessa cláusula.

O exemplo de código a seguir mostra como chamar o método `Median` para uma matriz de inteiros e para uma matriz de cadeias de caracteres. Será calculada a mediana dos comprimentos das cadeias de caracteres na matriz. O exemplo também mostra como passar o parâmetro delegado <xref:System.Func%602> ao método `Median` para cada caso.

```csharp
int[] numbers3 = { 1, 2, 3, 4, 5 };

/*
  You can use the num=>num lambda expression as a parameter for the Median method
  so that the compiler will implicitly convert its value to double.
  If there is no implicit conversion, the compiler will display an error message.
*/

var query3 = numbers3.Median(num => num);

Console.WriteLine("int: Median = " + query3);

string[] numbers4 = { "one", "two", "three", "four", "five" };

// With the generic overload, you can also use numeric properties of objects.

var query4 = numbers4.Median(str => str.Length);

Console.WriteLine("String: Median = " + query4);

/*
 This code produces the following output:

 Integer: Median = 3
 String: Median = 4
*/
```

## <a name="adding-a-method-that-returns-a-collection"></a>Adicionando um método que retorna uma coleção

Você pode estender a interface <xref:System.Collections.Generic.IEnumerable%601> com um método de consulta personalizada que retorna uma sequência de valores. Nesse caso, o método deve retornar uma coleção do tipo <xref:System.Collections.Generic.IEnumerable%601>. Esses métodos podem ser usados para aplicar transformações de dados ou filtros a uma sequência de valores.

O exemplo a seguir mostra como criar um método de extensão chamado `AlternateElements` que retorna todos os outros elementos em uma coleção, começando pelo primeiro elemento.

```csharp
// Extension method for the IEnumerable<T> interface.
// The method returns every other element of a sequence.

public static IEnumerable<T> AlternateElements<T>(this IEnumerable<T> source)
{
    List<T> list = new List<T>();

    int i = 0;

    foreach (var element in source)
    {
        if (i % 2 == 0)
        {
            list.Add(element);
        }

        i++;
    }

    return list;
}
```

Você pode chamar esse método de extensão para qualquer coleção enumerável exatamente como chamaria outros métodos da interface <xref:System.Collections.Generic.IEnumerable%601>, conforme mostrado no código a seguir:

```csharp
string[] strings = { "a", "b", "c", "d", "e" };

var query = strings.AlternateElements();

foreach (var element in query)
{
    Console.WriteLine(element);
}
/*
 This code produces the following output:

 a
 c
 e
*/
```

## <a name="see-also"></a>Confira também

- <xref:System.Collections.Generic.IEnumerable%601>
- [Métodos de Extensão](../../classes-and-structs/extension-methods.md)
