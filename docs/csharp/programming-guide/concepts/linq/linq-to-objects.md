---
title: LINQ to Objects (C#)
description: Saiba mais sobre LINQ to Objects em C#, que usa consultas LINQ com qualquer coleção IEnumerable ou IEnumerable <T> sem um provedor LINQ intermediário ou API.
ms.date: 07/20/2015
ms.assetid: c5c2c178-3529-4f6c-b3df-2d5267af7f22
ms.openlocfilehash: 7b67690ee13f207441bc94155acd91047b63b3df
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87165554"
---
# <a name="linq-to-objects-c"></a>LINQ to Objects (C#)

O termo "LINQ to Objects" refere-se ao uso de consultas LINQ com qualquer coleção <xref:System.Collections.IEnumerable> ou <xref:System.Collections.Generic.IEnumerable%601> diretamente, sem o uso de uma API ou provedor LINQ intermediário como o [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md) ou [LINQ to XML](./linq-to-xml-overview.md). Você pode usar o LINQ para consultar qualquer coleção enumerável ​​como <xref:System.Collections.Generic.List%601>, <xref:System.Array> ou <xref:System.Collections.Generic.Dictionary%602>. A coleção pode ser definida pelo usuário ou pode ser retornada por uma API do .NET.  
  
 Basicamente, o LINQ to Objects representa uma nova abordagem às coleções. Na forma antiga, você precisava escrever loops `foreach` complexos que especificavam como recuperar dados de uma coleção. Na abordagem da LINQ, você escreve o código declarativo que descreve o que você deseja recuperar.  
  
 Além disso, as consultas LINQ oferecem três principais vantagens sobre os loops `foreach` tradicionais:  
  
- Elas são mais concisas e legíveis, especialmente quando você filtra várias condições.  
  
- Elas fornecem poderosos recursos de filtragem, ordenação e agrupamento com um mínimo de código do aplicativo.  
  
- Elas podem ser movidas para outras fontes de dados com pouca ou nenhuma modificação.  
  
 Em geral, quanto mais complexa for a operação que você deseja executar nos dados, mais benefícios você perceberá usando LINQ em vez de técnicas de iteração tradicionais.  
  
 O objetivo desta seção é demonstrar a abordagem LINQ com alguns exemplos selecionados. Não tem a intenção de ser exaustiva.  
  
## <a name="in-this-section"></a>Nesta seção  
 [LINQ e cadeias de caracteres (C#)](./linq-and-strings.md)  
 Explica como a LINQ pode ser usada para consultar e transformar cadeias de caracteres e coleções de cadeias de caracteres. Também inclui links para artigos que demonstram esses princípios.  
  
 [LINQ e reflexão (C#)](how-to-query-an-assembly-s-metadata-with-reflection-linq.md)  
 Contém um link para um exemplo que demonstra como a LINQ usa a reflexão.  
  
 [LINQ e diretórios de arquivos (C#)](./linq-and-file-directories.md)  
 Explica como a LINQ pode ser usada para interagir com sistemas de arquivos. Também inclui links para artigos que demonstram esses conceitos.  
  
 [Como consultar uma ArrayList com LINQ (C#)](./how-to-query-an-arraylist-with-linq.md)  
 Demonstra como consultar um ArrayList no C#.  
  
 [Como adicionar métodos personalizados para consultas LINQ (C#)](./how-to-add-custom-methods-for-linq-queries.md)  
 Explica como estender o conjunto de métodos que você pode usar para consultas LINQ, adicionando os métodos de extensão à interface <xref:System.Collections.Generic.IEnumerable%601>.  
  
 [LINQ (Consulta Integrada à Linguagem) (C#)](./index.md)  
 Fornece links para artigos que explicam o LINQ e fornecem exemplos de código que executa consultas.
