---
title: 'Mitigação: período de bloqueio de pool'
description: Saiba como atenuar os problemas causados pelo período de bloqueio do pool de conexões que está sendo removido para conexões com bancos de dados SQL do Azure.
ms.date: 03/30/2017
ms.assetid: 92d2de20-79be-4df1-b182-144143a8866a
ms.openlocfilehash: be60fe87952697d964571176743a4e6f839c4894
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86475405"
---
# <a name="mitigation-pool-blocking-period"></a>Mitigação: período de bloqueio de pool
O pool de conexão no período de bloqueio foi removido para conexões com bancos de dados SQL do Azure.  
  
## <a name="additional-description"></a>Descrição adicional  
 No .NET Framework 4.6.1 e versões anteriores, quando um aplicativo encontra uma falha de conexão transitória ao se conectar a um banco de dados, a tentativa de conexão não pode ser recuperada rapidamente, pois o pool de conexões armazena em cache o erro e o gera novamente por 5 segundos a 1 minuto. Para saber mais, confira [SQL Server Connection Pooling (ADO.NET)](../data/adonet/sql-server-connection-pooling.md) (Pool de conexão do SQL Server (ADO.NET)). Esse comportamento é problemático para conexões com bancos de dados SQL do Azure que, muitas vezes, falham com erros transitórios que normalmente são recuperados em alguns segundos. O recurso de bloqueio do pool de conexões significa que o aplicativo não pode se conectar ao banco de dados por um período extenso, mesmo que o banco de dados esteja disponível. Esse comportamento é especialmente problemático para aplicativos Web que se conectam aos bancos de dados SQL do Azure e que precisam renderizar em poucos segundos.  
  
 Do .NET Framework 4.6.2 em diante, para solicitações de abertura da conexão com bancos de dados SQL do Azure conhecidos (*.database.windows.net, \*.database.chinacloudapi.cn, \*.database.usgovcloudapi.net, \*.database.cloudapi.de), os erros de abertura da conexão não são armazenados em cache. Para todas as outras tentativas de conexão, o período de bloqueio do pool de conexões continuará sendo imposto.  
  
## <a name="impact"></a>Impacto  
 Essa alteração permite que a tentativa de abertura da conexão seja repetida imediatamente para bancos de dados SQL do Azure, melhorando, assim, o desempenho dos aplicativos habilitados para a nuvem.  
  
## <a name="mitigation"></a>Atenuação  
 Para aplicativos que são afetados negativamente por essa alteração, o período de bloqueio do pool de conexões pode ser configurado pela definição da nova propriedade <xref:System.Data.SqlClient.SqlConnectionStringBuilder.PoolBlockingPeriod%2A?displayProperty=nameWithType>.  O valor da propriedade é membro da enumeração <xref:System.Data.SqlClient.PoolBlockingPeriod?displayProperty=nameWithType> que pode assumir um dos três valores:  
  
- <xref:System.Data.SqlClient.PoolBlockingPeriod.AlwaysBlock?displayProperty=nameWithType>
  
- <xref:System.Data.SqlClient.PoolBlockingPeriod.Auto?displayProperty=nameWithType>
  
- <xref:System.Data.SqlClient.PoolBlockingPeriod.NeverBlock?displayProperty=nameWithType>
  
 É possível restaurar o comportamento anterior definindo a propriedade <xref:System.Data.SqlClient.SqlConnectionStringBuilder.PoolBlockingPeriod%2A> como <xref:System.Data.SqlClient.PoolBlockingPeriod.AlwaysBlock?displayProperty=nameWithType>.  
  
## <a name="see-also"></a>Consulte também

- [Compatibilidade de aplicativos](application-compatibility.md)
