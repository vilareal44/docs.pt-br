---
title: <serviceDebug>
ms.date: 03/30/2017
ms.assetid: 6d7ea986-f232-49fe-842c-f934d9966889
ms.openlocfilehash: 4eb79cc91ef489501c4c8bb6311f240d855ed053
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70399624"
---
# \<serviceDebug>
Especifica recursos de depuração e de informações de ajuda para um serviço Windows Communication Foundation (WCF).  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<serviceDebug>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<serviceDebug httpHelpPageBinding="String"
              httpHelpPageBindingConfiguration="String"
              httpHelpPageEnabled="Boolean"
              httpHelpPageUrl="Uri"
              httpsHelpPageBinding="String"
              httpsHelpPageBindingConfiguration="String"
              httpsHelpPageEnabled="Boolean"
              httpsHelpPageUrl="Uri"
              includeExceptionDetailInFaults="Boolean" />
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|httpHelpPageBinding|Um valor de cadeia de caracteres que especifica o tipo de associação a ser usado quando o HTTP é utilizado para acessar a página de ajuda do serviço.<br /><br /> Somente associações com elementos de associação interna com suporte <xref:System.ServiceModel.Channels.IReplyChannel?displayProperty=nameWithType> terão suporte. Além disso, a <xref:System.ServiceModel.Channels.MessageVersion?displayProperty=nameWithType> propriedade da associação deve ser <xref:System.ServiceModel.Channels.MessageVersion.None?displayProperty=nameWithType> .|  
|httpHelpPageBindingConfiguration|Uma cadeia de caracteres que especifica o nome da associação que é especificada no `httpHelpPageBinding` atributo, que faz referência às informações de configuração adicionais dessa associação. O mesmo nome deve ser definido na `<bindings>` seção.|  
|httpHelpPageEnabled|Um valor booliano que controla se o WCF publica uma página de ajuda HTML no endereço especificado pelo `httpHelpPageUrl` atributo. O padrão é `true`.<br /><br /> Você pode definir essa propriedade como `false` para desabilitar a publicação de uma página de ajuda HTML visível para navegadores HTML.<br /><br /> Para garantir que a página de ajuda HTML seja publicada no local controlado pelo `httpHelpPageUrl` atributo, você deve definir esse atributo como `true` . Além disso, uma das seguintes condições também deve ser atendida:<br /><br /> -O `httpHelpPageUrl` atributo é um endereço absoluto que dá suporte ao esquema de protocolo http.<br />-Há um endereço base para o serviço que dá suporte ao esquema de protocolo HTTP.<br /><br /> Embora uma exceção seja lançada se um endereço absoluto que não dá suporte ao esquema de protocolo HTTP é atribuído ao `httpHelpPageUrl` atributo, qualquer outro cenário no qual nenhum dos critérios anteriores é atendido resulta em nenhuma exceção e nenhuma página de ajuda HTML.|  
|httpHelpPageUrl|Um URI que especifica a URL relativa ou absoluta baseada em HTTP do arquivo de ajuda HTML personalizado que o usuário vê quando o ponto de extremidade é exibido usando um navegador HTML.<br /><br /> Você pode usar esse atributo para habilitar o uso de um arquivo de ajuda HTML personalizado que é retornado de uma solicitação HTTP/Get, por exemplo, de um navegador HTML. O local do arquivo de ajuda HTML é resolvido da seguinte maneira.<br /><br /> 1. se o valor desse atributo for um endereço relativo, o local do arquivo de ajuda HTML será o valor do endereço base do serviço que dá suporte a solicitações HTTP, mais esse valor de propriedade.<br />2. se o valor desse atributo for um endereço absoluto e oferecer suporte a solicitações HTTP, o local do arquivo de ajuda HTML será o valor dessa propriedade.<br />3. se o valor desse atributo for absoluto, mas não oferecer suporte a solicitações HTTP, uma exceção será lançada.<br /><br /> Esse atributo é válido somente quando o `httpHelpPageEnabled` atributo é `true` .|  
|httpsHelpPageBinding|Um valor de cadeia de caracteres que especifica o tipo de associação a ser usado quando HTTPS é utilizado para acessar a página de ajuda do serviço.<br /><br /> Somente associações com elementos de associação interna com suporte <xref:System.ServiceModel.Channels.IReplyChannel> terão suporte. Além disso, a <xref:System.ServiceModel.Channels.MessageVersion?displayProperty=nameWithType> propriedade da associação deve ser <xref:System.ServiceModel.Channels.MessageVersion.None?displayProperty=nameWithType> .|  
|httpsHelpPageBindingConfiguration|Uma cadeia de caracteres que especifica o nome da associação que é especificada no `httpsHelpPageBinding` atributo, que faz referência às informações de configuração adicionais dessa associação. O mesmo nome deve ser definido na `<bindings>` seção.|  
|httpsHelpPageEnabled|Um valor booliano que controla se o WCF publica uma página de ajuda HTML no endereço especificado pelo `httpsHelpPageUrl` atributo. O padrão é `true`.<br /><br /> Você pode definir essa propriedade como `false` para desabilitar a publicação de uma página de ajuda HTML visível para navegadores HTML.<br /><br /> Para garantir que a página de ajuda HTML seja publicada no local controlado pelo `httpsHelpPageUrl` atributo, você deve definir esse atributo como `true` . Além disso, uma das seguintes condições também deve ser atendida:<br /><br /> -O `httpsHelpPageUrl` atributo é um endereço absoluto que dá suporte ao esquema de protocolo HTTPS.<br />-Há um endereço base para o serviço que dá suporte ao esquema de protocolo HTTPS.<br /><br /> Embora uma exceção seja lançada se um endereço absoluto que não dá suporte ao esquema de protocolo HTTPS é atribuído ao `httpsHelpPageUrl` atributo, qualquer outro cenário no qual nenhum dos critérios anteriores é atendido resulta em nenhuma exceção e nenhuma página de ajuda HTML.|  
|httpsHelpPageUrl|Um URI que especifica a URL relativa ou absoluta baseada em HTTPS do arquivo de ajuda HTML personalizado que o usuário vê quando o ponto de extremidade é exibido usando um navegador HTML.<br /><br /> Você pode usar esse atributo para habilitar o uso de um arquivo de ajuda HTML personalizado que é retornado de uma solicitação HTTPS/Get, por exemplo, de um navegador HTML. O local do arquivo de ajuda HTML é resolvido da seguinte maneira:<br /><br /> -Se o valor dessa propriedade for um endereço relativo, o local do arquivo de ajuda HTML será o valor do endereço base de serviço que dá suporte a solicitações HTTPS, mais esse valor de propriedade.<br />-Se o valor dessa propriedade for um endereço absoluto e oferecer suporte a solicitações HTTPS, o local do arquivo de ajuda HTML será o valor dessa propriedade.<br />-Se o valor dessa propriedade for absoluto, mas não oferecer suporte a solicitações HTTPS, uma exceção será lançada.<br /><br /> Esse atributo é válido somente quando o `httpHelpPageEnabled` atributo é `true` .|  
|includeExceptionDetailInFaults|Um valor que especifica se as informações de exceção gerenciadas devem ser incluídas nos detalhes de falhas de SOAP retornadas ao cliente para fins de depuração. O padrão é `false`.<br /><br /> Se você definir esse atributo como `true` , poderá habilitar o fluxo de informações de exceção gerenciadas para o cliente para fins de depuração, bem como a publicação de arquivos de informações HTML para usuários que navegam no serviço em navegadores da Web. **Cuidado:**  Retornar informações de exceção gerenciada aos clientes pode ser um risco de segurança. Isso ocorre porque os detalhes da exceção expõem informações sobre a implementação do serviço interno que pode ser usada por clientes não autorizados.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<behavior>](behavior-of-endpointbehaviors.md)|Especifica um elemento de comportamento.|  
  
