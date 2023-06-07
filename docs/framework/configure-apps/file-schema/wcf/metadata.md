---
title: <metadata>
ms.date: 03/30/2017
ms.assetid: d09653eb-e355-4c73-b87b-28f93d56480d
ms.openlocfilehash: 028e4d3fbe7bce06caa7497c8f95f3b293a4b068
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70855233"
---
# \<metadata>
Especifica como os metadados de serviço podem ser processados.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<client>**](client.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<metadata>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<system.serviceModel>
  <client>
    <metadata>
      <policyImporters>
        <policyImporter type="string" />
      </policyImporters>
      <wsdlImporters>
        <wsdlImporter type="string" />
      </wsdlImporters>
    </metadata>
  </client>
</system.serviceModel>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
 Nenhum.  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<policyImporters>](policyimporters.md)|Especifica todos os importadores de política que controlam a importação de declarações de política personalizada sobre associações. Um importador de política é usado para pesquisar declarações de política personalizadas sobre recursos de associação, bem como anexar um elemento de ligação personalizado que implementa os recursos exigidos pela declaração.|  
|[\<wsdlImporters>](wsdlimporters.md)|Especifica todos os importadores WSDL que importam metadados do WSDL (Web Services Description Language) 1,1 com anexos de WS-Policy. Um importador WSDL é usado para importar metadados, bem como converter essas informações em várias classes que representam informações de contrato e de ponto de extremidade. Ele pode importar seletivamente informações de contrato e de ponto de extremidade e propriedades que expõem quaisquer erros de importação e aceitam informações de tipo relevantes para o processo de importação e conversão. Ele também dá suporte à importação de informações de associação e propriedades que fornecem acesso a documentos de política, documentos WSDL, extensões WSDL e documentos de esquema XML.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<client>](client.md)|A seção cliente define uma lista de pontos de extremidade aos quais um cliente pode se conectar.|  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.MetadataElement>
- <xref:System.ServiceModel.Configuration.PolicyImporterElementCollection>
- <xref:System.ServiceModel.Configuration.WsdlImporterElementCollection>
- <xref:System.ServiceModel.Description.MetadataImporter>
- <xref:System.ServiceModel.Description.WsdlImporter>
- [Configuração de cliente do WCF](../../../wcf/feature-details/client-configuration.md)
- [Clientes](../../../wcf/feature-details/clients.md)
