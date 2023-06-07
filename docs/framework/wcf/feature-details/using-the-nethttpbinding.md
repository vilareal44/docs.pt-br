---
title: Usando o NetHttpBinding
ms.date: 03/30/2017
ms.assetid: fe134acf-ceca-49de-84a9-05a37e3841f1
ms.openlocfilehash: ac6fc658731d032051f2dfd4058397f9b9a55828
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84585630"
---
# <a name="using-the-nethttpbinding"></a>Usando o NetHttpBinding
O <xref:System.ServiceModel.NetHttpBinding> é uma associação criada para consumir HTTP ou serviços WebSocket e usa a codificação binária por padrão. O <xref:System.ServiceModel.NetHttpBinding> detectará se tiver sido usado com contrato de solicitação-resposta ou contrato de duplex e alterará seu comportamento para corresponder. Ele usará HTTP para contratos de solicitação-resposta e WebSockets para contratos duplex. Esse comportamento pode ser substituído usando a configuração de <xref:System.ServiceModel.Channels.WebSocketTransportUsage>:  
  
1. <xref:System.ServiceModel.Channels.WebSocketTransportUsage.Always>-Isso força o WebSockets a ser usado mesmo para contratos de solicitação-resposta.  
  
2. <xref:System.ServiceModel.Channels.WebSocketTransportUsage.Never>-Isso impede que WebSockets sejam usados. Tentar usar um contrato duplex com esta configuração resultará em uma exceção.  
  
3. <xref:System.ServiceModel.Channels.WebSocketTransportUsage.WhenDuplex>-Esse é o valor padrão e se comporta conforme descrito acima.  
  
 O <xref:System.ServiceModel.NetHttpBinding> oferece suporte a sessões confiáveis no modo HTTP e no modo WebSocket. No modo WebSocket as sessões são fornecidas pelo transporte.  
  
> [!WARNING]
> Quando usar o <xref:System.ServiceModel.NetHttpBinding> e o TransferMode da associação estiver definido como TransferMode.Streamed, os fluxos grandes podem causar um deadlock e a chamada atingirá o tempo limite. Para solucionar esse problema, envie mensagens menores ou use TransferMode.Buffered.  
  
## <a name="configuring-a-service-to-use-nethttpbinding"></a>Configurando um serviço para usar NetHttpBinding  
 O <xref:System.ServiceModel.NetHttpBinding> pode ser configurado da mesma maneira que qualquer outra associação. O snippet da configuração a seguir demonstra como configurar um serviço WCF com <xref:System.ServiceModel.NetHttpBinding>.  
  
```xml  
<system.serviceModel>  
    <services>  
      <service name="WcfService1.Service1">  
        <endpoint address=""  
                  binding="netHttpBinding"  
                  contract="WcfService1.IService1"/>  
        <endpoint address="mex"  
                  binding="mexHttpBinding"  
                  contract="IMetadataExchange"/>  
      </service>  
    </services>  
    <bindings>  
      <netHttpBinding>  
        <binding name="My_NetHttpBindingConfig">  
          <webSocketSettings transportUsage="WhenDuplex"/>  
        </binding>  
      </netHttpBinding>  
    </bindings>  
    ...
  </system.serviceModel>  
```  
  
 O snippet de código a seguir mostra como adicionar o <xref:System.ServiceModel.NetHttpBinding> no código.  
  
```csharp  
ServiceHost svchost = new ServiceHost(typeof(Service1), baseAddress);  
            NetHttpBinding binding = new NetHttpBinding();  
            svchost.AddServiceEndpoint(typeof(IService1), binding, address);
        }  
```  
  
## <a name="see-also"></a>Consulte também

- [Configurando associações para serviços](../configuring-bindings-for-wcf-services.md)
- [Associações](bindings.md)
- [Associações fornecidas pelo sistema](../system-provided-bindings.md)
- [Serviços duplex](duplex-services.md)