## <a name="remarks"></a>Comentários  
 Configuração `includeExceptionDetailInFaults` para `true` permite que o serviço retorne qualquer exceção que é lançada pelo código do aplicativo, mesmo se a exceção não for declarada usando o <xref:System.ServiceModel.FaultContractAttribute> . Essa configuração é útil ao depurar casos em que o servidor está lançando uma exceção inesperada. Usando esse atributo, uma forma serializada da exceção desconhecida é retornada e você pode examinar mais detalhes da exceção.  
  
> [!CAUTION]
> Retornar informações de exceção gerenciada aos clientes pode ser um risco de segurança, pois os detalhes da exceção expõem informações sobre a implementação do serviço interno que poderia ser usada por clientes não autorizados. Devido aos problemas de segurança envolvidos, é altamente recomendável que você faça isso apenas em cenários de depuração controlados. Você deve definir `includeExceptionDetailInFaults` como `false` ao implantar seu aplicativo.  
  
 Para obter detalhes sobre os problemas de segurança relacionados à exceção gerenciada, consulte [especificando e manipulando falhas em contratos e serviços](../../../wcf/specifying-and-handling-faults-in-contracts-and-services.md). Para obter um exemplo de código, consulte [comportamento de depuração do serviço](../../../wcf/samples/service-debug-behavior.md).  
  
 Você também pode definir `httpsHelpPageEnabled` e `httpsHelpPageUrl` habilitar ou desabilitar a página de ajuda. Cada serviço pode, opcionalmente, expor uma página de ajuda que contém informações sobre o serviço, incluindo o ponto de extremidade para obter o WSDL para o serviço. Isso pode ser habilitado pela configuração `httpHelpPageEnabled` para `true` . Isso permite que a página de ajuda seja retornada a uma solicitação GET para o endereço base do serviço. Você pode alterar esse endereço definindo o `httpHelpPageUrl` atributo. Além disso, você pode torná-la segura usando HTTPS em vez de HTTP.  
  
 Os `httpHelpPageBinding` atributos e opcionais `httpHelpPageBinding` permitem configurar as associações usadas para acessar a página da Web do serviço. Se não forem especificadas, as associações padrão ( `HttpTransportBindingElement` , no caso de http e `HttpsTransportBindingElement` , no caso de HTTPS) serão usadas para acesso à página de ajuda de serviço conforme apropriado. Observe que você não pode usar esses atributos com as associações internas do WCF. Somente associações com elementos de associação interna que dão suporte a xref: System. ServiceModel. Channels. IReplyChannel> terão suporte. Além disso, a <xref:System.ServiceModel.Channels.MessageVersion?displayProperty=nameWithType> propriedade da associação deve ser <xref:System.ServiceModel.Channels.MessageVersion.None?displayProperty=nameWithType> .  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.ServiceDebugElement>
- <xref:System.ServiceModel.Description.ServiceDebugBehavior>
- [Especificando e lidando com falhas em contratos e serviços](../../../wcf/specifying-and-handling-faults-in-contracts-and-services.md)
- [Tratamento de exceções e falhas](../../../wcf/extending/handling-exceptions-and-faults.md)
- [Comportamento de depuração de serviço](../../../wcf/samples/service-debug-behavior.md)
