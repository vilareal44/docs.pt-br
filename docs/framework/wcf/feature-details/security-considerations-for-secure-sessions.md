---
title: Considerações de segurança para sessões seguras
ms.date: 03/30/2017
ms.assetid: 0d5be591-9a7b-4a6f-a906-95d3abafe8db
ms.openlocfilehash: 587897cc296523e0bfd5a4d4fa50b1e145cb69fb
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84601043"
---
# <a name="security-considerations-for-secure-sessions"></a>Considerações de segurança para sessões seguras
Você deve considerar os itens a seguir que afetam a segurança ao implementar sessões seguras. Para obter mais informações sobre considerações de segurança, consulte [considerações de segurança](security-considerations-in-wcf.md) e [práticas recomendadas de segurança](best-practices-for-security-in-wcf.md).  
  
## <a name="secure-sessions-and-metadata"></a>Proteger sessões e metadados  
 Quando uma sessão segura é estabelecida e a <xref:System.ServiceModel.Security.Tokens.SecureConversationSecurityTokenParameters.RequireCancellation%2A> propriedade é definida como `false` , Windows Communication Foundation (WCF) envia uma `mssp:MustNotSendCancel` asserção como parte dos metadados no documento WSDL (Web Services Description Language) para o ponto de extremidade de serviço. A `mssp:MustNotSendCancel` declaração informa aos clientes que o serviço não responde às solicitações para cancelar a sessão segura. Quando a <xref:System.ServiceModel.Security.Tokens.SecureConversationSecurityTokenParameters.RequireCancellation%2A> propriedade é definida como `true` , o WCF não emite uma `mssp:MustNotSendCancel` ASSERÇÃO no documento WSDL. Espera-se que os clientes enviem uma solicitação de cancelamento ao serviço quando não precisarem mais da sessão segura. Quando um cliente é gerado usando a [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md), o código do cliente reage adequadamente à presença ou à ausência da `mssp:MustNotSendCancel` asserção.  
  
## <a name="secure-conversations-and-custom-tokens"></a>Conversas seguras e tokens personalizados  
 Há alguns problemas com a combinação de tokens personalizados e chaves derivadas devido à maneira como ele é definido na especificação WS-SecureConversation. A especificação diz que `wsse:SecurityTokenReference` é um elemento opcional que faz referência ao token derivado: " `/wsc:DerivedKeyToken/wsse:SecurityTokenReference` este elemento opcional é usado para especificar token de contexto de segurança, token de segurança ou chave compartilhada/segredo usado para a derivação. Se não for especificado, supõe-se que o destinatário possa determinar a chave compartilhada a partir do contexto da mensagem. Se o contexto não puder ser determinado, uma falha como `wsc:UnknownDerivationSource` deve ser gerada. "  
  
 Isso significa que, se você quiser que um token personalizado seja derivado, deverá encapsular seu tipo de cláusula em um `SecurityTokenReference` elemento. Há uma opção para desativar a derivação, mas o padrão é derivar chaves. Se você falhar ao encapsular a chave, a serialização do token de chave derivada será bem-sucedida, mas a tentativa de desserializar ele lançará uma exceção.  
  
## <a name="see-also"></a>Consulte também

- [Como desabilitar sessões seguranças em uma WSFederationHttpBinding](how-to-disable-secure-sessions-on-a-wsfederationhttpbinding.md)
- [Considerações sobre segurança](security-considerations-in-wcf.md)
- [Práticas recomendadas de segurança](best-practices-for-security-in-wcf.md)
