---
title: <claimsAuthorizationManager>
ms.date: 03/30/2017
ms.assetid: 9354eee3-f692-4ad6-8427-3169686b8bcc
author: BrucePerlerMS
ms.openlocfilehash: ddbe8a862940272e4192a3f4c0abdc1f9e8b5d48
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70252077"
---
# \<claimsAuthorizationManager>
Registra um Gerenciador de autorização de declarações para as declarações de entrada.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel>**](system-identitymodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<identityConfiguration>**](identityconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<claimsAuthorizationManager>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<system.identityModel>  
  <identityConfiguration>  
    <claimsAuthorizationManager type = xs:string>  
      <optionalConfigurationElements />  
    </claimsAuthorizationManager>  
  </identityConfiguration>  
</system.identityModel>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|type|Um tipo personalizado que deriva da <xref:System.Security.Claims.ClaimsAuthorizationManager> classe. Para obter mais informações sobre como especificar o `type` atributo, consulte [referências de tipo personalizado](../windows-workflow-foundation/index.md).|  
  
### <a name="child-elements"></a>Elementos filho  
 Se não houver nenhum `type` atributo ou se o `type` atributo fizer referência à <xref:System.Security.Claims.ClaimsAuthenticationManager> classe, o `<claimsAuthorizationManager>` elemento não assumirá elementos filho; no entanto, as classes derivadas de <xref:System.Security.Claims.ClaimsAuthorizationManager> podem definir elementos de configuração filho.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<identityConfiguration>](identityconfiguration.md)|Especifica as configurações de identidade de nível de serviço.|  
  
## <a name="remarks"></a>Comentários  
 O comportamento padrão fornecido por meio da <xref:System.Security.Claims.ClaimsAuthorizationManager> classe sempre autoriza as declarações de entrada. Se nenhum `type` atributo for especificado ou se o `type` atributo especificar a <xref:System.Security.Claims.ClaimsAuthorizationManager> classe, o `<claimsAuthorizationManager>` elemento não assumirá elementos filho. Você pode especificar o `type` atributo para registrar um tipo derivado da <xref:System.Security.Claims.ClaimsAuthorizationManager> classe para implementar o comportamento personalizado. Classes derivadas podem dar suporte à configuração por meio de elementos filho do `<claimsAuthorizationManager>` elemento, substituindo o <xref:System.Security.Claims.ClaimsAuthorizationManager.LoadCustomConfiguration%2A> método para lidar com esses elementos. O esquema definido para os elementos filho é até o designer da classe.  
  
> [!IMPORTANT]
> Ao usar o <xref:System.IdentityModel.Services.ClaimsPrincipalPermission> ou a <xref:System.IdentityModel.Services.ClaimsPrincipalPermissionAttribute> classe para fornecer controle de acesso baseado em declarações em seu código, a configuração de identidade referenciada pelo `<federationConfiguration>` elemento configura o Gerenciador de autorização de declarações e a política que é usada para tomar decisões de autorização. Isso é verdade, mesmo em cenários que não são cenários da Web passivos, por exemplo Windows Communication Foundation (WCF) ou um aplicativo que não é baseado na Web. Se o aplicativo não for um aplicativo Web passivo, o `<claimsAuthorizationManager>` elemento (e seus elementos de política filho, se presentes) da configuração de identidade referenciada serão as únicas configurações aplicadas. Todas as outras configurações são ignoradas. Para obter mais informações, consulte o [\<federationConfiguration>](federationconfiguration.md) elemento.  
  
 Esse elemento define a <xref:System.IdentityModel.Configuration.IdentityConfiguration.ClaimsAuthorizationManager%2A?displayProperty=nameWithType> propriedade.  
  
## <a name="example"></a>Exemplo  
 O XML a seguir mostra a configuração de um Gerenciador de autorização de declarações que implementa a política composta por pares de recurso-ação, cada um dos quais especifica combinações booleanas das declarações que um solicitante deve ter para executar a ação no recurso. O código que implementa o Gerenciador de autorização de declarações capaz de usar essa política pode ser encontrado no `ClaimsBasedAuthorization` exemplo.  
  
```xml  
<system.identityModel>  
    <identityConfiguration>  
      <claimsAuthorizationManager type="ClaimsAuthorizationLibrary.MyClaimsAuthorizationManager, ClaimsAuthorizationLibrary">  
        <policy resource="http://localhost:28491/Developers.aspx" action="GET">  
          <or>  
            <claim claimType="http://schemas.microsoft.com/ws/2008/06/identity/claims/role" claimValue="developer" />  
            <claim claimType="http://schemas.xmlsoap.org/claims/Group" claimValue="Administrator" />  
          </or>  
        </policy>  
        <policy resource="http://localhost:28491/Administrators.aspx" action="GET">  
          <and>  
            <claim claimType="http://schemas.xmlsoap.org/claims/Group" claimValue="Administrator" />  
            <claim claimType="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/country" claimValue="USA" />  
          </and>  
        </policy>  
        <policy resource="http://localhost:28491/Default.aspx" action="GET">  
        </policy>  
        <policy resource="http://localhost:28491/" action="GET">  
        </policy>  
        <policy resource="http://localhost:28491/Claims.aspx" action="GET">  
        </policy>  
      </claimsAuthorizationManager>  
    <identityConfiguration>  
<system.identityModel>  
```
