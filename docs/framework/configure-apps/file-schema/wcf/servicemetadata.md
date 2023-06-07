---
title: <serviceMetadata>
ms.date: 03/30/2017
ms.assetid: 2b4c3b4c-31d4-4908-a9b7-5bb411c221f2
ms.openlocfilehash: c421273d1d08db047a51f1f1e4f9d6c908f12986
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "84201781"
---
# \<serviceMetadata>
Especifica a publicação de metadados de serviço e informações associadas.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<serviceMetadata>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<serviceMetadata externalMetadataLocation="String"
                 httpGetBinding="String"
                 httpGetBindingConfiguration="String"
                 httpGetEnabled="Boolean"
                 httpGetUrl="String"
                 httpsGetBinding="String"
                 httpsGetBindingConfiguration="String"
                 httpsGetEnabled="Boolean"
                 httpsGetUrl="String"
                 policyVersion="Policy12/Policy15" />
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|externalMetadataLocation|Um URI que contém o local de um arquivo WSDL, que é retornado ao usuário em resposta às solicitações WSDL e MEX em vez do WSDL gerado automaticamente. Quando esse atributo não é definido, o WSDL padrão é retornado. O padrão é uma cadeia de caracteres vazia.|  
|httpGetBinding|Uma cadeia de caracteres que especifica o tipo de associação que será usado para recuperação de metadados via HTTP GET. Essa configuração é opcional. Se não for especificado, as associações padrão serão usadas.<br /><br /> Somente associações com elementos de associação interna com suporte <xref:System.ServiceModel.Channels.IReplyChannel> terão suporte. Além disso, a <xref:System.ServiceModel.Channels.MessageVersion> propriedade da associação deve ser <xref:System.ServiceModel.Channels.MessageVersion.None%2A> .|  
|httpGetBindingConfiguration|Uma cadeia de caracteres que define o nome da associação que é especificada no `httpGetBinding` atributo, que faz referência às informações de configuração adicionais dessa associação. O mesmo nome deve ser definido na `<bindings>` seção.|  
|httpGetEnabled|Um valor booliano que especifica se os metadados de serviço devem ser publicados para recuperação usando uma solicitação HTTP/Get. O padrão é `false`.<br /><br /> Se o atributo httpGetUrl não for especificado, o endereço no qual os metadados são publicados será o endereço de serviço mais um "? WSDL". Por exemplo, se o endereço do serviço for `http://localhost:8080/CalculatorService` , o endereço de metadados http/Get será `http://localhost:8080/CalculatorService?wsdl` .<br /><br /> Se essa propriedade for `false` , ou o endereço do serviço não for baseado em http ou HTTPS, "? WSDL" será ignorado.|  
|httpGetUrl|Um URI que especifica o endereço no qual os metadados são publicados para recuperação usando uma solicitação HTTP/Get. Se um URI relativo for especificado, ele será tratado como relativo ao endereço base do serviço.|  
|httpsGetBinding|Uma cadeia de caracteres que especifica o tipo de associação que será usado para recuperação de metadados via HTTPS GET. Essa configuração é opcional. Se não for especificado, as associações padrão serão usadas.<br /><br /> Somente associações com elementos de associação interna com suporte <xref:System.ServiceModel.Channels.IReplyChannel> terão suporte. Além disso, a <xref:System.ServiceModel.Channels.MessageVersion> propriedade da associação deve ser <xref:System.ServiceModel.Channels.MessageVersion.None%2A> .|  
|httpsGetBindingConfiguration|Uma cadeia de caracteres que define o nome da associação que é especificada no `httpsGetBinding` atributo, que faz referência às informações de configuração adicionais dessa associação. O mesmo nome deve ser definido na `<bindings>` seção.|  
|httpsGetEnabled|Um valor booliano que especifica se os metadados de serviço devem ser publicados para recuperação usando uma solicitação HTTPS/Get. O padrão é `false`.<br /><br /> Se o atributo httpsGetUrl não for especificado, o endereço no qual os metadados são publicados será o endereço de serviço mais um "? WSDL". Por exemplo, se o endereço do serviço for " https://localhost:8080/CalculatorService ", o endereço de metadados http/Get será " https://localhost:8080/CalculatorService?wsdl ".<br /><br /> Se essa propriedade for `false` , ou o endereço do serviço não for baseado em http ou HTTPS, "? WSDL" será ignorado.|  
|httpsGetUrl|Um URI que especifica o endereço no qual os metadados são publicados para recuperação usando uma solicitação HTTPS/Get.|  
|policyVersion|Uma cadeia de caracteres que especifica a versão da especificação de WS-Policy que está sendo usada. Esse atributo é do tipo <xref:System.ServiceModel.Description.PolicyVersion> .|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<behavior>](behavior-of-endpointbehaviors.md)|Especifica um elemento de comportamento.|  
  
