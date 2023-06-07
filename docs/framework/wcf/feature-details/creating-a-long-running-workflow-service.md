---
title: Criando um serviço de fluxo de trabalho de execução longa
ms.date: 03/30/2017
ms.assetid: 4c39bd04-5b8a-4562-a343-2c63c2821345
ms.openlocfilehash: 4ae01201230bf848c045158424db60097d8dd767
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599341"
---
# <a name="create-a-long-running-workflow-service"></a>Criar um serviço de fluxo de trabalho de execução longa

Este tópico descreve como criar um serviço de fluxo de trabalho de execução longa. Os serviços de fluxo de trabalho de longa execução podem ser executados por longos períodos de tempo. Em algum momento, o fluxo de trabalho pode ficar ocioso aguardando algumas informações adicionais. Quando isso ocorre, o fluxo de trabalho é persistido em um banco de dados SQL e removido da memória. Quando as informações adicionais ficarem disponíveis, a instância do fluxo de trabalho será carregada de volta na memória e continuará em execução.  Nesse cenário, você está implementando um sistema de pedidos muito simplificado.  O cliente envia uma mensagem inicial para o serviço de fluxo de trabalho para iniciar a ordem. Ele retorna uma ID do pedido para o cliente. Neste ponto, o serviço de fluxo de trabalho está aguardando outra mensagem do cliente e entra no estado ocioso e é persistido em um banco de dados SQL Server.  Quando o cliente envia a próxima mensagem para solicitar um item, o serviço de fluxo de trabalho é carregado de volta na memória e conclui o processamento do pedido. No exemplo de código, ele retorna uma cadeia de caracteres que informa que o item foi adicionado à ordem. O exemplo de código não deve ser um aplicativo do mundo real da tecnologia, mas sim um exemplo simples que ilustra os serviços de fluxo de trabalho de longa execução. Este tópico pressupõe que você saiba como criar projetos e soluções do Visual Studio 2012.

## <a name="prerequisites"></a>Pré-requisitos

Você deve ter o seguinte software instalado para usar este passo a passos:

1. Microsoft SQL Server 2008

2. Visual Studio 2012

3. O[!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)]

4. Você está familiarizado com o WCF e o Visual Studio 2012 e sabe como criar projetos/soluções.

## <a name="set-up-the-sql-database"></a>Configurar o banco de dados SQL

1. Para que as instâncias do serviço de fluxo de trabalho sejam persistidas, você deve ter Microsoft SQL Server instalado e deve configurar um banco de dados para armazenar as instâncias de fluxo de trabalho persistentes. Execute o Microsoft SQL Management Studio clicando no botão **Iniciar** , selecionando **todos os programas**, **Microsoft SQL Server 2008**e **Microsoft SQL Management Studio**.

2. Clique no botão **conectar** para fazer logon na instância de SQL Server

3. Clique com o botão direito do mouse em **bancos** de dados no modo de exibição de árvore e selecione **novo Database..** para criar um novo banco de dados chamado `SQLPersistenceStore` .

4. Execute o arquivo de script SqlWorkflowInstanceStoreSchema. SQL localizado no diretório C:\Windows\Microsoft.NET\Framework\v4.0\SQL\en no banco de dados SQLPersistenceStore para configurar os esquemas de banco de dados necessários.

5. Execute o arquivo de script SqlWorkflowInstanceStoreLogic. SQL localizado no diretório C:\Windows\Microsoft.NET\Framework\v4.0\SQL\en no banco de dados SQLPersistenceStore para configurar a lógica de banco de dados necessária.

## <a name="create-the-web-hosted-workflow-service"></a>Criar o serviço de fluxo de trabalho hospedado na Web

1. Crie uma solução vazia do Visual Studio 2012, nomeie-a `OrderProcessing` .

2. Adicione um novo projeto de aplicativo do serviço de fluxo de trabalho WCF chamado `OrderService` à solução.

