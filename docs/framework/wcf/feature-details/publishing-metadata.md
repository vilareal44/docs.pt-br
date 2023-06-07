---
title: Publicando metadados
description: Saiba como os serviços WCF publicam metadados publicando um ou mais pontos de extremidade de metadados, tornando os metadados disponíveis usando protocolos padrão.
ms.date: 03/30/2017
helpviewer_keywords:
- metadata [WCF], publishing
ms.assetid: 3a56831a-cabc-45c0-bd02-12e2e9bd7313
ms.openlocfilehash: 2aa6d877db4e5b09b4c594e6e87b63fb6c04703b
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85244953"
---
# <a name="publishing-metadata"></a>Publicando metadados
Os serviços Windows Communication Foundation (WCF) publicam metadados publicando um ou mais pontos de extremidade de metadados. A publicação de metadados de serviço torna os metadados disponíveis usando protocolos padronizados, como WS-MetadataExchange (MEX) e solicitações HTTP/GET. Os pontos de extremidade de metadados são semelhantes a outros pontos de extremidade de serviço, pois têm um endereço, uma associação e um contrato, e podem ser adicionados a um host de serviço por meio de configuração ou código imperativo.  
  
## <a name="publishing-metadata-endpoints"></a>Publicando pontos de extremidade de metadados  
 Para publicar pontos de extremidade de metadados para um serviço WCF, primeiro você deve adicionar o <xref:System.ServiceModel.Description.ServiceMetadataBehavior> comportamento do serviço ao serviço. A adição de uma <xref:System.ServiceModel.Description.ServiceMetadataBehavior?displayProperty=nameWithType> instância permite que o serviço exponha pontos de extremidade de metadados. Depois de adicionar o <xref:System.ServiceModel.Description.ServiceMetadataBehavior?displayProperty=nameWithType> comportamento do serviço, você pode expor os pontos de extremidade de metadados que dão suporte ao protocolo Mex ou que respondem às solicitações HTTP/Get.  
  
 O <xref:System.ServiceModel.Description.ServiceMetadataBehavior?displayProperty=nameWithType> usa um <xref:System.ServiceModel.Description.WsdlExporter> para exportar metadados para todos os pontos de extremidade de serviço em seu serviço. Para obter mais informações sobre como exportar metadados de um serviço, consulte Exportando [e importando metadados](exporting-and-importing-metadata.md).  
  
 O <xref:System.ServiceModel.Description.ServiceMetadataBehavior?displayProperty=nameWithType> adiciona uma <xref:System.ServiceModel.Description.ServiceMetadataExtension> instância como uma extensão ao seu host de serviço. O <xref:System.ServiceModel.Description.ServiceMetadataExtension?displayProperty=nameWithType> fornece a implementação para os protocolos de publicação de metadados. Você também pode usar o <xref:System.ServiceModel.Description.ServiceMetadataExtension?displayProperty=nameWithType> para obter os metadados do serviço em tempo de execução acessando a <xref:System.ServiceModel.Description.ServiceMetadataExtension.Metadata%2A?displayProperty=nameWithType> propriedade.  
  
### <a name="mex-metadata-endpoints"></a>Pontos de extremidade de metadados MEX  
 Para adicionar pontos de extremidade de metadados que usam o protocolo MEX, adicione pontos de extremidade de serviço ao host de serviço que usa o `IMetadataExchange` contrato de serviço. O WCF inclui uma <xref:System.ServiceModel.Description.IMetadataExchange> interface com esse nome de contrato de serviço que você pode usar como parte do modelo de programação do WCF. Os pontos de extremidade do WS-MetadataExchange, ou pontos de extremidade MEX, podem usar uma das quatro associações padrão que os métodos de fábrica estáticos expõem na <xref:System.ServiceModel.Description.MetadataExchangeBindings> classe para corresponder às associações padrão usadas pelas ferramentas do WCF, como Svcutil.exe. Você também pode configurar pontos de extremidade de metadados MEX usando sua própria associação personalizada.  
  
### <a name="http-get-metadata-endpoints"></a>Pontos de extremidade de metadados HTTP GET  
 Para adicionar um ponto de extremidade de metadados ao serviço que responde às solicitações HTTP/GET, defina a <xref:System.ServiceModel.Description.ServiceMetadataBehavior.HttpGetEnabled%2A> propriedade no <xref:System.ServiceModel.Description.ServiceMetadataBehavior?displayProperty=nameWithType> para `true` . Você também pode configurar um ponto de extremidade de metadados que usa HTTPS definindo a <xref:System.ServiceModel.Description.ServiceMetadataBehavior.HttpsGetEnabled%2A> propriedade no <xref:System.ServiceModel.Description.ServiceMetadataBehavior?displayProperty=nameWithType> para `true` .  
  
## <a name="in-this-section"></a>Nesta seção  
 [Como publicar metadados para um serviço usando um arquivo de configuração](how-to-publish-metadata-for-a-service-using-a-configuration-file.md)  
 Demonstra como configurar um serviço WCF para publicar metadados para que os clientes possam recuperar os metadados usando uma solicitação WS-MetadataExchange ou HTTP/GET usando a `?wsdl` cadeia de caracteres de consulta.  
  
 [Como publicar metadados utilizando código para um serviço](how-to-publish-metadata-for-a-service-using-code.md)  
 Demonstra como habilitar a publicação de metadados para um serviço WCF no código para que os clientes possam recuperar os metadados usando uma solicitação WS-MetadataExchange ou HTTP/GET usando a `?wsdl` cadeia de caracteres de consulta.  
  
## <a name="reference"></a>Referência  
 <xref:System.ServiceModel.Description.ServiceMetadataBehavior>  
  
 <xref:System.ServiceModel.Description.IMetadataExchange>  
  
 <xref:System.ServiceModel.Description.ServiceMetadataExtension>  
  
 <xref:System.ServiceModel.Description.MetadataExchangeBindings>  
  
## <a name="see-also"></a>Veja também

- [Exportando e importando metadados](exporting-and-importing-metadata.md)
