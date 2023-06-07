---
title: Propagação
ms.date: 03/30/2017
ms.assetid: f8181e75-d693-48d1-b333-a776ad3b382a
ms.openlocfilehash: 732ae5cb1ce311b78728f8d5de0fd9102bf32499
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84578949"
---
# <a name="propagation"></a>Propagação
Este tópico descreve a propagação de atividade no modelo de rastreamento Windows Communication Foundation (WCF).  
  
## <a name="using-propagation-to-correlate-activities-across-endpoints"></a>Usando a propagação para correlacionar atividades entre pontos de extremidade  
 A propagação fornece ao usuário uma correlação direta de rastreamentos de erros para a mesma unidade de processamento entre pontos de extremidade de aplicativo, por exemplo, uma solicitação. Os erros emitidos em diferentes pontos de extremidade para a mesma unidade de processamento são agrupados na mesma atividade, mesmo entre domínios de aplicativo. Isso é feito por meio da propagação da ID da atividade nos cabeçalhos da mensagem. Portanto, se um cliente expirar devido a um erro interno no servidor, os dois erros aparecerão na mesma atividade para correlação direta.  
  
 Para fazer isso, use a `ActivityTracing` configuração, conforme demonstrado no exemplo anterior. Além disso, defina o `propagateActivity` atributo para a `System.ServiceModel` origem do rastreamento em todos os pontos de extremidade.  
  
```xml  
<source name="System.ServiceModel" switchValue="Verbose,ActivityTracing" propagateActivity="true" >  
```  
  
 A propagação de atividade é um recurso configurável que faz com que o WCF adicione um cabeçalho a mensagens de saída, que inclui a ID de atividade no TLS. Ao incluir isso nos rastreamentos subsequentes no lado do servidor, podemos correlacionar as atividades do cliente e do servidor.  
  
## <a name="propagation-definition"></a>Definição de propagação  
 A gAId da atividade M será propagada para a atividade N se todas as condições a seguir se aplicarem.  
  
- N é criado por causa de M  
  
- O gAId da M é conhecido como N  
  
- GAId de N é igual a gAId de M.  
  
 O gAId é propagado por meio do cabeçalho da mensagem ActivityId, conforme ilustrado no esquema XML a seguir.  
  
```xml  
<xsd:element name="ActivityId" type="integer" minOccurs="0">  
  <xsd:attribute name="CorrelationId" type="integer" minOccurs="0"/>  
</xsd:element>  
```  
  
 Veja a seguir um exemplo do cabeçalho da mensagem.  
  
```xml  
<MessageLogTraceRecord>  
  <s:Envelope xmlns:s="http://www.w3.org/2003/05/soap-envelope"
                      xmlns:a="http://www.w3.org/2005/08/addressing">  
    <s:Header>  
      <a:Action s:mustUnderstand="1">http://Microsoft.ServiceModel.Samples/ICalculator/Subtract  
      </a:Action>  
      <a:MessageID>urn:uuid:f0091eae-d339-4c7e-9408-ece34602f1ce  
      </a:MessageID>  
      <ActivityId CorrelationId="f94c6af1-7d5d-4295-b693-4670a8a0ce34"
               xmlns="http://schemas.microsoft.com/2004/09/ServiceModel/Diagnostics">  
        17f59a29-b435-4a15-bf7b-642ffc40eac8  
      </ActivityId>  
      <a:ReplyTo>  
          <a:Address>http://www.w3.org/2005/08/addressing/anonymous</a:Address>  
      </a:ReplyTo>  
      <a:To s:mustUnderstand="1">net.tcp://localhost/servicemodelsamples/service</a:To>  
   </s:Header>  
   <s:Body>  
     <Subtract xmlns="http://Microsoft.ServiceModel.Samples">  
       <n1>145</n1>  
       <n2>76.54</n2>  
     </Subtract>  
   </s:Body>  
  </s:Envelope>  
</MessageLogTraceRecord>  
```  
  
## <a name="propagation-and-activity-boundaries"></a>Limites de atividade e propagação  
 Quando a ID da atividade é propagada entre pontos de extremidade, o receptor da mensagem emite um rastreamento de início e parada com essa ID de atividade (propagada). Portanto, há um rastreamento de iniciar e parar com esse gAId de cada origem de rastreamento. Se os pontos de extremidade estiverem no mesmo processo e usarem o mesmo nome de origem de rastreamento, vários iniciar e parar com o mesmo layout (mesmo gAId, mesma origem de rastreamento, mesmo processo) serão criados.  
  
## <a name="synchronization"></a>Sincronização  
 Para sincronizar eventos entre pontos de extremidade que são executados em computadores diferentes, uma CorrelationId é adicionada ao cabeçalho ActivityId que é propagado nas mensagens. As ferramentas podem usar essa ID para sincronizar eventos entre máquinas com discrepância de relógio. Especificamente, a ferramenta do Visualizador de rastreamento de serviço usa essa ID para mostrar fluxos de mensagens entre pontos de extremidade.  
  
## <a name="see-also"></a>Consulte também

- [Configurando o rastreamento](configuring-tracing.md)
- [Utilizando o visualizador de rastreamento de serviço para visualização de rastreamento correlacionados e soluções de problemas](using-service-trace-viewer-for-viewing-correlated-traces-and-troubleshooting.md)
- [Cenários de rastreamento ponta a ponta](end-to-end-tracing-scenarios.md)
- [Ferramenta Visualizador de rastreamento de serviço (SvcTraceViewer.exe)](../../service-trace-viewer-tool-svctraceviewer-exe.md)
