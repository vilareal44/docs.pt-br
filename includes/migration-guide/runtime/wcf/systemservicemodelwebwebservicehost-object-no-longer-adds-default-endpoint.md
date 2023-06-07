---
ms.openlocfilehash: 57f68c10d5d1edc9b8deaca2f4fe7edda2dd69b0
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619868"
---
### <a name="systemservicemodelwebwebservicehost-object-no-longer-adds-a-default-endpoint"></a>O objeto System.ServiceModel.Web.WebServiceHost não adiciona mais um ponto de extremidade padrão

#### <a name="details"></a>Detalhes

O objeto <xref:System.ServiceModel.Web.WebServiceHost> não adicionará um ponto de extremidade padrão se um ponto de extremidade explícito tiver sido adicionado pelo código do aplicativo.

#### <a name="suggestion"></a>Sugestão

Se os usuários forem esperar para poderem se conectar a um ponto de extremidade padrão e outros pontos de extremidade explícitos forem adicionados ao <xref:System.ServiceModel.Web.WebServiceHost?displayProperty=fullName>, os pontos de extremidade padrão também deverão ser explicitamente adicionados (usando <xref:System.ServiceModel.ServiceHostBase.AddDefaultEndpoints?displayProperty=fullName>).

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.ServiceModel.ServiceHost.AddServiceEndpoint(System.Type,System.ServiceModel.Channels.Binding,System.String)?displayProperty=nameWithType></li><li><xref:System.ServiceModel.ServiceHost.AddServiceEndpoint(System.Type,System.ServiceModel.Channels.Binding,System.Uri)?displayProperty=nameWithType></li><li><xref:System.ServiceModel.ServiceHost.AddServiceEndpoint(System.Type,System.ServiceModel.Channels.Binding,System.String,System.Uri)?displayProperty=nameWithType></li><li><xref:System.ServiceModel.ServiceHost.AddServiceEndpoint(System.Type,System.ServiceModel.Channels.Binding,System.Uri,System.Uri)?displayProperty=nameWithType></li><li><xref:System.ServiceModel.ServiceHost.AddServiceEndpoint(System.Type,System.ServiceModel.Channels.Binding,System.Uri,System.Uri)?displayProperty=nameWithType></li><li><xref:System.ServiceModel.ServiceHostBase.AddServiceEndpoint(System.ServiceModel.Description.ServiceEndpoint)?displayProperty=nameWithType></li><li><xref:System.ServiceModel.ServiceHostBase.AddServiceEndpoint(System.String,System.ServiceModel.Channels.Binding,System.String)?displayProperty=nameWithType></li><li><xref:System.ServiceModel.ServiceHostBase.AddServiceEndpoint(System.String,System.ServiceModel.Channels.Binding,System.Uri)?displayProperty=nameWithType></li><li><xref:System.ServiceModel.ServiceHostBase.AddServiceEndpoint(System.String,System.ServiceModel.Channels.Binding,System.String,System.Uri)?displayProperty=nameWithType></li><li><xref:System.ServiceModel.ServiceHostBase.AddServiceEndpoint(System.String,System.ServiceModel.Channels.Binding,System.Uri,System.Uri)?displayProperty=nameWithType></li></ul>|
