---
title: <issuerNameRegistry>
ms.date: 03/30/2017
ms.assetid: 58b39d12-c953-40c4-88af-d7eb3343ca28
author: BrucePerlerMS
ms.openlocfilehash: 209e702da80f2569f2b6c068f50f1af4489157f6
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70251960"
---
# \<issuerNameRegistry>
Configura o registro de nome do emissor que é usado pelos manipuladores na coleção de manipuladores de token.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel>**](system-identitymodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<identityConfiguration>**](identityconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<securityTokenHandlers>**](securitytokenhandlers.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<securityTokenHandlerConfiguration>**](securitytokenhandlerconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<issuerNameRegistry>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<system.identityModel>  
  <identityConfiguration>  
    <securityTokenHandlers>  
      <securityTokenHandlerConfiguration>  
        <issuerNameRegistry type=xs:string>  
          <optionalCustomConfigurationElements />  
        </issuerNameRegistry>  
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
|type|Um tipo que deriva da <xref:System.IdentityModel.Tokens.IssuerNameRegistry> classe. Para obter mais informações sobre como especificar um personalizado `type` , consulte [referências de tipo personalizado].|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<trustedIssuers>](trustedissuers.md)|Quando o `type` atributo especifica o registro de nome do emissor baseado em configuração (a <xref:System.IdentityModel.Tokens.ConfigurationBasedIssuerNameRegistry> classe), o [\<trustedIssuers>](trustedissuers.md) elemento deve ser especificado. O [\<trustedIssuers>](trustedissuers.md) elemento pode levar `<add>` , `<clear>` ou `<remove>` elementos como elementos filho.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<securityTokenHandlerConfiguration>](securitytokenhandlerconfiguration.md)|Fornece a configuração para uma coleção de manipuladores de token de segurança.|  
  
## <a name="remarks"></a>Comentários  
 Todos os tokens do emissor são validados usando um registro de nome do emissor. Esse é um objeto derivado da <xref:System.IdentityModel.Tokens.IssuerNameRegistry> classe. O registro de nome do emissor é usado para associar um nome mnemônico ao material criptográfico que é necessário para verificar as assinaturas de tokens produzidas pelo emissor correspondente. O registro de nome do emissor mantém uma lista de emissores que são confiáveis para o aplicativo RP (terceira parte confiável). O tipo do registro de nome do emissor é especificado usando o `type` atributo. O `<issuerNameRegistry>` elemento pode ter um ou mais elementos filho que fornecem a configuração para o tipo especificado. Você fornece a lógica que processa esses elementos filho substituindo o <xref:System.IdentityModel.Tokens.IssuerNameRegistry.LoadCustomConfiguration%2A> método.  
  
 O WIF fornece um tipo de registro de nome de emissor único pronto para uso, a <xref:System.IdentityModel.Tokens.ConfigurationBasedIssuerNameRegistry> classe. Essa classe usa um conjunto de certificados de emissor confiável que são especificados na configuração do. Ele requer um elemento de configuração filho, `<trustedIssuers>` , sob o qual a coleção de certificados de emissor confiável está configurada. Os certificados confiáveis são especificados usando a forma codificada ASN. 1 da impressão digital do certificado e são adicionados ou removidos da coleção `<add>` usando `<clear>` elementos, ou `<remove>` .  
  
 O `<issuerNameRegistry>` elemento é representado pela <xref:System.IdentityModel.Configuration.IssuerNameRegistryElement> classe.  
  
> [!NOTE]
> A especificação do `<issuerNameRegistry>` elemento como um elemento filho do [\<identityConfiguration>](identityconfiguration.md) elemento foi preterida, mas ainda tem suporte para compatibilidade com versões anteriores. As configurações no `<securityTokenHandlerConfiguration>` elemento substituem aquelas no `<identityConfiguration>` elemento.  
  
## <a name="example"></a>Exemplo  
 O XML a seguir mostra como especificar o registro de nome do emissor baseado em configuração.  
  
```xml  
<issuerNameRegistry type="System.IdentityModel.Tokens.ConfigurationBasedIssuerNameRegistry, System.IdentityModel, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089">  
  <trustedIssuers>  
    <add thumbprint="9B74CB … 1EF40D0" name="LocalSTS" />  
  </trustedIssuers>  
</issuerNameRegistry>  
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.IdentityModel.Tokens.IssuerNameRegistry>
- <xref:System.IdentityModel.Tokens.ConfigurationBasedIssuerNameRegistry>
