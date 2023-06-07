---
title: Elemento <endpoint>
ms.date: 03/30/2017
ms.assetid: 2fc8fedc-78d0-4e87-8142-fbfd26c15a4e
ms.openlocfilehash: fb9d3bf9b5f1a742abcc70d78af026c179ec4c4d
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70855385"
---
# <a name="endpoint-element"></a>Elemento \<endpoint>
Especifica a associação, o contrato e as propriedades de endereço para um ponto de extremidade de serviço, que é usado para expor serviços.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<services>**](services.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<service>**](service.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<endpoint>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<endpoint address="String"
          behaviorConfiguration="String"
          binding="String"
          bindingConfiguration="String"
          bindingName="String"
          bindingNamespace="String"
          contract="String"
          endpointConfiguration="String"
          isSystemEndpoint="Boolean"
          kind="String"
          listenUriMode="Explicit/Unique"
          listenUri="Uri">
</endpoint>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|address|Uma cadeia de caracteres que contém o endereço do ponto de extremidade. O endereço pode ser especificado como um endereço absoluto ou relativo. Se um endereço relativo for fornecido, espera-se que o host forneça um endereço base apropriado para o esquema de transporte usado na associação. Se um endereço não estiver configurado, presume-se que o endereço base seja o endereço para esse ponto de extremidade.<br /><br /> O padrão é uma cadeia de caracteres vazia.|  
|behaviorConfiguration|Uma cadeia de caracteres que contém o nome do comportamento a ser usado no ponto de extremidade.|  
|associação|Atributo de cadeia de caracteres necessário que especifica o tipo de associação a ser usado. O tipo deve ter uma seção de configuração registrada para ser referenciado. O tipo é registrado pelo nome da seção, em vez de pelo nome do tipo da associação.|  
|bindingConfiguration|Uma cadeia de caracteres que especifica o nome de associação da associação a ser usada quando o ponto de extremidade é instanciado. O nome da associação deve estar no escopo no ponto em que o ponto de extremidade é definido. O padrão é uma cadeia de caracteres vazia.<br /><br /> Esse atributo é usado em conjunto com `binding` para fazer referência a uma configuração de associação específica no arquivo de configuração. Defina esse atributo se você estiver tentando usar uma associação personalizada. Caso contrário, uma exceção pode ser lançada.|  
|BindingName|Uma cadeia de caracteres que especifica o nome qualificado exclusivo da Associação para exportação de definição por meio de WSDL. O padrão é uma cadeia de caracteres vazia.|  
|bindingNamespace|Uma cadeia de caracteres que especifica o nome qualificado do namespace da Associação para exportação de definição por meio de WSDL. O padrão é uma cadeia de caracteres vazia.|  
|contrato|Uma cadeia de caracteres que indica qual contrato este ponto de extremidade está expondo. O assembly deve implementar o tipo de contrato. Se uma implementação de serviço implementar um único tipo de contrato, essa propriedade poderá ser omitida. O padrão é uma cadeia de caracteres vazia.|  
|endpointConfiguration|Uma cadeia de caracteres que especifica o nome do ponto de extremidade padrão que é definido pelo `kind` atributo, que faz referência às informações de configuração adicionais desse ponto de extremidade padrão. O mesmo nome deve ser definido na `<standardEndpoints>` seção.|  
|isSystemEndpoint|Um valor booliano que especifica se um ponto de extremidade é um ponto de extremidade de infraestrutura.|  
|kind|Uma cadeia de caracteres que especifica o tipo de ponto de extremidade padrão aplicado. O tipo deve ser registrado na `<extensions>` seção ou em Machine. config. Se nada for especificado, um ponto de extremidade de serviço comum será criado.|  
|listenUriMode|Especifica como o transporte trata o `ListenUri` fornecido para o serviço escutar. Os valores válidos são<br /><br /> -Explícito<br />-Exclusivo<br /><br /> O valor padrão é Explicit.|  
|listenUri|Uma cadeia de caracteres que especifica o URI no qual o ponto de extremidade de serviço escuta. O padrão é uma cadeia de caracteres vazia.|  
|name|Atributo opcional. Uma cadeia de caracteres que especifica o nome do ponto de extremidade de serviço. O valor padrão é a concatenação do nome da associação e o nome da descrição do contrato. Os serviços podem ter vários pontos de extremidade e, portanto, o atributo do Endpoint `name` é diferente do nome do serviço.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<headers>](headers.md)|Uma coleção de cabeçalhos de endereço.|  
|[\<identity>](identity.md)|Uma identidade que permite a autenticação de um ponto de extremidade por outros pontos de extremidades que trocam mensagens com ele.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<service>](service.md)|Uma seção de configuração que define uma lista de pontos de extremidade aos quais um cliente pode se conectar.|  
  
## <a name="example"></a>Exemplo  
 Este é um exemplo de uma configuração de ponto de extremidade de serviço.  
  
```xml  
<endpoint address="/HelloWorld/"
          bindingConfiguration="usingDefaults"
          bindingName="MyBinding"
          binding="customBinding"
          contract="HelloWorld">
  <headers>
    <region xmlns="http://tempuri.org/">EastCoast</region>
    <member xmlns="http://tempuri.org/">Gold</member>
  </headers>
</endpoint>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.ServiceEndpointElement>
- <xref:System.ServiceModel.EndpointAddress>
- <xref:System.ServiceModel.Description.ServiceEndpoint>
- [Pontos de extremidade: endereços, associações e contratos](../../../wcf/feature-details/endpoints-addresses-bindings-and-contracts.md)
- [Como criar um ponto de extremidade de serviço em configuração](../../../wcf/feature-details/how-to-create-a-service-endpoint-in-configuration.md)
