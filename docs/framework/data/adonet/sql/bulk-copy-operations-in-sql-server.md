---
title: Operações de cópia em massa no SQL Server
description: Aprenda a usar a classe SqlBulkCopy para escrever soluções de código gerenciado que copiam arquivos grandes em massa em tabelas ou exibições em bancos de dados SQL Server.
ms.date: 03/30/2017
ms.assetid: 83a7a0d2-8018-4354-97b9-0b1d99f8342b
ms.openlocfilehash: 4f877836aa45efe162cce3c42cb5733f86deab2c
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286514"
---
# <a name="bulk-copy-operations-in-sql-server"></a>Operações de cópia em massa no SQL Server
O Microsoft SQL Server inclui um utilitário de linha de comando popular chamado **bcp** para copiar rapidamente grandes arquivos em massa em tabelas ou exibições em bancos de dados do SQL Server. A classe <xref:System.Data.SqlClient.SqlBulkCopy> permite escrever soluções de código gerenciado que fornecem funcionalidade semelhante. Há outras maneiras de carregar dados em uma tabela do SQL Server (instruções INSERT, por exemplo), mas <xref:System.Data.SqlClient.SqlBulkCopy> oferece uma vantagem de desempenho significativa sobre eles.  
  
 A classe <xref:System.Data.SqlClient.SqlBulkCopy> pode ser usada para gravar dados somente em tabelas do SQL Server. Mas a fonte de dados não está limitada a SQL Server; qualquer fonte de dados pode ser usada, desde que os dados possam ser carregados em uma <xref:System.Data.DataTable> instância ou lidos com uma <xref:System.Data.IDataReader> instância do.  
  
 Usando a classe <xref:System.Data.SqlClient.SqlBulkCopy>, você pode executar:  
  
- Uma única operação de cópia em massa  
  
- Várias operações de cópia em massa  
  
- Uma operação de cópia em massa dentro de uma transação  
  
> [!NOTE]
> Ao usar o .NET Framework versão 1.1 ou anterior (que não dá suporte à classe <xref:System.Data.SqlClient.SqlBulkCopy>), você pode executar a instrução **BULK INSERT** do Transact-SQL no SQL Server usando o objeto <xref:System.Data.SqlClient.SqlCommand>.  
  
## <a name="in-this-section"></a>Nesta seção  
 [Configuração de exemplo de cópia em massa](bulk-copy-example-setup.md)  
 Descreve as tabelas usadas nos exemplos de cópia em massa e fornece scripts SQL para criar as tabelas no banco de dados AdventureWorks.  
  
 [Operações de cópia em massa única](single-bulk-copy-operations.md)  
 Descreve como fazer uma cópia em massa de dados em uma instância do SQL Server usando a classe <xref:System.Data.SqlClient.SqlBulkCopy> e como executar a operação de cópia em massa usando instruções Transact-SQL e a classe <xref:System.Data.SqlClient.SqlCommand>.  
  
 [Várias operações de cópia em massa](multiple-bulk-copy-operations.md)  
 Descreve como realizar várias operações de cópia em massa de dados em uma instância do SQL Server usando a classe <xref:System.Data.SqlClient.SqlBulkCopy>.  
  
 [Operações de cópia em massa e transação](transaction-and-bulk-copy-operations.md)  
 Descreve como executar uma operação de cópia em massa em uma transação, incluindo como fazer commit ou reverter a transação.  
  
## <a name="see-also"></a>Veja também

- [SQL Server e ADO.NET](index.md)
- [Visão geral do ADO.NET](../ado-net-overview.md)
