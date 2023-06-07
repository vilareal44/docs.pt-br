---
title: Cliente e serviço sem segurança na Intranet
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: f450f5d4-3547-47ec-9320-2809e6a12634
ms.openlocfilehash: 591f7db0f6b4e928a991961d3bc7c404f41028bf
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84579274"
---
# <a name="intranet-unsecured-client-and-service"></a>Cliente e serviço sem segurança na Intranet
A ilustração a seguir descreve um serviço de Windows Communication Foundation simples (WCF) desenvolvido para fornecer informações sobre uma rede privada segura para um aplicativo WCF. A segurança não é necessária porque os dados são de baixa importância, a rede deve ser inerentemente segura ou a segurança é fornecida por uma camada abaixo da infraestrutura do WCF.  
  
 ![Cenário de cliente e serviço desprotegido da intranet.](./media/intranet-unsecured-client-and-service/unsecured-web-client-service.gif)  
  
|Característica|Descrição|  
|--------------------|-----------------|  
|Modo de segurança|Nenhum|  
|Transport|TCP|  
|Associação|<xref:System.ServiceModel.NetTcpBinding>|  
|Interoperabilidade|Somente WCF|  
|Autenticação|Nenhum|  
|Integridade|Nenhum|  
|Confidencialidade|Nenhum|  
  
## <a name="service"></a>Serviço  
 O código e a configuração a seguir devem ser executados de forma independente. Realize um dos seguintes procedimentos:  
  
- Crie um serviço autônomo usando o código sem configuração.  
  
- Crie um serviço usando a configuração fornecida, mas não defina nenhum ponto de extremidade.  
  
### <a name="code"></a>Código  
 O código a seguir mostra como criar um ponto de extremidade sem segurança:  
  
 [!code-csharp[C_UnsecuredService#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_unsecuredservice/cs/source.cs#2)]
 [!code-vb[C_UnsecuredService#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_unsecuredservice/vb/source.vb#2)]  
  
### <a name="configuration"></a>Configuração  
 O código a seguir configura o mesmo ponto de extremidade usando a configuração:  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<configuration>  
  <system.serviceModel>  
    <behaviors />  
    <services>  
      <service behaviorConfiguration=""
               name="ServiceModel.Calculator">  
        <endpoint address="net.tcp://localhost:8008/Calculator"
                  binding="netTcpBinding"  
                  bindingConfiguration="tcp_Unsecured"
                  name="netTcp_ICalculator"  
                  contract="ServiceModel.ICalculator" />  
      </service>  
    </services>  
    <bindings>  
      <netTcpBinding>  
        <binding name="tcp_Unsecured">  
          <security mode="None" />  
        </binding>  
      </netTcpBinding>  
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
 O código a seguir mostra um cliente WCF básico que acessa um ponto de extremidade desprotegido usando o protocolo TCP.  
  
 [!code-csharp[C_UnsecuredClient#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_unsecuredclient/cs/source.cs#2)]
 [!code-vb[C_UnsecuredClient#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_unsecuredclient/vb/source.vb#2)]  
  
### <a name="configuration"></a>Configuração  
 O código de configuração a seguir se aplica ao cliente:  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<configuration>  
  <system.serviceModel>  
    <bindings>  
      <netTcpBinding>  
        <binding name="NetTcpBinding_ICalculator" >  
          <security mode="None">  
          </security>  
        </binding>  
      </netTcpBinding>  
    </bindings>  
    <client>  
      <endpoint address="net.tcp://machineName:8008/Calculator "  
                binding="netTcpBinding"
                bindingConfiguration="NetTcpBinding_ICalculator"  
                contract="ICalculator"
                name="NetTcpBinding_ICalculator" />  
    </client>  
  </system.serviceModel>  
</configuration>  
```  
  
## <a name="see-also"></a>Consulte também

- <xref:System.ServiceModel.NetTcpBinding>
- [Visão geral de segurança](security-overview.md)
- [Modelo de segurança para o Windows Server app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ee677202(v=azure.10))
