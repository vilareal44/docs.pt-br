---
title: Como criar um serviço de fluxo de trabalho com atividades de mensagens
ms.date: 03/30/2017
ms.assetid: 53d094e2-6901-4aa1-88b8-024b27ccf78b
ms.openlocfilehash: b4991fc9f8a6c45cae3943f1506247c42ed2b30b
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597118"
---
# <a name="how-to-create-a-workflow-service-with-messaging-activities"></a>Como criar um serviço de fluxo de trabalho com atividades de mensagens
Este tópico descreve como criar um serviço de fluxo de trabalho simples usando atividades de mensagens. Este tópico se concentra na mecânica de criação de um serviço de fluxo de trabalho em que o serviço consiste exclusivamente em atividades de mensagens. Em um serviço do mundo real, o fluxo de trabalho contém muitas outras atividades. O serviço implementa uma operação chamada Echo, que usa uma cadeia de caracteres e retorna a cadeia de caracteres para o chamador. Este tópico é o primeiro de uma série de dois tópicos. O próximo tópico [como: acessar um serviço de um aplicativo de fluxo de trabalho](how-to-access-a-service-from-a-workflow-application.md) discute como criar um aplicativo de fluxo de trabalho que pode chamar o serviço criado neste tópico.  
  
### <a name="to-create-a-workflow-service-project"></a>Para criar um projeto de serviço de fluxo de trabalho  
  
1. Inicie o Visual Studio 2012.  
  
2. Clique no menu **arquivo** , selecione **novo**e, em seguida, **projeto** para exibir a **caixa de diálogo novo projeto**. Selecione **fluxo de trabalho** na lista de modelos instalados e **aplicativo de serviço de fluxo de trabalho WCF** na lista de tipos de projeto. Nomeie o projeto `MyWFService` e use o local padrão, conforme mostrado na ilustração a seguir.  
  
     Clique no botão **OK** para ignorar a **caixa de diálogo novo projeto**.  
  
3. Quando o projeto é criado, o arquivo Service1. xamlx é aberto no designer, conforme mostrado na ilustração a seguir.  
  
     ![Captura de tela mostra o arquivo de Service1. xamlx aberto no designer.](./media/how-to-create-a-workflow-service-with-messaging-activities/default-workflow-service.jpg)  
  
     Clique com o botão direito do mouse na atividade rotulada **serviço sequencial** e selecione **excluir**.  
  
### <a name="to-implement-the-workflow-service"></a>Para implementar o serviço de fluxo de trabalho  
  
1. Selecione a guia **caixa de ferramentas** no lado esquerdo da tela para exibir a caixa de ferramentas e clique na anotação para manter a janela aberta. Expanda a seção **mensagens** da caixa de ferramentas para exibir as atividades de mensagens e os modelos de atividade de mensagens, conforme mostrado na ilustração a seguir.  
  
     ![Captura de tela que mostra a caixa de ferramentas com a seção de mensagens expandida.](./media/how-to-create-a-workflow-service-with-messaging-activities/toolbox-messaging-section.jpg)  
  
2. Arraste e solte um modelo **ReceiveAndSendReply** para o designer de fluxo de trabalho. Isso cria uma <xref:System.Activities.Statements.Sequence> atividade com uma atividade **Receive** seguida por uma <xref:System.ServiceModel.Activities.SendReply> atividade, conforme mostrado na ilustração a seguir.  
  
     ![Captura de tela que mostra o modelo ReceiveAndSendReply.](./media/how-to-create-a-workflow-service-with-messaging-activities/receiveandsendreply-template.jpg)  
  
     Observe que a <xref:System.ServiceModel.Activities.SendReply> propriedade da atividade <xref:System.ServiceModel.Activities.SendReply.Request%2A> está definida como `Receive` , o nome da <xref:System.ServiceModel.Activities.Receive> atividade à qual a <xref:System.ServiceModel.Activities.SendReply> atividade está respondendo.  
  
3. No <xref:System.ServiceModel.Activities.Receive> tipo de atividade na `Echo` caixa de texto rotulada como **OperationName**. Isso define o nome da operação que o serviço implementa.  
  
     ![Captura de tela que mostra onde especificar o nome da operação.](./media/how-to-create-a-workflow-service-with-messaging-activities/define-operation-name.jpg)  
  
4. Com a <xref:System.ServiceModel.Activities.Receive> atividade selecionada, abra a janela Propriedades se ainda não estiver aberta clicando no **menu Exibir** e selecionando **janela Propriedades**. Na **janela Propriedades** , role para baixo até ver **CanCreateInstance** e clique na caixa de seleção, conforme mostrado na ilustração a seguir. Essa configuração permite que o host do serviço de fluxo de trabalho crie uma nova instância do serviço (se necessário) quando uma mensagem é recebida.  
  
     ![Captura de tela que mostra a propriedade CanCreateInstance.](./media/how-to-create-a-workflow-service-with-messaging-activities/cancreateinstance-property.jpg)  
  
