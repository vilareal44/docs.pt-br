---
title: <caches>
ms.date: 03/30/2017
ms.assetid: 4651091b-3a20-40d8-b293-4408c0710143
author: BrucePerlerMS
ms.openlocfilehash: 80f435b52fd7657c5cd44538028d6080beffe0b5
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70252155"
---
# \<caches>
Registra os caches usados para tokens de sessão e detecção de reprodução de token.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel>**](system-identitymodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<identityConfiguration>**](identityconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<caches>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<system.identityModel>  
  <identityConfiguration>  
    <caches>  
    </caches>  
  </identityConfiguration>  
</system.identityModel>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
 Nenhum  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<sessionSecurityTokenCache>](sessionsecuritytokencache.md)|Registra um cache para tokens de sessão com um serviço ou uma coleção de manipulador de token de segurança.|  
|[\<tokenReplayCache>](tokenreplaycache.md)|Registra um cache de reprodução de token com um serviço ou uma coleção de manipulador de token de segurança.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<identityConfiguration>](identityconfiguration.md)|Especifica as configurações de identidade de nível de serviço.|  
|[\<securityTokenHandlerConfiguration>](securitytokenhandlerconfiguration.md)|Fornece a configuração para uma coleção de manipuladores de token de segurança.|  
  
## <a name="remarks"></a>Comentários  
 Um `<caches>` elemento pode ser especificado no nível de serviço sob o `<identityConfiguration>` elemento ou no nível de coleção do manipulador de token de segurança sob o `<securityTokenHandlerConfiguration>` elemento. As configurações em uma coleção de manipulador de tokens substituem aquelas especificadas no serviço.  
  
 O `<caches>` elemento é representado pela <xref:System.IdentityModel.Configuration.IdentityModelCachesElement> classe. Os caches configurados são representados pela <xref:System.IdentityModel.Configuration.IdentityModelCaches> classe.  
  
## <a name="example"></a>Exemplo  
 O XML a seguir mostra a configuração de um cache personalizado para manter os tokens de segurança de sessão ( <xref:System.IdentityModel.Tokens.SessionSecurityToken> ). A configuração é obtida do `ClaimsAwareWebFarm` exemplo.  
  
```xml  
<caches>  
  <sessionSecurityTokenCache type="CacheLibrary.SharedSessionSecurityTokenCache, CacheLibrary">  
    <!--cacheServiceAddress points to the centralized session security token cache service running in the web farm.-->  
    <cacheServiceAddress url="http://localhost:4161/SessionSecurityTokenCacheService.svc" />  
  </sessionSecurityTokenCache>  
</caches>  
```
