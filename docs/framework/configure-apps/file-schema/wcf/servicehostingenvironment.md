---
title: <serviceHostingEnvironment>
ms.date: 03/30/2017
ms.assetid: 4f8a7c4f-e735-4987-979a-b74fcdae2652
ms.openlocfilehash: 165dbed1b78d00f8d4dd3e482b9fee8a23db60da
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70399623"
---
# \<serviceHostingEnvironment>
Esse elemento define o tipo que o ambiente de Hospedagem de serviço instancia para um transporte específico. Se esse elemento estiver vazio, o tipo padrão será usado. Esse elemento só pode ser usado nos arquivos de configuração no nível do aplicativo ou do computador.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<serviceHostingEnvironment>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<serviceHostingEnvironment aspNetCompatibilityEnabled="Boolean"
                           minFreeMemoryPercentageToActivateService="Integer"
                           multipleSiteBindingsEnabled="Boolean">
  <baseAddressPrefixFilters>
    <add prefix="string" />
  </baseAddressPrefixFilters>
  <serviceActivations>
    <add factory="String"
         service="String" />
  </serviceActivations>
  <transportConfigurationTypes>
    <add name="String"
         transportConfigurationType="String" />
  </transportConfigurationTypes>
</serviceHostingEnvironment>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|aspNetCompatibilityEnabled|Um valor booliano que indica se o modo de compatibilidade ASP.NET foi ativado para o aplicativo atual. O padrão é `false`.<br /><br /> Quando esse atributo é definido como `true` , as solicitações para Windows Communication Foundation (WCF) fluem por meio do pipeline HTTP ASP.net e a comunicação por protocolos não http é proibida. Para obter mais informações, consulte [Serviços WCF e ASP.net](../../../wcf/feature-details/wcf-services-and-aspnet.md).|  
|minFreeMemoryPercentageToActivateService|Um inteiro que especifica a quantidade mínima de memória livre que deve estar disponível para o sistema, antes que um serviço WCF possa ser ativado. **Cuidado:**  A especificação desse atributo junto com a confiança parcial no arquivo Web. config de um serviço WCF resultará em um <xref:System.Security.SecurityException> quando o serviço for executado.|  
|multipleSiteBindingsEnabled|Um valor booliano que especifica se várias associações IIS por site estão habilitadas.<br /><br /> O IIS consiste em sites da Web, que são contêineres para aplicativos virtuais que contêm diretórios virtuais. O aplicativo em um site pode ser acessado por meio de uma ou mais associações do IIS. Uma associação do IIS fornece duas informações: um protocolo de associação e informações de associação. O protocolo de associação define o esquema sobre o qual ocorre a comunicação e informações de associação são as informações usadas para acessar o site. Um exemplo de protocolo de associação pode ser HTTP, enquanto as informações de associação podem conter um endereço IP, uma porta, um cabeçalho de host, etc.<br /><br /> O IIS dá suporte à especificação de várias associações do IIS por site, o que resulta em vários endereços base por esquema. No entanto, um serviço de Windows Communication Foundation (WCF) hospedado em um site permite a associação a apenas um baseAddress por esquema.<br /><br /> Para habilitar várias associações do IIS por site para um serviço Windows Communication Foundation (WCF), defina esse atributo como `true` . Observe que a associação de vários sites tem suporte apenas para o protocolo HTTP. O endereço dos pontos de extremidade no arquivo de configuração precisa ser um URI completo.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<baseAddressPrefixFilters>](baseaddressprefixfilters.md)|Uma coleção de elementos de configuração que especificam filtros de prefixo para os endereços base usados pelo host de serviço.|  
|[\<serviceActivations>](serviceactivations.md)|Uma seção de configuração que descreve as configurações de ativação.|  
|[\<transportConfigurationTypes>](transportconfigurationtypes.md)|Uma coleção de elementos de configuração que identificam o tipo de um transporte específico.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|serviceModel|O elemento raiz de todos os elementos de configuração de Windows Communication Foundation (WCF).|  
  
## <a name="remarks"></a>Comentários  
 Por padrão, os serviços WCF são executados lado a lado com ASP.NET em domínios de aplicativo hospedado (AppDomain). Embora o WCF e o ASP.NET possam coexistir no mesmo AppDomain, as solicitações do WCF não são processadas pelo pipeline HTTP do ASP.NET por padrão. Como resultado, vários elementos da plataforma de aplicativo ASP.NET não estão disponíveis para os serviços WCF. Isso inclui  
  
- Autorização de arquivo/URL ASP.NET  
  
- Personificação do ASP.NET  
  
- Estado de sessão baseado em cookie  
  
- HttpContext. Current  
  
- Extensibilidade de pipeline por meio de HttpModule personalizado  
  
 Se os serviços WCF precisarem funcionar no contexto ASP.NET e se comunicarem somente por meio de HTTP, você poderá usar o modo de compatibilidade ASP.NET do WCF. Esse modo é ativado quando o `aspNetCompatibilityEnabled` atributo é definido como `true` no nível do aplicativo. As implementações de serviço devem declarar sua capacidade de execução no modo de compatibilidade usando a <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsAttribute> classe. Quando o modo de compatibilidade estiver habilitado,  
  
- A autorização de arquivo/URL ASP.NET é imposta antes da autorização do WCF. Uma decisão de autorização é baseada na identidade de nível de transporte da solicitação. As identidades no nível da mensagem são ignoradas.  
  
- As operações de serviço WCF começam a ser executadas no contexto de representação ASP.NET. Se a representação do ASP.NET e a representação do WCF estiverem habilitadas para um serviço específico, o contexto de representação do WCF será aplicado.  
  
- HttpContext. Current pode ser usado no código do serviço WCF, e os serviços são impedidos de expor pontos de extremidade não HTTP.  
  
- As solicitações do WCF são processadas pelo pipeline ASP.NET. HttpModules que foram configurados para atuar em solicitações de entrada também podem processar solicitações do WCF. Eles podem incluir componentes de plataforma ASP.NET (por exemplo, <xref:System.Web.SessionState.SessionStateModule> ), bem como módulos de terceiros personalizados.  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir mostra como habilitar o modo de compatibilidade ASP.  
  
## <a name="code"></a>Código  
  
```xml  
<serviceHostingEnvironment aspNetCompatibilityEnabled="true"/>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.ServiceHostingEnvironmentSection>
- <xref:System.ServiceModel.ServiceHostingEnvironment>
- [Hosting](../../../wcf/feature-details/hosting.md)
- [Serviços WCF e ASP.NET](../../../wcf/feature-details/wcf-services-and-aspnet.md)
