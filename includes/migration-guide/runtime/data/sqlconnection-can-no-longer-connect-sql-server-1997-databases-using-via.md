---
ms.openlocfilehash: 241184d61d718fedfea396260e739d2dbc05c305
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619791"
---
### <a name="sqlconnection-can-no-longer-connect-to-sql-server-1997-or-databases-using-the-via-adapter"></a>SqlConnection não pode mais se conectar ao SQL Server 1997 ou a bancos de dados usando o adaptador VIA

#### <a name="details"></a>Detalhes

Conexões a bancos de dados do SQL Server usando o [Protocolo VIA (Virtual Interface Adapter)](https://docs.microsoft.com/previous-versions/sql/sql-server-2008-r2/ms191229(v=sql.105)) não têm mais suporte. O protocolo usado para se conectar a um banco de dados do SQL Server fica visível na cadeia de conexão. Uma conexão VIA conterá via:&lt;nomedoservidor&gt;. Se esse aplicativo estiver se conectando ao SQL por meio de um protocolo diferente de VIA (TCP: ou NP: por exemplo), nenhuma alteração significativa será encontrada. Além disso, não há mais suporte para conexões com SQL Server 7 (1997).

#### <a name="suggestion"></a>Sugestão

O protocolo VIA foi preterido, de modo que um protocolo alternativo deve ser usado para se conectar a bancos de dados SQL. O protocolo mais usado é o TCP/IP. Para obter mais informações sobre como se conectar por meio de TCP/IP, confira [Habilitar o protocolo TCP/IP para uma instância de banco de dados](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/bb909712(v=vs.90)). Se o banco de dados for acessado somente de dentro de uma intranet, o protocolo de pipes compartilhado poderá fornecer um melhor desempenho se a rede estiver lenta.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Data.SqlClient.SqlConnection.%23ctor(System.String)></li><li><xref:System.Data.SqlClient.SqlConnection.%23ctor(System.String,System.Data.SqlClient.SqlCredential)></li></ul>|
