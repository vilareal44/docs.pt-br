---
title: 'Exemplos de sintaxe da consulta com base em método: Adição a operadores'
description: Use estes exemplos para aprender a usar os métodos Join e GroupJoin para consultar um modelo usando a sintaxe de consulta baseada em método em LINQ to Entities.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: d00f0efa-9084-4c17-843f-54904fcb4204
ms.openlocfilehash: 3b1445b39bdcd9a9b4d0672be0598233319cb85d
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286825"
---
# <a name="method-based-query-syntax-examples-join-operators"></a>Exemplos de sintaxe da consulta com base em método: Adição a operadores
Os exemplos neste tópico demonstram como usar os <xref:System.Linq.Enumerable.Join%2A> métodos e <xref:System.Linq.Enumerable.GroupJoin%2A> para consultar o [modelo de vendas AdventureWorks](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks) usando a sintaxe de consulta baseada em método. O Modelo de vendas AdventureWorks usado nesses exemplos é criado a partir das tabelas Contact, Address, Product, SalesOrderHeader e SalesOrderDetail no banco de dados de exemplo AdventureWorks.  
  
 Os exemplos neste tópico usam as seguintes `using` / `Imports` instruções:  
  
 [!code-csharp[DP L2E Examples#ImportsUsing](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Examples/CS/Program.cs#importsusing)]
 [!code-vb[DP L2E Examples#ImportsUsing](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Examples/VB/Module1.vb#importsusing)]  
  
## <a name="groupjoin"></a>GroupJoin  
  
### <a name="example"></a>Exemplo  
 O exemplo a seguir executa um <xref:System.Linq.Enumerable.GroupJoin%2A> nas tabelas SalesOrderHeader e SalesOrderDetail para localizar o número de pedidos por cliente. Um group join é o equivalente a um left outer join, que retorna cada elemento da primeira fonte de dados (esquerda), mesmo se nenhum elemento correlacionado estiver na outra fonte de dados.  
  
 [!code-csharp[DP L2E Examples#GroupJoin2_MQ](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Examples/CS/Program.cs#groupjoin2_mq)]
 [!code-vb[DP L2E Examples#GroupJoin2_MQ](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Examples/VB/Module1.vb#groupjoin2_mq)]  
  
### <a name="example"></a>Exemplo  
 O exemplo a seguir executa um <xref:System.Linq.Enumerable.GroupJoin%2A> nas tabelas Contact e SalesOrderHeader para localizar o número de pedidos por contato. A contagem do pedido e as identificações para cada contato são exibidos.  
  
 [!code-csharp[DP L2E Examples#GroupJoin_MQ](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Examples/CS/Program.cs#groupjoin_mq)]
 [!code-vb[DP L2E Examples#GroupJoin_MQ](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Examples/VB/Module1.vb#groupjoin_mq)]  
  
## <a name="join"></a>Join  
  
### <a name="example"></a>Exemplo  
 O exemplo a seguir executa uma união sobre as tabelas de contatos e de SalesOrderHeader.  
  
 [!code-csharp[DP L2E Examples#JoinSimple_MQ](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Examples/CS/Program.cs#joinsimple_mq)]
 [!code-vb[DP L2E Examples#JoinSimple_MQ](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Examples/VB/Module1.vb#joinsimple_mq)]  
  
### <a name="example"></a>Exemplo  
 O exemplo a seguir executa uma união sobre as tabelas de contatos e de SalesOrderHeader, agrupamento os resultados pela identificação de contatos  
  
 [!code-csharp[DP L2E Examples#JoinWithGroupedResults_MQ](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Examples/CS/Program.cs#joinwithgroupedresults_mq)]
 [!code-vb[DP L2E Examples#JoinWithGroupedResults_MQ](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Examples/VB/Module1.vb#joinwithgroupedresults_mq)]  
  
## <a name="see-also"></a>Veja também

- [Consultas no LINQ to Entities](queries-in-linq-to-entities.md)
