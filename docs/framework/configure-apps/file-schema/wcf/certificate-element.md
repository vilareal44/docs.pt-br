---
title: Elemento <certificate>
ms.date: 03/30/2017
ms.assetid: 9b3d9233-ef35-477a-bf5d-efd1e80a52f4
ms.openlocfilehash: e28e7d16073a56f3b6126439644bfff86c9af18b
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70400549"
---
# <a name="certificate-element"></a>Elemento \<certificate>
Especifica um certificado X. 509 a ser usado para assinar e criptografar mensagens para clientes ponto a ponto.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<endpointBehaviors>**](endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<clientCredentials>**](clientcredentials.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<peer>**](peer-of-clientcredentials-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<certificate>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<certificate findValue="String"
             storeLocation="LocalMachine/CurrentUser"
             storeName="AddressBook/AuthRoot/CertificateAuthority/Disallowed/My/Root/TrustedPeople/TrustedPublisher"
             X509FindType="FindByThumbPrint/FindBySubjectName/FindBySubjectDistinguishedName/FindByIssuerName/FindByIssuerDistinguishedName/FindBySerialNumber/FindByTimeValid/FindByTimeNotYetValid/FindByTemplateName/FindByApplicationPolicy/FindByCertificatePolicy/FindByExtension/FindByKeyUsage/FindBySubjectKeyIdentifier" />
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`findValue`|Uma cadeia de caracteres que contém o valor a ser procurado no repositório de certificados X. 509. O tipo contido no atributo deve atender aos requisitos do especificado `x509FindType` . O padrão é uma cadeia de caracteres vazia.|  
|`storeLocation`|Especifica o local do repositório de certificados X. 509 que o cliente usa para validar o certificado do par. Os valores válidos incluem os seguintes:<br /><br /> -LocalMachine: o repositório de certificados atribuído ao computador local.<br />-CurrentUser: o repositório de certificados atribuído ao usuário atual.<br /><br /> O padrão é LocalMachine.|  
|`storeName`|Especifica o nome do repositório de certificados X.509 a ser aberto. Os valores válidos incluem os seguintes:<br /><br /> -Catálogo: repositório de certificados para outros usuários.<br />-AuthRoot: repositório de certificados para CAs (autoridades de certificação) de terceiros.<br />-CertificateAuthority: repositório de certificados para CAs (autoridades de certificação) intermediárias.<br />-Não permitido: repositório de certificados para certificados revogados.<br />-My: repositório de certificados para certificados pessoais.<br />-Root: repositório de certificados para autoridades de certificação raiz confiáveis (CAs).<br />-TrustedPeople: repositório de certificados para pessoas e recursos diretamente confiáveis.<br />-TrustedPublisher: repositório de certificados para Publicadores diretamente confiáveis.<br /><br /> O padrão é My.|  
|`X509FindType`|Define o tipo de pesquisa de X.509 a ser executada. Os valores válidos incluem os seguintes:<br /><br /> -FindByThumbPrint<br />-FindBySubjectName<br />- FindBySubjectDistinguishedName<br />- FindByIssuerName<br />- FindByIssuerDistinguishedName<br />- FindBySerialNumber<br />- FindByTimeValid<br />- FindByTimeNotYetValid<br />- FindByTemplateName<br />- FindByApplicationPolicy<br />- FindByCertificatePolicy<br />- FindByExtension<br />- FindByKeyUsage<br />- FindBySubjectKeyIdentifier<br /><br /> O tipo contido no `findValue` atributo deve atender aos requisitos do especificado `X509FindType` .<br /><br /> O valor padrão é FindBySubjectDistinguishedName.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<peer>](peer-of-clientcredentials-element.md)|Especifica as credenciais usadas ao autenticar clientes ponto a ponto.|  
  
## <a name="remarks"></a>Comentários  
 Este elemento de configuração contém uma instância X509Certificate2 usada ao autenticar vizinhos na malha par.  
  
 Para obter mais informações sobre a programação ponto a ponto, consulte [rede ponto a ponto](../../../wcf/feature-details/peer-to-peer-networking.md).  
  
## <a name="example"></a>Exemplo  
 O código a seguir especifica como localizar o certificado usado em um cenário ponto a ponto.  
  
```xml  
<behaviors>
  <endpointBehaviors>
    <behavior name="MyEndpointBehavior">
      <clientCredentials>
        <peer>
          <certificate findValue="www.contoso.com"
                       storeLocation="LocalMachine"
                       x509FindType="FindByIssuerName" />
        </peer>
      </clientCredentials>
    </behavior>
  </endpointBehaviors>
</behaviors>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.PeerCredentialElement>
- <xref:System.ServiceModel.Configuration.PeerCredentialElement.Certificate%2A>
- <xref:System.ServiceModel.Configuration.X509PeerCertificateElement>
- <xref:System.ServiceModel.Security.PeerCredential.Certificate%2A>
- [Trabalhando com certificados](../../../wcf/feature-details/working-with-certificates.md)
- [Rede ponto a ponto](../../../wcf/feature-details/peer-to-peer-networking.md)
- [Autenticação de mensagem de canal par](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/aa967730(v=vs.90))
- [Autenticação personalizada do canal par](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751447(v=vs.90))
- [Protegendo aplicativos de canal par](../../../wcf/feature-details/securing-peer-channel-applications.md)
