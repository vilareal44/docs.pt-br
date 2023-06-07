---
title: 'Mitigação: protocolos TLS'
description: Saiba mais sobre o impacto e a mitigação das alterações do protocolo TLS a partir do .NET Framework 4,6.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 33f97d13-3022-43da-8b18-cdb5c88df9c2
ms.openlocfilehash: bb5aab3361663d7b5401d7e68688265fbc65b36f
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86475353"
---
# <a name="mitigation-tls-protocols"></a>Mitigação: protocolos TLS
A partir do .NET Framework 4.6, as classes <xref:System.Net.ServicePointManager?displayProperty=nameWithType> e <xref:System.Net.Security.SslStream?displayProperty=nameWithType> têm permissão para usar um dos três protocolos a seguir: Tls1.0, Tls1.1 ou Tls 1.2. Não há suporte para o protocolo SSL 3.0 e a criptografia RC4.  
  
## <a name="impact"></a>Impacto  
 Essa alteração afeta:  
  
- Qualquer aplicativo que usa o SSL para se comunicar com um servidor HTTPS ou um servidor de soquete usando qualquer um dos seguintes tipos: <xref:System.Net.Http.HttpClient>, <xref:System.Net.HttpWebRequest>, <xref:System.Net.FtpWebRequest>, <xref:System.Net.Mail.SmtpClient> e <xref:System.Net.Security.SslStream>.  
  
- Qualquer aplicativo do lado do servidor que não possa ser atualizado para dar suporte ao Tls1.0, Tls1.1 ou Tls 1.2.  
  
## <a name="mitigation"></a>Atenuação  
 A mitigação recomendada é fazer upgrade do aplicativo do lado do servidor para Tls1.0, Tls1.1 ou Tls 1.2. Se não for viável ou se os aplicativos cliente estiverem desfeitos, a classe <xref:System.AppContext> poderá ser usada para recusar esse recurso de duas maneiras:  
  
- De modo programático, usando um snippet de código como o seguinte:  
  
     [!code-csharp[AppCompat.SSLProtocols#1](../../../samples/snippets/csharp/VS_Snippets_CLR/appcompat.sslprotocols/cs/program.cs#1)]
     [!code-vb[AppCompat.SSLProtocols#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/appcompat.sslprotocols/vb/module1.vb#1)]  
  
     Como o objeto <xref:System.Net.ServicePointManager> foi inicializado apenas uma vez, definir essas configurações de compatibilidade deverá ser a primeira coisa que o aplicativo fará.  
  
- Adicionando a seguinte linha à [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) seção do arquivo de app.config:  
  
    ```xml  
    <AppContextSwitchOverrides value="Switch.System.Net.DontEnableSchUseStrongCrypto=true"/>  
    ```  
  
 No entanto, observe que não é recomendável recusar o comportamento padrão, pois isso torna o aplicativo menos seguro.  
  
## <a name="see-also"></a>Consulte também

- [Compatibilidade de aplicativos](application-compatibility.md)
