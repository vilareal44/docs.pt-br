---
title: Como criar uma associação duplex federada
ms.date: 03/30/2017
ms.assetid: 4331d2bc-5455-492a-9189-634a82597726
ms.openlocfilehash: e93651ce9fe9dae55c299fcb061da6bdc4b6bc5e
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84598964"
---
# <a name="how-to-create-a-duplex-federated-binding"></a>Como criar uma associação duplex federada

<xref:System.ServiceModel.WSFederationHttpBinding>o só dá suporte aos contratos de troca de datagrama e solicitação/resposta. Para usar o contrato de troca de mensagens duplex, você deve criar uma associação personalizada. Os procedimentos a seguir mostram como fazer isso na configuração, usando a segurança do modo de mensagem para os transportes HTTP e TCP e usando a segurança de modo misto para o transporte TCP. O código de exemplo mostrando todas as 3 associações está no final deste tópico.

Você também pode criar a associação no código. Para obter uma descrição da pilha de elementos de associação a ser criada, consulte [como: criar uma associação personalizada usando o SecurityBindingElement](how-to-create-a-custom-binding-using-the-securitybindingelement.md).

## <a name="to-create-a-duplex-federated-custom-binding-with-http"></a>Para criar uma associação personalizada de duplex federado com HTTP

1. No [\<bindings>](../../configure-apps/file-schema/wcf/bindings.md) nó do arquivo de configuração, crie um [\<customBinding>](../../configure-apps/file-schema/wcf/custombinding.md) elemento.

2. Dentro do [\<customBinding>](../../configure-apps/file-schema/wcf/custombinding.md) elemento, crie um [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) elemento com o `name` atributo definido como `FederationDuplexHttpMessageSecurityBinding` .

3. Dentro do [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) elemento, crie um [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) elemento com o `authenticationMode` atributo definido como `SecureConversation` .

4. Dentro do [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) elemento, crie um [\<secureConversationBootstrap>](../../configure-apps/file-schema/wcf/secureconversationbootstrap.md) elemento com o `authenticationMode` atributo definido como `IssuedTokenForCertificate` ou `IssuedTokenForSslNegotiated` .

5. Seguindo o [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) elemento, crie um [\<compositeDuplex>](../../configure-apps/file-schema/wcf/compositeduplex.md) elemento vazio.

6. Seguindo o [\<compositeDuplex>](../../configure-apps/file-schema/wcf/compositeduplex.md) elemento, crie um [\<oneWay>](../../configure-apps/file-schema/wcf/oneway.md) elemento vazio.

7. Seguindo o [\<oneWay>](../../configure-apps/file-schema/wcf/oneway.md) elemento, crie um [\<httpTransport>](../../configure-apps/file-schema/wcf/httptransport.md) elemento vazio.

## <a name="to-create-a-duplex-federated-custom-binding-with-tcp-message-security-mode"></a>Para criar uma associação personalizada de duplex federado com o modo de segurança de mensagem TCP

1. No [\<bindings>](../../configure-apps/file-schema/wcf/bindings.md) nó do arquivo de configuração, crie um [\<customBinding>](../../configure-apps/file-schema/wcf/custombinding.md) elemento.

2. Dentro do [\<customBinding>](../../configure-apps/file-schema/wcf/custombinding.md) elemento, crie um [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) elemento com o `name` atributo definido como `FederationDuplexTcpMessageSecurityBinding` .

3. Dentro do [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) elemento, crie um [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) elemento com o `authenticationMode` atributo definido como `SecureConversation` .

4. Dentro do [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) elemento, crie um [\<secureConversationBootstrap>](../../configure-apps/file-schema/wcf/secureconversationbootstrap.md) elemento com o `authenticationMode` atributo definido como `IssuedTokenForCertificate` ou `IssuedTokenForSslNegotiated` .

5. Seguindo o [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) elemento, crie um [\<tcpTransport>](../../configure-apps/file-schema/wcf/tcptransport.md) elemento vazio.

## <a name="to-create-a-duplex-federated-custom-binding-with-tcp-mixed-security-mode"></a>Para criar uma associação personalizada de duplex federado com o modo de segurança mista TCP

1. No [\<bindings>](../../configure-apps/file-schema/wcf/bindings.md) nó do arquivo de configuração, crie um [\<customBinding>](../../configure-apps/file-schema/wcf/custombinding.md) elemento.

2. Dentro do [\<customBinding>](../../configure-apps/file-schema/wcf/custombinding.md) elemento, crie um [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) elemento com o `name` atributo definido como `FederationDuplexTcpTransportSecurityWithMessageCredentialBinding` .

3. Dentro do [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) elemento, crie um [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) elemento com o `authenticationMode` atributo definido como `SecureConversation` .

4. Dentro do [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) elemento, crie um [\<secureConversationBootstrap>](../../configure-apps/file-schema/wcf/secureconversationbootstrap.md) elemento com o `authenticationMode` atributo definido como `IssuedTokenForCertificate` ou `IssuedTokenForSslNegotiated` .

5. Seguindo o [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) elemento, crie um [\<sslStreamSecurity>](../../configure-apps/file-schema/wcf/sslstreamsecurity.md) elemento vazio.

6. Seguindo o [\<sslStreamSecurity>](../../configure-apps/file-schema/wcf/sslstreamsecurity.md) elemento, crie um [\<tcpTransport>](../../configure-apps/file-schema/wcf/tcptransport.md) elemento vazio.

## <a name="code-sample"></a>Exemplo de código

### <a name="sample-with-3-bindings"></a>Exemplo com 3 associações

1. Insira o código a seguir no arquivo de configuração.

## <a name="example"></a>Exemplo

```xml
<bindings>
   <customBinding>
      <binding name="FederationDuplexHttpMessageSecurityBinding">
<!-- duplex contract requires secure conversation with require cancellation = true -->
          <security authenticationMode="SecureConversation">
              <secureConversationBootstrap authenticationMode="IssuedTokenForSslNegotiated" />
          </security>
          <compositeDuplex />
          <oneWay />
          <httpTransport />
       </binding>
<!-- duplex over https is not supported -->
       <binding name="FederationDuplexTcpMessageSecurityBinding">
<!-- duplex contract requires secure conversation with require cancellation = true -->
          <security authenticationMode="SecureConversation">
              <secureConversationBootstrap authenticationMode="IssuedTokenForSslNegotiated" />
          </security>
          <tcpTransport />
       </binding>
       <binding name="FederationDuplexTcpTransportSecurityWithMessageCredentialsBinding">
<!-- duplex contract requires secure conversation with require cancellation = true -->
          <security authenticationMode="SecureConversation">
              <secureConversationBootstrap authenticationMode="IssuedTokenOverTransport" />
          </security>
<!-- requireClientCertificate = true or <windowsStreamSecurity /> can be used, but does not make sense for most scenarios -->
          <sslStreamSecurity />
          <tcpTransport />
       </binding>
    </customBinding>
</bindings>
```
