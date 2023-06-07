---
ms.openlocfilehash: c967a5b09b5e9ffeee7bff046f0c96469bc7fb02
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85615589"
---
### <a name="clickonce-supports-sha-256-on-40-targeted-apps"></a>ClickOnce é compatível com SHA-256 em aplicativos destinados ao 4.0

#### <a name="details"></a>Detalhes

Anteriormente, um aplicativo ClickOnce com um certificado assinado com SHA-256 exigia a presença do .NET Framework 4.5 ou posterior, mesmo se o aplicativo fosse direcionado ao 4.0. Agora, os aplicativos ClickOnce direcionados ao .NET Framework 4.0 podem ser executados no .NET Framework 4.0, mesmo quando assinados com SHA-256.

#### <a name="suggestion"></a>Sugestão

Essa alteração elimina essa dependência e permite que certificados SHA-256 sejam usados para assinar aplicativos ClickOnce destinados ao .NET Framework 4 e versões anteriores.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.6         |
| Type    | Redirecionando |
