---
title: Segurança de transporte com autenticação básica
description: Examine este cenário WCF, que mostra a autenticação básica para um serviço e cliente WCF. O serviço precisa de um certificado válido que o cliente confie.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: b54f491d-196b-4279-876c-76b83ec0442c
ms.openlocfilehash: f15a19feaed631a76948efd24ee225acf789cb2d
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85244849"
---
# <a name="transport-security-with-basic-authentication"></a>Segurança de transporte com autenticação básica
A ilustração a seguir mostra um serviço e cliente do Windows Communication Foundation (WCF). O servidor precisa de um certificado X. 509 válido que possa ser usado para protocolo SSL (SSL) e os clientes devem confiar no certificado do servidor. Além disso, o serviço Web já tem uma implementação SSL que pode ser usada. Para obter mais informações sobre como habilitar a autenticação básica no Serviços de Informações da Internet (IIS), consulte <https://docs.microsoft.com/iis/configuration/system.webserver/security/authentication/basicauthentication> .  
  
 ![Captura de tela que mostra a segurança de transporte com autenticação básica.](./media/transport-security-with-basic-authentication/transport-security-basic-authentication.gif)  
  
|Característica|Descrição|  
|--------------------|-----------------|  
|Modo de segurança|Transporte|  
|Interoperabilidade|Com serviços e clientes de serviços Web existentes|  
|Autenticação (servidor)<br /><br /> Autenticação (cliente)|Sim (usando HTTPS)<br /><br /> Sim (por nome de usuário/senha)|  
|Integridade|Yes|  
|Confidencialidade|Yes|  
|Transporte|HTTPS|  
|Associação|<xref:System.ServiceModel.WSHttpBinding>|  
  
## <a name="service"></a>Serviço  
 O código e a configuração a seguir devem ser executados de forma independente. Realize um dos seguintes procedimentos:  
  
- Crie um serviço autônomo usando o código sem configuração.  
  
- Crie um serviço usando a configuração fornecida, mas não defina nenhum ponto de extremidade.  
  
### <a name="code"></a>Código  
 O código a seguir mostra como criar um ponto de extremidade de serviço que usa um nome de usuário de domínio do Windows e uma senha para a segurança da transferência. Observe que o serviço requer um certificado X. 509 para autenticar no cliente. Para obter mais informações, consulte [trabalhando com certificados](working-with-certificates.md) e [como configurar uma porta com um certificado SSL](how-to-configure-a-port-with-an-ssl-certificate.md).  
  
 [!code-csharp[C_SecurityScenarios#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securityscenarios/cs/source.cs#1)]
 [!code-vb[C_SecurityScenarios#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securityscenarios/vb/source.vb#1)]  
  
## <a name="configuration"></a>Configuração  
 O seguinte configura um serviço para usar a autenticação básica com segurança em nível de transporte:  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<configuration>  
    <system.serviceModel>  
        <bindings>  
            <wsHttpBinding>  
                <binding name="UsernameWithTransport">  
                    <security mode="Transport">  
                        <transport clientCredentialType="Basic" />  
                    </security>  
                </binding>  
            </wsHttpBinding>  
        </bindings>  
        <services>  
            <service name="BasicAuthentication.Calculator">  
                <endpoint address="https://localhost/Calculator"  
                          binding="wsHttpBinding"
                          bindingConfiguration="UsernameWithTransport"  
                          name="BasicEndpoint"
                          contract="BasicAuthentication.ICalculator" />  
            </service>  
        </services>  
    </system.serviceModel>  
</configuration>  
```  
  
## <a name="client"></a>Cliente  
  
### <a name="code"></a>Código  
 O código a seguir mostra o código do cliente que inclui o nome de usuário e a senha. Observe que o usuário deve fornecer um nome de usuário e senha do Windows válidos. O código para retornar o nome de usuário e a senha não é mostrado aqui. Use uma caixa de diálogo ou outra interface para consultar o usuário para obter as informações.  
  
> [!NOTE]
> O nome de usuário e a senha só podem ser definidos usando o código.  
  
 [!code-csharp[C_SecurityScenarios#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securityscenarios/cs/source.cs#2)]
 [!code-vb[C_SecurityScenarios#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securityscenarios/vb/source.vb#2)]  
  
### <a name="configuration"></a>Configuração  
 O código a seguir mostra a configuração do cliente.  
  
> [!NOTE]
> Não é possível usar a configuração para definir o nome de usuário e a senha. A configuração mostrada aqui deve ser aumentada usando o código para definir o nome de usuário e a senha.  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<configuration>  
  <system.serviceModel>  
    <bindings>  
      <wsHttpBinding>  
        <binding name="WSHttpBinding_ICalculator" >  
          <security mode="Transport">  
            <transport clientCredentialType="Basic" />  
          </security>  
        </binding>  
      </wsHttpBinding>  
    </bindings>  
    <client>  
      <endpoint address="https://machineName/Calculator"
                binding="wsHttpBinding"  
                bindingConfiguration="WSHttpBinding_ICalculator"
                contract="ICalculator"  
                name="WSHttpBinding_ICalculator" />  
    </client>  
  </system.serviceModel>  
</configuration>  
```  
  
## <a name="see-also"></a>Veja também

- <xref:System.ServiceModel.ClientBase%601.ClientCredentials%2A>
- <xref:System.ServiceModel.Security.UserNamePasswordClientCredential>
- [Trabalhando com certificados](working-with-certificates.md)
- [Como configurar uma porta com um certificado SSL](how-to-configure-a-port-with-an-ssl-certificate.md)
- [Visão geral de segurança](security-overview.md)
- [\<clientCredentials>](../../configure-apps/file-schema/wcf/clientcredentials.md)
- [Modelo de segurança para o Windows Server app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ee677202(v=azure.10))
