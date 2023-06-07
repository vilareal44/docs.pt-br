---
ms.openlocfilehash: 23ba6064a283b47312a66f3636c2834a7d58106e
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619772"
---
### <a name="adonet-now-attempts-to-automatically-reconnect-broken-sql-connections"></a>Agora, ADO.NET tentará reconectar automaticamente conexões SQL interrompidas

#### <a name="details"></a>Detalhes

A partir do .NET Framework 4.5.1, o .NET Framework tentará reconectar automaticamente conexões SQL interrompidas. Embora, normalmente, isso torne os aplicativos mais confiáveis, há casos de borda em que um aplicativo precisa saber que a conexão foi perdida para tomar medidas sobre a reconexão.

#### <a name="suggestion"></a>Sugestão

Se esse recurso for indesejável devido a preocupações de compatibilidade, ele poderá ser desabilitado por meio da configuração da propriedade <xref:System.Data.SqlClient.SqlConnectionStringBuilder.ConnectRetryCount?displayProperty=fullName> de uma cadeia de conexão (ou <xref:System.Data.SqlClient.SqlConnectionStringBuilder?displayProperty=fullName>) para 0.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5.1|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Data.IDbConnection.ConnectionString?displayProperty=nameWithType></li><li><xref:System.Data.SqlClient.SqlConnection.ConnectionString?displayProperty=nameWithType></li><li><xref:System.Configuration.ConnectionStringSettings.ConnectionString?displayProperty=nameWithType></li><li><xref:System.Data.Common.DbConnection.ConnectionString?displayProperty=nameWithType></li><li><xref:System.Data.Common.DbConnectionStringBuilder.ConnectionString?displayProperty=nameWithType></li><li><xref:System.Data.SqlClient.SqlConnectionStringBuilder.%23ctor></li><li><xref:System.Data.SqlClient.SqlConnectionStringBuilder.%23ctor(System.String)></li><li><xref:System.Data.Common.DbConnectionStringBuilder.%23ctor></li><li><xref:System.Data.Common.DbConnectionStringBuilder.%23ctor(System.Boolean)></li></ul>|
