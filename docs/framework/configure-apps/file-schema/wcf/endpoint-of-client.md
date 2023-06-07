---
title: <endpoint> de <client>
ms.date: 03/30/2017
ms.assetid: de6238ae-bbf8-48e9-a1b5-e24c0bea8afa
ms.openlocfilehash: f1ffbc1e8efac70523d7f631c8cf9ba9a1622bfc
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70855322"
---
# <a name="endpoint-of-client"></a>\<endpoint> de \<client>
Especifica o contrato, a associação e as propriedades de endereço do ponto de extremidade do canal, que é usado pelos clientes para se conectar a pontos de extremidades de serviço no servidor.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<client>**](client.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<endpoint>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<endpoint address="String"
          behaviorConfiguration="String"
          binding="String"
          bindingConfiguration="String"
          contract="String"
          endpointConfiguration="String"
          kind="String"
          name="String">
</endpoint>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|address|Atributo de cadeia de caracteres obrigatório.<br /><br /> Especifica o endereço do ponto de extremidade. O padrão é uma cadeia de caracteres vazia. O endereço deve ser um URI absoluto.|  
|behaviorConfiguration|Uma cadeia de caracteres que contém o nome do comportamento do comportamento a ser usado para instanciar o ponto de extremidade. O nome do comportamento deve estar no escopo no ponto em que o serviço é definido. O padrão é uma cadeia de caracteres vazia.|  
|associação|Atributo de cadeia de caracteres obrigatório.<br /><br /> Uma cadeia de caracteres que indica o tipo de associação a ser usado. O tipo deve ter uma seção de configuração registrada para ser referenciado. O tipo é registrado pelo nome da seção, em vez de pelo nome do tipo da associação.|  
|bindingConfiguration|Opcional. Uma cadeia de caracteres que contém o nome da configuração de associação a ser usada quando o ponto de extremidade é instanciado. A configuração de associação deve estar no escopo no ponto em que o ponto de extremidade é definido. O padrão é uma cadeia de caracteres vazia.<br /><br /> Esse atributo é usado em conjunto com `binding` para fazer referência a uma configuração de associação específica no arquivo de configuração. Defina esse atributo se você estiver tentando usar uma associação personalizada. Caso contrário, uma exceção pode ser lançada.|  
|contrato|Atributo de cadeia de caracteres obrigatório.<br /><br /> Uma cadeia de caracteres que indica qual contrato este ponto de extremidade está expondo. O assembly deve implementar o tipo de contrato.|  
|endpointConfiguration|Uma cadeia de caracteres que especifica o nome do ponto de extremidade padrão que é definido pelo `kind` atributo, que faz referência às informações de configuração adicionais desse ponto de extremidade padrão. O mesmo nome deve ser definido na `<standardEndpoints>` seção.|  
|kind|Uma cadeia de caracteres que especifica o tipo de ponto de extremidade padrão aplicado. O tipo deve ser registrado na `<extensions>` seção ou em Machine. config. Se nada for especificado, um ponto de extremidade de canal comum será criado.|  
|name|Atributo de cadeia de caracteres opcional. Esse atributo identifica exclusivamente um ponto de extremidade para um determinado contrato. Você pode definir vários clientes para um determinado tipo de contrato. Cada definição deve ser diferenciada por um nome de configuração exclusivo. Se esse atributo for omitido, o ponto de extremidade correspondente será usado como o ponto de extremidade padrão associado ao tipo de contrato especificado. O padrão é uma cadeia de caracteres vazia.<br /><br /> O `name` atributo de uma associação é usado para exportação de definição por meio de WSDL.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<headers>](headers.md)|Uma coleção de cabeçalhos de endereço.|  
|[\<identity>](identity.md)|Uma identidade que permite a autenticação de um ponto de extremidade por outros pontos de extremidades que trocam mensagens com ele.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<client>](client.md)|Uma seção de configuração que define uma lista de pontos de extremidade aos quais um cliente pode se conectar.|  
  
## <a name="example"></a>Exemplo  
 Este é um exemplo de uma configuração de ponto de extremidade de canal.  
  
```xml  
<endpoint address="/HelloWorld/"
          bindingConfiguration="usingDefaults"
          name="MyBinding"
          binding="customBinding"
          contract="HelloWorld">
</endpoint>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.ChannelEndpointElement>
- <xref:System.ServiceModel.Configuration.ClientSection>
- <xref:System.ServiceModel.Configuration.ChannelEndpointElementCollection>
- <xref:System.ServiceModel.Configuration.ClientSection.Endpoints%2A>
- <xref:System.ServiceModel.Configuration.ChannelEndpointElement>
- [Configuração de cliente do WCF](../../../wcf/feature-details/client-configuration.md)
- [Clientes](../../../wcf/feature-details/clients.md)
