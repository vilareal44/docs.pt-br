---
title: <bindingElementExtensions>
ms.date: 03/30/2017
ms.assetid: bb597fc0-c947-451c-afda-bf23d42f4f4d
ms.openlocfilehash: c323a65ace332d2ecd1e03330dddbe7ca17ff5bd
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "69926362"
---
# \<bindingElementExtensions>
Esta seção habilita o uso de um elemento de associação personalizado de um computador ou arquivo de configuração de aplicativo. Você pode adicionar um elemento de associação personalizado a essa coleção usando a `add` palavra-chave e definindo o `type` atributo do elemento como uma extensão de elemento de associação, bem como o `name` atributo para o elemento de associação personalizado.  
  
 As extensões de associação permitem que o usuário crie elementos de associação definidos pelo usuário para uso como parte de associações personalizadas. Programaticamente, uma extensão de associação é um tipo que implementa a classe abstrata <xref:System.ServiceModel.Channels.BindingElement> . No arquivo de configuração, a `bindingElementExtensions` seção é usada para definir um elemento de extensão.  
  
 O exemplo a seguir usa o `add` elemento, bem como o `name` atributo para adicionar uma extensão de associação à `bindingElementExtensions` seção do arquivo de configuração.  
  
```xml  
<system.serviceModel>
  <extensions>
    <bindingElementExtensions>
      <add name="udpTransport"
           type="Microsoft.ServiceModel.Samples.UdpTransportSection, UdpTransport,
                 Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    </bindingElementExtensions>
  </extensions>
</system.serviceModel>
```  
  
 Para adicionar habilidades de configuração ao elemento, o usuário precisa escrever e registrar um `bindingElementExtensionSection` elemento. Para obter mais informações sobre isso, consulte a <xref:System.Configuration> documentação.  
  
 Depois que o elemento e seu tipo de configuração são definidos, a extensão pode ser usada como parte de uma associação personalizada, conforme mostrado no exemplo a seguir.  
  
```xml  
<customBinding>
  <binding name="test2">
    <udpTransport />
    <binaryMessageEncoding maxReadPoolSize="211"
                           maxWritePoolSize="2132"
                           maxSessionSize="3141" />
  </binding>
</customBinding>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.BindingElementExtensionElement>
- [Estendendo associações](../../../wcf/extending/extending-bindings.md)
