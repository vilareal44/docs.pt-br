---
title: Como configurar credenciais em um serviço de federação
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF, federation
- federation
ms.assetid: 149ab165-0ef3-490a-83a9-4322a07bd98a
ms.openlocfilehash: 05f35bbb7dbb34cd4067c407578038cbb4eff70f
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599133"
---
# <a name="how-to-configure-credentials-on-a-federation-service"></a>Como configurar credenciais em um serviço de federação
No Windows Communication Foundation (WCF), a criação de um serviço federado consiste nos seguintes procedimentos principais:  
  
1. Configurando uma <xref:System.ServiceModel.WSFederationHttpBinding> associação personalizada ou semelhante. Para obter mais informações sobre como criar uma associação apropriada, consulte [como: criar um WSFederationHttpBinding](how-to-create-a-wsfederationhttpbinding.md).  
  
2. Configurar o <xref:System.ServiceModel.Security.IssuedTokenServiceCredential> que controla como os tokens emitidos apresentados ao serviço são autenticados.  
  
 Este tópico fornece detalhes sobre a segunda etapa. Para obter mais informações sobre como um serviço federado funciona, consulte [Federação](federation.md).  
  
### <a name="to-set-the-properties-of-issuedtokenservicecredential-in-code"></a>Para definir as propriedades de IssuedTokenServiceCredential no código  
  
1. Use a <xref:System.ServiceModel.Description.ServiceCredentials.IssuedTokenAuthentication%2A> propriedade da <xref:System.ServiceModel.Description.ServiceCredentials> classe para retornar uma referência a uma <xref:System.ServiceModel.Security.IssuedTokenServiceCredential> instância. A propriedade é acessada da <xref:System.ServiceModel.ServiceHostBase.Credentials%2A> propriedade da <xref:System.ServiceModel.ServiceHostBase> classe.  
  
2. Defina a <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.AllowUntrustedRsaIssuers%2A> propriedade como `true` se os tokens emitidos por conta própria, como os cartões do CardSpace, forem autenticados. O padrão é `false`.  
  
3. Preencha a coleção retornada pela <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.KnownCertificates%2A> propriedade com instâncias da <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> classe. Cada instância representa um emissor do qual o serviço irá autenticar tokens.  
  
    > [!NOTE]
    > Ao contrário da coleção do lado do cliente retornada pela <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential.ScopedCertificates%2A> propriedade, a coleção de certificados conhecida não é uma coleção com chave. O serviço aceita os tokens que os certificados especificados emitem independentemente do endereço do cliente que enviou a mensagem que contém o token emitido (sujeito às restrições adicionais, que são descritas posteriormente neste tópico).  
  
4. Defina a <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.CertificateValidationMode%2A> propriedade como um dos <xref:System.ServiceModel.Security.X509CertificateValidationMode> valores de enumeração. Isso pode ser feito apenas no código. O padrão é <xref:System.IdentityModel.Selectors.X509CertificateValidator.ChainTrust%2A>.  
  
5. Se a <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.CertificateValidationMode%2A> propriedade for definida como <xref:System.ServiceModel.Security.X509CertificateValidationMode.Custom> , atribua uma instância da <xref:System.IdentityModel.Selectors.X509CertificateValidator> classe personalizada à <xref:System.ServiceModel.Security.X509ServiceCertificateAuthentication.CustomCertificateValidator%2A> propriedade.  
  
6. Se o <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.CertificateValidationMode%2A> for definido como `ChainTrust` ou `PeerOrChainTrust` , defina a <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.RevocationMode%2A> propriedade como um valor apropriado da <xref:System.Security.Cryptography.X509Certificates.X509RevocationMode> enumeração. Observe que o modo de revogação não é usado `PeerTrust` nos `Custom` modos de validação ou.  
  
7. Se necessário, atribua uma instância de uma <xref:System.IdentityModel.Tokens.SamlSerializer> classe personalizada à <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.SamlSerializer%2A> propriedade. Um serializador SAML (Security Asserties Markup Language) personalizado é necessário, por exemplo, para analisar declarações SAML personalizadas.  
  
### <a name="to-set-the-properties-of-issuedtokenservicecredential-in-configuration"></a>Para definir as propriedades de IssuedTokenServiceCredential na configuração  
  
