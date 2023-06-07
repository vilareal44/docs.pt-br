---
ms.openlocfilehash: 986b647047aaa4a185c1403e96e499ae587bea98
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617517"
---
### <a name="httpruntimeappdomainapppath-throws-a-nullreferenceexception"></a>HttpRuntime.AppDomainAppPath gera NullReferenceException

#### <a name="details"></a>Detalhes

No .NET Framework 4.6.2, o runtime gera `T:System.NullReferenceException` ao recuperar um valor `P:System.Web.HttpRuntime.AppDomainAppPath` que inclui caracteres nulos. No .NET Framework 4.6.1 e versões anteriores, o runtime gera `T:System.ArgumentNullException`.

#### <a name="suggestion"></a>Sugestão

Você pode fazer o seguinte para responder a essa alteração:

- Identifique o `T:System.NullReferenceException` se o aplicativo estiver sendo executado no .NET Framework 4.6.2.
- Atualize para o .NET Framework 4.7, que restaura o comportamento anterior e gera `T:System.ArgumentNullException`.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.6.2       |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Web.HttpRuntime.AppDomainAppPath?displayProperty=nameWithType>
