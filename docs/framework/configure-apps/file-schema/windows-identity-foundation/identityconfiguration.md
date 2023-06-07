---
title: <identityConfiguration>
ms.date: 03/30/2017
ms.assetid: 1db76253-07da-447b-9e7a-3705c7228cf4
author: BrucePerlerMS
ms.openlocfilehash: 0fa8c574fd5663606cf081f1000a24884306edfe
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70251989"
---
# \<identityConfiguration>

Especifica as configurações de identidade de nível de serviço.

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel>**](system-identitymodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<identityConfiguration>**  

## <a name="syntax"></a>Sintaxe

```xml
<system.identityModel>
  <identityConfiguration
      name=xs:string
      saveBootstrapContext=xs:boolean>
      maximumClockSkew=TimeSpan >
  </identityConfiguration>
</system.identityModel>
```

## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai.

### <a name="attributes"></a>Atributos

|Atributo|Descrição|
|---------------|-----------------|
|name|O nome da seção de configuração de identidade. Você pode usar esse nome para fazer referência a uma seção de configuração específica. Se nenhum `name` atributo for especificado, a seção definirá a configuração padrão. A configuração padrão é sempre usada para cenários de federação passiva. Para obter mais informações, consulte o [\<federationConfiguration>](federationconfiguration.md) elemento.|
|saveBootstrapContext|Especifica se os tokens de inicialização devem ser incluídos no token de sessão. O valor também pode ser definido em uma coleção de manipuladores de token, definindo o `saveBootstrapContext` atributo no [\<securityTokenHandlerConfiguration>](securitytokenhandlerconfiguration.md) elemento. Um valor definido na coleção de manipuladores de token substitui o valor definido no serviço.|
|maximumClockSkew|Um <xref:System.TimeSpan> valor que especifica a distorção máxima permitida do relógio. Controla o máximo permitido de distorção de relógio ao executar operações sensíveis ao tempo, como validar o tempo de expiração de uma sessão de entrada. O padrão é 5 minutos, "00:05:00". Para obter mais informações sobre como especificar <xref:System.TimeSpan> valores, consulte [valores de TimeSpan](../windows-workflow-foundation/index.md). A distorção máxima do relógio também pode ser definida em uma coleção de manipulador de tokens definindo o `maximumClockSkew` atributo no [\<securityTokenHandlerConfiguration>](securitytokenhandlerconfiguration.md) elemento. Um valor definido na coleção de manipuladores de token substitui o valor definido no serviço.|

### <a name="child-elements"></a>Elementos filho

|Elemento|Descrição|
|-------------|-----------------|
|[\<caches>](caches.md)|Registra os caches usados para tokens de sessão e detecção de reprodução de token. Pode ser especificado no nível de serviço ou em uma coleção de manipulador de token de segurança. Opcional.|
|[\<certificateValidation>](certificatevalidation.md)|Controla as configurações que os manipuladores de token usam para validar certificados. Pode ser especificado no nível de serviço ou em uma coleção de manipulador de token de segurança. Opcional.|
|[\<claimsAuthenticationManager>](claimsauthenticationmanager.md)|Registra um Gerenciador de autenticação de declarações para as declarações de entrada. Opcional.|
|[\<claimsAuthorizationManager>](claimsauthorizationmanager.md)|Registra um Gerenciador de autorização de declarações para as declarações de entrada. Opcional.|
|[\<claimTypeRequired>](claimtyperequired.md)|Especifica o conjunto de declarações necessárias para tokens de segurança de entrada. Opcional.|
|[\<securityTokenHandlers>](securitytokenhandlers.md)|Especifica uma coleção de manipuladores de token de segurança. Zero ou mais coleções de manipuladores de token de segurança podem ser especificadas. Opcional.|
|[\<tokenReplayDetection>](tokenreplaydetection.md)|Habilita a detecção de reprodução de token e especifica o tempo de expiração para tokens. Pode ser especificado no nível de serviço ou em uma coleção de manipulador de token de segurança. Opcional.|

### <a name="parent-elements"></a>Elementos pai

|Elemento|Descrição|
|-------------|-----------------|
|[\<system.identityModel>](system-identitymodel.md)|Fornece a configuração para habilitar as opções do Windows Identity Foundation (WIF) em aplicativos.|

## <a name="remarks"></a>Comentários

Várias configurações de identidade podem ser definidas, cada uma com um nome exclusivo. O comportamento é o seguinte:

1. Se nenhum `<identityConfiguration>` elemento for especificado. Uma configuração de identidade padrão é criada em tempo de execução e preenchida com valores padrão.

2. Se um único `<identityConfiguration>` elemento for especificado. É a configuração de identidade padrão. Não importa se ele é nomeado ou não.

3. Se vários `<identityConfiguration>` elementos forem especificados. O elemento sem nome especifica a configuração de identidade padrão. É recomendável que, quando você especificar vários `<identityConfiguration>` elementos, um deles não seja nomeado.

> [!WARNING]
> Se você especificar vários `<identityConfiguration>` elementos, um deles deverá ser sem nome. O elemento sem nome será a configuração de identidade padrão.

 Algumas das configurações especificadas no `<identityConfiguration>` elemento podem ser substituídas por configurações em uma coleção de manipulador de token de segurança ou por configurações em manipuladores de token de segurança individuais.

> [!IMPORTANT]
> Ao usar o <xref:System.IdentityModel.Services.ClaimsPrincipalPermission> ou a <xref:System.IdentityModel.Services.ClaimsPrincipalPermissionAttribute> classe para fornecer controle de acesso baseado em declarações em seu código, a configuração de identidade referenciada pelo `<federationConfiguration>` elemento configura o Gerenciador de autorização de declarações e a política que é usada para tomar decisões de autorização. Isso é verdade, mesmo em cenários que não são cenários da Web passivos, por exemplo Windows Communication Foundation (WCF) ou um aplicativo que não é baseado na Web. Se o aplicativo não for um aplicativo Web passivo, o [\<claimsAuthorizationManager>](claimsauthorizationmanager.md) elemento (e seus elementos de política filho, se presentes) da configuração de identidade referenciada serão as únicas configurações aplicadas. Todas as outras configurações são ignoradas. Para obter mais informações, consulte o [\<federationConfiguration>](federationconfiguration.md) elemento.

O `<identityConfiguration>` elemento é representado pela <xref:System.IdentityModel.Configuration.IdentityConfigurationElement> classe. Uma seção de configuração de identidade é representada pela <xref:System.IdentityModel.Configuration.IdentityConfiguration> classe.

> [!IMPORTANT]
> A especificação dos elementos a seguir como elementos filho do `<identityConfiguration>` elemento foi preterida, embora o comportamento ainda tenha suporte para compatibilidade com versões anteriores. Em vez disso, esses elementos devem ser especificados no [\<securityTokenHandlerConfiguration>](securitytokenhandlerconfiguration.md) elemento.
>
> - [\<audienceUris>](audienceuris.md)
> - [\<issuerNameRegistry>](issuernameregistry.md)
> - [\<issuerTokenResolver>](issuertokenresolver.md)
> - [\<serviceTokenResolver>](servicetokenresolver.md)

## <a name="example"></a>Exemplo

O exemplo a seguir cria uma configuração de identidade chamada "alternateConfiguration". A configuração de identidade especifica as configurações padrão.

```xml
<system.identityModel>
    <identityConfiguration name="alternateConfiguration"/>
</system.identityModel>
```

## <a name="see-also"></a>Confira também

- <xref:System.IdentityModel.Configuration.IdentityConfiguration>
- <xref:System.IdentityModel.Configuration.IdentityConfigurationElement>
