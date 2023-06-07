---
ms.openlocfilehash: 768a948849064cedb38110f5ed271717442325c0
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619797"
---
### <a name="log-file-name-created-by-the-objectcontextcreatedatabase-method-has-changed-to-match-sql-server-specifications"></a>O nome do arquivo de log criado pelo método ObjectContext.CreateDatabase foi alterado para corresponder às especificações do SQL Server

#### <a name="details"></a>Detalhes

Quando o método <xref:System.Data.Objects.ObjectContext.CreateDatabase?displayProperty=fullName> é chamado diretamente ou usando Code First com o provedor SqlClient e um valor AttachDBFilename na cadeia de conexão, ele cria um arquivo de log chamado filename_log.ldf em vez de filename.ldf (onde filename é o nome do arquivo especificado pelo valor AttachDBFilename). Essa alteração melhora a depuração ao fornecer um arquivo de log chamado de acordo com especificações do SQL Server.

#### <a name="suggestion"></a>Sugestão

Se o nome do arquivo de log for importante para um aplicativo, o aplicativo deverá ser atualizado para esperar o formato de nome de arquivo _log.ldf padrão.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Data.Objects.ObjectContext.CreateDatabase?displayProperty=nameWithType></li></ul>|
