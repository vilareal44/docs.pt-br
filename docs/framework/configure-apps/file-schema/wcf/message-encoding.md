---
title: Decodificador de mensagens
ms.date: 03/30/2017
ms.assetid: f30ee941-aca9-4c67-82a5-421568496f07
ms.openlocfilehash: 8e5a71095ba62e0e2e6592c8b7b83b67602ef7e7
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "69931593"
---
# <a name="message-encoding"></a>Decodificador de mensagens
A codificação é o processo de transformar um conjunto de caracteres Unicode em uma sequência de bytes. A decodificação é o processo reverso. O Windows Communication Foundation (WCF) inclui três tipos de codificação para mensagens SOAP: texto, binário e MTOM (mecanismo de otimização de transmissão de mensagens).  
  
 A `binaryMessageEncoding` seção de configuração especifica a codificação de caracteres e o controle de versão de mensagem usados para mensagens XML baseadas em binário. O codificador de mensagens binárias codifica as mensagens de Windows Communication Foundation (WCF) em binário na conexão. Embora essa codificação resulte em uma transmissão muito rápida de mensagens, a interoperabilidade baseada em padrões WS-* é perdida.  
  
 A `mtomMessageEncoding` seção de configuração especifica a codificação de caracteres e o controle de versão de mensagem usados para uma mensagem usando uma codificação MTOM (mecanismo de otimização de transmissão de mensagens). (MTOM) é uma tecnologia eficiente para transmitir dados binários em mensagens de Windows Communication Foundation (WCF). O codificador MTOM tenta fazer um equilíbrio entre a eficiência e a interoperabilidade. A codificação MTOM transmite a maioria dos XML na forma textual, mas otimiza grandes blocos de dados binários transmitindo-os como estão, sem conversão em texto.  
  
 A `textMessageEncoding` seção de configuração especifica um codificador de texto usado para criar mensagens baseadas em texto na conexão. As mensagens produzidas por esse codificador são adequadas para a interoperabilidade baseada em WS-*. O serviço Web ou o cliente de serviço Web geralmente pode entender XML textual. No entanto, transmitir grandes blocos de dados binários como texto é o método menos eficiente para codificar mensagens XML  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Channels.CustomBinding>
- <xref:System.ServiceModel.Channels.MessageEncodingBindingElement>
- [Associações](../../../wcf/bindings.md)
- [Estendendo associações](../../../wcf/extending/extending-bindings.md)
- [Associações personalizadas](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
- [Escolhendo um codificador de mensagem](../../../wcf/feature-details/choosing-a-message-encoder.md)
