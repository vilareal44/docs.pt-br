---
title: Ataques por repetição
ms.date: 03/30/2017
ms.assetid: 7a17e040-93cd-4432-81b9-9f62fec78c8f
ms.openlocfilehash: 47a4726859605415b4e3e1b4d523f2f8059a3989
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84586293"
---
# <a name="replay-attacks"></a>Ataques por repetição
Um *ataque de reprodução* ocorre quando um invasor copia um fluxo de mensagens entre duas partes e repete o fluxo para uma ou mais das partes. A menos que seja atenuado, os computadores sujeitos ao ataque processam o fluxo como mensagens legítimas, resultando em uma variedade de consequências inadequadas, como ordens redundantes de um item.  
  
## <a name="bindings-may-be-subject-to-reflection-attacks"></a>As associações podem estar sujeitas a ataques de reflexo  
 Os *ataques de reflexo* são repetições de mensagens de volta para um remetente como se tivessem vindo do receptor como resposta. A *detecção de reprodução* padrão no mecanismo de Windows Communication Foundation (WCF) não trata isso automaticamente.  
  
 Os ataques de reflexão são mitigados por padrão porque o modelo de serviço do WCF adiciona uma ID de mensagem assinada para solicitar mensagens e espera um `relates-to` cabeçalho assinado em mensagens de resposta. Consequentemente, a mensagem de solicitação não pode ser repetida como uma resposta. Em cenários de RM (mensagens confiáveis seguras), os ataques de reflexo são mitigados porque:  
  
- Os esquemas de mensagem de resposta Create Sequence e Create Sequence são diferentes.  
  
- Para sequências simplex, as mensagens de sequência que o cliente envia não podem ser reproduzidas para ele porque o cliente não pode entender essas mensagens.  
  
- Para sequências duplex, as duas IDs de sequência devem ser exclusivas. Portanto, uma mensagem de sequência de saída não pode ser repetida como uma mensagem de sequência de entrada (todos os cabeçalhos e corpos de sequência também são assinados).  
  
 As únicas associações que são suscetíveis a ataques de reflexão são aquelas sem WS-Addressing: associações personalizadas que têm o WS-Addressing desabilitado e usam a segurança baseada em chave simétrica. O <xref:System.ServiceModel.BasicHttpBinding> não usa o WS-Addressing por padrão, mas não usa a segurança baseada em chave simétrica de forma a permitir que ela fique vulnerável a esse ataque.  
  
 A mitigação de associações personalizadas é não estabelecer o contexto de segurança ou exigir cabeçalhos WS-Addressing.  
  
## <a name="web-farm-attacker-replays-request-to-multiple-nodes"></a>Web farm: o invasor repete a solicitação para vários nós  
 Um cliente usa um serviço que é implementado em um Web farm. Um invasor repete uma solicitação que foi enviada para um nó no farm para outro nó no farm. Além disso, se um serviço for reiniciado, o cache de reprodução será liberado, permitindo que um invasor reproduza a solicitação. (O cache contém os valores de assinatura de mensagem usados anteriormente e que impede as repetições para que essas assinaturas possam ser usadas apenas uma vez. Os caches de reprodução não são compartilhados em um Web farm.)  
  
 Atenuantes incluem:  
  
- Use a segurança do modo de mensagem com tokens de contexto de segurança com estado (com ou sem conversa segura habilitada). Para obter mais informações, consulte [como: criar um token de contexto de segurança para uma sessão segura](how-to-create-a-security-context-token-for-a-secure-session.md).  
  
- Configure o serviço para usar a segurança em nível de transporte.  
  
## <a name="see-also"></a>Consulte também

- [Considerações sobre segurança](security-considerations-in-wcf.md)
- [Divulgação de informações](information-disclosure.md)
- [Elevação de privilégio](elevation-of-privilege.md)
- [Negação de serviço](denial-of-service.md)
- [Adulteração](tampering.md)
- [Cenários sem suporte](unsupported-scenarios.md)
