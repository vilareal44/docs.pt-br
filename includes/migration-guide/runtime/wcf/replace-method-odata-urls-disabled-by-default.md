---
ms.openlocfilehash: b2fcacdb02c411c4dcb12051bf0c6759faccdea2
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619867"
---
### <a name="the-replace-method-in-odata-urls-is-disabled-by-default"></a>O método Replace nas URLs do OData é desabilitado por padrão

#### <a name="details"></a>Detalhes

A partir do .NET Framework 4.5, o método Replace em URLs do OData é desabilitado por padrão. Quando o Replace do OData está desabilitado (agora por padrão), qualquer solicitação de usuário, incluindo funções de substituição (que são incomuns), falha.

#### <a name="suggestion"></a>Sugestão

Se o método de substituição for necessário (o que é incomum), ele poderá ser reabilitado por meio das configurações (<xref:System.Data.Services.Configuration.DataServicesFeaturesSection.ReplaceFunction?displayProperty=fullName>). No entanto, um método de substituição habilitado pode dar abertura para vulnerabilidades de segurança, devendo ser usado somente depois de análise cuidadosa.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Data.Services.DataService%601?displayProperty=nameWithType></li></ul>|
