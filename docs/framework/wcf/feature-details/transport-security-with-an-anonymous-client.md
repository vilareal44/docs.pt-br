---
title: Segurança de transporte com um cliente anônimo
description: Examine este cenário WCF, que usa a segurança de transporte para autenticar um servidor usando um certificado que o cliente confia. O cliente não está autenticado.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 056653a5-384e-4a02-ae3c-1b0157d2ccb4
ms.openlocfilehash: 08cfb8c1a5581f17a251224430018764bed80b0f
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245005"
---
# <a name="transport-security-with-an-anonymous-client"></a>Segurança de transporte com um cliente anônimo

Este cenário de Windows Communication Foundation (WCF) usa a segurança de transporte (HTTPS) para garantir a confidencialidade e a integridade. O servidor deve ser autenticado com um certificado protocolo SSL (SSL) e os clientes devem confiar no certificado do servidor. O cliente não é autenticado por nenhum mecanismo e, portanto, é anônimo.

Para um aplicativo de exemplo, consulte [segurança de transporte do WS](../samples/ws-transport-security.md). Para obter mais informações sobre segurança de transporte, consulte [visão geral de segurança de transporte](transport-security-overview.md).

Para obter mais informações sobre como usar um certificado com um serviço, consulte [trabalhando com certificados](working-with-certificates.md) e [como configurar uma porta com um certificado SSL](how-to-configure-a-port-with-an-ssl-certificate.md).

![Usando a segurança de transporte com um cliente anônimo](./media/8fa2e931-0cfb-4aaa-9272-91d652b85d8d.gif)

|Característica|Descrição|
|--------------------|-----------------|
|Modo de segurança|Transporte|
|Interoperabilidade|Com os serviços Web e clientes existentes|
|Autenticação (servidor)<br /><br /> Autenticação (cliente)|Yes<br /><br /> Nível do aplicativo (sem suporte do WCF)|
|Integridade|Yes|
|Confidencialidade|Yes|
|Transporte|HTTPS|
|Associação|<xref:System.ServiceModel.WSHttpBinding>|

## <a name="service"></a>Serviço

O código e a configuração a seguir devem ser executados de forma independente. Realize um dos seguintes procedimentos:

- Crie um serviço autônomo usando o código sem configuração.

- Crie um serviço usando a configuração fornecida, mas não defina nenhum ponto de extremidade.

### <a name="code"></a>Código

O código a seguir mostra como criar um ponto de extremidade usando a segurança de transporte:

[!code-csharp[c_SecurityScenarios#5](~/samples/snippets/csharp/VS_Snippets_CFX/c_securityscenarios/cs/source.cs#5)]
[!code-vb[c_SecurityScenarios#5](~/samples/snippets/visualbasic/VS_Snippets_CFX/c_securityscenarios/vb/source.vb#5)]

### <a name="configuration"></a>Configuração

O código a seguir configura o mesmo ponto de extremidade usando a configuração. O cliente não é autenticado por nenhum mecanismo e, portanto, é anônimo.

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="ServiceModel.Calculator">
        <endpoint address="https://localhost/Calculator"
                  binding="wsHttpBinding"
                  bindingConfiguration="WSHttpBinding_ICalculator"
                  name="SecuredByTransportEndpoint"
                  contract="ServiceModel.ICalculator" />
      </service>
    </services>
    <bindings>
      <wsHttpBinding>
        <binding name="WSHttpBinding_ICalculator">
          <security mode="Transport">
            <transport clientCredentialType="None" />
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

     [!code-csharp[C_SecurityScenarios#0](~/samples/snippets/csharp/VS_Snippets_CFX/c_securityscenarios/cs/source.cs#0)]
     [!code-vb[C_SecurityScenarios#0](~/samples/snippets/visualbasic/VS_Snippets_CFX/c_securityscenarios/vb/source.vb#0)]

### <a name="code"></a>Código

[!code-csharp[c_SecurityScenarios#6](~/samples/snippets/csharp/VS_Snippets_CFX/c_securityscenarios/cs/source.cs#6)]
[!code-vb[c_SecurityScenarios#6](~/samples/snippets/visualbasic/VS_Snippets_CFX/c_securityscenarios/vb/source.vb#6)]

### <a name="configuration"></a>Configuração

A configuração a seguir pode ser usada em vez do código para configurar o serviço.

```xml
<configuration>
  <system.serviceModel>
    <bindings>
      <wsHttpBinding>
        <binding name="WSHttpBinding_ICalculator" >
          <security mode="Transport">
            <transport clientCredentialType="None" />
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

- [Visão geral de segurança](security-overview.md)
- [Segurança de transporte WS](../samples/ws-transport-security.md)
- [Visão geral de segurança de transporte](transport-security-overview.md)
- [Modelo de segurança para o Windows Server app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ee677202(v=azure.10))
