---
title: Associações e segurança
description: Descubra como selecionar a ligação certa para suas necessidades de segurança. As associações fornecidas pelo sistema incluídas com o WCF fornecem uma maneira rápida de programar aplicativos WCF.
ms.date: 03/30/2017
helpviewer_keywords:
- bindings [WCF], security
- WCF security
- Windows Communication Foundation, security
- bindings [WCF]
ms.assetid: 4de03dd3-968a-4e65-af43-516e903d7f95
ms.openlocfilehash: e012ec9ad340c74f5bc776cfc6d8b88326210fec
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245318"
---
# <a name="bindings-and-security"></a>Associações e segurança

As associações fornecidas pelo sistema incluídas com o Windows Communication Foundation (WCF) oferecem uma maneira rápida de programar aplicativos WCF. Com uma exceção, todas as associações têm um esquema de segurança padrão habilitado. Este tópico ajuda você a selecionar a ligação certa para suas necessidades de segurança.

Para obter uma visão geral da segurança do WCF, consulte [visão geral de segurança](security-overview.md). Para obter mais informações sobre como programar o WCF usando associações, consulte [Programming WCF Security](programming-wcf-security.md).

Se você já tiver selecionado uma associação, poderá saber mais sobre os comportamentos de tempo de execução associados à segurança em [comportamentos de segurança](security-behaviors-in-wcf.md).

Algumas funções de segurança não são programáveis usando as associações fornecidas pelo sistema. Para obter mais controle usando uma associação personalizada, consulte [recursos de segurança com associações personalizadas](security-capabilities-with-custom-bindings.md).

## <a name="security-functions-of-bindings"></a>Funções de segurança de associações

O WCF inclui uma série de associações fornecidas pelo sistema que atendem à maioria das necessidades. Se uma ligação específica não for suficiente, você também poderá criar uma associação personalizada. Para obter uma lista de associações fornecidas pelo sistema, consulte [associações fornecidas pelo sistema](../system-provided-bindings.md). Para obter mais informações sobre associações personalizadas, consulte [associações personalizadas](../extending/custom-bindings.md).

Cada associação no WCF tem duas formas: como uma API e um elemento XML usado em um arquivo de configuração. Por exemplo, a `WSHttpBinding` (API) tem um equivalente no [\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md) .

A seção a seguir lista os dois formulários para cada associação e resume os recursos de segurança.

### <a name="basichttp"></a>BasicHttp

No código, use a <xref:System.ServiceModel.BasicHttpBinding> classe; em configuração, use o [\<basicHttpBinding>](../../configure-apps/file-schema/wcf/basichttpbinding.md) .

Essa associação foi projetada para uso com uma variedade de tecnologias existentes, incluindo as seguintes:

- ASP.NET Web Services (ASMX), versão 1.

- Aplicativos WSE (Web Service Enhancements).

