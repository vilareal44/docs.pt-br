---
title: <transport> de <wsHttpBinding>
ms.date: 03/30/2017
ms.assetid: 21e38acf-450a-4bda-82b6-de305e1f7cd8
ms.openlocfilehash: 1afeed62fcbf3b083d69a7cedb7eb80b81f5c17b
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73732742"
---
# <a name="transport-of-wshttpbinding"></a>\<transport> de \<wsHttpBinding>

Define as configurações de autenticação para o transporte HTTP.

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<wsHttpBinding>**](wshttpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<security>**](security-of-wshttpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<transport>**  

## <a name="syntax"></a>Sintaxe

```xml
<wsHttpBinding>
  <binding>
    <security mode="None|Transport|TransportWithMessageCredential|TransportCredentialOnly">
      <transport clientCredentialType="Basic|Certificate|Digest|None|Ntlm|Windows"
                 proxyCredentialType="Basic|Digest|None|Ntlm|Windows"
                 realm="string" />
        <extendedProtectionPolicy policyEnforcement="Never|WhenSupported|Always"
                                  protectionScenario="TransportSelected|TrustedProxy">
          <customServiceNames>
          </customServiceNames>
        </extendedProtectionPolicy>
      </transport>
    </security>
  </binding>
</wsHttpBinding>
```

## <a name="type"></a>Type

<xref:System.ServiceModel.HttpTransportSecurity>

## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai.

### <a name="attributes"></a>Atributos

|Atributo|Descrição|
|---------------|-----------------|
|`clientCredentialType`|Especifica a credencial usada para autenticar o cliente para o serviço. Esse atributo é do tipo <xref:System.ServiceModel.HttpClientCredentialType> .|
|`proxyCredentialType`|Especifica a credencial usada para autenticar o cliente para um proxy de domínio. Esse atributo é do tipo <xref:System.ServiceModel.HttpProxyCredentialType> .|
|`realm`|Uma cadeia de caracteres que especifica o realm de autenticação para Digest ou autenticação básica. O padrão é uma cadeia de caracteres vazia.<br /><br /> Um realm de autenticação especifica pelo menos o nome do host que executa a autenticação. Ele também pode especificar uma coleção de usuários que tem acesso. Um usuário pode consultar o realm de autenticação para determinar que um dos vários nomes de usuário e senhas possíveis podem ser usados.|
|`policyEnforcement`|Essa enumeração especifica quando o <xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy> deve ser imposto.<br /><br /> 1. nunca – a política nunca é imposta (a proteção estendida está desabilitada).<br />2. WhenSupported – a política será imposta somente se o cliente oferecer suporte à proteção estendida.<br />3. sempre – a política é sempre imposta. Os clientes que não dão suporte à proteção estendida não serão autenticados.|

## <a name="clientcredentialtype-attribute"></a>Atributo clientCredentialtype

|Valor|Descrição|
|-----------|-----------------|
|`None`|A segurança é desabilitada.|
|`Basic`|Usa a autenticação básica.|
|`Digest`|Usa a autenticação Digest.|
|`Ntlm`|Usa a autenticação NTLM como um fallback com um domínio do Windows.|
|`Windows`|Usa a autenticação integrada do Windows.|
|`Certificate`|Usa certificados X. 509 para autenticar o cliente.|

## <a name="proxycredentialtype-attribute"></a>Atributo proxyCredentialType

|Valor|Descrição|
|-----------|-----------------|
|`None`|A segurança é desabilitada.|
|`Basic`|Usa a autenticação básica.|
|`Digest`|Usa a autenticação Digest.|
|`Ntlm`|Usa NTLM como um fallback com um domínio do Windows.|
|`Windows`|Usa a autenticação integrada do Windows.|
|`Certificate`|Usa certificados X. 509 para autenticar o cliente.|

### <a name="child-elements"></a>Elementos filho

Nenhum.

### <a name="parent-elements"></a>Elementos pai

|Elemento|Descrição|
|-------------|-----------------|
|[\<security>](security-of-wshttpbinding.md)|Representa os recursos de segurança do [\<wsHttpBinding>](wshttpbinding.md) .|

## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.HttpTransportSecurity>
- <xref:System.ServiceModel.WSHttpSecurity.Transport%2A>
- <xref:System.ServiceModel.Configuration.WSHttpSecurityElement.Transport%2A>
- <xref:System.ServiceModel.Configuration.HttpTransportSecurityElement>
- [Protegendo serviços e clientes](../../../wcf/feature-details/securing-services-and-clients.md)
- [Associações](../../../wcf/bindings.md)
- [Configurando associações fornecidas pelo sistema](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [Usando associações para configurar serviços e clientes](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
