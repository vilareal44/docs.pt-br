---
ms.openlocfilehash: 0fe07ac21effacffc56d37ccb46a121f443acd20
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619770"
---
### <a name="sharing-session-state-with-aspnet-stateserver-requires-all-servers-in-the-web-farm-to-use-the-same-net-framework-version"></a>Compartilhamento de estado de sessão com StateServer Asp.Net exige que todos os servidores no web farm usem a mesma versão do .NET Framework

#### <a name="details"></a>Detalhes

Ao habilitar o estado de sessão <xref:System.Web.SessionState.SessionStateMode.StateServer?displayProperty=fullName>, todos os servidores no web farm fornecido devem usar a mesma versão do .NET Framework para o estado ser compartilhado corretamente.

#### <a name="suggestion"></a>Sugestão

Certifique-se de atualizar as versões do .NET Framework em servidores Web que compartilham o estado ao mesmo tempo.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Web.SessionState.SessionStateMode.StateServer?displayProperty=nameWithType></li></ul>|