- Perfil básico, conforme definido na especificação WS-I (Web Services Interoperability) ( <https://go.microsoft.com/fwlink/?LinkId=38955> ).

- Perfil de segurança básica, conforme definido em WS-I.

Por padrão, essa associação não é segura. Ele foi projetado para interoperar com os serviços ASMX. Quando a segurança está habilitada, a associação é projetada para interoperação direta com mecanismos de segurança do Serviços de Informações da Internet (IIS), como autenticação básica, Digest e segurança integrada do Windows. Para obter mais informações, consulte [visão geral de segurança de transporte](transport-security-overview.md). Essa associação dá suporte ao seguinte:

- Segurança de transporte HTTPS.

- Autenticação básica HTTP.

- WS-Security.

Para obter mais informações, consulte <xref:System.ServiceModel.BasicHttpSecurity>, <xref:System.ServiceModel.BasicHttpMessageSecurity>, <xref:System.ServiceModel.BasicHttpMessageCredentialType> e <xref:System.ServiceModel.BasicHttpSecurityMode>.

### <a name="wshttpbinding"></a>WSHttpBinding

No código, use a <xref:System.ServiceModel.WSHttpBinding> classe; em configuração, use o [\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md) .

Por padrão, essa associação implementa a especificação WS-Security e fornece interoperabilidade com serviços que implementam as especificações WS-*. Ele dá suporte ao seguinte:

- Segurança de transporte HTTPS.

- WS-Security.

- Proteção de transporte HTTPS com segurança de credencial de mensagem SOAP para autenticar o chamador.

Para obter mais informações, consulte,,,,, <xref:System.ServiceModel.WSHttpSecurity> <xref:System.ServiceModel.MessageSecurityOverHttp> <xref:System.ServiceModel.MessageCredentialType> <xref:System.ServiceModel.SecurityMode> <xref:System.ServiceModel.HttpTransportSecurity> <xref:System.ServiceModel.HttpClientCredentialType> e <xref:System.ServiceModel.HttpProxyCredentialType> .

### <a name="wsdualhttpbinding"></a>WSDualHttpBinding

No código, use a <xref:System.ServiceModel.WSDualHttpBinding> classe; em configuração, use o [\<wsDualHttpBinding>](../../configure-apps/file-schema/wcf/wsdualhttpbinding.md) .

Essa associação foi projetada para habilitar aplicativos de serviço duplex. Essa associação implementa a especificação WS-Security para segurança de transferência baseada em mensagem. A segurança de transporte não está disponível. Por padrão, ele fornece os seguintes recursos:

- Implementa mensagens WS-Reliable para confiabilidade.

- Implementa o WS-Security para segurança de transferência e autenticação.

- Usa HTTP para entrega de mensagem.

- Usa codificação de mensagem de texto/XML.

 Usando o WS-Security (segurança de camada de mensagem), a associação permite que você configure os seguintes parâmetros:

- O conjunto de algoritmos de segurança para determinar o algoritmo criptográfico.

- Opções de associação para o seguinte:

  - Fornecendo credenciais de serviço disponíveis fora de banda no cliente.

  - Fornecer credenciais de serviço negociadas do serviço como parte da configuração do canal.

Para obter mais informações, consulte <xref:System.ServiceModel.WSDualHttpSecurity> e <xref:System.ServiceModel.WSDualHttpSecurityMode>.

### <a name="nettcpbinding"></a>NetTcpBinding

No código, use a <xref:System.ServiceModel.NetTcpBinding> classe; em configuração, use o [\<netTcpBinding>](../../configure-apps/file-schema/wcf/nettcpbinding.md) .

Essa associação é otimizada para comunicação entre computadores. Por padrão, ele tem as seguintes características:

- Implementa a segurança da camada de transporte.

- Aproveita a segurança do Windows para a segurança de transferência e a autenticação.

- Usa TCP para transporte.

- Implementa a codificação de mensagem binária.

- Implementa o sistema de mensagens WS-Reliable.

As opções incluem:

- Segurança de camada de mensagem (usando o WS-Security).

- Segurança de transporte com credencial de mensagem — confidencialidade e integridade fornecidas pela TLS (segurança da camada de transporte) sobre TCP e credenciais para autorização fornecidas pelo WS-Security.

Para obter mais informações, consulte <xref:System.ServiceModel.NetTcpSecurity>, <xref:System.ServiceModel.TcpTransportSecurity>, <xref:System.ServiceModel.TcpClientCredentialType>, <xref:System.ServiceModel.MessageSecurityOverTcp> e <xref:System.ServiceModel.MessageCredentialType>.

### <a name="netnamedpipebinding"></a>NetNamedPipeBinding

No código, use a <xref:System.ServiceModel.NetNamedPipeBinding> classe; em configuração, use o [\<netNamedPipeBinding>](../../configure-apps/file-schema/wcf/netnamedpipebinding.md) .

Essa associação é otimizada para comunicação entre processos (geralmente no mesmo computador). Por padrão, essa associação tem as seguintes características:

- Usa segurança de transporte para transferência e autenticação de mensagens.

- Usa pipes nomeados para entrega de mensagem.

- Implementa a codificação de mensagem binária.

- Criptografia e assinatura de mensagens.

As opções incluem:

- Autenticação usando a segurança do Windows.

Para obter mais informações, consulte <xref:System.ServiceModel.NetNamedPipeSecurity>, <xref:System.ServiceModel.NetNamedPipeSecurityMode> e <xref:System.ServiceModel.NamedPipeTransportSecurity>.

### <a name="msmqintegrationbinding"></a>MsmqIntegrationBinding

No código, use a <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding> classe; em configuração, use o [\<msmqIntegrationBinding>](../../configure-apps/file-schema/wcf/msmqintegrationbinding.md) .

Essa associação é otimizada para a criação de clientes WCF e serviços que interoperam com pontos de extremidade do MSMQ (enfileiramento de mensagens) não WCF.

Por padrão, essa associação usa a segurança de transporte e fornece as seguintes características de segurança:

- A segurança pode ser desabilitada (nenhuma).

- Segurança de transporte do MSMQ (transporte).

Para obter mais informações, consulte <xref:System.ServiceModel.NetMsmqSecurity> e <xref:System.ServiceModel.NetMsmqSecurityMode>.

### <a name="netmsmqbinding"></a>NetMsmqBinding

No código, use a <xref:System.ServiceModel.NetMsmqBinding> classe; em configuração, use o [\<netMsmqBinding>](../../configure-apps/file-schema/wcf/netmsmqbinding.md) .

Essa associação destina-se ao uso durante a criação de serviços WCF que exigem suporte a mensagens em fila MSMQ.

Por padrão, essa associação usa a segurança de transporte e fornece as seguintes características de segurança:

- A segurança pode ser desabilitada (nenhuma).

- Segurança de transporte do MSMQ (transporte).

- Segurança de mensagem baseada em SOAP (mensagem).

- Segurança simultânea de transporte e mensagem (ambas).

- Tipos de credencial de cliente com suporte: nenhum, Windows, nome de usuário, certificado, IssuedToken.

A <xref:System.ServiceModel.MessageCredentialType.Certificate> credencial tem suporte apenas quando o modo de segurança está definido como <xref:System.ServiceModel.NetMsmqSecurityMode.Both> ou <xref:System.ServiceModel.NetMsmqSecurityMode.Message> .

Para obter mais informações, consulte <xref:System.ServiceModel.MessageSecurityOverMsmq> e <xref:System.ServiceModel.MsmqTransportSecurity>.

### <a name="wsfederationhttpbinding"></a>WSFederationHttpBinding

No código, use a <xref:System.ServiceModel.WSFederationHttpBinding> classe; em configuração, use o [\<wsFederationHttpBinding>](../../configure-apps/file-schema/wcf/wsfederationhttpbinding.md) .

Por padrão, essa associação usa o WS-Security (segurança de camada de mensagem).

Para obter mais informações, consulte [Federação](federation.md), <xref:System.ServiceModel.WSFederationHttpSecurity> e <xref:System.ServiceModel.WSFederationHttpSecurityMode> .

## <a name="custom-bindings"></a>Associações personalizadas

Se nenhuma das associações fornecidas pelo sistema atender aos requisitos, você poderá criar uma associação personalizada com um elemento de associação de segurança personalizado. Para obter mais informações, consulte [recursos de segurança com associações personalizadas](security-capabilities-with-custom-bindings.md).

## <a name="binding-choices"></a>Opções de associação

A tabela a seguir resume os recursos oferecidos na configuração do modo de segurança, ou seja, lista os recursos disponíveis quando o modo de segurança é definido como `Transport` , `Message` ou `TransportWithMessageCredential` . Use esta tabela para ajudá-lo a encontrar os recursos de segurança que seu aplicativo requer.

|Configuração|Recursos|
|-------------|--------------|
|Transporte|Autenticação de servidor<br /><br /> Autenticação de cliente<br /><br /> Segurança ponto a ponto<br /><br /> Interoperabilidade<br /><br /> Aceleração de hardware<br /><br /> Alta taxa de transferência<br /><br /> Proteger o firewall<br /><br /> Aplicativos de alta latência<br /><br /> Nova criptografia em vários saltos|
|Mensagem|Autenticação de servidor<br /><br /> Autenticação de cliente<br /><br /> Segurança de ponta a ponta<br /><br /> Interoperabilidade<br /><br /> Declarações avançadas<br /><br /> Federação<br /><br /> Autenticação multifator<br /><br /> Tokens personalizados<br /><br /> Serviço de Notary/carimbo de data/hora<br /><br /> Aplicativos de alta latência<br /><br /> Persistência de assinaturas de mensagem|
|TransportWithMessageCredential|Autenticação de servidor<br /><br /> Autenticação de cliente<br /><br /> Segurança ponto a ponto<br /><br /> Interoperabilidade<br /><br /> Aceleração de hardware<br /><br /> Alta taxa de transferência<br /><br /> Declarações de cliente avançadas<br /><br /> Federação<br /><br /> Autenticação multifator<br /><br /> Tokens personalizados<br /><br /> Proteger o firewall<br /><br /> Aplicativos de alta latência<br /><br /> Nova criptografia em vários saltos|

A tabela a seguir lista as associações que dão suporte às várias configurações de modo. Selecione uma associação da tabela a ser usada para criar o ponto de extremidade de serviço.

|Associação|Suporte ao modo de transporte|Suporte ao modo de mensagem|Suporte do TransportWithMessageCredential|
|-------------|----------------------------|--------------------------|--------------------------------------------|
|`BasicHttpBinding`|Yes|Yes|Yes|
|`WSHttpBinding`|Yes|Yes|Sim|
|`WSDualHttpBinding`|Não|Sim|Não|
|`NetTcpBinding`|Sim|Yes|Yes|
|`NetNamedPipeBinding`|Sim|No|Não|
|`NetMsmqBinding`|Sim|Sim|Não|
|`MsmqIntegrationBinding`|Sim|No|Não|
|`wsFederationHttpBinding`|Não|Sim|Yes|

## <a name="transport-credentials-in-bindings"></a>Credenciais de transporte em associações

A tabela a seguir lista os tipos de credenciais de cliente disponíveis ao usar o `BasicHttpBinding` ou o `WSHttpBinding` no modo de segurança de transporte.

|Type|Descrição|
|----------|-----------------|
|Nenhum|Especifica que o cliente não precisa apresentar nenhuma credencial. Isso se traduz em um cliente anônimo.|
|Basic|Autenticação básica. Para obter mais informações, consulte RFC 2617 – autenticação HTTP: autenticação básica e resumida, disponível em <https://go.microsoft.com/fwlink/?LinkId=84023> .|
|Digest|Autenticação resumida. Para obter mais informações, consulte RFC 2617 – autenticação HTTP: autenticação básica e resumida, disponível em <https://go.microsoft.com/fwlink/?LinkId=84023> .|
|NTLM|Autenticação NTLM (NT LAN Manager).|
|Windows|Autenticação do Windows.|
|Certificado|Autenticação executada usando um certificado.|
|IssuedToken|Permite que o serviço exija que o cliente seja autenticado usando um token emitido por um serviço de token de segurança ou pelo CardSpace. Para obter mais informações, consulte [Federação e tokens emitidos](federation-and-issued-tokens.md).|

### <a name="message-client-credentials-in-bindings"></a>Credenciais de cliente de mensagem em associações

A tabela a seguir lista os tipos de credenciais de cliente disponíveis ao usar uma associação no modo de segurança da mensagem.

|Type|Descrição|
|----------|-----------------|
|Nenhum|Permite que o serviço interaja com clientes anônimos.|
|Windows|Permite que as trocas de mensagens SOAP sejam feitas sob o contexto autenticado de uma credencial do Windows.|
|UserName|Permite que o serviço exija que o cliente seja autenticado usando uma credencial de nome de usuário. Observe que, quando o modo de segurança é definido como `TransportWithMessageCredential` , o WCF não dá suporte ao envio de um resumo de senha ou à derivação de chaves usando a senha e usando essas chaves para a segurança do modo de mensagem. Dessa forma, o WCF impõe que o transporte seja protegido ao usar credenciais de nome de usuário.|
|Certificado|Permite que o serviço exija que o cliente seja autenticado usando um certificado.|
|IssuedToken|Permite que o serviço use um serviço de token de segurança para fornecer um token personalizado.|

## <a name="see-also"></a>Veja também

- [Visão geral de segurança](security-overview.md)
- [Protegendo serviços e clientes](securing-services-and-clients.md)
- [Selecionando um tipo de credencial](selecting-a-credential-type.md)
- [Recursos de segurança com associações personalizadas](security-capabilities-with-custom-bindings.md)
- [Comportamentos de segurança](security-behaviors-in-wcf.md)
- [Modelo de segurança para o Windows Server app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ee677202(v=azure.10))
