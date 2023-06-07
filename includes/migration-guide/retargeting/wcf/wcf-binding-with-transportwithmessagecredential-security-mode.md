---
ms.openlocfilehash: 0e949cbdeda99dd7b94e919b903a21171a57f527
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614285"
---
### <a name="wcf-binding-with-the-transportwithmessagecredential-security-mode"></a>Associação do WCF com o modo de segurança TransportWithMessageCredential

#### <a name="details"></a>Detalhes

Começando com o .NET Framework 4.6.1, a associação do WCF que usar o modo de segurança TransportWithMessageCredential poderá ser configurada para receber mensagens com cabeçalhos &quot;to&quot; não assinados para chaves de segurança assimétricas. Por padrão, os cabeçalhos &quot;to&quot; não assinados continuarão a ser rejeitados no NET Framework 4.6.1. Eles só serão aceitos se o aplicativo aceitar esse novo modo de operação usando a opção de configuração Switch.System.ServiceModel.AllowUnsignedToHeader.

#### <a name="suggestion"></a>Sugestão

Como esse é um recurso de aceitação, ele não deve afetar o comportamento dos aplicativos existentes.<br/>Para controlar se o novo comportamento está sendo usado ou não, use a seguinte configuração:

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.ServiceModel.AllowUnsignedToHeader=true" />
</runtime>
```

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Transparente |
| Versão | 4.6.1       |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.ServiceModel.BasicHttpSecurityMode.TransportWithMessageCredential?displayProperty=nameWithType>
- <xref:System.ServiceModel.BasicHttpsSecurityMode.TransportWithMessageCredential?displayProperty=nameWithType>
- <xref:System.ServiceModel.SecurityMode.TransportWithMessageCredential?displayProperty=nameWithType>
- <xref:System.ServiceModel.WSFederationHttpSecurityMode.TransportWithMessageCredential?displayProperty=nameWithType>
