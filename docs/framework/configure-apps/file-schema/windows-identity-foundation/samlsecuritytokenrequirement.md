---
title: <samlSecurityTokenRequirement>
ms.date: 03/30/2017
ms.assetid: 09202d12-88d3-49cc-953b-703bcc1690eb
author: BrucePerlerMS
ms.openlocfilehash: b27f337189a7d0b66ffd38e032b5eb864e5094a1
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79152626"
---
# \<samlSecurityTokenRequirement>
Fornece a configuração para a <xref:System.IdentityModel.Tokens.SamlSecurityTokenHandler> classe, a <xref:System.IdentityModel.Tokens.Saml2SecurityTokenHandler> classe ou uma classe derivada de qualquer uma dessas classes. Representado pela <xref:System.IdentityModel.Tokens.SamlSecurityTokenRequirement> classe.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel>**](system-identitymodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<identityConfiguration>**](identityconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<securityTokenHandlers>**](securitytokenhandlers.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<add>**](add.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<samlSecurityTokenRequirement>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<system.identityModel>  
  <identityConfiguration>  
    <securityTokenHandlers>  
      <add>  
        <samlSecurityTokenRequirement
            issuerCertificateValidationMode="None||ChainTrust||PeerTrust||PeerOrChainTrust||Custom"  
            issuerCertificateRevocationMode="NoCheck||Offline||Online"  
            issuerCertificateTrustedStoreLocation="CurrentLocation||LocalMachine"  
            issuerCertificateValidator="Namespace.Class Assembly"  
            mapToWindows=xs:boolean  
          <nameClaimType value=xs:string />  
          <roleClaimType value=xs:string />  
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
|mapToWindows|Especifica se o manipulador de token deve mapear o token de validação para uma conta do Windows usando a declaração UPN de entrada. O padrão é "falso".|  
|issuerCertificateRevocationMode|Um <xref:System.Security.Cryptography.X509Certificates.X509RevocationMode> valor que especifica o modo de revogação a ser usado para o certificado X. 509. O valor padrão é "online".|  
|issuerCertificateValidationMode|Um <xref:System.ServiceModel.Security.X509CertificateValidationMode> valor que especifica o modo de validação a ser usado para o certificado X. 509. O valor padrão é "PeerOrChainTrust".|  
|issuerCertificateTrustedStoreLocation|Um <xref:System.Security.Cryptography.X509Certificates.StoreLocation> valor que especifica o repositório de certificados X. 509. O valor padrão é "LocalMachine".|  
|issuerCertificateValidator|Um tipo personalizado que deriva de <xref:System.IdentityModel.Selectors.X509CertificateValidator> . Se o `issuerCertificateValidationMode` atributo for "Custom", uma instância desse tipo será usada para validação do certificado do emissor.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<nameClaimType>](nameclaimtype.md)|Define o tipo de declaração que especifica a <xref:System.Security.Principal.IIdentity.Name%2A> propriedade.|  
|[\<roleClaimType>](roleclaimtype.md)|Especifica o tipo de declaração que define as declarações de tipo de função na coleção de <xref:System.Security.Claims.ClaimsIdentity> objetos retornada pelo <xref:System.IdentityModel.Tokens.SecurityTokenHandler.ValidateToken%2A> método do manipulador de token.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<add>](add.md)|Adiciona o manipulador de token de segurança especificado à coleção de manipulador de token.|  
  
## <a name="remarks"></a>Comentários  
 O `<samlSecurityTokenRequirement>` elemento é representado pela <xref:System.IdentityModel.Tokens.SamlSecurityTokenRequirement> classe no modelo de objeto e é usado para configurar a `SamlSecurityTokenRequirement` propriedade em um <xref:System.IdentityModel.Tokens.SamlSecurityTokenHandler> ou um <xref:System.IdentityModel.Tokens.Saml2SecurityTokenHandler> .  
  
## <a name="example"></a>Exemplo  
  
```xml  
<add type="System.IdentityModel.Tokens.SamlSecurityTokenHandler, System.IdentityModel">  
    <samlSecurityTokenRequirement issuerCertificateValidationMode="PeerOrChainTrust"  
                                  issuerCertificateRevocationMode="Online"  
                                  issuerCertificateTrustedStoreLocation="LocalMachine"  
                                  mapToWindows="false">  
  
        <nameClaimType value="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name" />  
        <roleClaimType value="schemas.microsoft.com/ws/2006/04/identity/claims/role" />  
    </samlSecurityTokenRequirement>  
</add>  
```
