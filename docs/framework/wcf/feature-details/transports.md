---
title: Transportes no Windows Communication Foundation
ms.date: 03/30/2017
helpviewer_keywords:
- transports [WCF]
- WCF, transports
- Windows Communication Foundation, transports
ms.assetid: 005c894b-af70-48aa-a1c1-c99338083c27
ms.openlocfilehash: 077d63d8038b245a68083611897c1e6c68971071
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84598665"
---
# <a name="transports-in-windows-communication-foundation"></a>Transportes no Windows Communication Foundation
A camada de transporte está no nível mais baixo da pilha de canais. Os transportes principais usados no Windows Communication Foundation (WCF) são HTTP, HTTPS, TCP e pipes nomeados. Os tópicos nesta seção abordam a escolha entre esses transportes, a configuração do transporte e a definição de propriedades de ajuste.  
  
 O WCF inclui transportes adicionais. Para obter informações sobre o transporte do serviço de enfileiramento de mensagens (também conhecido como MSMQ), consulte [filas e sessões confiáveis](queues-and-reliable-sessions.md). Para obter informações sobre o transporte ponto a ponto, consulte [rede ponto a ponto](peer-to-peer-networking.md).  
  
## <a name="in-this-section"></a>Nesta seção  
 [Selecionando um transporte](choosing-a-transport.md)  
 Descreve os três transportes principais e considerações na seleção de um.  
  
 [Escolhendo um codificador de mensagem](choosing-a-message-encoder.md)  
 Descreve os fatores a serem considerados ao escolher um elemento de associação de codificação de mensagem.  
  
 [Transmissão de transferência de mensagem](streaming-message-transfer.md)  
 Descreve como configurar a camada de transporte para fazer streaming.  
  
 [Configurando HTTP e HTTPS](configuring-http-and-https.md)  
 Descreve como configurar os elementos de associação de transporte HTTP e HTTPS.  
  
 [How to: Replace the WCF URL Reservation with a Restricted Reservation](how-to-replace-the-wcf-url-reservation-with-a-restricted-reservation.md)  
 Descreve como usar reservas restritas do WCFURL.  
  
 [Cotas de transporte](transport-quotas.md)  
 Descreve as considerações sobre como definir as cotas disponíveis na camada de transporte.  
  
 [Trabalhando com NATs e firewalls](working-with-nats-and-firewalls.md)  
 Descreve como configurar a camada de transporte quando as mensagens são enviadas ou recebidas atrás de um firewall ou quando a conversão de endereços de rede (NAT) está presente.  
  
 [Compartilhamento de porta Net.TCP](net-tcp-port-sharing.md)  
 Descreve como usar o componente de compartilhamento de porta Net. TCP do WCF.  
  
## <a name="reference"></a>Referência  
 <xref:System.ServiceModel.Channels.HttpTransportBindingElement>  
  
 <xref:System.ServiceModel.Channels.HttpsTransportBindingElement>  
  
 <xref:System.ServiceModel.Channels.TcpTransportBindingElement>  
  
 <xref:System.ServiceModel.Channels.NamedPipeTransportBindingElement>  
  
## <a name="related-sections"></a>Seções relacionadas  
 [Associações](bindings.md)  
  
 [Estendendo associações](../extending/extending-bindings.md)
