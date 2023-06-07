---
title: Como hospedar um serviço do WCF em um aplicativo gerenciado
description: Saiba como hospedar um serviço WCF dentro de um aplicativo gerenciado criando um serviço auto-hospedado e testando-o.
ms.date: 09/17/2018
dev_langs:
- csharp
- vb
ms.assetid: 5eb29db0-b6dc-4e77-8c68-0a62f79d743b
ms.openlocfilehash: 7d1d61b683f60a6c643d2a2f03d367a6ae6c6c15
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246162"
---
# <a name="how-to-host-a-wcf-service-in-a-managed-app"></a>Como hospedar um serviço WCF em um aplicativo gerenciado

Para hospedar um serviço dentro de um aplicativo gerenciado, insira o código para o serviço dentro do código do aplicativo gerenciado, defina um ponto de extremidade para o serviço de forma imperativa no código, declarativamente por meio da configuração ou usando pontos de extremidades padrão e, em seguida, crie uma instância do <xref:System.ServiceModel.ServiceHost> .

Para começar a receber mensagens, ligue para <xref:System.ServiceModel.ICommunicationObject.Open%2A> <xref:System.ServiceModel.ServiceHost> . Isso cria e abre o ouvinte para o serviço. Hospedar um serviço dessa maneira geralmente é chamado de "hospedagem interna" porque o aplicativo gerenciado está fazendo o próprio trabalho de hospedagem. Para fechar o serviço, ligue <xref:System.ServiceModel.Channels.CommunicationObject.Close%2A?displayProperty=nameWithType> para <xref:System.ServiceModel.ServiceHost> .

Um serviço também pode ser hospedado em um serviço gerenciado do Windows, no Serviços de Informações da Internet (IIS) ou no WAS (serviço de ativação de processos do Windows). Para obter mais informações sobre opções de hospedagem para um serviço, consulte [serviços de hospedagem](hosting-services.md).

Hospedar um serviço em um aplicativo gerenciado é a opção mais flexível porque requer a infraestrutura menos implantada. Para obter mais informações sobre como hospedar serviços em aplicativos gerenciados, consulte [hospedagem em um aplicativo gerenciado](./feature-details/hosting-in-a-managed-application.md).

O procedimento a seguir demonstra como implementar um serviço hospedado automaticamente em um aplicativo de console.

## <a name="create-a-self-hosted-service"></a>Criar um serviço auto-hospedado

1. Criar um novo aplicativo de console:

   1. Abra o Visual Studio e selecione **novo**  >  **projeto** no menu **arquivo** .

   2. Na lista **modelos instalados** , selecione **Visual C#** ou **Visual Basic**e, em seguida, selecione área de **trabalho do Windows**.

   3. Selecione o modelo de **aplicativo de console** . Digite `SelfHost` a caixa **nome** e escolha **OK**.

2. Clique com o botão direito do mouse em **SelfHost** em **Gerenciador de soluções** e selecione **Adicionar referência**. Selecione **System. ServiceModel** na guia **.net** e, em seguida, escolha **OK**.

    > [!TIP]
    > Se a janela de **Gerenciador de soluções** não estiver visível, selecione **Gerenciador de soluções** no menu **Exibir** .

