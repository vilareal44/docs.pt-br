---
title: Protegendo aplicativos de canal par
ms.date: 03/30/2017
ms.assetid: d4a0311d-3f78-4525-9c4b-5c93c4492f28
ms.openlocfilehash: a77449710e9093bc8ea2d5446e6359c26a3d1c1e
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84589871"
---
# <a name="securing-peer-channel-applications"></a>Protegendo aplicativos de canal par
Como outras associações no WinFX, o `NetPeerTcpBinding` tem segurança habilitada por padrão e oferece segurança baseada em transporte e mensagem (ou ambos). Este tópico aborda esses dois tipos de segurança. O tipo de segurança é especificado pela marca de modo de segurança na especificação de ligação ( <xref:System.ServiceModel.NetPeerTcpBinding.Security%2A> `Mode` ).  
  
## <a name="transport-based-security"></a>Segurança baseada em transporte  
 O canal par dá suporte a dois tipos de credenciais de autenticação para proteger o transporte, e ambos exigem a configuração da `ClientCredentialSettings.Peer` propriedade no associado `ChannelFactory` :  
  
- Senha. Os clientes usam o conhecimento de uma senha secreta para autenticar conexões. Quando esse tipo de credencial é usado, o `ClientCredentialSettings.Peer.MeshPassword` deve transportar uma senha válida e, opcionalmente, uma `X509Certificate2` instância.  
  
- Certificado. A autenticação de aplicativo específica é usada. Quando esse tipo de credencial é usado, você deve usar uma implementação concreta do <xref:System.IdentityModel.Selectors.X509CertificateValidator> no `ClientCredentialSettings.Peer.PeerAuthentication` .  
  
## <a name="message-based-security"></a>Segurança baseada em mensagem  
 Usando a segurança de mensagem, um aplicativo pode assinar mensagens de saída para que todas as partes receptoras possam verificar se a mensagem foi enviada por uma parte confiável e que a mensagem não foi violada. Atualmente, o canal par dá suporte apenas à assinatura de mensagem de credencial X. 509.  
  
## <a name="best-practices"></a>Práticas Recomendadas  
  
- Esta seção aborda as práticas recomendadas para proteger os aplicativos de canal par.  
  
### <a name="enable-security-with-peer-channel-applications"></a>Habilitar a segurança com aplicativos de canal de emparelhamento  
 Devido à natureza distribuída dos protocolos de canal de par, é difícil impor a associação, a confidencialidade e a privacidade da malha em uma malha não segura. Também é importante lembrar-se de proteger a comunicação entre clientes e o serviço resolvedor. Em PNRP (Peer Name Resolution Protocol), use nomes seguros para evitar falsificação e outros ataques comuns. Proteger um serviço de resolvedor personalizado habilitando a segurança na conexão que os clientes usam para entrar em contato com o serviço de resolvedor, incluindo segurança baseada em mensagem e transporte.  
  
### <a name="use-the-strongest-possible-security-model"></a>Usar o modelo de segurança mais forte possível  
 Por exemplo, se cada membro da malha precisar ser identificado individualmente, use o modelo de autenticação baseada em certificado. Se isso não for possível, use a autenticação baseada em senha seguindo as recomendações atuais para mantê-las seguras. Isso inclui o compartilhamento de senhas somente com partes confiáveis, a transmissão de senhas usando uma mídia segura, a alteração de senhas com frequência e a garantia de que as senhas sejam fortes (pelo menos oito caracteres, inclua pelo menos uma letra de ambos os casos, um dígito e um caractere especial).  
  
### <a name="never-accept-self-signed-certificates"></a>Nunca aceitar certificados autoassinados  
 Nunca aceite uma credencial de certificado com base em nomes de entidades. Observe que qualquer pessoa pode criar um certificado e qualquer pessoa pode escolher um nome que você está validando. Para evitar a possibilidade de falsificação, valide certificados com base em credenciais de autoridade emissora (um emissor confiável ou uma autoridade de certificação raiz).  
  
### <a name="use-message-authentication"></a>Usar autenticação de mensagens  
 Use a autenticação de mensagens para verificar se uma mensagem foi originada de uma fonte confiável e se ninguém foi adulterada com a mensagem durante a transmissão. Sem a autenticação de mensagens, é fácil para um cliente mal-intencionado falsificar ou adulterar mensagens na malha.  
  
## <a name="peer-channel-code-examples"></a>Exemplos de código de canal par  
 [Cenários de canal par](peer-channel-scenarios.md)  
  
## <a name="see-also"></a>Consulte também

- [Segurança de canal par](peer-channel-security.md)
- [Compilando um aplicativo de canal par](building-a-peer-channel-application.md)
