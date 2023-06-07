---
title: Como conectar-se a um banco de dados
description: Saiba como usar DataContext para se conectar a um banco de dados no LINQ to SQL. Consulte estes exemplos para usar DataContext para se conectar a um banco de dados e obter linhas.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: c33d74b3-530d-421b-a121-96786dd263a5
ms.openlocfilehash: c3320a598cb8407ab584530c615c2e5ef0de53c8
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286398"
---
# <a name="how-to-connect-to-a-database"></a>Como conectar-se a um banco de dados
O <xref:System.Data.Linq.DataContext> é o principal conduto pelo qual você se conecta a um banco de dados, recupera objetos deles e envia alterações de volta para ele. Você usa o da <xref:System.Data.Linq.DataContext> mesma maneira que usaria um ADO.NET <xref:System.Data.SqlClient.SqlConnection> . Na verdade, o <xref:System.Data.Linq.DataContext> é inicializado com uma conexão ou com uma cadeia de conexão que você fornece. Para obter mais informações, consulte [métodos DataContext (O/R Designer)](/visualstudio/data-tools/datacontext-methods-o-r-designer).  
  
 O objetivo do <xref:System.Data.Linq.DataContext> é converter suas solicitações de objetos em consultas SQL a serem feitas no banco de dados e montar objetos a partir dos resultados. O <xref:System.Data.Linq.DataContext> habilita o LINQ (consulta integrada à linguagem) implementando o mesmo padrão de operador que os operadores de consulta padrão, como `Where` e `Select` .  
  
> [!IMPORTANT]
> É muito importante manter uma conexão segura. Para obter mais informações, consulte [segurança em LINQ to SQL](security-in-linq-to-sql.md).  
  
## <a name="example"></a>Exemplo  
 No exemplo a seguir, o <xref:System.Data.Linq.DataContext> é usado para conexão ao banco de dados de exemplo Northwind e recuperar linhas de clientes cuja cidade é Londres.  
  
 [!code-csharp[DLinqCommunicatingWithDatabase#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqCommunicatingWithDatabase/cs/Program.cs#1)]
 [!code-vb[DLinqCommunicatingWithDatabase#1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqCommunicatingWithDatabase/vb/Module1.vb#1)]  
  
 Cada tabela do banco de dados é representada como uma coleção de `Table` disponível por meio do método <xref:System.Data.Linq.DataContext.GetTable%2A> usando a classe de entidade para identificá-la.  
  
## <a name="example"></a>Exemplo  
 A prática recomendada é declarar um <xref:System.Data.Linq.DataContext> fortemente tipado em vez de depender da classe básica <xref:System.Data.Linq.DataContext> e do método <xref:System.Data.Linq.DataContext.GetTable%2A>. Um <xref:System.Data.Linq.DataContext> fortemente tipado declara todas as coleções de `Table` como membros do contexto, como no exemplo a seguir.  
  
 [!code-csharp[DLinqCommunicatingWithDatabase#2](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqCommunicatingWithDatabase/cs/Program.cs#2)]
 [!code-vb[DLinqCommunicatingWithDatabase#2](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqCommunicatingWithDatabase/vb/Module1.vb#2)]  
  
 Você pode expressar a consulta para clientes de Londres de maneira mais simples, como:  
  
 [!code-csharp[DLinqCommunicatingWithDatabase#5](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqCommunicatingWithDatabase/cs/Program.cs#5)]
 [!code-vb[DLinqCommunicatingWithDatabase#5](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqCommunicatingWithDatabase/vb/Module1.vb#5)]  
  
## <a name="see-also"></a>Veja também

- [Comunicação com o banco de dados](communicating-with-the-database.md)
