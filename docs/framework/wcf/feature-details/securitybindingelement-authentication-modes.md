---
title: SecurityBindingElement Authentication Modes
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 12300bf4-c730-4405-9f65-d286f68b5a43
ms.openlocfilehash: 163645c16097b5371369618f2bd5f333feb75747
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84595136"
---
# <a name="securitybindingelement-authentication-modes"></a>SecurityBindingElement Authentication Modes
O Windows Communication Foundation (WCF) fornece vários modos pelos quais os clientes e serviços se autenticam entre si. Você pode criar elementos de associação de segurança para esses modos de autenticação usando métodos estáticos na <xref:System.ServiceModel.Channels.SecurityBindingElement> classe ou por meio da configuração. Este tópico descreve brevemente os 18 modos de autenticação.  
  
 Para obter um exemplo de como usar o elemento para um dos modos de autenticação, consulte [como: criar um SecurityBindingElement para um modo de autenticação especificado](how-to-create-a-securitybindingelement-for-a-specified-authentication-mode.md).  
  
## <a name="basic-configuration-programming"></a>Programação de configuração básica  
 O procedimento a seguir descreve como definir o modo de autenticação em um arquivo de configuração.  
  
#### <a name="to-set-the-authentication-mode-in-configuration"></a>Para definir o modo de autenticação na configuração  
  
1. Para o [\<bindings>](../../configure-apps/file-schema/wcf/bindings.md) elemento, adicione um [\<customBinding>](../../configure-apps/file-schema/wcf/custombinding.md) .  
  
2. Como um elemento filho, adicione um [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) elemento ao `<customBinding>` elemento.  
  
3. Adicione um `<security>` elemento ao `<binding>` elemento.  
  
4. Defina o `authenticationMode` atributo como um dos valores descritos abaixo. Por exemplo, o código a seguir define o modo como `AnonymousForCertificate` .  
  
    ```xml  
    <bindings>  
      <customBinding>  
        <binding name="SecureCustomBinding">  
         <security authenticationMode ="AnonymousForCertificate" />  
        </binding>  
      </customBinding>  
    </bindings>  
    ```  
  
#### <a name="to-set-the-mode-programmatically"></a>Para definir o modo programaticamente  
  
1. Determine o tipo de retorno, que pode ser um dos seguintes: <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> , <xref:System.ServiceModel.Channels.TransportSecurityBindingElement> , <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement> ou <xref:System.ServiceModel.Channels.SecurityBindingElement> .  
  
