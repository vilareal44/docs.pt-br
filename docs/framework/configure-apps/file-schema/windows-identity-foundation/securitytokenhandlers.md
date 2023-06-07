---
title: <securityTokenHandlers>
ms.date: 03/30/2017
ms.assetid: f11a631d-4094-4e11-bb03-4ede74b30281
author: BrucePerlerMS
ms.openlocfilehash: 017309436660991c69da569e9cc4219e842ecaa3
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70251871"
---
# \<securityTokenHandlers>
Especifica uma coleção de manipuladores de token de segurança que são registrados com o ponto de extremidade.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel>**](system-identitymodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<identityConfiguration>**](identityconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<securityTokenHandlers>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<system.identityModel>  
  <identityConfiguration>  
    <securityTokenHandlers>  
    </securityTokenHandlers>  
  </identityConfiguration>  
</system.identityModel>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|name|Especifica o nome de uma coleção de manipulador de token. Os únicos valores reconhecidos pela estrutura são "ActAs" e "OnBehalfOf". Se as coleções do manipulador de token forem especificadas com um desses nomes, a coleção será usada ao processar os tokens ActAs ou OnBehalfOf, respectivamente.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<add>](add.md)|Adiciona um manipulador de token de segurança à coleção do manipulador de token.|  
|[\<clear>](clear.md)|Limpa todos os manipuladores de token de segurança da coleção do manipulador de token.|  
|[\<remove>](remove.md)|Remove um manipulador de token de segurança da coleção de manipuladores de token.|  
|[\<securityTokenHandlerConfiguration>](securitytokenhandlerconfiguration.md)|Fornece a configuração para a coleção de manipuladores de token.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<identityConfiguration>](identityconfiguration.md)|Especifica as configurações de identidade de nível de serviço.|  
  
## <a name="remarks"></a>Comentários  
 Você pode especificar uma ou mais coleções nomeadas de manipuladores de token de segurança em uma configuração de serviço. Você pode especificar um nome para uma coleção usando o `name` atributo. Os únicos nomes que a estrutura manipula são "ActAs" e "OnBehalfOf". Se houver manipuladores nessas coleções, eles serão usados por um serviço de token de segurança (STS) em vez dos manipuladores padrão durante o processamento `ActAs` e os `OnBehalfOf` tokens.  
  
 Por padrão, a coleção é populada com os seguintes tipos de manipuladores:,,,,, <xref:System.IdentityModel.Tokens.SamlSecurityTokenHandler> <xref:System.IdentityModel.Tokens.Saml2SecurityTokenHandler> <xref:System.IdentityModel.Tokens.KerberosSecurityTokenHandler> <xref:System.IdentityModel.Tokens.WindowsUserNameSecurityTokenHandler> <xref:System.IdentityModel.Tokens.RsaSecurityTokenHandler> <xref:System.IdentityModel.Tokens.X509SecurityTokenHandler> e <xref:System.IdentityModel.Tokens.EncryptedSecurityTokenHandler> . Você pode modificar a coleção usando os `<add>` elementos, `<remove>` e `<clear>` . Você deve garantir que apenas um único manipulador de qualquer tipo específico exista na coleção. Por exemplo, se você derivar um manipulador da <xref:System.IdentityModel.Tokens.Saml2SecurityTokenHandler> classe, o manipulador ou o <xref:System.IdentityModel.Tokens.Saml2SecurityTokenHandler> poderá ser configurado em uma única coleção, mas não em ambos.  
  
 Use o `<securityTokenHandlerConfiguration>` elemento para especificar as definições de configuração para os manipuladores na coleção. As configurações especificadas por meio desse elemento substituem aquelas especificadas no serviço por meio do [\<identityConfiguration>](identityconfiguration.md) elemento. Alguns manipuladores (incluindo vários dos tipos de manipuladores internos) podem dar suporte à configuração adicional por meio de um elemento filho do `<add>` elemento. As configurações especificadas em um manipulador substituem as configurações equivalentes especificadas na coleção ou no serviço.
