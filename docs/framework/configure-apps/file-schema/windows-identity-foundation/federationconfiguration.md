---
title: <federationConfiguration>
ms.date: 03/30/2017
ms.assetid: 8b14054c-6d07-46ab-ab58-03f14beac0f2
author: BrucePerlerMS
ms.openlocfilehash: bcd8e00b770517e3faff011b4acee08ebdc5a0df
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79152730"
---
# \<federationConfiguration>
Configura o <xref:System.IdentityModel.Services.WSFederationAuthenticationModule> (WSFAM) e o <xref:System.IdentityModel.Services.SessionAuthenticationModule> (Sam) ao usar a autenticação federada por meio do protocolo WS-Federation. Configura o <xref:System.Security.Claims.ClaimsAuthorizationManager> ao usar a <xref:System.IdentityModel.Services.ClaimsPrincipalPermission> <xref:System.IdentityModel.Services.ClaimsPrincipalPermissionAttribute> classe ou para fornecer controle de acesso baseado em declarações.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel.services>**](system-identitymodel-services.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<federationConfiguration>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<system.identityModel.services>  
  <federationConfiguration name=xs:string identityConfigurationName=xs:string>  
  </federationConfiguration>  
</system.identityModel.services>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|name|O nome deste elemento de configuração da Federação. Esse atributo fornece principalmente um ponto de extensibilidade para protocolos futuros. Opcional.|  
|identityConfigurationName|O nome da seção de configuração de identidade, conforme especificado em um [\<identityConfiguration>](identityconfiguration.md) elemento a ser usado. Se esse atributo não for especificado, a seção de configuração de identidade padrão será usada. Opcional.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<cookieHandler>](cookiehandler.md)|Configura o manipulador de cookies usado pelo SAM. Opcional.|  
|[\<serviceCertificate>](servicecertificate.md)|Configura o certificado que é usado para criptografar e descriptografar tokens. Opcional.|  
|[\<wsFederation>](wsfederation.md)|Configura o módulo de autenticação do WS-Federation (WSFAM). Opcional.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<system.identityModel.services>](system-identitymodel-services.md)|A seção de configuração para autenticação usando o protocolo WS-Federation.|  
  
## <a name="remarks"></a>Comentários  
 O \<federationConfiguration> elemento fornece configurações em dois cenários diferentes:  
  
- Ao usar o WS-Federation em um aplicativo Web passivo, o elemento contém configurações que definem o <xref:System.IdentityModel.Services.WSFederationAuthenticationModule> (WSFAM) e o <xref:System.IdentityModel.Services.SessionAuthenticationModule> (Sam). Ele também faz referência à configuração de identidade a ser usada para configurar manipuladores de token de segurança e certificados, além de componentes como o Gerenciador de autorização de declarações e o Gerenciador de autenticação de declarações.  
  
- Ao usar o <xref:System.IdentityModel.Services.ClaimsPrincipalPermission> ou a <xref:System.IdentityModel.Services.ClaimsPrincipalPermissionAttribute> classe para fornecer controle de acesso baseado em declarações em seu código, o elemento faz referência à configuração de identidade que configura o Gerenciador de autorização de declarações e a política que é usada para tomar decisões de autorização. Isso é verdade, mesmo em cenários que não são cenários da Web passivos; por exemplo, aplicativos Windows Communication Foundation (WCF) ou um aplicativo que não é baseado na Web. Se o aplicativo não for um aplicativo Web passivo, o [\<claimsAuthorizationManager>](claimsauthorizationmanager.md) elemento (e seus elementos filho de política, se presente) da configuração de identidade referenciada pelo `<federationConfiguration>` elemento serão as únicas configurações aplicadas. Todos os outros são ignorados.  
  
 Independentemente do cenário, o tempo de execução carrega a configuração de Federação padrão. O comportamento é definido da seguinte maneira:  
  
