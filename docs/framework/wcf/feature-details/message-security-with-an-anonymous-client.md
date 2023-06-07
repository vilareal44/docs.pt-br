---
title: Mensagem de segurança com um cliente anônimo
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: cad53e1a-b7c9-4064-bc87-508c3d1dce49
ms.openlocfilehash: 058163c96bba036c3183695bf986b4d0424271ac
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84595213"
---
# <a name="message-security-with-an-anonymous-client"></a>Mensagem de segurança com um cliente anônimo

O cenário a seguir mostra um cliente e um serviço protegido pela segurança de mensagem do Windows Communication Foundation (WCF). Uma meta de design é usar a segurança de mensagens em vez da segurança de transporte, para que, no futuro, ela possa dar suporte a um modelo mais rico baseado em declarações. Para obter mais informações sobre como usar declarações avançadas para autorização, consulte [Gerenciando declarações e autorização com o modelo de identidade](managing-claims-and-authorization-with-the-identity-model.md).

Para um aplicativo de exemplo, consulte [segurança da mensagem anônimo](../samples/message-security-anonymous.md).

![Segurança de mensagem com um cliente anônimo](media/b361a565-831c-4c10-90d7-66d8eeece0a1.gif "b361a565-831c-4c10-90d7-66d8eeece0a1")

|Característica|Descrição|
|--------------------|-----------------|
|Modo de segurança|Mensagem|
|Interoperabilidade|Somente WCF|
|Autenticação (servidor)|A negociação inicial requer autenticação de servidor, mas não autenticação de cliente|
|Autenticação (cliente)|Nenhum|
|Integridade|Sim, usando o contexto de segurança compartilhado|
|Confidencialidade|Sim, usando o contexto de segurança compartilhado|
|Transport|HTTP|

## <a name="service"></a>Serviço

O código e a configuração a seguir devem ser executados de forma independente. Realize um dos seguintes procedimentos:

- Crie um serviço autônomo usando o código sem configuração.

- Crie um serviço usando a configuração fornecida, mas não defina nenhum ponto de extremidade.

### <a name="code"></a>Código

O código a seguir mostra como criar um ponto de extremidade de serviço que usa a segurança de mensagem.

[!code-csharp[C_SecurityScenarios#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securityscenarios/cs/source.cs#8)]
[!code-vb[C_SecurityScenarios#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securityscenarios/vb/source.vb#8)]

### <a name="configuration"></a>Configuração

A configuração a seguir pode ser usada em vez do código. O elemento de comportamento do serviço é usado para especificar um certificado que é usado para autenticar o serviço para o cliente. O elemento de serviço deve especificar o comportamento usando o `behaviorConfiguration` atributo. O elemento Binding especifica que o tipo de credencial do cliente é `None` , permitindo que clientes anônimos usem o serviço.

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.serviceModel>
    <behaviors>
      <serviceBehaviors>
        <behavior name="ServiceCredentialsBehavior">
          <serviceCredentials>
            <serviceCertificate findValue="contoso.com"
                                storeLocation="LocalMachine"
                                storeName="My" />
          </serviceCredentials>
        </behavior>
      </serviceBehaviors>
    </behaviors>
    <services>
      <service behaviorConfiguration="ServiceCredentialsBehavior"
               name="ServiceModel.Calculator">
        <endpoint address="http://localhost/Calculator"
                  binding="wsHttpBinding"
                  bindingConfiguration="WSHttpBinding_ICalculator"
                  name="CalculatorService"
                  contract="ServiceModel.ICalculator" />
      </service>
    </services>
    <bindings>
      <wsHttpBinding>
        <binding name="WSHttpBinding_ICalculator" >
          <security mode="Message">
            <message clientCredentialType="None" />
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

O código a seguir cria uma instância do cliente. A associação usa a segurança do modo de mensagem e o tipo de credencial do cliente é definido como nenhum.

[!code-csharp[C_SecurityScenarios#15](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securityscenarios/cs/source.cs#15)]
[!code-vb[C_SecurityScenarios#15](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securityscenarios/vb/source.vb#15)]

### <a name="configuration"></a>Configuração

O código a seguir configura o cliente.

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.serviceModel>
    <bindings>
      <wsHttpBinding>
        <binding name="WSHttpBinding_ICalculator" >
          <security mode="Message">
            <message clientCredentialType="None" />
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
          <dns value="contoso.com" />
        </identity>
      </endpoint>
    </client>
  </system.serviceModel>
</configuration>
```

## <a name="see-also"></a>Consulte também

- [Visão geral de segurança](security-overview.md)
- [Segurança de aplicativos distribuídos](distributed-application-security.md)
- [Segurança de mensagem anônima](../samples/message-security-anonymous.md)
- [Identidade e autenticação de serviço](service-identity-and-authentication.md)
- [Modelo de segurança para o Windows Server app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ee677202(v=azure.10))
