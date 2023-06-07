---
ms.openlocfilehash: 416590c7bd959eea011b7e7c5db48f22d349d0f5
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85615590"
---
### <a name="apps-published-with-clickonce-that-use-a-sha-256-code-signing-certificate-may-fail-on-windows-2003"></a>Os aplicativos publicados com ClickOnce que usam um certificado de autenticação de código SHA-256 podem falhar no Windows 2003

#### <a name="details"></a>Detalhes

O executável é assinado com SHA256. Antes, ele era assinado com SHA1, independentemente se o certificado de assinatura de código era SHA-1 ou SHA-256. Isso se aplica a:

- Todos os aplicativos compilados com o Visual Studio 2012 ou posterior.
- Os aplicativos compilados com o Visual Studio 2010 ou anteriores em sistemas com o .NET Framework 4.5.
Além disso, se o .NET Framework 4.5 ou posterior estiver presente, o manifesto do ClickOnce também será assinado com certificados SHA-256 para SHA-256, independentemente da versão do .NET Framework na qual ele foi compilado.

#### <a name="suggestion"></a>Sugestão

A mudança na assinatura do executável do ClickOnce afeta apenas os sistemas Windows Server 2003; eles exigem a instalação do KB 938397. A alteração na assinatura do manifesto com SHA-256, mesmo quando um aplicativo destina-se ao .NET Framework 4.0 ou versões anteriores, introduz uma dependência de runtime no .NET Framework 4.5 ou uma versão posterior.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.5         |
| Type    | Redirecionando |
