---
title: <peerAuthentication>
ms.date: 03/30/2017
ms.assetid: ad545e6f-f06e-4549-ac92-09d758d5c636
ms.openlocfilehash: 118159617a7f4c27ecc5e8fe077c28cfefac8537
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70397613"
---
# \<peerAuthentication>
Especifica as configurações de autenticação para um certificado par usado por um nó par.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceCredentials>**](servicecredentials.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<peer>**](peer-of-servicecredentials.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<peerAuthentication>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<peerAuthentication customCertificateValidatorType="namespace.typeName, [,AssemblyName] [,Version=version number] [,Culture=culture] [,PublicKeyToken=token]"
                    certificateValidationMode="ChainTrust/None/PeerTrust/PeerOrChainTrust/Custom"
                    revocationMode="NoCheck/Online/Offline"
                    trustedStoreLocation="CurrentUser/LocalMachine" />
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`certificateValidationMode`|Enumeração opcional. Especifica um dos três modos usados para validar as credenciais. Esse atributo é do tipo <xref:System.ServiceModel.Security.X509CertificateValidationMode> . Se definido como `Custom` , um `customCertificateValidator` também deverá ser fornecido.|  
|`customCertificateValidatorType`|Cadeia de caracteres opcional. Especifica um tipo e um assembly usados para validar um tipo personalizado. Esse atributo deve ser definido quando `certificateValidationMode` é definido como `Custom` . Esse atributo é do tipo <xref:System.IdentityModel.Selectors.X509CertificateValidator> . O Windows Communication Foundation (WCF) fornece um validador de certificado par padrão que verifica o certificado de mesmo nível no repositório de pessoas confiáveis. Ele também verifica se o certificado se encadeia em uma raiz válida. Você pode implementar um validador personalizado para especificar um comportamento diferente e usar esse atributo para apontar para o validador personalizado.|  
|`revocationMode`|Enumeração opcional. Especifica o modo de revogação de certificado. Esse atributo é do tipo <xref:System.Security.Cryptography.X509Certificates.X509RevocationMode> . O sistema verifica se o certificado de mesmo nível não foi revogado ao procurá-lo na lista de certificados revogados. Essa verificação pode ser executada marcando-se online ou em uma lista de revogação armazenada em cache. A verificação de revogação pode ser desativada definindo esse atributo como NoCheck.|  
|`trustedStoreLocation`|Enumeração opcional. Especifica o local de repositório confiável em que o certificado par é validado pelo sistema de segurança do WCF. Esse atributo é do tipo <xref:System.Security.Cryptography.X509Certificates.StoreLocation> .|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<peer>](peer-of-servicecredentials.md)|Especifica as credenciais atuais para um nó par.|  
  
## <a name="remarks"></a>Comentários  
 O `<authentication>` elemento corresponde à <xref:System.ServiceModel.Security.X509PeerCertificateAuthentication> classe. Esse elemento especifica um validador, que é invocado durante a autenticação de vizinho para vizinho na malha. Quando um novo par tenta estabelecer uma conexão vizinha, ele passa sua própria credencial para o par de resposta. O validador do respondente é chamado para verificar a credencial da parte remota. Sempre que uma conexão de mesmo nível é estabelecida na malha, ambos os pares são mutuamente autenticados, o que significa que os validadores em ambas as extremidades são invocados.  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.PeerCredentialElement>
- <xref:System.ServiceModel.Security.X509PeerCertificateAuthentication>
- <xref:System.ServiceModel.Security.PeerCredential.PeerAuthentication%2A>
- <xref:System.ServiceModel.Configuration.PeerCredentialElement.PeerAuthentication%2A>
- <xref:System.ServiceModel.Configuration.X509PeerCertificateAuthenticationElement>
- [Trabalhando com certificados](../../../wcf/feature-details/working-with-certificates.md)
- [Rede ponto a ponto](../../../wcf/feature-details/peer-to-peer-networking.md)
- [Autenticação de mensagem de canal par](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/aa967730(v=vs.90))
- [Autenticação personalizada do canal par](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751447(v=vs.90))
- [Protegendo aplicativos de canal par](../../../wcf/feature-details/securing-peer-channel-applications.md)
