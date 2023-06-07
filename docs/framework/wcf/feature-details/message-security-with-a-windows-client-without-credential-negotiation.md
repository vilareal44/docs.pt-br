---
title: Segurança de mensagem com um cliente Windows sem negociação de credencial
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: fc07a26c-cbee-41c5-8fb0-329085fef749
ms.openlocfilehash: 7845bc45d0baecb07e4c03531f21d900c4e23bf7
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84595239"
---
# <a name="message-security-with-a-windows-client-without-credential-negotiation"></a>Segurança de mensagem com um cliente Windows sem negociação de credencial

O cenário a seguir mostra um cliente Windows Communication Foundation (WCF) e um serviço protegido pelo protocolo Kerberos.

O serviço e o cliente estão no mesmo domínio ou em domínios confiáveis.

> [!NOTE]
> A diferença entre esse cenário e a [segurança da mensagem com um cliente do Windows](message-security-with-a-windows-client.md) é que esse cenário não negocia a credencial do serviço com o serviço antes de enviar a mensagem do aplicativo. Além disso, como isso requer o protocolo Kerberos, esse cenário requer um ambiente de domínio do Windows.

![Segurança de mensagem sem negociação de credencial](media/0c9f9baa-2439-4ef9-92f4-43c242d85d0d.gif "0c9f9baa-2439-4ef9-92f4-43c242d85d0d")

|Característica|Descrição|
|--------------------|-----------------|
|Modo de segurança|Mensagem|
|Interoperabilidade|Sim, WS-Security com clientes compatíveis com o perfil de token Kerberos|
|Autenticação (servidor)|Autenticação mútua do servidor e do cliente|
|Autenticação (cliente)|Autenticação mútua do servidor e do cliente|
|Integridade|Sim|
|Confidencialidade|Sim|
|Transport|HTTP|
|Associação|<xref:System.ServiceModel.WSHttpBinding>|

## <a name="service"></a>Serviço

O código e a configuração a seguir devem ser executados de forma independente. Realize um dos seguintes procedimentos:

- Crie um serviço autônomo usando o código sem configuração.

- Crie um serviço usando a configuração fornecida, mas não defina nenhum ponto de extremidade.

### <a name="code"></a>Código

O código a seguir cria um ponto de extremidade de serviço que usa segurança de mensagem. O código desabilita a negociação de credencial de serviço e o estabelecimento de um símbolo de contexto de segurança (SCT).

> [!NOTE]
> Para usar o tipo de credencial do Windows sem negociação, a conta de usuário do serviço deve ter acesso ao SPN (nome da entidade de serviço) registrado com o domínio Active Directory. É possível fazer isso de duas formas:

1. Use a `NetworkService` `LocalSystem` conta ou para executar o serviço. Como essas contas têm acesso ao SPN do computador que é estabelecido quando o computador ingressa no domínio de Active Directory, o WCF gera automaticamente o elemento SPN apropriado dentro do ponto de extremidade do serviço nos metadados do serviço (linguagem de descrição de serviços Web ou WSDL).

2. Use uma conta de domínio Active Directory arbitrária para executar o serviço. Nesse caso, você precisa estabelecer um SPN para essa conta de domínio. Uma maneira de fazer isso é usar a ferramenta utilitário SetSPN. exe. Depois que o SPN for criado para a conta do serviço, configure o WCF para publicar esse SPN nos clientes do serviço por meio de seus metadados (WSDL). Isso é feito definindo a identidade do ponto de extremidade para o ponto de extremidade exposto, por meio de um arquivo ou código de configuração de aplicativo. O exemplo a seguir publica a identidade programaticamente.