2. Chame o método estático apropriado da <xref:System.ServiceModel.Channels.SecurityBindingElement> classe. Por exemplo, o código a seguir chama o <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateAnonymousForCertificateBindingElement%2A> método.  
  
     [!code-csharp[c_CustomBindingsAuthMode#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_custombindingsauthmode/cs/source.cs#3)]
     [!code-vb[c_CustomBindingsAuthMode#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_custombindingsauthmode/vb/source.vb#3)]  
  
3. Use o elemento de associação para criar a associação personalizada. Para obter mais informações, consulte [associações personalizadas](../extending/custom-bindings.md).  
  
## <a name="mode-descriptions"></a>Descrições de modo  
  
### <a name="anonymousforcertificate"></a>AnonymousForCertificate  
 Com esse modo de autenticação, o cliente é anônimo e o serviço é autenticado usando um certificado X. 509. O elemento de associação de segurança é <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> retornado pelo <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateAnonymousForCertificateBindingElement%2A> método. Como alternativa, defina o `authenticationMode` atributo do elemento <`security`> como `AnonymousForCertificate` .  
  
### <a name="anonymousforsslnegotiated"></a>AnonymousForSslNegotiated  
 Com esse modo de autenticação, o cliente é anônimo e o serviço é autenticado usando um certificado X. 509 que é negociado em tempo de execução. O elemento de associação de segurança é <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> retornado pelo <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateSslNegotiationBindingElement%2A> método quando um valor de `false` é passado para o primeiro parâmetro. Como alternativa, defina o `authenticationMode` atributo como `AnonymousForSslNegotiated` .  
  
### <a name="certificateovertransport"></a>CertificateOverTransport  
 Com esse modo de autenticação, o cliente é autenticado usando um certificado X. 509 que aparece na camada SOAP como um token de suporte de endosso; ou seja, um token que assina a assinatura da mensagem. O serviço é autenticado usando um certificado X. 509 na camada de transporte. O elemento de associação de segurança é <xref:System.ServiceModel.Channels.TransportSecurityBindingElement> retornado pelo <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateCertificateOverTransportBindingElement%2A> método. Como alternativa, defina o `authenticationMode` atributo como `CertificateOverTransport` .  
  
### <a name="issuedtoken"></a>IssuedToken  
 Com esse modo de autenticação, o cliente não se autentica no serviço, como tal; em vez disso, o cliente é autenticado em um serviço de token de segurança e recebe um token SAML, que ele apresenta ao servidor para provar seu conhecimento de uma chave compartilhada. O serviço não é autenticado para o cliente, como tal, mas o serviço de token de segurança criptografa a chave compartilhada como parte do token emitido para que apenas o serviço possa descriptografar a chave. O elemento de associação de segurança é <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> retornado pelo <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateIssuedTokenBindingElement%2A> método. Como alternativa, defina o `authenticationMode` atributo como `IssuedToken` .  
  
### <a name="issuedtokenforcertificate"></a>IssuedTokenForCertificate  
 Com esse modo de autenticação, o cliente não se autentica no serviço, como tal; em vez disso, o cliente é autenticado em um serviço de token de segurança e recebe um token SAML, que ele apresenta ao servidor para provar seu conhecimento de uma chave compartilhada. O token emitido aparece na camada SOAP como um token de suporte de endosso ou um token de portador; ou seja, um token que assina a assinatura da mensagem. O serviço é autenticado no cliente usando um certificado X. 509. O elemento de associação de segurança é <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> retornado pelo <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateIssuedTokenForCertificateBindingElement%2A> método. Como alternativa, defina o `authenticationMode` atributo como `IssuedTokenForCertificate` .  
  
### <a name="issuedtokenforsslnegotiated"></a>IssuedTokenForSslNegotiated  
 Com esse modo de autenticação, o cliente não se autentica no serviço, como tal; em vez disso, o cliente é autenticado em um serviço de token de segurança e recebe um token SAML, que ele apresenta ao servidor para provar seu conhecimento de uma chave compartilhada. O token emitido aparece na camada SOAP como um token de suporte de endosso ou um token de portador; ou seja, um token que assina a assinatura da mensagem. O serviço é autenticado usando um certificado X. 509. O elemento de associação de segurança é <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> retornado pelo <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateIssuedTokenForSslBindingElement%2A> método. Como alternativa, defina o `authenticationMode` atributo como `IssuedTokenForSslnegotiated` .  
  
### <a name="issuedtokenovertransport"></a>IssuedTokenOverTransport  
 Com esse modo de autenticação, o cliente não se autentica no serviço, como tal; em vez disso, o cliente é autenticado em um serviço de token de segurança e recebe um token SAML, que ele apresenta ao servidor para provar seu conhecimento de uma chave compartilhada. O token emitido aparece na camada SOAP como um token de suporte de endosso ou um token de portador; ou seja, um token que assina a assinatura da mensagem. O serviço é autenticado usando um certificado X. 509 na camada de transporte. O elemento de associação de segurança é `TransportSecurityBindingElement` retornado pelo <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateIssuedTokenOverTransportBindingElement%2A> método. Como alternativa, defina o `authenticationMode` atributo como `IssuedTokenOverTransport` .  
  
### <a name="kerberos"></a>Kerberos  
 Com esse modo de autenticação, o cliente é autenticado no serviço usando um tíquete Kerberos. Esse mesmo tíquete também fornece autenticação de servidor. O elemento de associação de segurança é `SymmetricSecurityBindingElement` retornado pelo <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateKerberosBindingElement%2A> método. Como alternativa, defina o `authenticationMode` atributo como `Kerberos` .  
  
> [!NOTE]
> Para usar esse modo de autenticação, a conta de serviço deve ser associada a um SPN (nome da entidade de serviço). Para fazer isso, execute o serviço na conta de serviço de rede ou na conta sistema LOCAL. Como alternativa, use a ferramenta SetSpn. exe para criar um SPN para a conta de serviço. Em ambos os casos, o cliente deve usar o SPN correto no [\<servicePrincipalName>](../../configure-apps/file-schema/wcf/serviceprincipalname.md) elemento ou usando o <xref:System.ServiceModel.EndpointAddress> Construtor. Para obter mais informações, consulte [identidade de serviço e autenticação](service-identity-and-authentication.md).  
  
> [!NOTE]
> Quando o `Kerberos` modo de autenticação é usado, <xref:System.Security.Principal.TokenImpersonationLevel.Anonymous> os <xref:System.Security.Principal.TokenImpersonationLevel.Delegation> níveis de representação e não têm suporte.  
  
### <a name="kerberosovertransport"></a>KerberosOverTransport  
 Com esse modo de autenticação, o cliente é autenticado no serviço usando um tíquete Kerberos. O token Kerberos é exibido na camada SOAP como um token de suporte de endosso; ou seja, um token que assina a assinatura da mensagem. O serviço é autenticado usando um certificado X. 509 na camada de transporte. O elemento de associação de segurança é `TransportSecurityBindingElement` retornado pelo <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateKerberosOverTransportBindingElement%2A> método. Como alternativa, defina o `authenticationMode` atributo como `KerberosOverTransport` .  
  
> [!NOTE]
> Para usar esse modo de autenticação, a conta de serviço deve ser associada a um SPN. Para fazer isso, execute o serviço na conta de serviço de rede ou na conta sistema LOCAL. Como alternativa, use a ferramenta SetSpn. exe para criar um SPN para a conta de serviço. Em ambos os casos, o cliente deve usar o SPN correto no [\<servicePrincipalName>](../../configure-apps/file-schema/wcf/serviceprincipalname.md) elemento ou usando o <xref:System.ServiceModel.EndpointAddress> Construtor. Para obter mais informações, consulte [identidade de serviço e autenticação](service-identity-and-authentication.md).  
  
### <a name="mutualcertificate"></a>MutualCertificate  
 Com esse modo de autenticação, o cliente é autenticado usando um certificado X. 509 que aparece na camada SOAP como um token de suporte de endosso; ou seja, um token que assina a assinatura da mensagem. O serviço também é autenticado usando um certificado X. 509. O elemento de associação de segurança é `SymmetricSecurityBindingElement` retornado pelo <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateMutualCertificateBindingElement%2A> método. Como alternativa, defina o `authenticationMode` atributo como `MutualCertificate` .  
  
### <a name="mutualcertificateduplex"></a>MutualCertificateDuplex  
 Com esse modo de autenticação, o cliente é autenticado usando um certificado X. 509 que aparece na camada SOAP como um token de suporte de endosso; ou seja, um token que assina a assinatura da mensagem. O serviço também é autenticado usando um certificado X. 509. A associação é `AsymmetricSecurityBindingElement` retornada pelo <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateMutualCertificateDuplexBindingElement%2A> método. Como alternativa, defina o `authenticationMode` atributo como `MutualCertificateDuplex` .  
  
### <a name="mutualsslnegotiated"></a>MutualSslNegotiated  
 Com esse modo de autenticação, o cliente e o serviço se autenticam usando certificados X. 509. O elemento de associação de segurança é `SymmetricSecurityBindingElement` retornado pelo <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateSslNegotiationBindingElement%2A> método quando um valor de `true` é passado para o primeiro parâmetro. Como alternativa, defina o `authenticationMode` atributo como `MutualSslNegotiated` .  
  
### <a name="secureconversation"></a>SecureConversation  
 O elemento de associação de segurança é `SymmetricSecurityBindingElement` retornado pelo <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateSecureConversationBindingElement%2A> método. Esse método usa um <xref:System.ServiceModel.Channels.SecurityBindingElement> como parâmetro, que é usado durante a inicialização para estabelecer a sessão segura. Como alternativa, defina o `authenticationMode` atributo como `SecureConversation` .  
  
 Se nenhuma associação de inicialização for especificada, o `SspiNegotiated` modo de autenticação será usado para inicialização.  
  
### <a name="sspinegotiation"></a>SspiNegotiation  
 Com esse modo de autenticação, um protocolo de negociação é usado para executar a autenticação de cliente e servidor. O Kerberos é usado, se possível; caso contrário, o NT LanMan (NTLM) será usado. O elemento de associação de segurança é `SymmetricSecurityBindingElement` retornado pelo <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateSspiNegotiationBindingElement%2A> método. Como alternativa, defina o `authenticationMode` atributo como `SspiNegotiated` .  
  
### <a name="sspinegotiatedovertransport"></a>SspiNegotiatedOverTransport  
 Com esse modo de autenticação, um protocolo de negociação é usado para executar a autenticação de cliente e servidor. O protocolo Kerberos é usado, se possível; caso contrário, o NTLM será usado. O token resultante aparece na camada SOAP como um token de suporte de endosso; ou seja, um token que assina a assinatura da mensagem. O serviço também é autenticado na camada de transporte por um certificado X. 509. O elemento de associação de segurança é `TransportSecurityBindingElement` retornado pelo <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateSspiNegotiationOverTransportBindingElement%2A> método. Como alternativa, defina o `authenticationMode` atributo como `SspiNegotiatedOverTransport` .  
  
### <a name="usernameforcertificate"></a>UserNameForCertificate  
 Com esse modo de autenticação, o cliente é autenticado no serviço usando um token de nome de usuário que aparece na camada SOAP como um token de suporte assinado; ou seja, um token assinado pela assinatura da mensagem. O serviço é autenticado no cliente usando um certificado X. 509. O elemento de associação de segurança é `SymmetricSecurityBindingElement` retornado pelo <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateUserNameForCertificateBindingElement%2A> método. Como alternativa, defina o `authenticationMode` atributo como `UserNameForCertificate` .  
  
 Para o `UserNameForCertificate` modo de autenticação, o cliente e o serviço devem usar o WS-Security 1,1.  
  
### <a name="usernameforsslnegotiated"></a>UserNameForSslNegotiated  
 Com esse modo de autenticação, o cliente é autenticado usando um token de nome de usuário que aparece na camada SOAP como um token de suporte assinado; ou seja, um token assinado pela assinatura da mensagem. O serviço é autenticado usando um certificado X. 509. O elemento de associação de segurança é `SymmetricSecurityBindingElement` retornado pelo <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateUserNameForSslBindingElement%2A> método. Como alternativa, defina o `authenticationMode` atributo como `UserNameForSslNegotiated` .  
  
### <a name="usernameovertransport"></a>UserNameOverTransport  
 Com esse modo de autenticação, o cliente é autenticado usando um token de nome de usuário que aparece na camada SOAP como um token de suporte assinado; ou seja, um token assinado pela assinatura da mensagem. O serviço é autenticado usando um certificado X. 509 na camada de transporte. O elemento de associação de segurança é `TransportSecurityBindingElement` retornado pelo <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateUserNameOverTransportBindingElement%2A> método. Como alternativa, defina o `authenticationMode` atributo como `UserNameOverTransport` .  
  
## <a name="see-also"></a>Consulte também

- <xref:System.ServiceModel.Channels.SecurityBindingElement>
- [Como criar um SecurityBindingElement para um modo de autenticação especificado](how-to-create-a-securitybindingelement-for-a-specified-authentication-mode.md)
