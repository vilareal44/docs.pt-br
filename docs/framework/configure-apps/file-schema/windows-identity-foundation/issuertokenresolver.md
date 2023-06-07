---
title: <issuerTokenResolver>
ms.date: 03/30/2017
ms.assetid: f74392f6-3f5b-4880-bd8a-3a9130d31e65
author: BrucePerlerMS
ms.openlocfilehash: 67d7e0aa5b6b05bfe8b17a1b1efebb1fbddbb0eb
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79152665"
---
# \<issuerTokenResolver>
Registra o resolvedor de token do emissor que é usado por manipuladores na coleção de manipulador de token. O resolvedor de token do emissor é usado para resolver o token de assinatura em mensagens e tokens de entrada.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel>**](system-identitymodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<identityConfiguration>**](identityconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<securityTokenHandlers>**](securitytokenhandlers.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<securityTokenHandlerConfiguration>**](securitytokenhandlerconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<issuerTokenResolver>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<system.identityModel>  
  <identityConfiguration>  
    <securityTokenHandlers>  
      <securityTokenHandlerConfiguration>  
        <issuerTokenResolver type=xs:string>  
        </issuerTokenResolver>  
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
|type|Especifica o tipo do resolvedor de token emissor. Deve ser a <xref:System.IdentityModel.Tokens.IssuerTokenResolver> classe ou um tipo derivado da <xref:System.IdentityModel.Tokens.IssuerTokenResolver> classe. Obrigatórios.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<securityTokenHandlerConfiguration>](securitytokenhandlerconfiguration.md)|Fornece a configuração para uma coleção de manipuladores de token de segurança.|  
  
## <a name="remarks"></a>Comentários  
 O resolvedor de token do emissor é usado para resolver o token de assinatura em mensagens e tokens de entrada. Ele é usado para recuperar o material criptográfico que é usado para verificar a assinatura. Você deve especificar o `type` atributo. O tipo especificado pode ser <xref:System.IdentityModel.Tokens.IssuerTokenResolver> ou um tipo personalizado que deriva da <xref:System.IdentityModel.Tokens.IssuerTokenResolver> classe.  
  
 Alguns manipuladores de token permitem que você especifique as configurações do resolvedor de token do emissor na configuração. As configurações em manipuladores de token individuais substituem aquelas especificadas na coleção do manipulador de token de segurança.  
  
> [!NOTE]
> A especificação do `<issuerTokenResolver>` elemento como um elemento filho do [\<identityConfiguration>](identityconfiguration.md) elemento foi preterida, mas ainda tem suporte para compatibilidade com versões anteriores. As configurações no `<securityTokenHandlerConfiguration>` elemento substituem aquelas no `<identityConfiguration>` elemento.  
  
## <a name="example"></a>Exemplo  
 O XML a seguir mostra a configuração para um resolvedor de token de emissor baseado em uma classe personalizada que deriva de <xref:System.IdentityModel.Tokens.IssuerTokenResolver> . O resolvedor de token mantém um dicionário de pares de chave de público-alvo que é inicializado a partir de um elemento de configuração personalizado ( `<AddAudienceKeyPair>` ) definido para a classe. A classe substitui o <xref:System.IdentityModel.Selectors.SecurityTokenResolver.LoadCustomConfiguration%2A> método para processar esse elemento. A substituição é mostrada no exemplo a seguir; no entanto, os métodos que ele chama não são mostrados para fins de brevidade. Para obter o exemplo completo, consulte o `CustomToken` exemplo.  
  
```xml  
<issuerTokenResolver type="SimpleWebToken.CustomIssuerTokenResolver, SimpleWebToken">  
  <AddAudienceKeyPair  symmetricKey="wAVkldQiFypTQ+kdNdGWCYCHRcee8XmXxOvgmak8vSY=" audience="http://localhost:19851/" />  
</issuerTokenResolver>  
```  
  
## <a name="example"></a>Exemplo
  
```csharp
public override void LoadCustomConfiguration(System.Xml.XmlNodeList nodelist)  
{  
    foreach (XmlNode node in nodelist)  
    {  
        XmlDictionaryReader rdr = XmlDictionaryReader.CreateDictionaryReader(new XmlTextReader(new StringReader(node.OuterXml)));  
        rdr.MoveToContent();  
  
        string symmetricKey = rdr.GetAttribute("symmetricKey");  
        string audience = rdr.GetAttribute("audience");  
  
        this.AddAudienceKeyPair(audience, symmetricKey);  
    }  
}  
```
  
## <a name="see-also"></a>Confira também

- <xref:System.IdentityModel.Tokens.IssuerTokenResolver>