Para obter mais informações sobre SPNs, o protocolo Kerberos e Active Directory, consulte [complemento técnico Kerberos para Windows](https://docs.microsoft.com/previous-versions/msp-n-p/ff649429(v=pandp.10)). Para obter mais informações sobre identidades de ponto de extremidade, consulte [modos de autenticação SecurityBindingElement](securitybindingelement-authentication-modes.md).

[!code-csharp[C_SecurityScenarios#12](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securityscenarios/cs/source.cs#12)]
[!code-vb[C_SecurityScenarios#12](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securityscenarios/vb/source.vb#12)]

### <a name="configuration"></a>Configuração

A configuração a seguir pode ser usada em vez do código.

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.serviceModel>
    <behaviors />
    <services>
      <service behaviorConfiguration="" name="ServiceModel.Calculator">
        <endpoint address="http://localhost/Calculator"
                  binding="wsHttpBinding"
                  bindingConfiguration="KerberosBinding"
                  name="WSHttpBinding_ICalculator"
                  contract="ServiceModel.ICalculator"
                  listenUri="net.tcp://localhost/metadata" >
         <identity>
            <servicePrincipalName value="service_spn_name" />
         </identity>
        </endpoint>
      </service>
    </services>
    <bindings>
      <wsHttpBinding>
        <binding name="KerberosBinding">
          <security>
            <message negotiateServiceCredential="false"
                     establishSecurityContext="false" />
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

O código a seguir configura o cliente. O modo de segurança é definido como mensagem e o tipo de credencial do cliente é definido como Windows. Observe que as <xref:System.ServiceModel.MessageSecurityOverHttp.NegotiateServiceCredential%2A> <xref:System.ServiceModel.NonDualMessageSecurityOverHttp.EstablishSecurityContext%2A> Propriedades e são definidas como `false` .

> [!NOTE]
> Para usar o tipo de credencial do Windows sem negociação, o cliente deve ser configurado com o SPN da conta do serviço antes de iniciar a comunicação com o serviço. O cliente usa o SPN para obter o token Kerberos para autenticar e proteger a comunicação com o serviço. O exemplo a seguir mostra como configurar o cliente com o SPN do serviço. Se você estiver usando a [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) para gerar o cliente, o SPN do serviço será propagado automaticamente para o cliente por meio dos metadados do serviço (WSDL), se os metadados do serviço contiverem essas informações. Para obter mais informações sobre como configurar o serviço para incluir seu SPN nos metadados do serviço, consulte a seção "serviço" mais adiante neste tópico.
>
> Para obter mais informações sobre SPNs, Kerberos e Active Directory, consulte [complemento técnico do Kerberos para Windows](https://docs.microsoft.com/previous-versions/msp-n-p/ff649429(v=pandp.10)). Para obter mais informações sobre identidades de ponto de extremidade, consulte o tópico [modos de autenticação SecurityBindingElement](securitybindingelement-authentication-modes.md) .

[!code-csharp[C_SecurityScenarios#19](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securityscenarios/cs/source.cs#19)]
[!code-vb[C_SecurityScenarios#19](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securityscenarios/vb/source.vb#19)]

### <a name="configuration"></a>Configuração

O código a seguir configura o cliente. Observe que o [\<servicePrincipalName>](../../configure-apps/file-schema/wcf/serviceprincipalname.md) elemento deve ser definido para corresponder ao SPN do serviço como registrado para a conta do serviço no domínio Active Directory.

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.serviceModel>
    <bindings>
      <wsHttpBinding>
        <binding name="WSHttpBinding_ICalculator" >
          <security mode="Message">
            <message clientCredentialType="Windows"
                     negotiateServiceCredential="false"
                     establishSecurityContext="false" />
          </security>
        </binding>
      </wsHttpBinding>
    </bindings>
    <client>
      <endpoint address="http://localhost/Calculator"
                binding="wsHttpBinding"
                bindingConfiguration="WSHttpBinding_ICalculator"
                contract="ICalculator"
                name="WSHttpBinding_ICalculator">
        <identity>
          <servicePrincipalName value="service_spn_name" />
        </identity>
      </endpoint>
    </client>
  </system.serviceModel>
</configuration>
```

## <a name="see-also"></a>Confira também

- [Visão geral de segurança](security-overview.md)
- [Identidade e autenticação de serviço](service-identity-and-authentication.md)
- [Modelo de segurança para o Windows Server app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ee677202(v=azure.10))