3. Na caixa de diálogo Propriedades do projeto, selecione a guia **Web** .

    1. Em **Iniciar ação** , selecione **página específica** e especifique `Service1.xamlx` .

        ![Propriedades de Web de projeto de serviço de fluxo de trabalho](./media/creating-a-long-running-workflow-service/start-action-specific-page-option.png "Criar a opção de página específica do serviço de fluxo de trabalho hospedado na Web")

    2. Em **servidores** , selecione **usar servidor Web local do IIS**.

        ![Configurações locais de servidor Web](./media/creating-a-long-running-workflow-service/use-local-web-server.png "Criar o serviço de fluxo de trabalho hospedado da Web-usar a opção de servidor Web IIS local")

        > [!WARNING]
        > Você deve executar o Visual Studio 2012 no modo de administrador para fazer essa configuração.

        Essas duas etapas configuram o projeto de serviço de fluxo de trabalho a ser hospedado pelo IIS.

4. Abra `Service1.xamlx` se ele ainda não estiver aberto e exclua as atividades **ReceiveRequest** e **SendResponse** existentes.

5. Selecione a atividade de **serviço sequencial** e clique no link **variáveis** e adicione as variáveis mostradas na ilustração a seguir. Isso adiciona algumas variáveis que serão usadas posteriormente no serviço de fluxo de trabalho.

    > [!NOTE]
    > Se CorrelationHandle não estiver na lista suspensa tipo de variável, selecione **procurar tipos** na lista suspensa. Digite CorrelationHandle na caixa **nome do tipo** , selecione CorrelationHandle na ListBox e clique em **OK**.

    ![Adicionar variáveis](./media/creating-a-long-running-workflow-service/add-variables-sequential-service-activity.gif "Adicione variáveis à atividade de serviço sequencial.")

6. Arraste e solte um modelo de atividade **ReceiveAndSendReply** na atividade de **serviço sequencial** . Esse conjunto de atividades receberá uma mensagem de um cliente e enviará uma resposta de volta.

    1. Selecione a atividade **receber** e defina as propriedades realçadas na ilustração a seguir.

        ![Definir propriedades de atividade de recebimento](./media/creating-a-long-running-workflow-service/set-receive-activity-properties.png "Defina as propriedades da atividade Receive.")

        A propriedade DisplayName define o nome exibido para a atividade Receive no designer. As propriedades ServiceContractName e OperationName especificam o nome do contrato de serviço e a operação que são implementados pela atividade Receive. Para obter mais informações sobre como os contratos são usados nos serviços de fluxo de trabalho, consulte [usando contratos no fluxo de trabalho](using-contracts-in-workflow.md).

    2. Clique no link **definir...** na atividade **ReceiveStartOrder** e defina as propriedades mostradas na ilustração a seguir.  Observe que o botão de opção **parâmetros** está selecionado, um parâmetro chamado `p_customerName` está associado à `customerName` variável. Isso configura a atividade **Receive** para receber alguns dados e associar esses dados a variáveis locais.

        ![Definindo os dados recebidos pela atividade de recebimento](./media/creating-a-long-running-workflow-service/set-properties-for-receive-content.png "Defina as propriedades dos dados recebidos pela atividade Receive.")

    3. Selecione a atividade **SendReplyToReceive** e defina a propriedade realçada mostrada na ilustração a seguir.

        ![Definindo as propriedades da atividade SendReply](./media/creating-a-long-running-workflow-service/set-properties-for-reply-activities.png "Setresponderproperties")

    4. Clique no link **definir...** na atividade **SendReplyToStartOrder** e defina as propriedades mostradas na ilustração a seguir. Observe que o botão de opção **parâmetros** está selecionado; um parâmetro chamado `p_orderId` está associado à `orderId` variável. Essa configuração especifica que a atividade SendReplyToStartOrder retornará um valor do tipo cadeia de caracteres para o chamador.

        ![Configurando os dados de conteúdo da atividade de SendReply](./media/creating-a-long-running-workflow-service/setreplycontent-for-sendreplytostartorder-activity.png "Defina a configuração para a atividade SetReplyToStartOrder.")

    5. Arraste e solte uma atividade atribuir entre as atividades **Receive** e **SendReply** e defina as propriedades conforme mostrado na ilustração a seguir:

        ![Adicionando uma atividade de atribuição](./media/creating-a-long-running-workflow-service/add-an-assign-activity.png "Adicionar uma atividade atribuir.")

        Isso cria uma nova ID de pedido e coloca o valor na variável orderId.

    6. Selecione a atividade **ReplyToStartOrder** . Na janela Propriedades, clique no botão de reticências de **CorrelationInitializers**. Selecione o link **Adicionar inicializador** , digite `orderIdHandle` na caixa de texto inicializador, selecione inicializador de correlação de consulta para o tipo de correlação e selecione p_orderId na caixa suspensa consultas XPath. Essas configurações são mostradas na ilustração a seguir. Clique em **OK**.  Isso Inicializa uma correlação entre o cliente e essa instância do serviço de fluxo de trabalho. Quando uma mensagem que contém essa ID de pedido é recebida, ela é roteada para essa instância do serviço de fluxo de trabalho.

        ![Adicionando um inicializador de correlação](./media/creating-a-long-running-workflow-service/add-correlationinitializers.png "Adicione um inicializador de correlação.")

