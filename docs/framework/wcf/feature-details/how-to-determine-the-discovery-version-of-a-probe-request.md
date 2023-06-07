---
title: Como determinar a versão de descoberta de uma solicitação de investigação
ms.date: 03/30/2017
ms.assetid: b3c4e2e2-2957-4074-ae6a-776a5ca84278
ms.openlocfilehash: 2b7e42714ae1d16a84bcb6f0fc79cf5b376a7a16
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84595408"
---
# <a name="how-todetermine-the-discovery-version-of-a-probe-request"></a>Como determinar a versão de descoberta de uma solicitação de investigação

Um proxy de descoberta pode expor vários pontos de extremidade de descoberta usando diferentes versões de descoberta. Quando uma solicitação de investigação de multicast UDP chega ao proxy, o proxy deve responder com uma mensagem de supressão de multicast. Para fazer isso, seria necessário saber a versão de descoberta da solicitação.

## <a name="to-determine-the-discovery-version-of-a-probe-request"></a>Para determinar a versão de descoberta de uma solicitação de investigação

No método que responde a uma solicitação de investigação (por exemplo <xref:System.ServiceModel.Discovery.DiscoveryProxy.OnBeginFind%2A?displayProperty=nameWithType> ), use a <xref:System.ServiceModel.OperationContext.Current%2A?displayProperty=nameWithType> propriedade estática para pesquisar por um <xref:System.ServiceModel.Discovery.DiscoveryOperationContextExtension> , conforme mostrado no código a seguir.

```csharp
DiscoveryOperationContextExtension doce = OperationContext.Current.Extensions.Find<DiscoveryOperationContextExtension>();
// Access the discovery version from the DiscoveryOperationContextExtension
doce.DiscoveryVersion;
```

## <a name="see-also"></a>Consulte também

- <xref:System.ServiceModel.Discovery.Configuration.AnnouncementEndpointElement.DiscoveryVersion%2A>
- [Implementando um proxy de descoberta](implementing-a-discovery-proxy.md)
