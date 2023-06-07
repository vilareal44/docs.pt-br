---
title: <serviceCertificate> de <serviceCredentials>
ms.date: 03/30/2017
ms.assetid: 597ae6d5-4938-4950-9f5e-b2280e816182
ms.openlocfilehash: 513dcad7f4325d653df87fe9cc27572c25e153c5
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70399667"
---
# <a name="servicecertificate-of-servicecredentials"></a>\<serviceCertificate> de \<serviceCredentials>
Especifique um certificado X. 509 que será usado para autenticar o serviço para clientes usando o modo de segurança da mensagem.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceCredentials>**](servicecredentials.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<serviceCertificate>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<serviceCertificate findValue="String"
                    storeLocation="LocalMachine/CurrentUser"
                    storeName="AddressBook/AuthRoot/CertificateAuthority/Disallowed/My/Root/TrustedPeople/TrustedPublisher"
                    x509FindType="FindByThumbprint/FindBySubjectName/FindBySubjectDistinguishedName/FindByIssuerName/FindByIssuerDistinguishedName/FindBySerialNumber/FindByTimeValid/FindByTimeNotYetValid/FindByTemplateName/FindByApplicationPolicy/FindByCertificatePolicy/FindByExtension/FindByKeyUsage/FindBySubjectKeyIdentifier" />
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`findValue`|Uma cadeia de caracteres que contém o valor a ser procurado no repositório de certificados X. 509. O tipo contido no atributo deve atender aos requisitos do X509FindType especificado. O padrão é uma cadeia de caracteres vazia.|  
|`storeLocation`|Especifica o local do repositório de certificados X. 509 que o cliente usa para validar o certificado do servidor. Os valores válidos incluem os seguintes:<br /><br /> -LocalMachine: o repositório de certificados atribuído ao computador local.<br />-CurrentUser: o repositório de certificados atribuído ao usuário atual.<br /><br /> O padrão é LocalMachine.|  
|`storeName`|Especifica o nome do repositório de certificados X.509 a ser aberto. Os valores válidos incluem os seguintes:<br /><br /> -Catálogo: repositório de certificados para outros usuários.<br />-AuthRoot: repositório de certificados para CAs (autoridades de certificação) de terceiros.<br />-CertificatAuthority: repositório de certificados para CAs (autoridades de certificação) intermediárias.<br />-Não permitido: repositório de certificados para certificados revogados.<br />-My: repositório de certificados para certificados pessoais.<br />-Root: repositório de certificados para autoridades de certificação raiz confiáveis (CAs).<br />-TrustedPeople: repositório de certificados para pessoas e recursos diretamente confiáveis.<br />-TrustedPublisher: repositório de certificados para editores diretamente confiáveis.<br /><br /> O padrão é My.|  
|`x509FindType`|Define o tipo de pesquisa de X.509 a ser executada. Os valores válidos incluem os seguintes:<br /><br /> -FindByThumbprint<br />-FindBySubjectName<br />- FindBySubjectDistinguishedName<br />- FindByIssuerName<br />- FindByIssuerDistinguishedName<br />- FindBySerialNumber<br />- FindByTimeValid<br />- FindByTimeNotYetValid<br />- FindByTemplateName<br />- FindByApplicationPolicy<br />- FindByCertificatePolicy<br />- FindByExtension<br />- FindByKeyUsage<br />- FindBySubjectKeyIdentifier<br /><br /> O tipo contido no `findValue` atributo deve atender aos requisitos do X509FindType especificado.<br /><br /> O valor padrão é FindBySubjectDistinguishedName.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<serviceCredentials>](servicecredentials.md)|Especifica a credencial a ser usada na autenticação do serviço e as configurações relacionadas à validação de credenciais do cliente.|  
  
## <a name="remarks"></a>Comentários  
 Use este elemento para especificar um certificado X. 509 que será usado para autenticar o serviço para clientes usando o modo de segurança da mensagem. Se você estiver usando um certificado que será renovado periodicamente, sua impressão digital será alterada. Nesse caso, use o nome da entidade como o `x509FindType` porque o certificado pode ser reemitido com o mesmo nome de assunto.  
  
 Para obter mais informações sobre como usar o elemento, consulte [como especificar valores de credencial do cliente](../../../wcf/how-to-specify-client-credential-values.md).  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.X509RecipientCertificateServiceElement>
- <xref:System.ServiceModel.Configuration.ServiceCredentialsElement.ServiceCertificate%2A>
- <xref:System.ServiceModel.Security.X509CertificateRecipientServiceCredential>
- <xref:System.ServiceModel.Description.ServiceCredentials.ServiceCertificate%2A>
- [Como especificar valores de credenciais do cliente](../../../wcf/how-to-specify-client-credential-values.md)
- [Comportamentos de segurança](../../../wcf/feature-details/security-behaviors-in-wcf.md)
