---
title: Como inspecionar ou modificar mensagens no cliente
description: Saiba como inspecionar ou modificar as mensagens de entrada ou saída em um cliente ou serviço WCF implementando a interface apropriada.
ms.date: 03/30/2017
ms.assetid: b8256335-f1c2-419f-b862-9f220ccad84c
ms.openlocfilehash: 6f6a3d20d7f3a9fb79de5cd3e29096e270d0f188
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247501"
---
# <a name="how-to-inspect-or-modify-messages-on-the-client"></a>Como inspecionar ou modificar mensagens no cliente
Você pode inspecionar ou modificar as mensagens de entrada ou saída em um cliente WCF implementando uma <xref:System.ServiceModel.Dispatcher.IClientMessageInspector?displayProperty=nameWithType> e inserindo-as no tempo de execução do cliente. Para obter mais informações, consulte [estendendo clientes](extending-clients.md). O recurso equivalente no serviço é o <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector?displayProperty=nameWithType> . Para obter um exemplo de código completo, consulte o exemplo de [inspetores de mensagem](../samples/message-inspectors.md) .  
  
### <a name="to-inspect-or-modify-messages"></a>Para inspecionar ou modificar mensagens  
  
1. Implemente a interface <xref:System.ServiceModel.Dispatcher.IClientMessageInspector?displayProperty=nameWithType>.  
  
2. Implemente um <xref:System.ServiceModel.Description.IEndpointBehavior?displayProperty=nameWithType> ou <xref:System.ServiceModel.Description.IContractBehavior?displayProperty=nameWithType> dependendo do escopo no qual você deseja inserir o Inspetor de mensagem do cliente. <xref:System.ServiceModel.Description.IEndpointBehavior?displayProperty=nameWithType>permite alterar o comportamento no nível do ponto de extremidade. <xref:System.ServiceModel.Description.IContractBehavior?displayProperty=nameWithType>permite alterar o comportamento no nível do contrato.  
  
3. Insira o comportamento antes de chamar o <xref:System.ServiceModel.ClientBase%601.Open%2A?displayProperty=nameWithType> método ou <xref:System.ServiceModel.ICommunicationObject.Open%2A?displayProperty=nameWithType> no <xref:System.ServiceModel.ChannelFactory%601?displayProperty=nameWithType> . Para obter detalhes, consulte [Configurando e estendendo o tempo de execução com comportamentos](configuring-and-extending-the-runtime-with-behaviors.md).  
  
## <a name="example"></a>Exemplo  
 Os exemplos de código a seguir mostram, em ordem:  
  
- Uma implementação de Inspetor de cliente.  
  
- Um comportamento de ponto de extremidade que insere o Inspetor.  
  
- Uma <xref:System.ServiceModel.Configuration.BehaviorExtensionElement> classe derivada que permite adicionar o comportamento em um arquivo de configuração.  
  
- Um arquivo de configuração que adiciona o comportamento do ponto de extremidade que insere o Inspetor de mensagem do cliente no tempo de execução do cliente.  
  
```csharp  
// Client message inspector  
public class SimpleMessageInspector : IClientMessageInspector  
{  
    public void AfterReceiveReply(ref System.ServiceModel.Channels.Message reply, object correlationState)  
    {  
        // Implement this method to inspect/modify messages after a message  
        // is received but prior to passing it back to the client
        Console.WriteLine("AfterReceiveReply called");  
    }  
  
    public object BeforeSendRequest(ref System.ServiceModel.Channels.Message request, IClientChannel channel)  
    {  
        // Implement this method to inspect/modify messages before they
        // are sent to the service  
        Console.WriteLine("BeforeSendRequest called");  
        return null;  
    }  
}  
```  
  
```csharp  
// Endpoint behavior  
public class SimpleEndpointBehavior : IEndpointBehavior  
{  
    public void AddBindingParameters(ServiceEndpoint endpoint, System.ServiceModel.Channels.BindingParameterCollection bindingParameters)  
    {  
         // No implementation necessary  
    }  
  
    public void ApplyClientBehavior(ServiceEndpoint endpoint, ClientRuntime clientRuntime)  
    {  
        clientRuntime.MessageInspectors.Add(new SimpleMessageInspector());  
    }  
  
    public void ApplyDispatchBehavior(ServiceEndpoint endpoint, EndpointDispatcher endpointDispatcher)  
    {  
         // No implementation necessary  
    }  
  
    public void Validate(ServiceEndpoint endpoint)  
    {  
         // No implementation necessary  
    }  
}  
```  
  
```csharp  
// Configuration element
public class SimpleBehaviorExtensionElement : BehaviorExtensionElement  
{  
    public override Type BehaviorType  
    {  
        get { return typeof(SimpleEndpointBehavior); }  
    }  
  
    protected override object CreateBehavior()  
    {  
         // Create the  endpoint behavior that will insert the message  
         // inspector into the client runtime  
        return new SimpleEndpointBehavior();  
    }  
}  
```  
  
```xml
<?xml version="1.0" encoding="utf-8" ?>  
<configuration>  
    <system.serviceModel>  
        <client>  
            <endpoint address="http://localhost:8080/SimpleService/"
                      binding="wsHttpBinding"
                      behaviorConfiguration="clientInspectorsAdded"
                      contract="ServiceReference1.IService1"  
                      name="WSHttpBinding_IService1"/>  
        </client>  
  
      <behaviors>  
        <endpointBehaviors>  
          <behavior name="clientInspectorsAdded">  
            <simpleBehaviorExtension />  
          </behavior>  
        </endpointBehaviors>  
      </behaviors>  
      <extensions>  
        <behaviorExtensions>  
          <add  
            name="simpleBehaviorExtension"  
            type="SimpleServiceLib.SimpleBehaviorExtensionElement, Host, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null"/>  
        </behaviorExtensions>  
      </extensions>  
    </system.serviceModel>  
</configuration>  
```  
  
## <a name="see-also"></a>Veja também

- <xref:System.ServiceModel.Dispatcher.IClientMessageInspector?displayProperty=nameWithType>
- <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector?displayProperty=nameWithType>
- [Configurando e estendendo o runtime com comportamentos](configuring-and-extending-the-runtime-with-behaviors.md)
