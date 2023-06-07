---
title: LINQ e tipos genéricos (C#)
description: Saiba mais sobre os conceitos básicos de tipos genéricos em C# que dão suporte a consultas.  Consultas LINQ são baseadas em tipos genéricos.
ms.date: 07/20/2015
helpviewer_keywords:
- LINQ [C#], generic types
- generic types [LINQ]
- generics [LINQ]
ms.assetid: 660e3799-25ca-462c-8c4a-8bce04fbb031
ms.openlocfilehash: 98054a4a21704293faa1194dac342bc48aef138d
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87165640"
---
# <a name="linq-and-generic-types-c"></a>LINQ e tipos genéricos (C#)
As consultas LINQ são baseadas em tipos genéricos, que foram introduzidos na versão 2,0 do .NET Framework. Não é necessário um conhecimento profundo sobre os genéricos antes de começar a escrever consultas. No entanto, convém entender dois conceitos básicos:  
  
1. Quando você cria uma instância de uma classe de coleção genérica, como <xref:System.Collections.Generic.List%601>, substitua o "T" pelo tipo dos objetos que a lista bloqueia. Por exemplo, uma lista de cadeias de caracteres é expressa como `List<string>` e uma lista de objetos `Customer` é expressa como `List<Customer>`. Uma lista genérica é fortemente tipada e oferece muitos benefícios em coleções que armazenam seus elementos como <xref:System.Object>. Se tentar adicionar um `Customer` em uma `List<string>`, você obterá um erro em tempo de compilação. É fácil usar coleções genéricas, porque você não precisa realizar a conversão de tipo em tempo de execução.  
  
2. A <xref:System.Collections.Generic.IEnumerable%601> é a interface que permite que as classes de coleção genérica sejam enumeradas usando a instrução `foreach`. Classes de coleção genéricas dão suporte a <xref:System.Collections.Generic.IEnumerable%601> do mesmo modo que classes de coleção não genéricas, tais como <xref:System.Collections.ArrayList>, dão suporte a <xref:System.Collections.IEnumerable>.  
  
 Para obter mais informações sobre os genéricos, consulte [Genéricos](../../generics/index.md).  
  
## <a name="ienumerablet-variables-in-linq-queries"></a>Variáveis IEnumerable<T\> em consultas LINQ  
 As variáveis de consulta LINQ são digitadas como <xref:System.Collections.Generic.IEnumerable%601> ou um tipo derivado, como <xref:System.Linq.IQueryable%601> . Ao se deparar com uma variável de consulta que é tipada como `IEnumerable<Customer>`, significa apenas que a consulta, quando for executada, produzirá uma sequência de zero ou mais objetos `Customer`.  
  
 [!code-csharp[csLINQGettingStarted#34](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#34)]  
  
 Para obter mais informações, consulte [Relacionamentos de tipo em operações de consulta LINQ](./type-relationships-in-linq-query-operations.md).  
  
## <a name="letting-the-compiler-handle-generic-type-declarations"></a>Permitir que o compilador manipule as declarações de tipo genérico  
 Se preferir, poderá evitar a sintaxe genérica, usando a palavra-chave [var](../../../language-reference/keywords/var.md). A palavra-chave `var` instrui o compilador a inferir o tipo de uma variável de consulta, examinando a fonte de dados especificada na cláusula `from`. O exemplo a seguir produz o mesmo código compilado que o exemplo anterior:  
  
 [!code-csharp[csLINQGettingStarted#35](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#35)]  
  
 A palavra-chave `var` é útil quando o tipo da variável for óbvio ou quando não é tão importante especificar explicitamente os tipos genéricos aninhados, como aqueles que são produzidos por consultas de grupo. É recomendável que você note que o código poderá se tornar mais difícil de ser lido por outras pessoas, caso você use a `var`. Para obter mais informações, consulte [Variáveis locais de tipo implícito](../../classes-and-structs/implicitly-typed-local-variables.md).  
  
## <a name="see-also"></a>Confira também

- [Genéricos](../../generics/index.md)
