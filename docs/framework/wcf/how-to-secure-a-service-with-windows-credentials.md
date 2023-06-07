---
title: Como proteger um serviço com credenciais Windows
description: Saiba como habilitar a segurança de transporte em um serviço WCF que reside em um domínio do Windows e é chamado por clientes no mesmo domínio.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF, security
ms.assetid: d171b5ca-96ef-47ff-800c-c138023cf76e
ms.openlocfilehash: 8ef164e1475bfd5f047a99426a2bed43a7aa7353
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85244625"
---
# <a name="how-to-secure-a-service-with-windows-credentials"></a>Como proteger um serviço com credenciais Windows

Este tópico mostra como habilitar a segurança de transporte em um serviço de Windows Communication Foundation (WCF) que reside em um domínio do Windows e é chamado por clientes no mesmo domínio. Para obter mais informações sobre esse cenário, consulte [segurança de transporte com autenticação do Windows](./feature-details/transport-security-with-windows-authentication.md). Para um aplicativo de exemplo, consulte o exemplo de [WSHttpBinding](./samples/wshttpbinding.md) .

Este tópico pressupõe que você tenha uma interface de contrato e implementação existentes já definidos e que o adicione a isso. Você também pode modificar um serviço e cliente existentes.

Você pode proteger um serviço com as credenciais do Windows completamente no código. Como alternativa, você pode omitir parte do código usando um arquivo de configuração. Este tópico mostra as duas maneiras. Certifique-se de usar apenas uma das maneiras, não ambas.

Os três primeiros procedimentos mostram como proteger o serviço usando código. O quarto e o quinto procedimento mostram como fazer isso com um arquivo de configuração.

## <a name="using-code"></a>Usando código

O código completo para o serviço e o cliente está na seção de exemplo no final deste tópico.

O primeiro procedimento percorre a criação e a configuração de uma <xref:System.ServiceModel.WSHttpBinding> classe no código. A associação usa o transporte HTTP. A mesma associação é usada no cliente.

#### <a name="to-create-a-wshttpbinding-that-uses-windows-credentials-and-message-security"></a>Para criar uma WSHttpBinding que usa as credenciais do Windows e a segurança da mensagem

1. O código desse procedimento é inserido no início do `Run` método da `Test` classe no código do serviço na seção de exemplo.

2. Criar uma instância da classe <xref:System.ServiceModel.WSHttpBinding>.

3. Defina a <xref:System.ServiceModel.WSHttpSecurity.Mode%2A> propriedade da <xref:System.ServiceModel.WSHttpSecurity> classe como <xref:System.ServiceModel.SecurityMode.Message> .

4. Defina a <xref:System.ServiceModel.MessageSecurityOverHttp.ClientCredentialType%2A> propriedade da <xref:System.ServiceModel.MessageSecurityOverHttp> classe como <xref:System.ServiceModel.MessageCredentialType.Windows> .

