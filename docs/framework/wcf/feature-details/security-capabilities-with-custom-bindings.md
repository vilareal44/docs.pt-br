---
title: Recursos de segurança com associações personalizadas
ms.date: 03/30/2017
ms.assetid: a2425679-484a-4e6c-9c98-7da7304f1516
ms.openlocfilehash: 48d17543f2b133c74bcfa82cfe1a2a0de28b1d01
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84595187"
---
# <a name="security-capabilities-with-custom-bindings"></a>Recursos de segurança com associações personalizadas
Você pode executar as tarefas de segurança mais comuns usando uma das associações fornecidas pelo sistema. No entanto, se você precisar de mais controle, poderá criar uma associação personalizada com uma <xref:System.ServiceModel.Channels.SecurityBindingElement> , conforme explicado nestes tópicos. Para obter mais informações sobre associações personalizadas, consulte [associações personalizadas](../extending/custom-bindings.md).  
  
## <a name="in-this-section"></a>Nesta seção  
 [SecurityBindingElement Authentication Modes](securitybindingelement-authentication-modes.md)  
 Descreve os modos de autenticação que são possíveis com uma associação personalizada.  
  
 [Como criar uma associação personalizada utilizando o SecurityBindingElement](how-to-create-a-custom-binding-using-the-securitybindingelement.md)  
 Descreve as etapas básicas para criar uma associação personalizada com um elemento de segurança.  
  
 [Como criar um SecurityBindingElement para um modo de autenticação especificado](how-to-create-a-securitybindingelement-for-a-specified-authentication-mode.md)  
 Descreve como criar um elemento de segurança para um modo de autenticação especificado.  
  
 [Como desabilitar sessões seguranças em uma WSFederationHttpBinding](how-to-disable-secure-sessions-on-a-wsfederationhttpbinding.md)  
 Descreve como desabilitar sessões seguras ao criar um serviço de Federação.  
  
 [Como habilitar a detecção de repetição de mensagem](how-to-enable-message-replay-detection.md)  
 Descreve como determinar quando ocorre um ataque de reprodução.  
  
 [Como criar uma credencial de suporte](how-to-create-a-supporting-credential.md)  
 Descreve como fornecer uma credencial de suporte a um serviço, se o serviço exigir.  
  
 [Como definir uma confirmação de assinatura](how-to-set-up-a-signature-confirmation.md)  
 Descreve as etapas para confirmar as assinaturas ao assinar digitalmente as mensagens.  
  
 [Como definir a precisão máxima do relógio](how-to-set-a-max-clock-skew.md)  
 Descreve como definir a diferença de tempo máxima permitida entre um serviço e um cliente.  
  
 [Como desabilitar criptografia de assinaturas digitais](how-to-disable-encryption-of-digital-signatures.md)  
 Descreve como desabilitar a criptografia de assinaturas digitais pode ter um benefício de desempenho.  
  
## <a name="reference"></a>Referência  
 <xref:System.ServiceModel.Channels.SecurityBindingElement>  
  
 [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md)  
  
## <a name="related-sections"></a>Seções relacionadas  
 [Noções básicas de nível de proteção](../understanding-protection-level.md)  
  
 [Protegendo serviços e clientes](securing-services-and-clients.md)  
  
## <a name="see-also"></a>Confira também

- [Associações e segurança](bindings-and-security.md)
- [Visão geral de segurança](security-overview.md)
- [Associações fornecidas pelo sistema](../system-provided-bindings.md)