1. Crie um `<issuedTokenAuthentication>` elemento como um filho de um `serviceCredentials` elemento <>.  
  
2. Defina o `allowUntrustedRsaIssuers` atributo do `<issuedTokenAuthentication>` elemento como `true` se estiver autenticando um token emitido por conta própria, como um cartão do CardSpace.  
  
3. Crie um `<knownCertificates>` elemento como um filho do `<issuedTokenAuthentication>` elemento.  
  
4. Crie zero ou mais `<add>` elementos como filhos do `<knownCertificates>` elemento e especifique como localizar o certificado usando os `storeLocation` atributos,, e `storeName` `x509FindType` `findValue` .  
  
5. Se necessário, defina o `samlSerializer` atributo do elemento <`issuedTokenAuthentication`> como o nome do tipo da classe personalizada <xref:System.IdentityModel.Tokens.SamlSerializer> .  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir define as propriedades de um <xref:System.ServiceModel.Security.IssuedTokenServiceCredential> no código.  
  
 [!code-csharp[C_FederatedService#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_federatedservice/cs/source.cs#2)]
 [!code-vb[C_FederatedService#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_federatedservice/vb/source.vb#2)]  
  
 Para que um serviço federado autentique um cliente, o seguinte deve ser verdadeiro sobre o token emitido:  
  
- Quando a assinatura digital do token emitido usa um identificador de chave de segurança RSA, a <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.AllowUntrustedRsaIssuers%2A> propriedade deve ser `true` .  
  
- Quando a assinatura do token emitido usa um número de série do emissor X. 509, X. 509 identificador de chave de entidade ou o identificador de segurança de impressão digital X. 509, o token emitido deve ser assinado por um certificado na coleção retornada pela <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.KnownCertificates%2A> propriedade da <xref:System.ServiceModel.Security.IssuedTokenServiceCredential> classe.  
  
- Quando o token emitido é assinado usando um certificado X. 509, o certificado deve validar de acordo com a semântica especificada pelo valor da <xref:System.ServiceModel.Security.X509ServiceCertificateAuthentication.CertificateValidationMode%2A> propriedade, independentemente de o certificado ter sido enviado à terceira parte confiável como uma <xref:System.IdentityModel.Tokens.X509RawDataKeyIdentifierClause> ou foi obtido da <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.KnownCertificates%2A> propriedade. Para obter mais informações sobre a validação de certificado X. 509, consulte [trabalhando com certificados](working-with-certificates.md).  
  
 Por exemplo, a configuração de <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.CertificateValidationMode%2A> para <xref:System.ServiceModel.Security.X509CertificateValidationMode.PeerTrust> autenticará qualquer token emitido cujo certificado de autenticação esteja no `TrustedPeople` repositório de certificados. Nesse caso, defina a <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.TrustedStoreLocation%2A> propriedade como <xref:System.Security.Cryptography.X509Certificates.StoreLocation.CurrentUser> ou <xref:System.Security.Cryptography.X509Certificates.StoreLocation.LocalMachine> . Você pode selecionar outros modos, incluindo <xref:System.ServiceModel.Security.X509CertificateValidationMode.Custom> . Quando `Custom` é selecionado, você deve atribuir uma instância da <xref:System.IdentityModel.Selectors.X509CertificateValidator> classe à <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.CustomCertificateValidator%2A> propriedade. O validador personalizado pode validar certificados usando qualquer critério que ele goste. Para obter mais informações, consulte [como: criar um serviço que emprega um validador de certificado personalizado](../extending/how-to-create-a-service-that-employs-a-custom-certificate-validator.md).  
  
## <a name="see-also"></a>Consulte também

- [Federação](federation.md)
- [Federação e confiabilidade](federation-and-trust.md)
- [Exemplo de federação](../samples/federation-sample.md)
- [Como desabilitar sessões seguranças em uma WSFederationHttpBinding](how-to-disable-secure-sessions-on-a-wsfederationhttpbinding.md)
- [Como criar um WSFederationHttpBinding](how-to-create-a-wsfederationhttpbinding.md)
- [Como criar um cliente federado](how-to-create-a-federated-client.md)
- [Trabalhando com certificados](working-with-certificates.md)
- [SecurityBindingElement Authentication Modes](securitybindingelement-authentication-modes.md)
