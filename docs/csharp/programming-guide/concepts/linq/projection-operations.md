---
title: Operações de projeção (C#)
description: Saiba mais sobre as operações de projeção. Essas operações transformam um objeto em um novo formulário que geralmente consiste apenas em propriedades que serão usadas posteriormente.
ms.date: 07/20/2015
ms.assetid: 98df573a-aad9-4b8c-9a71-844be2c4fb41
ms.openlocfilehash: 289100ac9afcfc0d5b93b5f963adc0a123e0a5af
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87299156"
---
# <a name="projection-operations-c"></a>Operações de projeção (C#)
Projeção refere-se à operação de transformar um objeto em um novo formulário que geralmente consiste apenas nas propriedades que serão usadas posteriormente. Usando a projeção, você pode construir um novo tipo que é criado de cada objeto. É possível projetar uma propriedade e executar uma função matemática nela. Também é possível projetar o objeto original sem alterá-lo.  
  
 Os métodos de operador de consulta padrão que realizam a projeção estão listados na seção a seguir.  
  
## <a name="methods"></a>Métodos  
  
|Nome do método|Descrição|Sintaxe de expressão de consulta C#|Mais informações|  
|-----------------|-----------------|---------------------------------|----------------------|  
|Selecionar|Projeta valores com base em uma função de transformação.|`select`|<xref:System.Linq.Enumerable.Select%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Select%2A?displayProperty=nameWithType>|  
|SelectMany|Projeta sequências de valores baseados em uma função de transformação e os mescla em uma sequência.|Use várias cláusulas `from`|<xref:System.Linq.Enumerable.SelectMany%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.SelectMany%2A?displayProperty=nameWithType>|  
  
## <a name="query-expression-syntax-examples"></a>Exemplos de sintaxe de expressão de consulta  
  
### <a name="select"></a>Selecionar  
 O exemplo a seguir usa a cláusula `select` para projetar a primeira letra de cada cadeia de caracteres em uma lista de cadeias de caracteres.  
  
```csharp  
List<string> words = new List<string>() { "an", "apple", "a", "day" };  
  
var query = from word in words  
            select word.Substring(0, 1);  
  
foreach (string s in query)  
    Console.WriteLine(s);  
  
/* This code produces the following output:  
  
    a  
    a  
    a  
    d  
*/  
```  
  
### <a name="selectmany"></a>SelectMany  
 O exemplo a seguir usa várias cláusulas `from` para projetar cada palavra de cada cadeia de caracteres em uma lista de cadeias de caracteres.  
  
```csharp  
List<string> phrases = new List<string>() { "an apple a day", "the quick brown fox" };  
  
var query = from phrase in phrases  
            from word in phrase.Split(' ')  
            select word;  
  
foreach (string s in query)  
    Console.WriteLine(s);  
  
/* This code produces the following output:  
  
    an  
    apple  
    a  
    day  
    the  
    quick  
    brown  
    fox  
*/  
```  
  
## <a name="select-versus-selectmany"></a>Select versus SelectMany  
 O trabalho de `Select()` e `SelectMany()` é produzir um valor (ou valores) de resultado dos valores de origem. `Select()` produz um valor de resultado para cada valor de origem. O resultado geral, portanto, é uma coleção que tem o mesmo número de elementos que a coleção de origem. Por outro lado, `SelectMany()` produz um único resultado geral que contém subcoleções concatenadas de cada valor de origem. A função de transformação passada como um argumento para `SelectMany()` deve retornar uma sequência enumerável de valores para cada valor de origem. Essas sequências enumeráveis, então, são concatenadas por `SelectMany()` para criar uma sequência grande.  
  
 As duas ilustrações a seguir mostram a diferença conceitual entre as ações desses dois métodos. Em cada caso, presuma que a função de seletor (transformação) seleciona a matriz de flores de cada valor de origem.  
  
 A ilustração mostra como `Select()` retorna uma coleção que tem o mesmo número de elementos que a coleção de origem.  
  
 ![Gráfico que mostra a ação de Select&#40;&#41;](./media/projection-operations/select-action-graphic.png)  
  
 Esta ilustração mostra como `SelectMany()` concatena a sequência intermediária de matrizes em um valor de resultado final que contém cada valor de cada matriz intermediária.  
  
 ![Gráfico mostrando a ação de SelectMany&#40;&#41;.](./media/projection-operations/select-many-action-graphic.png )  
  
### <a name="code-example"></a>Exemplo de código  
 O exemplo a seguir compara o comportamento de `Select()` e de `SelectMany()`. O código cria um "buquê" de flores usando os dois primeiros itens de cada lista de nomes de flor na coleção de origem. Neste exemplo, o "valor único" que a função de transformação <xref:System.Linq.Enumerable.Select%60%602%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2C%60%601%7D%29> usa é uma coleção de valores. Isso requer o loop `foreach` extra para enumerar cada cadeia de caracteres em cada subsequência.  
  
```csharp  
class Bouquet  
{  
    public List<string> Flowers { get; set; }  
}  
  
static void SelectVsSelectMany()  
{  
    List<Bouquet> bouquets = new List<Bouquet>() {  
        new Bouquet { Flowers = new List<string> { "sunflower", "daisy", "daffodil", "larkspur" }},  
        new Bouquet{ Flowers = new List<string> { "tulip", "rose", "orchid" }},  
        new Bouquet{ Flowers = new List<string> { "gladiolis", "lily", "snapdragon", "aster", "protea" }},  
        new Bouquet{ Flowers = new List<string> { "larkspur", "lilac", "iris", "dahlia" }}  
    };  
  
    // *********** Select ***********
    IEnumerable<List<string>> query1 = bouquets.Select(bq => bq.Flowers);  
  
    // ********* SelectMany *********  
    IEnumerable<string> query2 = bouquets.SelectMany(bq => bq.Flowers);  
  
    Console.WriteLine("Results by using Select():");  
    // Note the extra foreach loop here.  
    foreach (IEnumerable<String> collection in query1)  
        foreach (string item in collection)  
            Console.WriteLine(item);  
  
    Console.WriteLine("\nResults by using SelectMany():");  
    foreach (string item in query2)  
        Console.WriteLine(item);  
  
    /* This code produces the following output:  
  
       Results by using Select():  
        sunflower  
        daisy  
        daffodil  
        larkspur  
        tulip  
        rose  
        orchid  
        gladiolis  
        lily  
        snapdragon  
        aster  
        protea  
        larkspur  
        lilac  
        iris  
        dahlia  
  
       Results by using SelectMany():  
        sunflower  
        daisy  
        daffodil  
        larkspur  
        tulip  
        rose  
        orchid  
        gladiolis  
        lily  
        snapdragon  
        aster  
        protea  
        larkspur  
        lilac  
        iris  
        dahlia  
    */  
  
}  
```  
  
## <a name="see-also"></a>Veja também

- <xref:System.Linq>
- [Visão geral de operadores de consulta padrão (C#)](./standard-query-operators-overview.md)
- [cláusula SELECT](../../../language-reference/keywords/select-clause.md)
- [Como preencher coleções de objetos de várias fontes (LINQ) (C#)](./how-to-populate-object-collections-from-multiple-sources-linq.md)
- [Como dividir um arquivo em vários arquivos usando grupos (LINQ) (C#)](./how-to-split-a-file-into-many-files-by-using-groups-linq.md)
