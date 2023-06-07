---
title: Convertendo Tipos de Dados (C#)
description: Os métodos de conversão alteram o tipo dos objetos de entrada. Consulte operações de conversão em consultas LINQ em C#, como Enumerable. AsEnumerable e Enumerable. OfType.
ms.date: 07/20/2015
ms.assetid: 46e5682f-77a1-4302-8f93-a2b53c408808
ms.openlocfilehash: 3291690f9aaee945ca7feb04ebbc676db2612894
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87105482"
---
# <a name="converting-data-types-c"></a>Convertendo Tipos de Dados (C#)
Os métodos de conversão alteram o tipo dos objetos de entrada.

 As operações de conversão em consultas LINQ são úteis em diversas aplicações. A seguir estão alguns exemplos:

- O método <xref:System.Linq.Enumerable.AsEnumerable%2A?displayProperty=nameWithType> pode ser usado para ocultar a implementação personalizada de um tipo de um operador de consulta padrão.

- O método <xref:System.Linq.Enumerable.OfType%2A?displayProperty=nameWithType> pode ser usado para habilitar coleções sem parâmetros para consulta LINQ.

- Os métodos <xref:System.Linq.Enumerable.ToArray%2A?displayProperty=nameWithType>, <xref:System.Linq.Enumerable.ToDictionary%2A?displayProperty=nameWithType>, <xref:System.Linq.Enumerable.ToList%2A?displayProperty=nameWithType> e <xref:System.Linq.Enumerable.ToLookup%2A?displayProperty=nameWithType> podem ser usados para forçar a execução de consulta imediata em vez de adiá-la até que a consulta seja enumerada.

## <a name="methods"></a>Métodos
 A tabela a seguir lista os métodos de operador de consulta padrão que realizam conversões de tipo de dados.

 Os métodos de conversão nesta tabela cujos nomes começam com "As" alteram o tipo estático da coleção de origem, mas não a enumeram. Os métodos cujos nomes começam com "To" enumeram a coleção de origem e colocam os itens na coleção de tipo correspondente.

|Nome do método|Descrição|Sintaxe de expressão de consulta C#|Mais informações|
|-----------------|-----------------|---------------------------------|----------------------|
|AsEnumerable|Retorna a entrada digitada como <xref:System.Collections.Generic.IEnumerable%601>.|Não aplicável.|<xref:System.Linq.Enumerable.AsEnumerable%2A?displayProperty=nameWithType>|
|AsQueryable|Converte um <xref:System.Collections.IEnumerable> (genérico) em um <xref:System.Linq.IQueryable> (genérico).|Não aplicável.|<xref:System.Linq.Queryable.AsQueryable%2A?displayProperty=nameWithType>|
|Conversão|Converte os elementos de uma coleção em um tipo especificado.|Use uma variável de intervalo de tipo explícito. Por exemplo:<br /><br /> `from string str in words`|<xref:System.Linq.Enumerable.Cast%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Cast%2A?displayProperty=nameWithType>|
|OfType|Filtra valores, dependendo da capacidade de serem convertidos em um tipo especificado.|Não aplicável.|<xref:System.Linq.Enumerable.OfType%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.OfType%2A?displayProperty=nameWithType>|
|ToArray|Converte uma coleção em uma matriz. Esse método força a execução de consulta.|Não aplicável.|<xref:System.Linq.Enumerable.ToArray%2A?displayProperty=nameWithType>|
|ToDictionary|Coloca os elementos em um <xref:System.Collections.Generic.Dictionary%602> com base em uma função de seletor de chave. Esse método força a execução de consulta.|Não aplicável.|<xref:System.Linq.Enumerable.ToDictionary%2A?displayProperty=nameWithType>|
|ToList|Converte uma coleção em um <xref:System.Collections.Generic.List%601>. Esse método força a execução de consulta.|Não aplicável.|<xref:System.Linq.Enumerable.ToList%2A?displayProperty=nameWithType>|
|ToLookup|Coloca os elementos em um <xref:System.Linq.Lookup%602> (um dicionário one-to-many) com base em uma função de seletor de chave. Esse método força a execução de consulta.|Não aplicável.|<xref:System.Linq.Enumerable.ToLookup%2A?displayProperty=nameWithType>|

## <a name="query-expression-syntax-example"></a>Exemplo de sintaxe de expressão de consulta

O exemplo de código a seguir usa uma variável de intervalo digitada explicitamente para converter um tipo em um subtipo antes de acessar um membro que está disponível somente no subtipo.

```csharp
class Plant
{
    public string Name { get; set; }
}

class CarnivorousPlant : Plant
{
    public string TrapType { get; set; }
}

static void Cast()
{
    Plant[] plants = new Plant[] {
        new CarnivorousPlant { Name = "Venus Fly Trap", TrapType = "Snap Trap" },
        new CarnivorousPlant { Name = "Pitcher Plant", TrapType = "Pitfall Trap" },
        new CarnivorousPlant { Name = "Sundew", TrapType = "Flypaper Trap" },
        new CarnivorousPlant { Name = "Waterwheel Plant", TrapType = "Snap Trap" }
    };

    var query = from CarnivorousPlant cPlant in plants
                where cPlant.TrapType == "Snap Trap"
                select cPlant;

    foreach (Plant plant in query)
        Console.WriteLine(plant.Name);

    /* This code produces the following output:

        Venus Fly Trap
        Waterwheel Plant
    */
}
```

## <a name="see-also"></a>Confira também

- <xref:System.Linq>
- [Visão geral de operadores de consulta padrão (C#)](./standard-query-operators-overview.md)
- [cláusula from](../../../language-reference/keywords/from-clause.md)
- [Expressões de Consulta LINQ](../../../linq/index.md)
- [Como consultar uma ArrayList com LINQ (C#)](./how-to-query-an-arraylist-with-linq.md)
