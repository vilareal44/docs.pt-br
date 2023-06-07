---
title: <nameClaimType>
ms.date: 03/30/2017
ms.assetid: 17514d95-f0f5-4789-8e28-346640dc227c
author: BrucePerlerMS
ms.openlocfilehash: 4bf8ad2f70499edfc72dd9fcd9a5d8a0aafbbc66
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70251933"
---
# \<nameClaimType>
Define o tipo de declaração que especifica a <xref:System.Security.Principal.IIdentity.Name%2A> propriedade. O tipo de declaração é usado para pesquisar um <xref:System.Security.Claims.Claim> na coleção de <xref:System.Security.Claims.ClaimsIdentity> objetos retornados pelo <xref:System.IdentityModel.Tokens.SecurityTokenHandler.ValidateToken%2A> método desse manipulador de tokens. O valor da declaração de correspondência é definido como o nome do gerado a <xref:System.Security.Principal.IIdentity> partir desse manipulador de token.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel>**](system-identitymodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<identityConfiguration>**](identityconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<securityTokenHandlers>**](securitytokenhandlers.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<add>**](add.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<samlSecurityTokenRequirement>**](samlsecuritytokenrequirement.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<nameClaimType>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<system.identityModel>  
  <identityConfiguration>  
    <securityTokenHandlers>  
      <add>  
        <samlSecurityTokenRequirement>  
          <nameClaimType value=xs:string>  
          </nameClaimType>  
        </samlSecurityTokenRequirement>  
      </add>  
    </securityTokenHandlers>  
  </identityConfiguration>  
</system.identityModel>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|value|Uma cadeia de caracteres que especifica o URI que representa o tipo de declaração da declaração a ser usada para a <xref:System.Security.Principal.IIdentity.Name%2A> propriedade. Obrigatórios.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<samlSecurityTokenRequirement>](samlsecuritytokenrequirement.md)|Fornece a configuração para a <xref:System.IdentityModel.Tokens.SamlSecurityTokenHandler> classe, a <xref:System.IdentityModel.Tokens.Saml2SecurityTokenHandler> classe ou uma classe derivada de qualquer uma dessas classes.|  
  
## <a name="remarks"></a>Comentários  
 O `<nameClaimType>` elemento define a <xref:System.IdentityModel.Tokens.SamlSecurityTokenRequirement.NameClaimType%2A> propriedade quando um <xref:System.IdentityModel.Tokens.SamlSecurityTokenRequirement> objeto é inicializado a partir da configuração.  
  
## <a name="example"></a>Exemplo  
  
```xml  
<add type="System.IdentityModel.Tokens.SamlSecurityTokenHandler, System.IdentityModel">  
    <samlSecurityTokenRequirement>  
        <nameClaimType value="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name" />  
    </samlSecurityTokenRequirement>  
</add>  
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.IdentityModel.Tokens.SamlSecurityTokenRequirement.NameClaimType%2A>
