---
title: Utilizando uma associação personalizada com o canal cliente Discovery
ms.date: 03/30/2017
ms.assetid: 36f95e75-04f7-44f3-a995-a0d623624d7f
ms.openlocfilehash: 49983c3ab303d3839350af72b1aa4821c071fe99
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84595031"
---
# <a name="using-a-custom-binding-with-the-discovery-client-channel"></a>Utilizando uma associação personalizada com o canal cliente Discovery
Ao usar uma associação personalizada com o <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> , você deve definir um <xref:System.ServiceModel.Discovery.DiscoveryEndpointProvider> que cria <xref:System.ServiceModel.Discovery.DiscoveryEndpoint> instâncias.  
  
## <a name="creating-a-discoveryendpointprovider"></a>Criando um DiscoveryEndpointProvider  
 O <xref:System.ServiceModel.Discovery.DiscoveryEndpointProvider> é responsável pela criação de <xref:System.ServiceModel.Discovery.DiscoveryEndpoint> instâncias sob demanda. Para definir um provedor de ponto de extremidade de descoberta, derive uma classe de <xref:System.ServiceModel.Discovery.DiscoveryEndpointProvider> e substitua o <xref:System.ServiceModel.Discovery.DiscoveryEndpointProvider.GetDiscoveryEndpoint%2A> método e retorne um novo ponto de extremidade de descoberta. O exemplo a seguir mostra como criar um provedor de ponto de extremidade de descoberta.  
  
```csharp
// Extend DiscoveryEndpointProvider class to change the default DiscoveryEndpoint  
// to the DiscoveryClientBindingElement. The Discovery ClientChannel
// uses this endpoint to send Probe message.  
public class UdpDiscoveryEndpointProvider : DiscoveryEndpointProvider  
{  
   public override DiscoveryEndpoint GetDiscoveryEndpoint()  
   {  
      return new UdpDiscoveryEndpoint(DiscoveryVersion.WSDiscoveryApril2005);  
   }  
}  
```  
  
 Depois de definir o provedor de ponto de extremidade de descoberta, você pode criar uma associação personalizada e adicionar o <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> , que faz referência ao provedor de ponto de extremidade de descoberta, conforme mostrado no exemplo a seguir.  
  
```csharp
DiscoveryClientBindingElement discoveryBindingElement = new DiscoveryClientBindingElement();  
  
// Provide the search criteria and the endpoint over which the probe is sent.  
discoveryBindingElement.FindCriteria = new FindCriteria(typeof(ICalculatorService));  
discoveryBindingElement.DiscoveryEndpointProvider = new UdpDiscoveryEndpointProvider();  
  
CustomBinding customBinding = new CustomBinding(new NetTcpBinding());  
// Insert DiscoveryClientBindingElement at the top of the BindingElement stack.  
// An exception is thrown if this binding element is not at the top.  
customBinding.Elements.Insert(0, discoveryBindingElement);  
```  
  
 Para obter mais informações sobre como usar o canal do cliente de descoberta, consulte [usando o canal do cliente de descoberta](using-the-discovery-client-channel.md).
  
## <a name="see-also"></a>Consulte também

- [Visão geral de descoberta do WCF](wcf-discovery-overview.md)
- [Usando o canal cliente Discovery](using-the-discovery-client-channel.md)
