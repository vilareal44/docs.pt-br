---
title: Criando interfaces genéricas variantes (C#)
description: Saiba como criar interfaces genéricas variantes com parâmetros de tipo genérico covariant ou contravariant.
ms.date: 07/20/2015
ms.assetid: 30330ec4-9df2-4838-a535-6c406d0ed4df
ms.openlocfilehash: 38b32784b681e748cd508c3d431fd4b18ec2c81a
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87105725"
---
# <a name="creating-variant-generic-interfaces-c"></a>Criando interfaces genéricas variantes (C#)

Você pode declarar parâmetros de tipo genérico em interfaces como covariantes ou contravariantes. A *Covariância* permite que os métodos de interface tenham tipos de retorno mais derivados que aqueles definidos pelos parâmetros de tipo genérico. A *Contravariância* permite que os métodos de interface tenham tipos de argumentos que são menos derivados que aqueles especificados pelos parâmetros genéricos. Uma interface genérica que tenha parâmetros de tipo genérico covariantes ou contravariantes é chamada de *variante*.

> [!NOTE]
> O .NET Framework 4 introduziu o suporte à variação para diversas interfaces genéricas existentes. Para obter a lista das interfaces variantes no .NET, consulte [variação em interfaces genéricas (C#)](./variance-in-generic-interfaces.md).

## <a name="declaring-variant-generic-interfaces"></a>Declarando interfaces genéricas variantes

Você pode declarar interfaces genéricas variantes usando as palavras-chave `in` e `out` para parâmetros de tipo genérico.

> [!IMPORTANT]
> Os parâmetros `ref`, `in` e `out` no C# não podem ser variantes. Os tipos de valor também não dão suporte à variância.

Você pode declarar um parâmetro de tipo genérico como covariante usando a palavra-chave `out`. O tipo de covariante deve satisfazer as condições a seguir:

- O tipo é usado apenas como um tipo de retorno dos métodos de interface e não é usado como um tipo de argumentos de método. Isso é ilustrado no exemplo a seguir, no qual o tipo `R` é declarado covariante.

    ```csharp
    interface ICovariant<out R>
    {
        R GetSomething();
        // The following statement generates a compiler error.
        // void SetSomething(R sampleArg);

    }
    ```

    Há uma exceção a essa regra. Se você tiver um delegado genérico contravariante como um parâmetro de método, você poderá usar o tipo como um parâmetro de tipo genérico para o delegado. Isso é ilustrado pelo tipo `R` no exemplo a seguir. Para obter mais informações, consulte [Variância em delegados (C#)](./variance-in-delegates.md) e [Usando variância para delegados genéricos Func e Action (C#)](./using-variance-for-func-and-action-generic-delegates.md).

    ```csharp
    interface ICovariant<out R>
    {
        void DoSomething(Action<R> callback);
    }
    ```

- O tipo não é usado como uma restrição genérica para os métodos de interface. O código a seguir ilustra isso.

    ```csharp
    interface ICovariant<out R>
    {
        // The following statement generates a compiler error
        // because you can use only contravariant or invariant types
        // in generic constraints.
        // void DoSomething<T>() where T : R;
    }
    ```

Você pode declarar um parâmetro de tipo genérico como contravariante usando a palavra-chave `in`. O tipo contravariante pode ser usado apenas como um tipo de argumentos de método e não como um tipo de retorno dos métodos de interface. O tipo contravariante também pode ser usado para restrições genéricas. O código a seguir mostra como declarar uma interface contravariante e usar uma restrição genérica para um de seus métodos.

```csharp
interface IContravariant<in A>
{
    void SetSomething(A sampleArg);
    void DoSomething<T>() where T : A;
    // The following statement generates a compiler error.
    // A GetSomething();
}
```

Também é possível oferecer suporte à covariância e contravariância na mesma interface, mas para parâmetros de tipo diferentes, conforme mostrado no exemplo de código a seguir.

```csharp
interface IVariant<out R, in A>
{
    R GetSomething();
    void SetSomething(A sampleArg);
    R GetSetSomethings(A sampleArg);
}
```

## <a name="implementing-variant-generic-interfaces"></a>Implementando interfaces genéricas variantes

Você pode implementar interfaces genéricas variantes em classes, através da mesma sintaxe que é usada para interfaces invariantes. O exemplo de código a seguir mostra como implementar uma interface covariante em uma classe genérica.

```csharp
interface ICovariant<out R>
{
    R GetSomething();
}
class SampleImplementation<R> : ICovariant<R>
{
    public R GetSomething()
    {
        // Some code.
        return default(R);
    }
}
```

As classes que implementam interfaces variantes são invariantes. Por exemplo, considere o código a seguir.

```csharp
// The interface is covariant.
ICovariant<Button> ibutton = new SampleImplementation<Button>();
ICovariant<Object> iobj = ibutton;

// The class is invariant.
SampleImplementation<Button> button = new SampleImplementation<Button>();
// The following statement generates a compiler error
// because classes are invariant.
// SampleImplementation<Object> obj = button;
```

## <a name="extending-variant-generic-interfaces"></a>Estendendo interfaces genéricas variantes

Quando você estende uma interface genérica variante, é necessário usar as palavras-chave `in` e `out` para especificar explicitamente se a interface derivada dará suporte à variância. O compilador não deduz a variância da interface que está sendo estendida. Por exemplo, considere as seguintes interfaces.

```csharp
interface ICovariant<out T> { }
interface IInvariant<T> : ICovariant<T> { }
interface IExtCovariant<out T> : ICovariant<T> { }
```

Na interface `IInvariant<T>`, o parâmetro de tipo genérico `T` é invariante, enquanto que no `IExtCovariant<out T>` o parâmetro de tipo é covariante, embora as duas interfaces estendam a mesma interface. A mesma regra é aplicada aos parâmetros de tipo genérico contravariantes.

Você pode criar uma interface que estende tanto a interface em que o parâmetro de tipo genérico `T` é covariante, quanto a interface em que ele é contravariante, caso na interface de extensão o parâmetro de tipo genérico `T` seja invariante. Isso é ilustrado no exemplo de código a seguir.

```csharp
interface ICovariant<out T> { }
interface IContravariant<in T> { }
interface IInvariant<T> : ICovariant<T>, IContravariant<T> { }
```

No entanto, se um parâmetro de tipo genérico `T` for declarado covariante em uma interface, você não poderá declará-lo contravariante na interface de extensão ou vice-versa. Isso é ilustrado no exemplo de código a seguir.

```csharp
interface ICovariant<out T> { }
// The following statement generates a compiler error.
// interface ICoContraVariant<in T> : ICovariant<T> { }
```

### <a name="avoiding-ambiguity"></a>Evitando ambiguidade

Ao implementar interfaces genéricas variantes, a variância, às vezes, pode levar à ambiguidade. Essa ambiguidade deve ser evitada.

Por exemplo, se você implementar explicitamente a mesma interface genérica variante com parâmetros de tipo genérico diferentes em uma classe, isso poderá criar ambiguidade. O compilador não produz um erro nesse caso, mas não é especificado qual implementação de interface será escolhida em tempo de execução. Essa ambiguidade pode levar a bugs sutis em seu código. Considere o exemplo de código a seguir.

```csharp
// Simple class hierarchy.
class Animal { }
class Cat : Animal { }
class Dog : Animal { }

// This class introduces ambiguity
// because IEnumerable<out T> is covariant.
class Pets : IEnumerable<Cat>, IEnumerable<Dog>
{
    IEnumerator<Cat> IEnumerable<Cat>.GetEnumerator()
    {
        Console.WriteLine("Cat");
        // Some code.
        return null;
    }

    IEnumerator IEnumerable.GetEnumerator()
    {
        // Some code.
        return null;
    }

    IEnumerator<Dog> IEnumerable<Dog>.GetEnumerator()
    {
        Console.WriteLine("Dog");
        // Some code.
        return null;
    }
}
class Program
{
    public static void Test()
    {
        IEnumerable<Animal> pets = new Pets();
        pets.GetEnumerator();
    }
}
```

Neste exemplo, não está especificado como o método `pets.GetEnumerator` escolherá entre `Cat` e `Dog`. Isso poderá causar problemas em seu código.

## <a name="see-also"></a>Confira também

- [Variância em interfaces genéricas (C#)](./variance-in-generic-interfaces.md)
- [Usando variação para delegados genéricos Func e Action (C#)](./using-variance-for-func-and-action-generic-delegates.md)
