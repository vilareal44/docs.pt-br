---
title: Conectando-se a uma fonte de dados
deescription: Learn about Connection objects, used to connect to data sources in ADO.NET. The Connection object you choose depends on the type of data source.
ms.date: 03/30/2017
ms.assetid: 9abc3f92-1be3-4e1a-b360-762dc689650e
ms.openlocfilehash: a14fe179cf2fc8714a54e52252c53bd71346cad3
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84287071"
---
# <a name="connecting-to-a-data-source-in-adonet"></a>Conectando-se a uma fonte de dados no ADO.NET

No ADO.NET, você usa um objeto de **conexão** para se conectar a uma fonte de dados específica fornecendo informações de autenticação necessárias em uma cadeia de conexão. O objeto de **conexão** que você usa depende do tipo de fonte de dados.  
  
 Cada provedor de dados .NET Framework incluído no .NET Framework tem um objeto <xref:System.Data.Common.DbConnection>: o Provedor de Dados .NET Framework para OLE DB inclui um objeto <xref:System.Data.OleDb.OleDbConnection>, o Provedor de Dados .NET Framework para SQL Server inclui um objeto <xref:System.Data.SqlClient.SqlConnection>, o Provedor de Dados .NET Framework para ODBC inclui um objeto <xref:System.Data.Odbc.OdbcConnection> e o Provedor de Dados .NET Framework para Oracle inclui um objeto <xref:System.Data.OracleClient.OracleConnection>.  
  
## <a name="in-this-section"></a>Nesta seção  
 [Estabelecendo a conexão](establishing-the-connection.md)\
 Descreve como usar um objeto de **conexão** para estabelecer uma conexão com uma fonte de dados.  
  
 [Eventos de conexão](connection-events.md)\
 Descreve como usar um evento **InfoMessage** para recuperar mensagens informativas de uma fonte de dados.  
  
## <a name="see-also"></a>Veja também

- [Cadeias de conexão](connection-strings.md)
- [Pool de conexões](connection-pooling.md)
- [Comandos e parâmetros](commands-and-parameters.md)
- [DataAdapters e DataReaders](dataadapters-and-datareaders.md)
- [Transações e simultaneidade](transactions-and-concurrency.md)
- [Visão geral do ADO.NET](ado-net-overview.md)
