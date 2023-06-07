---
title: Segurança da mensagens com um cliente de certificado
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 99770573-c815-4428-a38c-e4335c8bd7ce
ms.openlocfilehash: 2b2717bc68da9f07cd38e10a5d75b2a7df9add45
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84602629"
---
# <a name="message-security-with-a-certificate-client"></a>Segurança da mensagens com um cliente de certificado
O cenário a seguir mostra um cliente Windows Communication Foundation (WCF) e um serviço protegido usando o modo de segurança da mensagem. O cliente e o serviço são autenticados com certificados. Para obter mais informações, consulte [segurança de aplicativo distribuído](distributed-application-security.md).

 ![Captura de tela que mostra um cliente com certificado.](./media/message-security-with-a-certificate-client/client-with-certificate.gif)  
  
 Para um aplicativo de exemplo, consulte [certificado de segurança da mensagem](../samples/message-security-certificate.md).  

|Característica|Descrição|  
|--------------------|-----------------|  
|Modo de segurança|Mensagem|  
|Interoperabilidade|Somente WCF|  
|Autenticação (servidor)|Usando o certificado de serviço|  
|Autenticação (cliente)|Usando o certificado do cliente|  
|Integridade|Sim|  
|Confidencialidade|Sim|  
|Transport|HTTP|  
|Associação|<xref:System.ServiceModel.WSHttpBinding>|  
  
## <a name="service"></a>Serviço  
 O código e a configuração a seguir devem ser executados de forma independente. Realize um dos seguintes procedimentos:  
  
- Crie um serviço autônomo usando o código sem configuração.  
  
- Crie um serviço usando a configuração fornecida, mas não defina nenhum ponto de extremidade.  
  
### <a name="code"></a>Código  
 O código a seguir mostra como criar um ponto de extremidade de serviço que usa segurança de mensagem para estabelecer um contexto seguro.  
  
 [!code-csharp[C_SecurityScenarios#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securityscenarios/cs/source.cs#10)]
 [!code-vb[C_SecurityScenarios#10](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securityscenarios/vb/source.vb#10)]  
  
### <a name="configuration"></a>Configuração  
 A configuração a seguir pode ser usada em vez do código.  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<configuration>  
  <system.serviceModel>  
    <behaviors>  
      <serviceBehaviors>  
        <behavior name="ServiceCredentialsBehavior">  
          <serviceCredentials>  
            <serviceCertificate findValue="Contoso.com"  
                                x509FindType="FindBySubjectName" />  
          </serviceCredentials>  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
    <services>  
      <service behaviorConfiguration="ServiceCredentialsBehavior"
               name="ServiceModel.Calculator">  
        <endpoint address="http://localhost/Calculator"
                  binding="wsHttpBinding"  
                  bindingConfiguration="MessageAndCertificateClient"
                  name="SecuredByClientCertificate"  
                  contract="ServiceModel.ICalculator" />  
      </service>  
    </services>  
    <bindings>  
      <wsHttpBinding>  
        <binding name="WSHttpBinding_ICalculator">  
          <security mode="Message">  
            <message clientCredentialType="Certificate" />  
          </security>  
        </binding>  
      </wsHttpBinding>  
    </bindings>  
    <client />  
  </system.serviceModel>  
</configuration>  
```  
  
## <a name="client"></a>Cliente  
 O código e a configuração a seguir devem ser executados de forma independente. Realize um dos seguintes procedimentos:  
  
- Crie um cliente autônomo usando o código (e o código do cliente).  
  
- Crie um cliente que não defina nenhum endereço de ponto de extremidade. Em vez disso, use o construtor do cliente que usa o nome da configuração como um argumento. Por exemplo:  
  
     [!code-csharp[C_SecurityScenarios#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securityscenarios/cs/source.cs#0)]
     [!code-vb[C_SecurityScenarios#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securityscenarios/vb/source.vb#0)]  
  
### <a name="code"></a>Código  
 O código a seguir cria o cliente. A associação é a segurança do modo de mensagem e o tipo de credencial do cliente é definido como `Certificate` .  
  
 [!code-csharp[C_SecurityScenarios#17](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securityscenarios/cs/source.cs#17)]
 [!code-vb[C_SecurityScenarios#17](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securityscenarios/vb/source.vb#17)]  
  
### <a name="configuration"></a>Configuração  
 A configuração a seguir especifica o certificado do cliente usando um comportamento de ponto de extremidade. Para obter mais informações sobre certificados, consulte [Working with Certificates](working-with-certificates.md) (Trabalhando com certificados). O código também usa um `identity` elemento <> para especificar um DNS (sistema de nomes de domínio) da identidade do servidor esperada. Para obter mais informações sobre identidade, consulte [identidade de serviço e autenticação](service-identity-and-authentication.md).  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<configuration>  
  <system.serviceModel>  
    <behaviors>  
      <endpointBehaviors>  
        <behavior name="endpointCredentialsBehavior">  
          <clientCredentials>  
            <clientCertificate findValue="Cohowinery.com"
               storeLocation="LocalMachine"  
              x509FindType="FindBySubjectName" />  
          </clientCredentials>  
        </behavior>  
      </endpointBehaviors>  
    </behaviors>  
    <bindings>  
      <wsHttpBinding>  
        <binding name="WSHttpBinding_ICalculator" >  
          <security mode="Message">  
            <message clientCredentialType="Certificate" />  
          </security>  
        </binding>  
      </wsHttpBinding>  
    </bindings>  
    <client>  
      <endpoint address="http://machineName/Calculator"
                behaviorConfiguration="endpointCredentialsBehavior"  
                binding="wsHttpBinding"  
                bindingConfiguration="WSHttpBinding_ICalculator"  
                contract="ICalculator"  
                name="WSHttpBinding_ICalculator">  
        <identity>  
          <dns value="Contoso.com" />  
        </identity>  
      </endpoint>  
    </client>  
  </system.serviceModel>  
</configuration>  
```  
  
## <a name="see-also"></a>Consulte também

- [Visão geral de segurança](security-overview.md)
- [Identidade e autenticação de serviço](service-identity-and-authentication.md)
- [Trabalhando com certificados](working-with-certificates.md)
- [Modelo de segurança para o Windows Server app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ee677202(v=azure.10))