## <a name="remarks"></a>Comentários  
 Este elemento de configuração permite que você controle os recursos de publicação de metadados de um serviço. Para evitar a divulgação não intencional de metadados de serviço potencialmente confidenciais, a configuração padrão para Windows Communication Foundation (WCF) Services desabilita a publicação de metadados. Esse comportamento é seguro por padrão, mas também significa que você não pode usar uma ferramenta de importação de metadados (como SvcUtil. exe) para gerar o código do cliente necessário para chamar o serviço, a menos que o comportamento de publicação de metadados do serviço esteja explicitamente habilitado na configuração. Usando esse elemento de configuração, você pode habilitar esse comportamento de publicação para seu serviço.  
  
 Para obter um exemplo detalhado de como configurar esse comportamento, consulte [comportamento de publicação de metadados](../../../wcf/samples/metadata-publishing-behavior.md).  
  
 Os `httpGetBinding` atributos e opcionais `httpsGetBinding` permitem configurar as associações usadas para recuperação de metadados via HTTP Get (ou HTTPS Get). Se não forem especificadas, as associações padrão ( `HttpTransportBindingElement` , no caso de http e `HttpsTransportBindingElement` , no caso de HTTPS) serão usadas para recuperação de metadados, conforme apropriado. Observe que você não pode usar esses atributos com as associações internas do WCF. Somente associações com elementos de associação interna com suporte <xref:System.ServiceModel.Channels.IReplyChannel> terão suporte. Além disso, a <xref:System.ServiceModel.Channels.MessageVersion> propriedade da associação deve ser <xref:System.ServiceModel.Channels.MessageVersion.None%2A> .  
  
 Para reduzir a exposição de um serviço a usuários mal-intencionados, é possível proteger a transferência usando o mecanismo SSL sobre HTTP (HTTPS). Para fazer isso, primeiro você deve associar um certificado X. 509 adequado a uma porta específica no computador que está hospedando o serviço. (Para obter mais informações, consulte [trabalhando com certificados](../../../wcf/feature-details/working-with-certificates.md).) Em segundo lugar, adicione esse elemento à configuração do serviço e defina o `httpsGetEnabled` atributo como `true` . Por fim, defina o `httpsGetUrl` atributo como a URL do ponto de extremidade de metadados de serviço, conforme mostrado no exemplo a seguir.  
  
```xml  
<behaviors>
  <serviceBehaviors>
    <behavior name="NewBehavior">
      <serviceMetadata httpsGetEnabled="true"
                       httpsGetUrl="https://myComputerName/myEndpoint" />
    </behavior>
  </serviceBehaviors>
</behaviors>
```  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir configura um serviço para expor metadados usando o \<serviceMetadata> elemento. Ele também configura um ponto de extremidade para expor o `IMetadataExchange` contrato como uma implementação de um protocolo de WS-MetadataExchange (MEX). O exemplo usa o `mexHttpBinding` , que é uma associação padrão de conveniência equivalente ao `wsHttpBinding` com o modo de segurança definido como `None` . Um endereço relativo de "MEX" é usado no ponto de extremidade, que, quando resolvido em relação ao endereço base de serviços, resulta em um endereço de ponto de extremidade de `http://localhost/servicemodelsamples/service.svc/mex` .  
  
```xml  
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceModel.Samples.CalculatorService"
               behaviorConfiguration="CalculatorServiceBehavior">
        <!-- This endpoint is exposed at the base address provided by the host: http://localhost/servicemodelsamples/service.svc -->
        <endpoint address=""
                  binding="wsHttpBinding"
                  contract="Microsoft.ServiceModel.Samples.ICalculator" />
        <!-- The mex endpoint is exposed at http://localhost/servicemodelsamples/service.svc/mex
             To expose the IMetadataExchange contract, you must enable the serviceMetadata behavior as demonstrated below. -->
        <endpoint address="mex"
                  binding="mexHttpBinding"
                  contract="IMetadataExchange" />
      </service>
    </services>
    <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->
    <behaviors>
      <serviceBehaviors>
        <behavior name="CalculatorServiceBehavior">
          <!-- The serviceMetadata behavior publishes metadata through the IMetadataExchange contract. When this behavior is
               present, you can expose this contract through an endpoint as shown above. Setting httpGetEnabled to true publishes
               the service's WSDL at the <baseaddress>?wsdl eg. http://localhost/servicemodelsamples/service.svc?wsdl -->
          <serviceMetadata httpGetEnabled="True" />
          <serviceDebug includeExceptionDetailInFaults="False" />
        </behavior>
      </serviceBehaviors>
    </behaviors>
  </system.serviceModel>
</configuration>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.ServiceMetadataPublishingElement>
- <xref:System.ServiceModel.Description.ServiceMetadataBehavior>
- [Comportamentos de segurança](../../../wcf/feature-details/security-behaviors-in-wcf.md)
- [Comportamento de publicação de metadados](../../../wcf/samples/metadata-publishing-behavior.md)