5. Selecione a <xref:System.Activities.Statements.Sequence> atividade e clique no botão **variáveis** no canto inferior esquerdo do designer. Isso exibe o editor de variáveis. Clique no link **criar variável** para adicionar uma variável para armazenar a cadeia de caracteres enviada para a operação. Nomeie a variável `msg` e defina seu tipo de **variável** como cadeia de caracteres, conforme mostrado na ilustração a seguir.  
  
     ![Captura de tela que mostra como adicionar uma variável.](./media/how-to-create-a-workflow-service-with-messaging-activities/add-variable-msg-string.jpg)  
  
     Clique no botão **variáveis** novamente para fechar o editor de variáveis.  
  
6. Clique no botão **definir..** link na caixa de texto **conteúdo** na <xref:System.ServiceModel.Activities.Receive> atividade para exibir o diálogo **definição de conteúdo** . Selecione o botão de opção **parâmetros** , clique no link **Adicionar novo parâmetro** , digite `inMsg` na caixa de texto **nome** , selecione cadeia de **caracteres** na caixa de listagem suspensa **tipo** e digite `msg` na caixa de texto **atribuir a** , conforme mostrado na ilustração a seguir.  
  
     ![Captura de tela que mostra a adição de conteúdo de parâmetros.](./media/how-to-create-a-workflow-service-with-messaging-activities/adding-parameters-content.jpg)  
  
     Isso especifica que a atividade Receive recebe o parâmetro String e que os dados estão associados à `msg` variável. Clique em **OK** para fechar a caixa de diálogo **definição de conteúdo** .  
  
7. Clique no link **definir...** na caixa **conteúdo** na <xref:System.ServiceModel.Activities.SendReply> atividade para exibir o diálogo **definição de conteúdo** . Selecione o botão de opção **parâmetros** , clique no link **Adicionar novo parâmetro** , digite a caixa de texto `outMsg` **nome** , selecione **cadeia de caracteres** na lista suspensa **tipo** e, `msg` na caixa de texto **valor** , conforme mostrado na ilustração a seguir.  
  
     ![Captura de tela que mostra como adicionar o parâmetro outMsg.](./media/how-to-create-a-workflow-service-with-messaging-activities/outmsg-parameters-content.jpg)  
  
     Isso especifica que a <xref:System.ServiceModel.Activities.SendReply> atividade envia um tipo de contrato de mensagem ou mensagem e que os dados estão associados à `msg` variável. Como essa é uma <xref:System.ServiceModel.Activities.SendReply> atividade, isso significa que os dados em `msg` são usados para popular a mensagem que a atividade envia de volta para o cliente. Clique em **OK** para fechar a caixa de diálogo **definição de conteúdo** .  
  
8. Salve e Compile a solução clicando no menu **Compilar** e selecionando **Compilar solução**.  
  
## <a name="configure-the-workflow-service-project"></a>Configurar o projeto de serviço de fluxo de trabalho  
 O serviço de fluxo de trabalho está concluído. Esta seção explica como configurar a solução de serviço de fluxo de trabalho para facilitar o host e a execução. Essa solução usa o ASP.NET Development Server para hospedar o serviço.  
  
#### <a name="to-set-project-start-up-options"></a>Para definir opções de inicialização do projeto  
  
1. Na **Gerenciador de soluções**, clique com o botão direito do mouse em **MyWFService** e selecione **Propriedades** para exibir a caixa de diálogo **Propriedades do projeto** .  
  
2. Selecione a guia **Web** e selecione **página específica** em **Iniciar ação** e digite `Service1.xamlx` na caixa de texto, conforme mostrado na ilustração a seguir.  
  
     ![Captura de tela que mostra a caixa de diálogo Propriedades do projeto.](./media/how-to-create-a-workflow-service-with-messaging-activities/project-properties-dialog.jpg)  
  
     Isso hospeda o serviço definido em Service1. xamlx no ASP.NET Development Server.  
  
3. Pressione CTRL + F5 para iniciar o serviço. O ícone de ASP.NET Development Server é exibido no lado inferior direito da área de trabalho, conforme mostrado na imagem a seguir.  
  
     ![Captura de tela que mostra o ícone do servidor do desenvolvedor do ASP.NET.](./media/how-to-create-a-workflow-service-with-messaging-activities/asp-net-dev-server-icon.jpg)  
  
     Além disso, o Internet Explorer exibe a página de ajuda do serviço WCF para o serviço.  
  
     ![Captura de tela que mostra a página de ajuda do serviço WCF.](./media/how-to-create-a-workflow-service-with-messaging-activities/wcf-service-help-page.jpg)  
  
4. Continue no tópico [como: acessar um serviço de um aplicativo de fluxo de trabalho](how-to-access-a-service-from-a-workflow-application.md) para criar um cliente de fluxo de trabalho que chama esse serviço.  
  
## <a name="see-also"></a>Consulte também

- [Serviços de fluxo de trabalho](workflow-services.md)
- [Visão geral de serviços de fluxo de trabalho de hospedagem](hosting-workflow-services-overview.md)
- [Atividades de mensagem](messaging-activities.md)
