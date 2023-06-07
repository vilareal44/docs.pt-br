---
title: <audienceUris>
ms.date: 03/30/2017
ms.assetid: 7a3d8515-d756-4afe-a22d-07cbe2217ee3
author: BrucePerlerMS
ms.openlocfilehash: bd04e4ebdf5c58adaeea0ff0ca5993d7d9ce38f1
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70252177"
---
# \<audienceUris>
Especifica o conjunto de URIs que são identificadores aceitáveis da terceira parte confiável (RP). Os tokens não serão aceitos se eles não forem analisados por uma das URIs de audiência permitidas.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel>**](system-identitymodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<identityConfiguration>**](identityconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<securityTokenHandlers>**](securitytokenhandlers.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<securityTokenHandlerConfiguration>**](securitytokenhandlerconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<audienceUris>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<system.identityModel>  
  <identityConfiguration>  
    <securityTokenHandlers>  
      <securityTokenHandlerConfiguration>  
        <audienceUris mode=xs:string>  
          <add value=xs:string />  
          <clear />  
          <remove value=xs:string />  
        </audienceUris>  
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
|mode|Um <xref:System.IdentityModel.Selectors.AudienceUriMode> valor que especifica se a restrição de audiência deve ser aplicada a um token de entrada. Os valores possíveis são "Always", "Never" e "BearerKeyOnly". O padrão é "Always". Opcional.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|`<add value=xs:string>`|Adiciona o URI especificado pelo `value` atributo à coleção audienceUris. O atributo `value` é necessário. O URI diferencia maiúsculas de minúsculas.|  
|`<clear>`|Limpa a coleção audienceUris. Todos os identificadores são removidos da coleção.|  
|`<remove value=xs:string>`|Remove o URI especificado pelo `value` atributo da coleção audienceUris. O atributo `value` é necessário. O URI diferencia maiúsculas de minúsculas.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<securityTokenHandlerConfiguration>](securitytokenhandlerconfiguration.md)|Fornece a configuração para uma coleção de manipuladores de token de segurança.|  
  
## <a name="remarks"></a>Comentários  
 Por padrão, a coleção está vazia; Use `<add>` `<clear>` `<remove>` os elementos, e para modificar a coleção. <xref:System.IdentityModel.Tokens.SamlSecurityTokenHandler>e <xref:System.IdentityModel.Tokens.Saml2SecurityTokenHandler> os objetos usam os valores na coleção de URI de audiência para configurar quaisquer restrições de URI de audiência permitidas em <xref:System.IdentityModel.Tokens.SamlSecurityTokenRequirement> objetos.  
  
 O `<audienceUris>` elemento é representado pela <xref:System.IdentityModel.Configuration.AudienceUriElementCollection> classe. Um URI individual adicionado à coleção é representado pela <xref:System.IdentityModel.Configuration.AudienceUriElement> classe.  
  
> [!NOTE]
> O uso do `<audienceUris>` elemento como um elemento filho do elemento foi [\<identityConfiguration>](identityconfiguration.md) preterido, mas ainda tem suporte para compatibilidade com versões anteriores. As configurações no `<securityTokenHandlerConfiguration>` elemento substituem aquelas no `<identityConfiguration>` elemento.  
  
## <a name="example"></a>Exemplo  
 O XML a seguir mostra como configurar os URIs de público aceitáveis para um aplicativo. Este exemplo configura um único URI. Os tokens com escopo definido para esse URI serão aceitos, todos os outros serão rejeitados.  
  
```xml  
<audienceUris>  
  <add value="http://localhost:19851/"/>  
</audienceUris>  
```
