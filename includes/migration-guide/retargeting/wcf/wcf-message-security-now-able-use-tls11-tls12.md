---
ms.openlocfilehash: c646372104457e8bc5d418744847f3ee771c8d8b
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614297"
---
### <a name="wcf-message-security-now-is-able-to-use-tls11-and-tls12"></a>A segurança de mensagens do WCF agora pode usar o TLS1.1 e o TLS1.2

#### <a name="details"></a>Detalhes

A partir do .NET Framework 4.7, os clientes podem configurar o TLS1.1 ou o TLS1.2 na segurança de mensagens do WCF, além do SSL3.0 e o TLS1.0, por meio das configurações do aplicativo.

#### <a name="suggestion"></a>Sugestão

No .NET Framework 4.7, a compatibilidade com o TLS1.1 e o TLS1.2 na segurança de mensagens do WCF é desabilitada por padrão. É possível habilitá-la adicionando a seguinte linha à seção `<runtime>` do arquivo web.config:

```xml
<runtime>
<AppContextSwitchOverrides value="Switch.System.ServiceModel.DisableUsingServicePointManagerSecurityProtocols=false;Switch.System.Net.DontEnableSchUseStrongCrypto=false" />
</runtime>
```

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.7         |
| Type    | Redirecionando |
