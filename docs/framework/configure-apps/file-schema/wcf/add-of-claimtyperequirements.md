---
title: <add> de <claimTypeRequirements>
ms.date: 03/30/2017
ms.assetid: c68e83c9-39e8-4264-b1ce-b6a9eb5b98aa
ms.openlocfilehash: 6ba935f7f6dae0e4d9e6581f53a50c684efcbed3
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79153081"
---
# <a name="add-of-claimtyperequirements"></a>\<add> de \<claimTypeRequirements>
Especifica os tipos de declarações obrigatórias e opcionais esperados para aparecer na credencial federada. Por exemplo, os serviços definem os requisitos de credenciais de entrada, que devem ter um determinado conjunto de tipos de declaração.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<customBinding>**](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<security>**](security-of-custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<issuedTokenParameters>**](issuedtokenparameters.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<claimTypeRequirements>**](claimtyperequirements-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<add>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<claimTypeRequirements>
  <add claimType="URI"
       isOptional="Boolean" />
</claimTypeRequirements>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|claimType|Um URI que define o tipo de uma declaração. Por exemplo, para comprar um produto de um site da Web, o usuário deve apresentar um cartão de crédito válido com limite de crédito suficiente. O tipo de declaração seria o URI do cartão de crédito.|  
|isOptional|Um valor booliano que especifica se isso é para uma declaração opcional. Defina esse atributo como `false` se for uma declaração necessária.<br /><br /> Você pode usar esse atributo quando o serviço solicitar algumas informações, mas não precisar dela. Por exemplo, se você precisar que o usuário insira seu nome, sobrenome e endereço, mas decida que o número de telefone é opcional.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<claimTypeRequirements>](claimtyperequirements-element.md)|Especifica uma coleção de tipos de declaração necessários.<br /><br /> Em um cenário federado, os serviços atendem aos requisitos de credenciais de entrada. Por exemplo, as credenciais de entrada devem ter um determinado conjunto de tipos de declaração. Cada elemento nessa coleção especifica os tipos de declarações obrigatórias e opcionais que devem aparecer em uma credencial federada.|  
  
## <a name="remarks"></a>Comentários  
 Em um cenário federado, os serviços atendem aos requisitos de credenciais de entrada. Por exemplo, as credenciais de entrada devem ter um determinado conjunto de tipos de declaração. Esse requisito é manifestado em uma política de segurança. Quando um cliente solicita credenciais de um serviço federado (por exemplo, CardSpace), ele coloca os requisitos em uma solicitação de token (RequestSecurityToken) para que o serviço federado possa emitir as credenciais que atendem aos requisitos de acordo.  
  
## <a name="example"></a>Exemplo  
 A configuração a seguir adiciona dois requisitos de tipo de declaração a uma associação de segurança.  
  
```xml  
<bindings>
  <wsFederationHttpBinding>
    <binding name="myFederatedBinding">
      <security mode="Message">
        <message issuedTokenType="urn:oasis:names:tc:SAML:1.0:assertion">
          <claimTypeRequirements>
            <add claimType="http://schemas.microsoft.com/ws/2005/05/identity/claims/EmailAddress" />
            <add claimType="http://schemas.microsoft.com/ws/2005/05/identity/claims/UserName"
                 optional="true" />
          </claimTypeRequirements>
        </message>
      </security>
    </binding>
  </wsFederationHttpBinding>
</bindings>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Security.Tokens.ClaimTypeRequirement>
- <xref:System.ServiceModel.Security.Tokens.IssuedSecurityTokenParameters.ClaimTypeRequirements%2A>
- <xref:System.ServiceModel.Configuration.IssuedTokenParametersElement.ClaimTypeRequirements%2A>
- <xref:System.ServiceModel.Configuration.ClaimTypeElementCollection>
- <xref:System.ServiceModel.Configuration.ClaimTypeElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [\<claimTypeRequirements>](claimtyperequirements-element.md)
- [Associações](../../../wcf/bindings.md)
- [Estendendo associações](../../../wcf/extending/extending-bindings.md)
- [Associações personalizadas](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
- [Como criar uma associação personalizada utilizando o SecurityBindingElement](../../../wcf/feature-details/how-to-create-a-custom-binding-using-the-securitybindingelement.md)
- [Segurança de associação personalizada](../../../wcf/samples/custom-binding-security.md)