7. Arraste e solte outra atividade **ReceiveAndSendReply** para o final do fluxo de trabalho (fora da **sequência** que contém as primeiras atividades **Receive** e **SendReply** ). Isso receberá a segunda mensagem enviada pelo cliente e responderá a ela.

    1. Selecione a **sequência** que contém as atividades **Receive** e **SendReply** adicionadas recentemente e clique no botão **variáveis** . Adicione a variável realçada na ilustração a seguir:

        ![Adicionando novas variáveis](./media/creating-a-long-running-workflow-service/add-the-itemid-variable.png "Adicione a variável ItemId.")

        Além disso `orderResult` , adicione como **cadeia de caracteres** no `Sequence` escopo.

    2. Selecione a atividade **receber** e defina as propriedades mostradas na ilustração a seguir:

        ![Definir as propriedades da atividade Receive](./media/creating-a-long-running-workflow-service/set-receive-activities-properties.png "Defina as propriedades de atividades de recebimento.")

        > [!NOTE]
        > Não se esqueça de alterar o campo **ServiceContractName** com `../IAddItem` .

    3. Clique no link **definir...** na atividade **ReceiveAddItem** e adicione os parâmetros mostrados na ilustração a seguir: isso configura a atividade Receive para aceitar dois parâmetros, a ID do pedido e a ID do item que está sendo ordenado.

        ![Especificando parâmetros para o segundo recebimento](./media/creating-a-long-running-workflow-service/add-receive-two-parameters.png "Configure a atividade Receive para receber dois parâmetros.")

    4. Clique no botão de reticências **CorrelateOn** e digite `orderIdHandle` . Em **consultas XPath**, clique na seta suspensa e selecione `p_orderId` . Isso configura a correlação na segunda atividade de recebimento. Para obter mais informações sobre correlação, consulte [correlação](correlation.md).

        ![Definindo a propriedade CorrelatesOn](./media/creating-a-long-running-workflow-service/correlateson-setting.png "Defina a propriedade CorrelatesOn.")

    5. Arraste e solte uma atividade **If** imediatamente após a atividade **ReceiveAddItem** . Essa atividade funciona exatamente como uma instrução If.

        1. Defina a propriedade **condição** como`itemId=="Zune HD" (itemId="Zune HD" for Visual Basic)`

        2. Arraste e solte uma atividade **assign** na seção **then,** e outra na seção **else** define as propriedades de **atribuir** atividades, conforme mostrado na ilustração a seguir.

            ![Atribuindo o resultado da chamada de serviço](./media/creating-a-long-running-workflow-service/assign-result-of-service-call.png "Atribua o resultado da chamada de serviço.")

            Se a condição for `true` a seção **then** será executada. Se a condição for `false` a seção **else** é executada.

        3. Selecione a atividade **SendReplyToReceive** e defina a propriedade **DisplayName** mostrada na ilustração a seguir.

            ![Definindo as propriedades da atividade de SendReply](./media/creating-a-long-running-workflow-service/send-reply-activity-property.png "Defina a propriedade de atividade SendReply.")

        4. Clique no link **definir...** na atividade **SetReplyToAddItem** e configure-o conforme mostrado na ilustração a seguir. Isso configura a atividade **SendReplyToAddItem** para retornar o valor na `orderResult` variável.

            ![Definindo a associação de dados para a atividade SendReply](./media/creating-a-long-running-workflow-service/set-property-for-sendreplytoadditem.gif "Defina a propriedade para a atividade SendReplyToAddItem.")

