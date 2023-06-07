---
title: <customCookieHandler>
ms.date: 03/30/2017
ms.assetid: a03b153d-5ec6-4915-9031-6f0c3fd348be
author: BrucePerlerMS
ms.openlocfilehash: e1f32e17cf0da5e948d778e8b61aca6053eff4ef
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70252012"
---
# \<customCookieHandler>
Define o tipo de manipulador de cookies personalizado. Esse elemento só poderá estar presente se o `mode` atributo do `<cookieHandler>` elemento for "Custom". O tipo personalizado deve ser derivado da <xref:System.IdentityModel.Services.CookieHandler> classe.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel.services>**](system-identitymodel-services.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<federationConfiguration>**](federationconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<cookieHandler>**](cookiehandler.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<customCookieHandler>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<system.identityModel.services>  
  <federationConfiguration>  
    <cookieHandler mode="Custom">  
      <customCookieHandler type="MyNamespace.MyCustomCookieHandler, MyAssembly" >  
      </customCookieHandler>  
    </cookieHandler>  
  </federationConfiguration>  
</system.identityModel.services>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|type|Especifica um tipo personalizado que deriva da <xref:System.IdentityModel.Services.CookieHandler> classe. Para obter mais informações sobre como especificar o `type` atributo, consulte [referências de tipo personalizado](../windows-workflow-foundation/index.md).|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<cookieHandler>](cookiehandler.md)|Configura o <xref:System.IdentityModel.Services.CookieHandler> que o <xref:System.IdentityModel.Services.SessionAuthenticationModule> usa para ler e gravar cookies.|  
  
## <a name="remarks"></a>Comentários  
 Quando você especifica um manipulador de cookie personalizado definindo o `mode` atributo do `<cookieHandler>` elemento como "Custom", você deve especificar o tipo do manipulador de cookie personalizado, incluindo um `<customCookieHandler>` elemento filho que faz referência ao tipo de manipulador de cookie. Este elemento não pode ser especificado quando o `mode` atributo está definido como "em bloco" ou "padrão". Os manipuladores de cookie personalizados devem derivar da <xref:System.IdentityModel.Services.CookieHandler> classe.  
  
 O `<customCookieHandler>` elemento é representado pela <xref:System.IdentityModel.Configuration.CustomTypeElement> classe.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir configura o SAM para usar um manipulador de cookie personalizado do tipo `MyNamespace.MyCustomCookieHandler` .  
  
```xml  
<cookieHandler mode="Custom">  
    <customCookieHandler type="MyNamespace.MyCustomCookieHandler, MyAssembly" />  
</cookieHandler>  
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.IdentityModel.Services.CookieHandler>
