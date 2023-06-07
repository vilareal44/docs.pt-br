---
title: <message> de <wsDualHttpBinding>
ms.date: 03/30/2017
ms.assetid: 75101744-eed8-4d61-91f4-5fc4473a21f2
ms.openlocfilehash: aef03634ed6156d3a7e052ccdbde35fdfda99cc3
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73736730"
---
# <a name="message-of-wsdualhttpbinding"></a>\<message> de \<wsDualHttpBinding>
Define a segurança em nível de mensagem para o [\<wsDualHttpBinding>](wsdualhttpbinding.md) .  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<wsDualHttpBinding>**](wsdualhttpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<security>**](security-of-wsdualhttpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<message>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<message clientCredentialType="None/Windows/UserName/Certificate/CardSpace"
         negotiateServiceCredential="Boolean"
         algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15" />
</message>
```  
  
## <a name="type"></a>Type  
 <xref:System.ServiceModel.MessageSecurityOverHttp>  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|algorithmSuite|Opcional. Define a criptografia de mensagem e os algoritmos de encapsulamento de chaves. Os algoritmos e os tamanhos de chave são determinados pela <xref:System.ServiceModel.Security.SecurityAlgorithmSuite> classe. Esses algoritmos são mapeados para aqueles especificados na especificação do WS-SecurityPolicy (Security Policy Language).<br /><br /> Veja abaixo os valores possíveis. O valor padrão é `Basic256`.|  
|clientCredentialType|Opcional. Especifica o tipo de credencial a ser usado ao executar a autenticação do cliente usando o modo de segurança `Message` . Veja abaixo os valores possíveis. O padrão é `Windows`.<br /><br /> Esse atributo é do tipo <xref:System.ServiceModel.MessageCredentialType> .|  
|negotiateServiceCredential|Opcional. Um valor booliano que especifica se a credencial de serviço é provisionada no cliente fora da banda ou se é obtida do serviço para o cliente por meio de um processo de negociação. Essa negociação é um precurso para a troca de mensagens usual.<br /><br /> Se o `clientCredentialType` atributo for igual a None, username ou Certificate, definir esse atributo como `false` implica que o certificado de serviço está disponível no cliente fora de banda e que o cliente precisa especificar o certificado de serviço (usando o [\<serviceCertificate>](servicecertificate-of-servicecredentials.md) ) no [\<serviceCredentials>](servicecredentials.md) comportamento do serviço. Esse modo é interoperável com pilhas SOAP que implementam WS-Trust e WS-SecureConversation.<br /><br /> Se o `ClientCredentialType` atributo for definido como `Windows` , definir esse atributo para `false` especifica a autenticação baseada em Kerberos. Isso significa que o cliente e o serviço devem fazer parte do mesmo domínio Kerberos. Esse modo é interoperável com pilhas SOAP que implementam o perfil de token Kerberos (conforme definido no OASIS WSS TC), bem como WS-Trust e WS-SecureConversation. Quando esse atributo é `true` , ele faz uma negociação SOAP .NET que túneis SPNEGO trocas por mensagens SOAP.<br /><br /> O padrão é `true`.|  
  
## <a name="algorithmsuite-attribute"></a>Atributo algorithmSuite  
  
|Valor|Descrição|  
|-----------|-----------------|  
|Basic128|Use criptografia Aes128, SHA1 para síntese de mensagem e rsa-oaep-mgf1p para encapsulamento de chave.|  
|Basic192|Use a criptografia Aes192, SHA1 para Message Digest, rsa-oaep-mgf1p para quebra automática de chave.|  
|Basic256|Use a criptografia Aes256, SHA1 para Message Digest, rsa-oaep-mgf1p para quebra automática de chave.|  
|Basic256Rsa15|Use Aes256 para criptografia de mensagem, SHA1 para Message Digest e Rsa15 para Key Wrap.|  
|Basic192Rsa15|Use Aes192 para criptografia de mensagem, SHA1 para Message Digest e Rsa15 para Key Wrap.|  
|TripleDes|Use a criptografia TripleDes, SHA1 para Message Digest, rsa-oaep-mgf1p para quebra automática de chave.|  
|Basic128Rsa15|Use Aes128 para criptografia de mensagem, SHA1 para Message Digest e Rsa15 para Key Wrap.|  
|TripleDesRsa15|Use a criptografia TripleDes, SHA1 para síntese de mensagem e Rsa15 para o encapsulamento de chave.|  
|Basic128Sha256|Use Aes256 para criptografia de mensagem, SHA256 para síntese de mensagem e rsa-oaep-mgf1p para quebra automática de chave.|  
|Basic192Sha256|Use Aes192 para criptografia de mensagem, SHA256 para síntese de mensagem e rsa-oaep-mgf1p para encapsulamento de chave.|  
|Basic256Sha256|Use Aes256 para criptografia de mensagem, SHA256 para síntese de mensagem e rsa-oaep-mgf1p para quebra automática de chave.|  
|TripleDesSha256|Use TripleDes para criptografia de mensagem, SHA256 para síntese de mensagem e rsa-oaep-mgf1p para encapsulamento de chave.|  
|Basic128Sha256Rsa15|Use Aes128 para criptografia de mensagem, SHA256 para síntese de mensagem e Rsa15 para quebra de chave.|  
|Basic192Sha256Rsa15|Use Aes192 para criptografia de mensagem, SHA256 para síntese de mensagem e Rsa15 para quebra de chave.|  
|Basic256Sha256Rsa15|Use Aes256 para criptografia de mensagem, SHA256 para síntese de mensagem e Rsa15 para quebra automática de chave.|  
|TripleDesSha256Rsa15|Use TripleDes para criptografia de mensagem, SHA256 para síntese de mensagem e Rsa15 para quebra de chave.|  
  
## <a name="clientcredentialtype-attribute"></a>Atributo clientCredentialtype  
  
|Valor|Descrição|  
|-----------|-----------------|  
|Nenhum|Isso permite que o serviço interaja com clientes anônimos. No lado do serviço, isso indica que o serviço não requer nenhuma credencial do cliente. No cliente, isso indica que o cliente não fornece nenhuma credencial de cliente.|  
|Windows|Permite que as trocas SOAP estejam sob o contexto autenticado de uma credencial do Windows. Se o `negotiateServiceCredential` atributo for definido como `true` , isso executará uma negociação SSPI ou o Kerberos (um padrão interoperável).|  
|UserName|Permite que o serviço exija que o cliente seja autenticado usando uma credencial de nome de usuário. O WCF não dá suporte ao envio de um resumo de senha ou à derivação de chaves usando a senha e usando essas chaves para segurança de mensagem. Dessa forma, o WCF impõe que o transporte seja protegido ao usar credenciais de nome de usuário. Esse modo de credencial resulta em um intercâmbio interoperável ou em uma negociação não interoperável com base no `negotiateServiceCredential` atributo.|  
|Certificado|Permite que o serviço exija que o cliente seja autenticado usando um certificado. Se o modo de segurança da mensagem for usado e o `negotiateServiceCredential` atributo for definido como `false` , o cliente precisará ser provisionado com o certificado de serviço.|  
|IssuedToken|Especifica um token personalizado, geralmente emitido por um serviço de token de segurança.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<security>](security-of-wsdualhttpbinding.md)|Define os recursos de segurança do [\<wsDualHttpBinding>](wsdualhttpbinding.md) .|  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.WSDualHttpSecurityElement.Message%2A>
- <xref:System.ServiceModel.WSDualHttpSecurity.Message%2A>
- <xref:System.ServiceModel.Configuration.MessageSecurityOverTcpElement>
- <xref:System.ServiceModel.MessageSecurityOverHttp>
- [Protegendo serviços e clientes](../../../wcf/feature-details/securing-services-and-clients.md)
- [Associações](../../../wcf/bindings.md)
- [Configurando associações fornecidas pelo sistema](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [Usando associações para configurar serviços e clientes](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
