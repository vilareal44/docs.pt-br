---
title: <securityTokenHandlerConfiguration>
ms.date: 03/30/2017
ms.assetid: 28724cc6-020c-4a06-9a1f-d7594f315019
author: BrucePerlerMS
ms.openlocfilehash: e3e65820fa4dc341371d4f67689a288cd3f63951
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79152561"
---
# \<securityTokenHandlerConfiguration>
Fornece a configuração para a coleção de manipuladores de token.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel>**](system-identitymodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<identityConfiguration>**](identityconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<securityTokenHandlers>**](securitytokenhandlers.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<securityTokenHandlerConfiguration>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<system.identityModel>  
  <identityConfiguration>  
    <securityTokenHandlers>  
      <securityTokenHandlerConfiguration saveBootstrapContext=xs:boolean  
          maximumClockSkew=TimeSpan>  
      </securityTokenHandlerConfiguration>  
    </securityTokenHandlers>  
  </identityConfiguration>  
</system.identityModel>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|saveBootstrapContext|Especifica se os tokens de inicialização devem ser incluídos no token de sessão. O valor também pode ser definido em uma coleção de manipuladores de token, definindo o `saveBootstrapContext` atributo no [\<identityConfiguration>](identityconfiguration.md) elemento. Um valor definido na coleção de manipuladores de token substitui o valor definido no serviço.|  
|maximumClockSkew|Um <xref:System.TimeSpan> valor que especifica a distorção máxima permitida do relógio. Controla o máximo permitido de distorção de relógio ao executar operações sensíveis ao tempo, como validar o tempo de expiração de uma sessão de entrada. O padrão é 5 minutos, "00:05:00". Para obter mais informações sobre como especificar <xref:System.TimeSpan> valores, consulte [valores de TimeSpan](../windows-workflow-foundation/index.md). A distorção máxima do relógio também pode ser definida no nível de serviço definindo o `maximumClockSkew` atributo no [\<identityConfiguration>](identityconfiguration.md) elemento. Um valor definido na coleção de manipuladores de token substitui o valor definido no serviço.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<audienceUris>](audienceuris.md)|Especifica o conjunto de URIs que são identificadores aceitáveis dessa terceira parte confiável. Opcional.|  
|[\<caches>](caches.md)|Registra os caches usados para tokens de sessão e detecção de reprodução de token. Pode ser especificado no nível de serviço ou em uma coleção de manipulador de token de segurança. Opcional.|  
|[\<certificateValidation>](certificatevalidation.md)|Controla as configurações que os manipuladores de token usam para validar certificados. Pode ser especificado no nível de serviço ou em uma coleção de manipulador de token de segurança. Essas configurações serão substituídas se um manipulador específico estiver configurado com seu próprio validador. Opcional.|  
|[\<issuerNameRegistry>](issuernameregistry.md)|Configura o registro de nome do emissor que é usado pelos manipuladores na coleção de manipuladores de token. Opcional.|  
|[\<issuerTokenResolver>](issuertokenresolver.md)|Registra o resolvedor de token do emissor que é usado por manipuladores na coleção de manipulador de token. O resolvedor de token do emissor é usado para resolver o token de assinatura em mensagens e tokens de entrada. Opcional.|  
|[\<serviceTokenResolver>](servicetokenresolver.md)|Registra o resolvedor de token de serviço que é usado pelos manipuladores na coleção de manipuladores de token. O resolvedor de token de serviço é usado para resolver o token de criptografia em mensagens e tokens de entrada. Opcional.|  
|[\<tokenReplayDetection>](tokenreplaydetection.md)|Habilita a detecção de reprodução de token e especifica o tempo de expiração para tokens. Pode ser especificado no nível de serviço ou em uma coleção de manipulador de token de segurança. Opcional.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<securityTokenHandlers>](securitytokenhandlers.md)|Especifica uma coleção de manipuladores de token de segurança que são registrados com o ponto de extremidade.|  
  
## <a name="remarks"></a>Comentários  
 Esta seção fornece valores de propriedade para um <xref:System.IdentityModel.Tokens.SecurityTokenHandlerConfiguration> objeto. As configurações definidas nesta seção substituem aquelas configuradas no serviço. Por sua vez, algumas dessas configurações podem ser substituídas pelas configurações que são especificadas quando um manipulador é adicionado à coleção do manipulador de token de segurança.  
  
## <a name="example"></a>Exemplo  
  
```xml  
<system.identityModel>  
  <identityConfiguration>  
    <securityTokenHandlers>
      <securityTokenHandlerConfiguration>  
  
        <audienceUris>  
          <clear/>  
          <add value="http://www.example.com/myapp/" />  
        </audienceUris>  
  
        <issuerNameRegistry type="System.IdentityModel.Tokens.ConfigurationBasedIssuerNameRegistry, System.IdentityModel">  
          <trustedIssuers>  
            <add thumbprint="97249e1a … 4c9158de" name="contoso.com" />  
          </trustedIssuers>  
        </issuerNameRegistry>  
  
        <issuerTokenResolver type="MyNamespace.CustomTokenResolver, MyAssembly" />  
  
        <serviceTokenResolver type="MyNamespace.CustomTokenResolver, MyAssembly" />  
  
      </securityTokenHandlerConfiguration>  
    </securityTokenHandlers>  
  </identityConfiguration>  
</system.identityModel>  
```
