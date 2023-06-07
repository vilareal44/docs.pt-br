---
title: Adulteração
ms.date: 03/30/2017
ms.assetid: 3bad93be-60bb-4f89-96ab-a1c3dc7c0fad
ms.openlocfilehash: e618ab369a46d403aa8db26c4b472e2be3634785
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600705"
---
# <a name="tampering"></a>Adulteração
A *violação* é o ato de alterar uma mensagem ou a entrega de uma mensagem, e usar a mensagem alterada para uma finalidade diferente da que foi pretendida.  
  
## <a name="do-not-disable-ws-addressing"></a>Não desabilitar o WS-Addressing  
 A especificação WS-Addressing fornece cabeçalhos de endereço em cada mensagem, permitindo que um destinatário da mensagem Verifique o remetente da mensagem. Você pode desabilitar esse recurso definindo a <xref:System.ServiceModel.Channels.MessageVersion.Addressing%2A> propriedade como <xref:System.ServiceModel.Channels.AddressingVersion.None%2A> .  
  
 Quando o modo de segurança é definido como mensagem e, se o WS-Addressing estiver desabilitado, um invasor poderá fazer uma solicitação de um cliente e enviá-lo para outro serviço, e o segundo serviço não terá como detectar se a mensagem veio do cliente original. Na verdade, o primeiro serviço pode fingir que é um cliente ao conversar com o segundo serviço.  
  
 Para atenuar isso, nunca defina a <xref:System.ServiceModel.Channels.MessageVersion.Addressing%2A> propriedade como <xref:System.ServiceModel.Channels.AddressingVersion.None%2A> e evite o uso de <xref:System.ServiceModel.Channels.MessageVersion> , como a <xref:System.ServiceModel.Channels.MessageVersion.Soap12%2A> propriedade estática, que define a <xref:System.ServiceModel.Channels.MessageVersion.Addressing%2A> propriedade como <xref:System.ServiceModel.Channels.AddressingVersion.None%2A> .  
  
## <a name="see-also"></a>Consulte também

- [Considerações sobre segurança](security-considerations-in-wcf.md)
- [Divulgação de informações](information-disclosure.md)
- [Elevação de privilégio](elevation-of-privilege.md)
- [Negação de serviço](denial-of-service.md)
- [Cenários sem suporte](unsupported-scenarios.md)
- [Ataques por repetição](replay-attacks.md)
