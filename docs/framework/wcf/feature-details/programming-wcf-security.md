---
title: Programação de segurança do WCF
description: Saiba como criar um aplicativo WCF seguro, incluindo autenticação, confidencialidade e integridade.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- message security [WCF], programming overview
ms.assetid: 739ec222-4eda-4cc9-a470-67e64a7a3f10
ms.openlocfilehash: 8e77c667dd8904c10bbab88e1413690677cef53b
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85244979"
---
# <a name="programming-wcf-security"></a>Programação de segurança do WCF
Este tópico descreve as tarefas de programação fundamentais usadas para criar um aplicativo de Windows Communication Foundation de segurança (WCF). Este tópico aborda somente autenticação, confidencialidade e integridade, coletivamente conhecido como *segurança de transferência*. Este tópico não abrange a autorização (o controle de acesso a recursos ou serviços); para obter informações sobre autorização, consulte [Authorization](authorization-in-wcf.md).  
  
> [!NOTE]
> Para obter uma introdução valiosa aos conceitos de segurança, especialmente em relação ao WCF, consulte o conjunto de tutoriais de padrões e práticas no MSDN em [cenários, padrões e diretrizes de implementação para o WSE (Web Services Enhancements) 3,0](https://docs.microsoft.com/previous-versions/msp-n-p/ff648183(v=pandp.10)).  
  
 A programação da segurança do WCF baseia-se em três etapas definindo o seguinte: o modo de segurança, um tipo de credencial de cliente e os valores de credencial. Você pode executar essas etapas por meio de código ou configuração.  
  
## <a name="setting-the-security-mode"></a>Configurando o modo de segurança  
 O seguinte explica as etapas gerais para a programação com o modo de segurança no WCF:  
  
1. Selecione uma das associações predefinidas apropriadas para os requisitos do aplicativo. Para obter uma lista das opções de associação, consulte [associações fornecidas pelo sistema](../system-provided-bindings.md). Por padrão, quase todas as ligações têm segurança habilitada. A única exceção é a <xref:System.ServiceModel.BasicHttpBinding> classe (usando a configuração, o [\<basicHttpBinding>](../../configure-apps/file-schema/wcf/basichttpbinding.md) ).  
  
     A associação selecionada determina o transporte. Por exemplo, <xref:System.ServiceModel.WSHttpBinding> usa http como o transporte; <xref:System.ServiceModel.NetTcpBinding> usa TCP.  
  
2. Selecione um dos modos de segurança para a associação. Observe que a associação selecionada determina as opções de modo disponíveis. Por exemplo, o <xref:System.ServiceModel.WSDualHttpBinding> não permite a segurança de transporte (não é uma opção). Da mesma forma, nem o <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding> nem o <xref:System.ServiceModel.NetNamedPipeBinding> permite a segurança da mensagem.  
  
     Você tem três opções:  
  
    1. `Transport`  
  
         A segurança de transporte depende do mecanismo que a associação que você selecionou usa. Por exemplo, se você estiver usando `WSHttpBinding` , o mecanismo de segurança será protocolo SSL (SSL) (também o mecanismo para o protocolo HTTPS). Em termos gerais, a principal vantagem da segurança de transporte é que ela fornece uma boa taxa de transferência, não importa qual transporte você está usando. No entanto, ele tem duas limitações: a primeira é que o mecanismo de transporte determina o tipo de credencial usado para autenticar um usuário. Essa é uma desvantagem somente se um serviço precisar interoperar com outros serviços que exigem diferentes tipos de credenciais. A segunda é que, como a segurança não é aplicada no nível de mensagem, a segurança é implementada de uma maneira de salto a ponta em vez de ponta a extremidade. Essa última limitação é um problema somente se o caminho da mensagem entre o cliente e o serviço inclui intermediários. Para obter mais informações sobre qual transporte usar, consulte [escolhendo um transporte](choosing-a-transport.md). Para obter mais informações sobre como usar a segurança de transporte, consulte [visão geral de segurança de transporte](transport-security-overview.md).  
  
    2. `Message`  
  
         Segurança de mensagem significa que cada mensagem inclui os cabeçalhos e os dados necessários para manter a mensagem segura. Como a composição dos cabeçalhos varia, você pode incluir qualquer número de credenciais. Isso se torna um fator se você estiver Interoperando com outros serviços que exigem um tipo de credencial específico que um mecanismo de transporte não pode fornecer, ou se a mensagem deve ser usada com mais de um serviço, em que cada serviço exige um tipo de credencial diferente.  
  
         Para obter mais informações, consulte [segurança da mensagem](message-security-in-wcf.md).  
  
    3. `TransportWithMessageCredential`  
  
         Essa escolha usa a camada de transporte para proteger a transferência de mensagens, enquanto cada mensagem inclui as credenciais avançadas de que outros serviços precisam. Isso combina a vantagem de desempenho da segurança de transporte com as valiosas vantagens das credenciais de segurança de mensagem. Isso está disponível com as seguintes associações: <xref:System.ServiceModel.BasicHttpBinding> ,, <xref:System.ServiceModel.WSFederationHttpBinding> <xref:System.ServiceModel.NetPeerTcpBinding> e <xref:System.ServiceModel.WSHttpBinding> .  
  
3. Se você decidir usar a segurança de transporte para HTTP (em outras palavras, HTTPS), também deverá configurar o host com um certificado SSL e habilitar o SSL em uma porta. Para obter mais informações, consulte [segurança de transporte http](http-transport-security.md).  
  
4. Se você estiver usando o <xref:System.ServiceModel.WSHttpBinding> e não precisar estabelecer uma sessão segura, defina a <xref:System.ServiceModel.NonDualMessageSecurityOverHttp.EstablishSecurityContext%2A> propriedade como `false` .  
  
     Uma sessão segura ocorre quando um cliente e um serviço criam um canal usando uma chave simétrica (tanto o cliente quanto o servidor usam a mesma chave para o comprimento de uma conversa, até que a caixa de diálogo seja fechada).  
  
## <a name="setting-the-client-credential-type"></a>Configurando o tipo de credencial do cliente  
 Selecione um tipo de credencial de cliente conforme apropriado. Para obter mais informações, consulte [selecionando um tipo de credencial](selecting-a-credential-type.md). Os seguintes tipos de credencial do cliente estão disponíveis:  
  
- `Windows`  
  
- `Certificate`  
  
- `Digest`  
  
- `Basic`  
  
- `UserName`  
  
- `NTLM`  
  
- `IssuedToken`  
  
 Dependendo de como você define o modo, você deve definir o tipo de credencial. Por exemplo, se você selecionou o `wsHttpBinding` e definiu o modo como "Message", você também pode definir o `clientCredentialType` atributo do elemento Message para um dos seguintes valores:,,, `None` `Windows` `UserName` `Certificate` e `IssuedToken` , conforme mostrado no exemplo de configuração a seguir.  
  
```xml  
<system.serviceModel>  
<bindings>  
  <wsHttpBinding>  
    <binding name="myBinding">  
      <security mode="Message"/>  
      <message clientCredentialType="Windows"/>  
    </binding>
  </wsHttpBinding>
</bindings>  
</system.serviceModel>  
```  
  
 Ou no código:  
  
 [!code-csharp[c_WsHttpService#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_wshttpservice/cs/source.cs#1)]
 [!code-vb[c_WsHttpService#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_wshttpservice/vb/source.vb#1)]  
  
## <a name="setting-service-credential-values"></a>Definindo valores de credenciais de serviço  
 Depois de selecionar um tipo de credencial de cliente, você deve definir as credenciais reais para o serviço e o cliente a serem usados. No serviço, as credenciais são definidas usando a <xref:System.ServiceModel.Description.ServiceCredentials> classe e retornadas pela <xref:System.ServiceModel.ServiceHostBase.Credentials%2A> propriedade da <xref:System.ServiceModel.ServiceHostBase> classe. A associação em uso implica o tipo de credencial de serviço, o modo de segurança escolhido e o tipo de credencial do cliente. O código a seguir define um certificado para uma credencial de serviço.  
  
 [!code-csharp[c_tcpService#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_tcpservice/cs/source.cs#3)]
 [!code-vb[c_tcpService#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_tcpservice/vb/source.vb#3)]  
  
## <a name="setting-client-credential-values"></a>Definindo valores de credenciais de cliente  
 No cliente, defina os valores de credencial do cliente usando a <xref:System.ServiceModel.Description.ClientCredentials> classe e retornados pela <xref:System.ServiceModel.ClientBase%601.ClientCredentials%2A> propriedade da <xref:System.ServiceModel.ClientBase%601> classe. O código a seguir define um certificado como uma credencial em um cliente usando o protocolo TCP.  
  
 [!code-csharp[c_TcpClient#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_tcpclient/cs/source.cs#1)]
 [!code-vb[c_TcpClient#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_tcpclient/vb/source.vb#1)]  
  
## <a name="see-also"></a>Veja também

- [Programação básica do WCF](../basic-wcf-programming.md)
- [Cenários comuns de segurança](common-security-scenarios.md)