3. Clique duas vezes em **Program.cs** ou **Module1. vb** em **Gerenciador de soluções** para abri-lo na janela de código, se ainda não estiver aberto. Adicione as seguintes instruções na parte superior do arquivo:

     [!code-csharp[CFX_SelfHost4#1](../../../samples/snippets/csharp/VS_Snippets_CFX/cfx_selfhost4/cs/program.cs#1)]
     [!code-vb[CFX_SelfHost4#1](../../../samples/snippets/visualbasic/VS_Snippets_CFX/cfx_selfhost4/vb/module1.vb#1)]

4. Defina e implemente um contrato de serviço. Este exemplo define um `HelloWorldService` que retorna uma mensagem com base na entrada para o serviço.

     [!code-csharp[CFX_SelfHost4#2](../../../samples/snippets/csharp/VS_Snippets_CFX/cfx_selfhost4/cs/program.cs#2)]
     [!code-vb[CFX_SelfHost4#2](../../../samples/snippets/visualbasic/VS_Snippets_CFX/cfx_selfhost4/vb/module1.vb#2)]

    > [!NOTE]
    > Para obter mais informações sobre como definir e implementar uma interface de serviço, consulte [como: definir um contrato de serviço](how-to-define-a-wcf-service-contract.md) e [como implementar um contrato de serviço](how-to-implement-a-wcf-contract.md).

5. Na parte superior do `Main` método, crie uma instância da <xref:System.Uri> classe com o endereço base para o serviço.

     [!code-csharp[CFX_SelfHost4#3](../../../samples/snippets/csharp/VS_Snippets_CFX/cfx_selfhost4/cs/program.cs#3)]
     [!code-vb[CFX_SelfHost4#3](../../../samples/snippets/visualbasic/VS_Snippets_CFX/cfx_selfhost4/vb/module1.vb#3)]

6. Crie uma instância da <xref:System.ServiceModel.ServiceHost> classe, passando uma <xref:System.Type> que represente o tipo de serviço e o endereço de base Uniform Resource Identifier (URI) para o <xref:System.ServiceModel.ServiceHost.%23ctor%28System.Type%2CSystem.Uri%5B%5D%29> . Habilite a publicação de metadados e, em seguida, chame o <xref:System.ServiceModel.ICommunicationObject.Open%2A> método no <xref:System.ServiceModel.ServiceHost> para inicializar o serviço e prepará-lo para receber mensagens.

     [!code-csharp[CFX_SelfHost4#4](../../../samples/snippets/csharp/VS_Snippets_CFX/cfx_selfhost4/cs/program.cs#4)]
     [!code-vb[CFX_SelfHost4#4](../../../samples/snippets/visualbasic/VS_Snippets_CFX/cfx_selfhost4/vb/module1.vb#4)]

    > [!NOTE]
    > Este exemplo usa pontos de extremidade padrão e nenhum arquivo de configuração é necessário para esse serviço. Se nenhum ponto de extremidade estiver configurado, o tempo de execução criará um ponto de extremidade para cada endereço base para cada contrato de serviço implementado pelo serviço. Para obter mais informações sobre pontos de extremidade padrão, consulte [configuração simplificada](simplified-configuration.md) e [configuração simplificada para serviços WCF](./samples/simplified-configuration-for-wcf-services.md).

7. Pressione **Ctrl** + **Shift** + **B** para criar a solução.

## <a name="test-the-service"></a>Teste o serviço

1. Pressione **Ctrl** + **F5** para executar o serviço.

2. Abra o **cliente de teste do WCF**.

    > [!TIP]
    > Para abrir o **cliente de teste do WCF**, abra prompt de comando do desenvolvedor para Visual Studio e execute **WcfTestClient.exe**.

3. Selecione **Adicionar serviço** no menu **arquivo** .

4. Digite `http://localhost:8080/hello` na caixa de endereço e clique em **OK**.

    > [!TIP]
    > Verifique se o serviço está em execução ou se essa etapa falha. Se você alterou o endereço base no código, use o endereço base modificado nesta etapa.

5. Clique duas vezes em **SayHello** no nó **meus projetos de serviço** . Digite seu nome na coluna **valor** na lista **solicitação** e clique em **invocar**.

   Uma mensagem de resposta é exibida na lista de **respostas** .

## <a name="example"></a>Exemplo

O exemplo a seguir cria um <xref:System.ServiceModel.ServiceHost> objeto para hospedar um serviço do tipo `HelloWorldService` e, em seguida, chama o <xref:System.ServiceModel.ICommunicationObject.Open%2A> método em <xref:System.ServiceModel.ServiceHost> . Um endereço base é fornecido no código, A publicação de metadados está habilitada e os pontos de extremidade padrão são usados.

[!code-csharp[CFX_SelfHost4#5](../../../samples/snippets/csharp/VS_Snippets_CFX/cfx_selfhost4/cs/program.cs#5)]
[!code-vb[CFX_SelfHost4#5](../../../samples/snippets/visualbasic/VS_Snippets_CFX/cfx_selfhost4/vb/module1.vb#5)]

## <a name="see-also"></a>Veja também

- <xref:System.Uri>
- <xref:System.Configuration.ConfigurationManager.AppSettings%2A>
- <xref:System.Configuration.ConfigurationManager>
- [Como hospedar um serviço WCF no IIS](./feature-details/how-to-host-a-wcf-service-in-iis.md)
- [Self-Host](./samples/self-host.md)
- [Hospedando serviços](hosting-services.md)
- [Como definir um contrato de serviço](how-to-define-a-wcf-service-contract.md)
- [Como implementar um contrato de serviço](how-to-implement-a-wcf-contract.md)
- [Ferramenta de utilitário de metadados ServiceModel (Svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md)
- [Usando associações para configurar serviços e clientes](using-bindings-to-configure-services-and-clients.md)
- [Associações fornecidas pelo sistema](system-provided-bindings.md)
