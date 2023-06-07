---
title: Como configurar serviços do WCF para interoperar com clientes WSE 3.0
ms.date: 03/30/2017
ms.assetid: 0f38c4a0-49a6-437c-bdde-ad1d138d3c4a
ms.openlocfilehash: 600b9c28d92f9e2b6e4d586b052cc5762d591521
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599055"
---
# <a name="how-to-configure-wcf-services-to-interoperate-with-wse-30-clients"></a>Como configurar serviços do WCF para interoperar com clientes WSE 3.0

Os serviços de Windows Communication Foundation (WCF) são compatíveis com nível de conexão com os serviços Web de aprimoramentos 3,0 para Microsoft .NET (WSE) quando os serviços WCF são configurados para usar a versão de agosto de 2004 da especificação WS-Addressing.

### <a name="to-enable-a-wcf-service-to-interoperate-with-wse-30-clients"></a>Para habilitar um serviço WCF para interoperar com clientes WSE 3,0

1. Defina uma associação personalizada para o serviço WCF.

    Para especificar que a versão de agosto de 2004 da especificação WS-Addressing seja usada para codificação de mensagens, uma associação personalizada deve ser criada.

    1. Adicione um filho [\<customBinding>](../../configure-apps/file-schema/wcf/custombinding.md) ao [\<bindings>](../../configure-apps/file-schema/wcf/bindings.md) do arquivo de configuração do serviço.

    2. Especifique um nome para a associação, adicionando um [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) ao [\<customBinding>](../../configure-apps/file-schema/wcf/custombinding.md) e definindo o `name` atributo.

    3. Especifique um modo de autenticação e a versão das especificações do WS-Security que são usadas para proteger mensagens que são compatíveis com o WSE 3,0, adicionando um filho [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) ao [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) .

        Para definir o modo de autenticação, defina o `authenticationMode` atributo do [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) . Um modo de autenticação é praticamente equivalente a uma declaração de segurança completa no WSE 3,0. A tabela a seguir mapeia modos de autenticação no WCF para declarações de segurança de uso imediato no WSE 3,0.

        |Modo de autenticação do WCF|Declaração de segurança do WSE 3,0 pronto para uso|
        |-----------------------------|----------------------------------------|
        |<xref:System.ServiceModel.Configuration.AuthenticationMode.AnonymousForCertificate>|`anonymousForCertificateSecurity`|
        |<xref:System.ServiceModel.Configuration.AuthenticationMode.Kerberos>|`kerberosSecurity`|
        |<xref:System.ServiceModel.Configuration.AuthenticationMode.MutualCertificate>|`mutualCertificate10Security`*|
        |<xref:System.ServiceModel.Configuration.AuthenticationMode.MutualCertificate>|`mutualCertificate11Security`*|
        |<xref:System.ServiceModel.Configuration.AuthenticationMode.UserNameOverTransport>|`usernameOverTransportSecurity`|
        |<xref:System.ServiceModel.Configuration.AuthenticationMode.UserNameForCertificate>|`usernameForCertificateSecurity`|

        \*Uma das principais diferenças entre a `mutualCertificate10Security` e as `mutualCertificate11Security` declarações de segurança fechadas é a versão da especificação WS-Security que o WSE usa para proteger as mensagens SOAP. Para `mutualCertificate10Security` , o WS-security 1,0 é usado, ao passo que WS-security 1,1 é usado para o `mutualCertificate11Security` . Para o WCF, a versão da especificação WS-Security é especificada no `messageSecurityVersion` atributo do [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) .

        Para definir a versão da especificação WS-Security usada para proteger mensagens SOAP, defina o `messageSecurityVersion` atributo do [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) . Para interoperar com o WSE 3,0, defina o valor do `messageSecurityVersion` atributo como <xref:System.ServiceModel.MessageSecurityVersion.WSSecurity11WSTrustFebruary2005WSSecureConversationFebruary2005WSSecurityPolicy11BasicSecurityProfile10%2A> .

    4. Especifique que a versão de agosto de 2004 da especificação WS-Addressing é usada pelo WCF adicionando um [\<textMessageEncoding>](../../configure-apps/file-schema/wcf/textmessageencoding.md) e definindo o `messageVersion` como seu valor para <xref:System.ServiceModel.Channels.MessageVersion.Soap11WSAddressingAugust2004%2A> .

        > [!NOTE]
        > Quando você estiver usando SOAP 1,2, defina o `messageVersion` atributo como <xref:System.ServiceModel.Channels.MessageVersion.Soap12WSAddressingAugust2004%2A> .

2. Especifique que o serviço usa a associação personalizada.

    1. Defina o `binding` atributo do [\<endpoint>](../../configure-apps/file-schema/wcf/endpoint-element.md) elemento como `customBinding` .

    2. Defina o `bindingConfiguration` atributo do [\<endpoint>](../../configure-apps/file-schema/wcf/endpoint-element.md) elemento para o valor especificado no `name` atributo do [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) para a associação personalizada.

## <a name="example"></a>Exemplo

O exemplo de código a seguir especifica que o `Service.HelloWorldService` usa uma associação personalizada para interoperar com clientes do WSE 3,0. A associação personalizada especifica que a versão de agosto de 2004 do WS-Addressing e o conjunto de especificações do WS-Security 1,1 são usados para codificar as mensagens trocadas. As mensagens são protegidas usando o <xref:System.ServiceModel.Configuration.AuthenticationMode.AnonymousForCertificate> modo de autenticação.

```xml
<configuration>
  <system.serviceModel>
    <services>
      <service
        behaviorConfiguration="ServiceBehavior"
        name="Service.HelloWorldService">
        <endpoint binding="customBinding" address=""
          bindingConfiguration="ServiceBinding"
          contract="Service.IHelloWorld"></endpoint>
      </service>
    </services>

    <bindings>
      <customBinding>
        <binding name="ServiceBinding">
          <security authenticationMode="AnonymousForCertificate"
                  messageProtectionOrder="SignBeforeEncrypt"
                  messageSecurityVersion="WSSecurity11WSTrustFebruary2005WSSecureConversationFebruary2005WSSecurityPolicy11BasicSecurityProfile10"
                  requireDerivedKeys="false">
          </security>
          <textMessageEncoding messageVersion ="Soap11WSAddressingAugust2004"></textMessageEncoding>
          <httpTransport/>
        </binding>
      </customBinding>
    </bindings>
    <behaviors>
      <behavior name="ServiceBehavior" returnUnknownExceptionsAsFaults="true">
        <serviceCredentials>
          <serviceCertificate findValue="CN=WCFQuickstartServer" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectDistinguishedName"/>
        </serviceCredentials>
      </behavior>
    </behaviors>
  </system.serviceModel>
</configuration>
```

## <a name="see-also"></a>Consulte também

- [Como personalizar uma associação fornecida pelo sistema](../extending/how-to-customize-a-system-provided-binding.md)
