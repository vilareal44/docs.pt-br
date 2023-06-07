---
title: <filters>
ms.date: 03/30/2017
ms.assetid: 37a87222-ec78-4728-8105-9ca1bd961f0c
ms.openlocfilehash: e4ce0452cc46a8f29334fa67f51f14b83290b1c8
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "69918882"
---
# \<filters>

O `filters` elemento contém uma coleção de filtros XPath usados para controlar que tipo de mensagem é registrada.

Os filtros são aplicados somente na camada de transporte, especificada por `logMessagesAtTransportLevel` is `true` . O nível de serviço e o log de mensagens malformados não são afetados por filtros.

Para adicionar um filtro à coleção, use a `add` palavra-chave. Quando um ou mais filtros são definidos, somente as mensagens que correspondem a pelo menos um dos filtros são registradas. Se nenhum filtro for definido, todas as mensagens passarão.

Os filtros dão suporte à sintaxe XPath completa e são aplicados na ordem em que aparecem no arquivo de configuração. Um filtro sintaticamente incorreto resulta em uma exceção de configuração.

Veja a seguir um exemplo de como configurar um filtro que registra somente as mensagens que têm uma seção de cabeçalho SOAP.
  
```xml  
<messageLogging logEntireMessage="true"
                logMalformedMessages="true"
                logMessagesAtServiceLevel="true"
                logMessagesAtTransportLevel="true"
                maxMessagesToLog="420">
  <filters>
    <add xmlns:soap="http://www.w3.org/2003/05/soap-envelope">
      /soap:Envelope/soap:Headers
    </add>
  </filters>
</messageLogging>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.DiagnosticSection>
- <xref:System.ServiceModel.Diagnostics>
- <xref:System.ServiceModel.Configuration.DiagnosticSection.MessageLogging%2A>
- <xref:System.ServiceModel.Configuration.MessageLoggingElement>
- <xref:System.ServiceModel.Configuration.MessageLoggingElement.Filters%2A>
- <xref:System.ServiceModel.Configuration.XPathMessageFilterElementCollection>
- <xref:System.ServiceModel.Configuration.XPathMessageFilterElement>
- <xref:System.ServiceModel.Dispatcher.XPathMessageFilter>
- [Configurando registros de mensagens em log](../../../wcf/diagnostics/configuring-message-logging.md)
- [\<messageLogging>](messagelogging.md)