8. Abra o arquivo Web. config e adicione os seguintes elementos na \<behavior> seção para habilitar a persistência do fluxo de trabalho.

    ```xml
    <sqlWorkflowInstanceStore connectionString="Data Source=your-machine\SQLExpress;Initial Catalog=SQLPersistenceStore;Integrated Security=True;Asynchronous Processing=True" instanceEncodingOption="None" instanceCompletionAction="DeleteAll" instanceLockedExceptionAction="BasicRetry" hostLockRenewalPeriod="00:00:30" runnableInstancesDetectionPeriod="00:00:02" />
              <workflowIdle timeToUnload="0"/>
    ```

    > [!WARNING]
    > Certifique-se de substituir o host e o nome da instância do SQL Server no trecho de código anterior.

9. Compile a solução.

## <a name="create-a-client-application-to-call-the-workflow-service"></a>Criar um aplicativo cliente para chamar o serviço de fluxo de trabalho

1. Adicione um novo projeto de aplicativo de console chamado `OrderClient` à solução.

2. Adicionar referências aos seguintes assemblies ao `OrderClient` projeto

    1. System.ServiceModel.dll

    2. System.ServiceModel.Activities.dll

3. Adicione uma referência de serviço ao serviço de fluxo de trabalho e especifique `OrderService` como o namespace.

4. No `Main()` método do projeto do cliente, adicione o seguinte código:

    ```csharp
    static void Main(string[] args)
    {
       // Send initial message to start the workflow service
       Console.WriteLine("Sending start message");
       StartOrderClient startProxy = new StartOrderClient();
       string orderId = startProxy.StartOrder("Kim Abercrombie");

       // The workflow service is now waiting for the second message to be sent
       Console.WriteLine("Workflow service is idle...");
       Console.WriteLine("Press [ENTER] to send an add item message to reactivate the workflow service...");
       Console.ReadLine();

       // Send the second message
       Console.WriteLine("Sending add item message");
       AddItemClient addProxy = new AddItemClient();
       AddItem item = new AddItem();
       item.p_itemId = "Zune HD";
       item.p_orderId = orderId;

       string orderResult = addProxy.AddItem(item);
       Console.WriteLine("Service returned: " + orderResult);
    }
    ```

5. Compile a solução e execute o `OrderClient` aplicativo. O cliente exibirá o seguinte texto:

    ```output
    Sending start messageWorkflow service is idle...Press [ENTER] to send an add item message to reactivate the workflow service...
    ```

6. Para verificar se o serviço de fluxo de trabalho foi persistido, inicie o SQL Server Management Studio acessando o menu **Iniciar** , selecionando **todos os programas**, **Microsoft SQL Server 2008**, **SQL Server Management Studio**.

    1. No painel esquerdo, expanda, **bancos de dados**, **SQLPersistenceStore**, **exibições** e clique com o botão direito do mouse em **System. Activities. DurableInstancing. instances** e selecione **selecionar as primeiras 1000 linhas**. No painel de **resultados** , verifique se você vê pelo menos uma instância listada. Pode haver outras instâncias de execuções anteriores se uma exceção ocorreu durante a execução. Você pode excluir as linhas existentes clicando com o botão direito do mouse em **System. Activities. DurableInstancing. Instances** e selecionando **editar as primeiras 200 linhas**, pressionando o botão **executar** , selecionando todas as linhas no painel de resultados e selecionando **excluir**.  Para verificar se a instância exibida no banco de dados é a instância que seu aplicativo criou, verifique se a exibição de instâncias está vazia antes de executar o cliente. Depois que o cliente estiver executando a execução da consulta novamente (selecione as primeiras 1000 linhas) e verifique se uma nova instância foi adicionada.

7. Pressione Enter para enviar a mensagem de adição de item ao serviço de fluxo de trabalho. O cliente exibirá o seguinte texto:

    ```output
    Sending add item messageService returned: Item added to orderPress any key to continue . . .
    ```

## <a name="see-also"></a>Consulte também

- [Serviços de fluxo de trabalho](workflow-services.md)
