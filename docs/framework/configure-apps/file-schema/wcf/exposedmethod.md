---
title: <exposedMethod>
ms.date: 03/30/2017
ms.assetid: 61c938cd-4ee9-4b06-ab28-922ef491ab11
ms.openlocfilehash: 46f2872fb289c2793c356ea179deb3ce52e6d65e
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70855301"
---
# \<exposedMethod>
Representa um método COM+ que está exposto quando a interface em um componente COM+ é exposta como um serviço Web.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<comContracts>**](comcontracts.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<comContract>**](comcontract.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<exposedMethods>**](exposedmethods.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<exposedMethod>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<comContracts>
  <comContract>
    <exposedMethods>
      <exposedMethod name="String" />
    </exposedMethods>
  </comContract>
</comContracts>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|name|Uma cadeia de caracteres que contém o método COM+ que é exposto quando a interface em um componente COM+ é exposta como um serviço Web.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<exposedMethods>](exposedmethods.md)|Uma coleção de [\<exposedMethod>](exposedmethod.md) elementos.|  
  
## <a name="remarks"></a>Comentários  
 A ferramenta de configuração de integração COM+ (ComSvcConfig. exe) pode ser usada para adicionar métodos específicos de uma interface COM para aparecer no contrato de serviço gerado.  
  
 Por exemplo, você pode usar o comando a seguir para adicionar os três métodos nomeados da `IFinances` interface com no `ItemOrders` . Componente financeiro, para o contrato de serviço gerado.  
  
 `ComSvcConfig.exe /i /application:OnlineStore /contract:ItemOrders.Financial,IFinances.{TransferFunds,AddFunds,RemoveFunds} /hosting:complus`  
  
 Quando você também executa o ComSvcConfig. exe, ele gera o seguinte contrato de serviço listando os métodos mencionados anteriormente como [\<exposedMethod>](exposedmethod.md) elementos.  
  
```xml  
<comContract contractType="{C551FBA9-E3AA-4272-8C2A-84BD8D290AC7}"
             name="IFinances"
             namespace="http://contoso.com/services/financial">
  <exposedMethod name="TransferFunds"/>
  <exposedMethod name="AddFunds"/>
  <exposedMethod name="RemoveFunds"/>
</comContract>
```  
  
 No momento da inicialização do serviço, o tempo de execução tenta gerar um contrato de serviço refletindo e adicionando apenas os métodos incluídos na lista de [\<exposedMethod>](exposedmethod.md) elementos. Um rastreamento é produzido para cada método de interface que não está incluído no contrato de serviço.  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.ComMethodElementCollection>
- <xref:System.ServiceModel.Configuration.ComMethodElement>
- [\<comContracts>](comcontracts.md)
- [Integração com aplicativos COM+](../../../wcf/feature-details/integrating-with-com-plus-applications.md)
- [Como configurar configurações de serviço de COM+](../../../wcf/feature-details/how-to-configure-com-service-settings.md)
