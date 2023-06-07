---
title: Como executar diretamente consultas SQL
description: Saiba como usar ExecuteQuery para executar uma consulta e, em seguida, converter os resultados diretamente em objetos em casos em que uma consulta de LINQ to SQL é insuficiente.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: e491b9bf-741a-4296-9f51-76c25ddf6a82
ms.openlocfilehash: 59bd404e41f6be1181d6a625c31ee23358db0df3
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286359"
---
# <a name="how-to-directly-execute-sql-queries"></a>Como executar diretamente consultas SQL
O [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] converte as consultas que você escreve em consultas SQL parametrizadas (em formato de texto) e as envia para o SQL Server para processamento.  
  
 O SQL não pode executar a variedade de métodos que podem estar disponíveis localmente para seu aplicativo. O [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] tenta converter esses métodos locais em operações e funções equivalentes que estão disponíveis no ambiente SQL. A maioria dos métodos e operadores em .NET Framework tipos internos têm traduções diretas para comandos SQL. Alguns podem ser gerados das funções que estão disponíveis. Aqueles que não podem ser produzidos geram exceções em tempo de execução. Para obter mais informações, consulte [mapeamento de tipo SQL-CLR](sql-clr-type-mapping.md).  
  
 Nos casos em que uma consulta de [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] é insuficiente para uma tarefa especializada, você pode usar o método <xref:System.Data.Linq.DataContext.ExecuteQuery%2A> para executar uma consulta SQL e, depois, converter o resultado da consulta diretamente em objetos.  
  
## <a name="example"></a>Exemplo  
 No exemplo a seguir, suponha que os dados da classe `Customer` são espalhados em duas tabelas (customer1 e customer2). A consulta retorna uma sequência de objetos `Customer`.  
  
 [!code-csharp[DLinqQuerying#4](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQuerying/cs/Program.cs#4)]
 [!code-vb[DLinqQuerying#4](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQuerying/vb/Module1.vb#4)]  
  
 Desde que os nomes de coluna nos resultados de tabela correspondam às propriedades da coluna de sua classe de entidade, o [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] cria seus objetos fora de qualquer consulta SQL.  
  
## <a name="example"></a>Exemplo  
 O método <xref:System.Data.Linq.DataContext.ExecuteQuery%2A> também permite parâmetros. Use um código como o mostrado a seguir para executar uma consulta parametrizada.  
  
 [!code-csharp[DLinqQuerying#5](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQuerying/cs/Program.cs#5)]
 [!code-vb[DLinqQuerying#5](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQuerying/vb/Module1.vb#5)]  
  
 Os parâmetros são expressos no texto da consulta usando as mesmas chaves usadas por `Console.WriteLine()` e `String.Format()`. Na verdade, `String.Format()` é realmente chamado na cadeia de caracteres de consulta que você fornece, substituindo os parâmetros de chaves com nomes de parâmetro gerados, como @p0 , @p1 ..., @p (n).  
  
## <a name="see-also"></a>Veja também

- [Informações básicas](background-information.md)
- [Consultando o banco de dados](querying-the-database.md)
