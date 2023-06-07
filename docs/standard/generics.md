---
title: Visão geral dos tipos genéricos (genéricos)
description: Saiba como os genéricos atuam como modelos de código que permitem que você defina estruturas de dados fortemente tipadas sem se comprometer com um tipo de dados real.
author: kuhlenh
ms.author: wiwagn
ms.date: 10/09/2018
ms.openlocfilehash: 99e3b589cd67c9d7026966d3d48d0e06a91fcc86
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84287539"
---
# <a name="generic-types-overview"></a>Visão geral de tipos genéricos

Desenvolvedores usam genéricos o tempo todo no .NET, seja implícita ou explicitamente. Ao usar o LINQ no .NET, você já percebeu que você está trabalhando com <xref:System.Collections.Generic.IEnumerable%601>? Ou, se você já viu um exemplo online de um "repositório genérico" para conversar com bancos de dados usando Entity Framework, você viu que a maioria dos métodos retorna `IQueryable<T>` ? Talvez você tenha se perguntado o que é o **T** nesses exemplos e por que ele está lá.

Introduzidos pela primeira vez no .NET Framework 2.0, os genéricos são essencialmente um "modelo de código" que permite aos desenvolvedores definir estruturas de dados [fortemente tipadas](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/hbzz1a9a(v=vs.100)) sem se comprometer com um tipo de dados real. Por exemplo, <xref:System.Collections.Generic.List%601> é uma [coleção genérica](xref:System.Collections.Generic) que pode ser declarada e usada com qualquer tipo, como `List<int>` , `List<string>` ou `List<Person>` .

Para entender por que os genéricos são úteis, vamos dar uma olhada em uma classe específica antes e depois adicionar os genéricos: <xref:System.Collections.ArrayList>. No .NET Framework 1.0, os elementos `ArrayList` eram do tipo <xref:System.Object>. Qualquer elemento adicionado à coleção foi convertido silenciosamente em um `Object` . O mesmo ocorreria ao ler elementos da lista. Esse processo é conhecido como [conversão boxing e unboxing](../csharp/programming-guide/types/boxing-and-unboxing.md) e afeta o desempenho. Além do desempenho, no entanto, não há como determinar o tipo de dados na lista em tempo de compilação, o que torna o código frágil. Genéricos resolvem esse problema definindo o tipo de dados que cada instância de lista conterá. Resumindo, você só pode adicionar inteiros a `List<int>` e só pode adicionar Pessoas a `List<Person>`.

Os genéricos também estão disponíveis em tempo de execução. O tempo de execução sabe que tipo de estrutura de dados você está usando e pode armazená-la na memória com mais eficiência.

O exemplo a seguir é um pequeno programa que ilustra a eficiência de saber o tipo de estrutura de dados em tempo de execução:

```csharp
  using System;
  using System.Collections;
  using System.Collections.Generic;
  using System.Diagnostics;

  namespace GenericsExample {
    class Program {
      static void Main(string[] args) {
        //generic list
        List<int> ListGeneric = new List<int> { 5, 9, 1, 4 };
        //non-generic list
        ArrayList ListNonGeneric = new ArrayList { 5, 9, 1, 4 };
        // timer for generic list sort
        Stopwatch s = Stopwatch.StartNew();
        ListGeneric.Sort();
        s.Stop();
        Console.WriteLine($"Generic Sort: {ListGeneric}  \n Time taken: {s.Elapsed.TotalMilliseconds}ms");

        //timer for non-generic list sort
        Stopwatch s2 = Stopwatch.StartNew();
        ListNonGeneric.Sort();
        s2.Stop();
        Console.WriteLine($"Non-Generic Sort: {ListNonGeneric}  \n Time taken: {s2.Elapsed.TotalMilliseconds}ms");
        Console.ReadLine();
      }
    }
  }
```

Este programa produz uma saída semelhante à seguinte:

```console
Generic Sort: System.Collections.Generic.List`1[System.Int32]
 Time taken: 0.0034ms
Non-Generic Sort: System.Collections.ArrayList
 Time taken: 0.2592ms
```

A primeira coisa que você pode observar aqui é que classificar a lista genérica é significativamente mais rápido do que classificar a lista não genérica. Você também observará que o tipo de lista genérico é distinto ([System.Int32]) enquanto o tipo da lista não genérico é generalizado. Como o tempo de execução sabe que o genérico `List<int>` é do tipo <xref:System.Int32> , ele pode armazenar os elementos da lista em uma matriz de inteiros subjacente na memória, enquanto o não genérico `ArrayList` precisa converter cada elemento da lista em um objeto. Como este exemplo mostra, as conversões extras levam tempo e reduzem a velocidade da classificação da lista.

Uma vantagem adicional de o runtime saber o tipo de seu genérico é uma melhor experiência de depuração. Quando você está depurando um genérico em C#, você sabe que tipo de cada elemento está na sua estrutura de dados. Sem os genéricos, você não faria ideia de qual o tipo de cada elemento.

## <a name="see-also"></a>Veja também

- [Guia de Programação em C# – Genéricos](../csharp/programming-guide/generics/index.md)
