---
title: Contratos de roteamento
ms.date: 03/30/2017
ms.assetid: 9ceea7ae-ea19-4cf9-ba4f-d071e236546d
ms.openlocfilehash: 69dff2c82f67a16d51e11a92052c59672a054e04
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84601069"
---
# <a name="routing-contracts"></a>Contratos de roteamento
Os contratos de roteamento definem os padrões de mensagem que o serviço de roteamento pode processar.  Cada contrato não tem tipo e permite que o serviço receba uma mensagem sem conhecimento do esquema ou da ação da mensagem. Isso permite que o serviço de roteamento direcione mensagens de forma genérica sem configuração adicional para as especificidades das mensagens subjacentes sendo roteadas.  
  
## <a name="routing-contracts"></a>Contratos de roteamento  
 Como o serviço de roteamento aceita um objeto de mensagem WCF genérico, a consideração mais importante ao selecionar um contrato é a forma do canal que será usado na comunicação com os clientes e serviços. Ao processar mensagens, o serviço de roteamento usa bombas de mensagens simétricas, portanto, geralmente, a forma do contrato de entrada deve corresponder à forma do contrato de saída. No entanto, há casos em que o Dispatcher do modelo de serviço pode modificar as formas, como quando o Dispatcher converte um canal duplex em um canal de solicitação-resposta, ou remove o suporte de sessão de um canal quando ele não é necessário e não está sendo usado (ou seja, quando **SessionMode. Allowed**, convertendo um **IInputSessionChannel** em um **IInputChannel**).  
  
 Para dar suporte a essas bombas de mensagem, o serviço de roteamento fornece contratos no <xref:System.ServiceModel.Routing> namespace, que devem ser usados ao definir os pontos de extremidade de serviço usados pelo serviço de roteamento. Esses contratos são sem tipo, o que permite o recebimento de qualquer tipo de mensagem ou ação e permite que o serviço de roteamento manipule mensagens sem conhecimento do esquema de mensagem específico. Para obter mais informações sobre os contratos usados pelo serviço de roteamento, consulte [contratos de roteamento](routing-contracts.md).  
  
 Os contratos fornecidos pelo serviço de roteamento estão localizados no <xref:System.ServiceModel.Routing> namespace e são descritos na tabela a seguir.  
  
|Contrato|Forma|Forma de canal|  
|--------------|-----------|-------------------|  
|<xref:System.ServiceModel.Routing.ISimplexDatagramRouter>|SessionMode = SessionMode. permitido<br /><br /> AsyncPattern = true<br /><br /> IsOneWay = verdadeiro|IInputChannel-> IOutputChannel|  
|<xref:System.ServiceModel.Routing.ISimplexSessionRouter>|SessionMode = SessionMode. obrigatório<br /><br /> AsyncPattern = true<br /><br /> IsOneWay = verdadeiro|IInputSessionChannel-> IOutputSessionChannel|  
|<xref:System.ServiceModel.Routing.IRequestReplyRouter>|SessionMode = SessionMode. permitido<br /><br /> AsyncPattern = true|IReplyChannel-> IRequestChannel|  
|<xref:System.ServiceModel.Routing.IDuplexSessionRouter>|SessionMode = SessionMode. obrigatório<br /><br /> CallbackContract = typeof (ISimplexSession)<br /><br /> AsyncPattern = true<br /><br /> IsOneWay = verdadeiro<br /><br /> TransactionFlow (TransactionFlowOption. Allowed)|IDuplexSessionChannel-> IDuplexSessionChannel|  
  
## <a name="see-also"></a>Consulte também

- [Serviço de roteamento](routing-service.md)
- [Introdução ao roteamento](routing-introduction.md)
