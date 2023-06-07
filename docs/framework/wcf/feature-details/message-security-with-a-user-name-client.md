---
title: Segurança de mensagem com um nome de usuário cliente
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 36335cb9-76b8-4443-92c7-44f081eabb21
ms.openlocfilehash: 9447487012cae370d35880e5b780465f9434051b
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84602616"
---
# <a name="message-security-with-a-user-name-client"></a>Segurança de mensagem com um nome de usuário cliente
A ilustração a seguir mostra um serviço de Windows Communication Foundation (WCF) e o cliente protegidos usando a segurança em nível de mensagem. O serviço é autenticado com um certificado X. 509. O cliente é autenticado usando um nome de usuário e senha.  
  
 Para um aplicativo de exemplo, consulte [segurança da mensagem nome de usuário](../samples/message-security-user-name.md).  
  
 ![Segurança de mensagem usando autenticação de nome de usuário](media/1fb10a61-7e1d-42f5-b1af-195bfee5b3c6.gif "1fb10a61-7e1d-42f5-b1af-195bfee5b3c6")  
  
|Característica|Descrição|  
|--------------------|-----------------|  
|Modo de segurança|Mensagem|  
|Interoperabilidade|Somente Windows Communication Foundation (WCF)|  
|Autenticação (servidor)|A negociação inicial requer autenticação do servidor|  
|Autenticação (cliente)|Nome de usuário/senha|  
|Integridade|Sim, usando o contexto de segurança compartilhado|  
|Confidencialidade|Sim, usando o contexto de segurança compartilhado|  
|Transport|HTTP|  
|Associação|<xref:System.ServiceModel.WSHttpBinding>|  
  
## <a name="service"></a>Serviço  
 O código e a configuração a seguir devem ser executados de forma independente. Realize um dos seguintes procedimentos:  
  
- Crie um serviço autônomo usando o código sem configuração.  
  
- Crie um serviço usando a configuração fornecida, mas não defina nenhum ponto de extremidade.  
  
### <a name="code"></a>Código  
 O código a seguir mostra como criar um ponto de extremidade de serviço que usa a segurança de mensagem.  
  
 [!code-csharp[C_SecurityScenarios#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securityscenarios/cs/source.cs#9)]
 [!code-vb[C_SecurityScenarios#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securityscenarios/vb/source.vb#9)]  
  
### <a name="configuration"></a>Configuração  
 A configuração a seguir pode ser usada em vez do código:  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<configuration>  
  <system.serviceModel>  
    <behaviors>  
      <serviceBehaviors>  
        <behavior name="ServiceCredentialsBehavior">  
          <serviceCredentials>  
            <serviceCertificate findValue="Contoso.com"
                                storeLocation="LocalMachine"  
                                storeName="My"
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
                  bindingConfiguration="MessageAndUserName"  
                  name="SecuredByTransportEndpoint"  
                  contract="ServiceModel.ICalculator" />  
      </service>  
    </services>  
    <bindings>  
      <wsHttpBinding>  
        <binding name="MessageAndUserName">  
          <security mode="Message">
            <message clientCredentialType="UserName" />  
          </security>  
        </binding>  
      </wsHttpBinding>  
    </bindings>  
    <client />  
  </system.serviceModel>  
</configuration>  
```  
  
## <a name="client"></a>Cliente  
  
### <a name="code"></a>Código  
 O código a seguir cria o cliente. A associação é a segurança do modo de mensagem e o tipo de credencial do cliente é definido como `UserName` . O nome de usuário e a senha só podem ser especificados usando código (não é configurável). O código para retornar o nome de usuário e a senha não é mostrado aqui porque ele deve ser feito no nível do aplicativo. Por exemplo, use uma caixa de diálogo Windows Forms para consultar o usuário para os dados.  
  
 [!code-csharp[C_SecurityScenarios#16](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securityscenarios/cs/source.cs#16)]
 [!code-vb[C_SecurityScenarios#16](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securityscenarios/vb/source.vb#16)]  
  
### <a name="configuration"></a>Configuração  
 O código a seguir configura o cliente. A associação é a segurança do modo de mensagem e o tipo de credencial do cliente é definido como `UserName` . O nome de usuário e a senha só podem ser especificados usando código (não é configurável).  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<configuration>  
  <system.serviceModel>  
    <bindings>  
      <wsHttpBinding>  
        <binding name="WSHttpBinding_ICalculator" >  
          <security mode="Message">  
            <message clientCredentialType="UserName" />  
          </security>  
        </binding>  
      </wsHttpBinding>  
    </bindings>  
    <client>  
      <endpoint address="http://machineName/Calculator"
                binding="wsHttpBinding"  
                bindingConfiguration="WSHttpBinding_ICalculator"
                contract="ICalculator"  
                name="WSHttpBinding_ICalculator">  
        <identity>  
          <dns value ="Contoso.com" />  
        </identity>  
      </endpoint>  
    </client>  
  </system.serviceModel>  
</configuration>  
```  
  
## <a name="see-also"></a>Consulte também

- [Visão geral de segurança](security-overview.md)
- [Message Security User Name](../samples/message-security-user-name.md)
- [Identidade e autenticação de serviço](service-identity-and-authentication.md)
- [\<identity>](../../configure-apps/file-schema/wcf/identity.md)
- [Modelo de segurança para o Windows Server app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ee677202(v=azure.10))