1. Se não houver nenhum `<federationConfiguration>` elemento presente, o tempo de execução criará uma configuração de Federação e a preencherá com valores padrão. Essa configuração de Federação padrão fará referência à configuração de identidade padrão.  
  
2. Se um único `<federationConfiguration>` elemento estiver presente, será a configuração de Federação padrão, independentemente de ter sido nomeada ou não nomeada. Se seu `identityConfiguration` atributo for especificado, a configuração de identidade nomeada será referenciada; caso contrário, a configuração de identidade padrão será referenciada.  
  
3. Se um elemento sem nome `<federationConfiguration>` estiver presente, será a configuração de Federação padrão. Se seu `identityConfiguration` atributo for especificado, a configuração de identidade nomeada será referenciada; caso contrário, a configuração de identidade padrão será referenciada.  
  
4. Se vários elementos nomeados `<federationConfiguration>` estiverem presentes e nenhum elemento sem nome `<federationConfiguration>` estiver presente, uma exceção será lançada.  
  
 Normalmente, apenas uma única `<federationConfiguration>` seção é definida. Esta seção é a configuração de Federação padrão. Você pode especificar vários elementos nomeados com exclusividade `<federationConfiguration>` ; no entanto, nesse caso, se quiser carregar uma configuração de Federação diferente da que não foi nomeada, você deverá fornecer um manipulador para o. <xref:System.IdentityModel.Services.FederatedAuthentication.FederationConfigurationCreated>e defina a <xref:System.IdentityModel.Services.Configuration.FederationConfigurationCreatedEventArgs.FederationConfiguration%2A?displayProperty=nameWithType> propriedade dentro do manipulador para um <xref:System.IdentityModel.Services.Configuration.FederationConfiguration> objeto inicializado com valores do elemento apropriado `<federationConfiguration>` no arquivo de configuração.  
  
 O `<federationConfiguration>` elemento é representado pela <xref:System.IdentityModel.Services.Configuration.FederationConfigurationElement> classe. O próprio objeto de configuração é representado pela <xref:System.IdentityModel.Services.Configuration.FederationConfiguration> classe. Uma única <xref:System.IdentityModel.Services.Configuration.FederationConfiguration> instância é definida na <xref:System.IdentityModel.Services.FederatedAuthentication.FederationConfiguration%2A?displayProperty=nameWithType> propriedade e fornece configuração federada para o aplicativo.  
  
## <a name="example"></a>Exemplo  
 O XML a seguir mostra um `<federationConfiguration>` elemento que especifica as configurações para o WSFAM e especifica que o manipulador de cookie padrão (uma instância da <xref:System.IdentityModel.Services.ChunkedCookieHandler> classe) seja usado pelo Sam.  
  
> [!WARNING]
> Neste exemplo, nem o manipulador de cookie nem WSFAM são necessários para usar HTTPS. Isso ocorre porque o `requireHttps` atributo no `<wsFederation>` elemento e o `requireSsl` atributo no `<cookieHandlerElement>` são `false` . Essas configurações não são recomendadas para a maioria dos ambientes de produção, pois podem representar um risco de segurança.  
  
```xml  
<system.identityModel.services>  
  <federationConfiguration>  
    <wsFederation passiveRedirectEnabled="true"
      issuer="http://localhost:15839/wsFederationSTS/Issue"
      realm="http://localhost:50969/" reply="http://localhost:50969/"
      requireHttps="false"
      signOutReply="http://localhost:50969/SignedOutPage.html"
      signOutQueryString="Param1=value2&Param2=value2"
      persistentCookiesOnPassiveRedirects="true" />  
    <cookieHandler requireSsl="false" />  
  </federationConfiguration>  
</system.identityModel.services>  
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.IdentityModel.Services.WSFederationAuthenticationModule>
- <xref:System.IdentityModel.Services.SessionAuthenticationModule>
- <xref:System.IdentityModel.Services.FederatedAuthentication.FederationConfiguration%2A?displayProperty=nameWithType>
- [\<identityConfiguration>](identityconfiguration.md)
