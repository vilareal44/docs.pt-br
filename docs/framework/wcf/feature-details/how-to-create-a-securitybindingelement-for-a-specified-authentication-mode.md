---
title: Como criar um SecurityBindingElement para um modo de autenticação especificado
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: a7c7747a-5b8c-463f-8493-7266dac75066
ms.openlocfilehash: 9aebe6d8cc82c454161daead49b55f02a1cca4a7
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84598938"
---
# <a name="how-to-create-a-securitybindingelement-for-a-specified-authentication-mode"></a>Como criar um SecurityBindingElement para um modo de autenticação especificado
O Windows Communication Foundation (WCF) fornece vários modos pelos quais os clientes e serviços se autenticam entre si. Você pode criar elementos de associação de segurança para esses modos de autenticação usando métodos estáticos na <xref:System.ServiceModel.Channels.SecurityBindingElement> classe ou por meio da configuração, conforme mostrado no exemplo a seguir.  
  
 Para obter mais informações sobre os 18 modos de autenticação, consulte [modos de autenticação SecurityBindingElement](securitybindingelement-authentication-modes.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir mostra métodos para criar associações para os vários modos de autenticação.  
  
> [!NOTE]
> Depois que uma instância do <xref:System.ServiceModel.Channels.SecurityBindingElement> objeto é criada, várias propriedades são imutáveis, como <xref:System.ServiceModel.Security.Tokens.IssuedSecurityTokenParameters.KeyType%2A> e <xref:System.ServiceModel.Channels.SecurityBindingElement.MessageSecurityVersion%2A> . A chamada a `set` essas propriedades não as altera.  
  
 [!code-csharp[c_CustomBindingsAuthMode#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_custombindingsauthmode/cs/source.cs#2)]
 [!code-vb[c_CustomBindingsAuthMode#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_custombindingsauthmode/vb/source.vb#2)]  
  
## <a name="see-also"></a>Consulte também

- [SecurityBindingElement Authentication Modes](securitybindingelement-authentication-modes.md)
- [Como criar uma associação personalizada utilizando o SecurityBindingElement](how-to-create-a-custom-binding-using-the-securitybindingelement.md)