5. O código para esse procedimento é o seguinte:

    [!code-csharp[c_SecureWindowsService#1](../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewindowsservice/cs/secureservice.cs#1)]
    [!code-vb[c_SecureWindowsService#1](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewindowsservice/vb/secureservice.vb#1)]

### <a name="using-the-binding-in-a-service"></a>Usando a associação em um serviço

Este é o segundo procedimento, que mostra como usar a associação em um serviço hospedado automaticamente. Para obter mais informações sobre serviços de hospedagem, consulte [serviços de hospedagem](hosting-services.md).

##### <a name="to-use-a-binding-in-a-service"></a>Para usar uma associação em um serviço

1. Insira o código deste procedimento após o código do procedimento anterior.

2. Crie uma <xref:System.Type> variável chamada `contractType` e atribua-a ao tipo da interface ( `ICalculator` ). Ao usar Visual Basic, use o `GetType` operador; ao usar o C#, use a `typeof` palavra-chave.

3. Crie uma segunda <xref:System.Type> variável chamada `serviceType` e atribua a ela o tipo do contrato implementado ( `Calculator` ).

4. Crie uma instância da <xref:System.Uri> classe chamada `baseAddress` com o endereço base do serviço. O endereço base deve ter um esquema que corresponda ao transporte. Nesse caso, o esquema de transporte é HTTP e o endereço inclui o Uniform Resource Identifier especial (URI) "localhost" e um número de porta (8036), bem como um endereço de ponto de extremidade base ("serviceModelSamples/): `http://localhost:8036/serviceModelSamples/` .

5. Crie uma instância da <xref:System.ServiceModel.ServiceHost> classe com as `serviceType` variáveis e `baseAddress` .

6. Adicione um ponto de extremidade ao serviço usando o `contractType` , a associação e um nome de ponto de extremidade (secureCalculator). Um cliente deve concatenar o endereço base e o nome do ponto de extremidade ao iniciar uma chamada para o serviço.

7. Chame o método <xref:System.ServiceModel.Channels.CommunicationObject.Open%2A> para iniciar o serviço. O código para esse procedimento é mostrado aqui:

    [!code-csharp[c_SecureWindowsService#2](../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewindowsservice/cs/secureservice.cs#2)]
    [!code-vb[c_SecureWindowsService#2](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewindowsservice/vb/secureservice.vb#2)]

### <a name="using-the-binding-in-a-client"></a>Usando a associação em um cliente

Este procedimento mostra como gerar um proxy que se comunica com o serviço. O proxy é gerado com a [ferramenta de utilitário de metadados ServiceModel (Svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md) que usa os metadados de serviço para criar o proxy.

Esse procedimento também cria uma instância da <xref:System.ServiceModel.WSHttpBinding> classe para se comunicar com o serviço e, em seguida, chama o serviço.

Este exemplo usa apenas o código para criar o cliente. Como alternativa, você pode usar um arquivo de configuração, que é mostrado na seção após este procedimento.

#### <a name="to-use-a-binding-in-a-client-with-code"></a>Para usar uma associação em um cliente com código

1. Use a ferramenta SvcUtil.exe para gerar o código de proxy dos metadados do serviço. Para obter mais informações, consulte [como: criar um cliente](how-to-create-a-wcf-client.md). O código de proxy gerado herda da <xref:System.ServiceModel.ClientBase%601> classe, o que garante que cada cliente tenha os construtores, métodos e propriedades necessários para se comunicar com um serviço WCF. Neste exemplo, o código gerado inclui a `CalculatorClient` classe, que implementa a `ICalculator` interface, permitindo a compatibilidade com o código do serviço.

2. O código deste procedimento é inserido no início do `Main` método do programa cliente.

3. Crie uma instância da <xref:System.ServiceModel.WSHttpBinding> classe e defina seu modo de segurança como `Message` e seu tipo de credencial de cliente como `Windows` . O exemplo nomeia a variável `clientBinding` .

4. Crie uma instância da <xref:System.ServiceModel.EndpointAddress> classe denominada `serviceAddress` . Inicialize a instância com o endereço base concatenado com o nome do ponto de extremidade.

5. Crie uma instância da classe de cliente gerada com as `serviceAddress` variáveis e `clientBinding` .

6. Chame o <xref:System.ServiceModel.ClientBase%601.Open%2A> método, conforme mostrado no código a seguir.

7. Chame o serviço e exiba os resultados.

    [!code-csharp[c_secureWindowsClient#1](../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewindowsclient/cs/secureclient.cs#1)]
    [!code-vb[c_secureWindowsClient#1](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewindowsclient/vb/secureclient.vb#1)]

## <a name="using-the-configuration-file"></a>Usando o arquivo de configuração

Em vez de criar a associação com código de procedimento, você pode usar o código a seguir mostrado para a seção de associações do arquivo de configuração.

Se você ainda não tiver um serviço definido, consulte [projetando e implementando serviços](designing-and-implementing-services.md)e [Configurando serviços](configuring-services.md).

> [!NOTE]
> Esse código de configuração é usado nos arquivos de configuração do serviço e do cliente.

#### <a name="to-enable-transfer-security-on-a-service-in-a-windows-domain-using-configuration"></a>Para habilitar a segurança de transferência em um serviço em um domínio do Windows usando a configuração

1. Adicione um [\<wsHttpBinding>](../configure-apps/file-schema/wcf/wshttpbinding.md) elemento à [\<bindings>](../configure-apps/file-schema/wcf/bindings.md) seção element do arquivo de configuração.

2. Adicione um `binding` elemento <> ao `WSHttpBinding` elemento> <e defina o `configurationName` atributo como um valor apropriado para seu aplicativo.

3. Adicione um `security` elemento <> e defina o `mode` atributo como Message.

4. Adicione um `message` elemento <> e defina o `clientCredentialType` atributo como Windows.

5. No arquivo de configuração do serviço, substitua a `<bindings>` seção pelo código a seguir. Se você ainda não tiver um arquivo de configuração de serviço, consulte [usando associações para configurar serviços e clientes](using-bindings-to-configure-services-and-clients.md).

    ```xml
    <bindings>
      <wsHttpBinding>
       <binding name = "wsHttpBinding_Calculator">
         <security mode="Message">
           <message clientCredentialType="Windows"/>
         </security>
        </binding>
      </wsHttpBinding>
    </bindings>
    ```

### <a name="using-the-binding-in-a-client"></a>Usando a associação em um cliente

Este procedimento mostra como gerar dois arquivos: um proxy que se comunica com o serviço e um arquivo de configuração. Ele também descreve as alterações no programa cliente, que é o terceiro arquivo usado no cliente.

#### <a name="to-use-a-binding-in-a-client-with-configuration"></a>Para usar uma associação em um cliente com configuração

1. Use a ferramenta SvcUtil.exe para gerar o código de proxy e o arquivo de configuração dos metadados do serviço. Para obter mais informações, consulte [como: criar um cliente](how-to-create-a-wcf-client.md).

2. Substitua a [\<bindings>](../configure-apps/file-schema/wcf/bindings.md) seção do arquivo de configuração gerado pelo código de configuração da seção anterior.

3. O código de procedimento é inserido no início do `Main` método do programa cliente.

4. Crie uma instância da classe de cliente gerada passando o nome da associação no arquivo de configuração como um parâmetro de entrada.

5. Chame o <xref:System.ServiceModel.ClientBase%601.Open%2A> método, conforme mostrado no código a seguir.

6. Chame o serviço e exiba os resultados.

    [!code-csharp[c_secureWindowsClient#2](../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewindowsclient/cs/secureclient.cs#2)]

## <a name="example"></a>Exemplo

[!code-csharp[c_SecureWindowsService#0](../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewindowsservice/cs/secureservice.cs#0)]

[!code-csharp[c_SecureWindowsClient#0](../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewindowsclient/cs/secureclient.cs#0)]
[!code-vb[c_SecureWindowsClient#0](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewindowsclient/vb/secureclient.vb#0)]

## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.WSHttpBinding>
- [Ferramenta de utilitário de metadados ServiceModel (Svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md)
- [Como criar um cliente](how-to-create-a-wcf-client.md)
- [Protegendo serviços](securing-services.md)
- [Visão geral de segurança](./feature-details/security-overview.md)
