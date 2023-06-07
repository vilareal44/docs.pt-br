---
title: <bindingExtensions>
ms.date: 03/30/2017
ms.assetid: 8373f94d-d095-486f-8f1e-4ac2f72b58c7
ms.openlocfilehash: bd6aeb32e0994bceb9e56bcb1c6267f4cb64a5a4
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73039138"
---
# \<bindingExtensions>

Esta seção habilita o uso de uma associação definida pelo usuário de um computador ou arquivo de configuração de aplicativo. Você pode adicionar uma associação definida pelo usuário a essa coleção usando a `add` palavra-chave e definindo o `type` atributo do elemento como uma associação definida pelo usuário, bem como o `name` atributo para o nome da associação definida pelo usuário.

As extensões de associação permitem que o usuário crie associações definidas pelo usuário para uso como parte de uma configuração de ponto de extremidade. Programaticamente, uma extensão de associação é um tipo que implementa a classe abstrata <xref:System.ServiceModel.Channels.Binding> .

O exemplo a seguir usa o `add` elemento, bem como o `name` atributo para adicionar uma extensão de associação à `bindingExtensions` seção do arquivo de configuração:

```xml
<system.serviceModel>
  <extensions>
    <bindingExtensions>
      <add name="MyBinding"
           type="Microsoft.ServiceModel.Samples.MyBinding, MyBinding,
                 Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    </bindingExtensions>
  </extensions>
</system.serviceModel>
```

Para adicionar habilidades de configuração ao elemento, o usuário precisa escrever e registrar um `bindingSection` elemento. Para obter mais informações sobre isso, consulte a <xref:System.Configuration> documentação.

Depois que o elemento e seu tipo de configuração são definidos, a extensão pode ser usada como parte de um ponto de extremidade, conforme mostrado no exemplo a seguir:

```xml
<services>
  <service name="MyService">
    <endpoint address="myAddress"
              binding="MyBinding" />
  </service>
</services>
```

## <a name="see-also"></a>Confira também

- [Estendendo associações](../../../wcf/extending/extending-bindings.md)
