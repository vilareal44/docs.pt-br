---
title: Criar um serviço de dados WCF no Visual Studio
description: Saiba como criar um serviço de dados de exemplo que usa WCF Data Services para expor um feed OData com base em um banco de dados de exemplo.
ms.date: 08/24/2018
dev_langs:
- csharp
- vb
ms.assetid: 34d1d971-5e18-4c22-9bf6-d3612e27ea59
ms.openlocfilehash: 739cb6971209792724a2e939ca4f4821d5879c8c
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247785"
---
# <a name="create-the-data-service"></a>Criar o serviço de dados

Neste tópico, você cria um serviço de dados de exemplo que usa WCF Data Services para expor um feed de Protocolo Open Data (OData) baseado no banco de dados de exemplo Northwind. A tarefa envolve as seguintes etapas básicas:

1. Crie um aplicativo Web ASP.NET.

2. Defina o modelo de dados usando as ferramentas de Modelo de Dados de Entidade.

3. Adicionar o serviço de dados para o aplicativo Web.

4. Habilitar o acesso ao serviço de dados.

## <a name="create-the-aspnet-web-app"></a>Criar o aplicativo Web ASP.NET

1. No Visual Studio, no menu **Arquivo**, selecione **Novo** > **Projeto**.

1. Na caixa de diálogo **novo projeto** , em Visual Basic ou Visual C#, selecione a categoria **Web** e, em seguida, selecione **aplicativo Web ASP.net**.

1. Insira `NorthwindService` como o nome do projeto e, em seguida, selecione **OK**.

1. Na caixa de diálogo **novo aplicativo Web ASP.net** , selecione **vazio** e, em seguida, selecione **OK**.

1. (Opcional) Especifique um número de porta específica para seu aplicativo Web. Observação: o número da porta `12345` é usado nesta série de tópicos de início rápido.

    1. No **Gerenciador de soluções**, clique com o botão direito do mouse no projeto ASP.NET que você acabou de criar e escolha **Propriedades**.

    2. Selecione a guia **Web** e defina o valor da caixa de texto **porta específica** como `12345` .

## <a name="define-the-data-model"></a>Definir o modelo de dados

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no nome do projeto ASP.net e clique em **Adicionar**  >  **novo item**.

2. Na caixa de diálogo **Adicionar novo item** , selecione a categoria de **dados** e, em seguida, selecione **ADO.NET modelo de dados de entidade**.

3. Para o nome do modelo de dados, digite `Northwind.edmx` .

4. No **Assistente de modelo de dados de entidade**, selecione **EF designer do banco de dados**e clique em **Avançar**.

5. Conecte o modelo de dados ao banco de dado executando uma das etapas a seguir e clique em **Avançar**:

    - Se você não tiver uma conexão de banco de dados já configurada, clique em **nova conexão** e crie uma nova conexão. Para obter mais informações, consulte [como: criar conexões para bancos de dados SQL Server](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/s4yys16a(v=vs.90)). Esta instância de SQL Server deve ter o banco de dados de exemplo Northwind anexado.

         \- ou –

    - Se você tiver uma conexão de banco de dados já configurada para se conectar ao banco de dados Northwind, selecione a conexão da lista de conexões.

6. Na página final do assistente, selecione as caixas de seleção para todas as tabelas no banco de dados e desmarque as caixas de seleção para exibições e procedimentos armazenados.

7. Clique em **Concluir** para fechar o assistente.

## <a name="create-the-wcf-data-service"></a>Criar o WCF Data Service

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto ASP.net e escolha **Adicionar**  >  **novo item**.

2. Na caixa de diálogo **Adicionar novo item** , selecione o modelo item do **WCF Data Service** na categoria **Web** .

   ![Modelo de item do WCF Data Service no Visual Studio 2015](./media/wcf-data-service-item-template.png)

   > [!NOTE]
   > O modelo do **WCF Data Service** está disponível no visual Studio 2015, mas não no visual Studio 2017 ou posterior.

3. Para o nome do serviço, digite `Northwind` .

     O Visual Studio cria a marcação XML e os arquivos de código do novo serviço. Por padrão, a janela do editor de códigos é aberta. Em **Gerenciador de soluções**, o serviço tem o nome Northwind com a extensão *. svc.cs* ou *. svc. vb*.

4. No código do serviço de dados, substitua o comentário `/* TODO: put your data source class name here */` na definição da classe que define o serviço de dados pelo tipo que é o contêiner de entidade do modelo de dados, que, neste caso, é `NorthwindEntities`. A definição da classe deve ter a seguinte aparência:

     [!code-csharp[Astoria Quickstart Service#ServiceDefinition](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_quickstart_service/cs/northwind.svc.cs#servicedefinition)]
     [!code-vb[Astoria Quickstart Service#ServiceDefinition](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_quickstart_service/vb/northwind.svc.vb#servicedefinition)]

## <a name="enable-access-to-data-service-resources"></a>Habilitar o acesso aos recursos do serviço de dados

1. No código para o serviço de dados, substitua o código de espaço reservado na função `InitializeService` pelo seguinte:

     [!code-csharp[Astoria Quickstart Service#AllReadConfig](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_quickstart_service/cs/northwind.svc.cs#allreadconfig)]
     [!code-vb[Astoria Quickstart Service#AllReadConfig](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_quickstart_service/vb/northwind.svc.vb#allreadconfig)]

     Isso permite que clientes autorizados tenham acesso de leitura e gravação aos recursos para os conjuntos de entidades especificados.

    > [!NOTE]
    > Qualquer cliente que possa acessar o aplicativo ASP.NET também pode acessar os recursos expostos pelo serviço de dados. Em um serviço de dados de produção, para impedir o acesso não autorizado a recursos, você também deverá proteger o próprio aplicativo. Para obter mais informações, consulte [securing WCF Data Services](securing-wcf-data-services.md).

## <a name="next-steps"></a>Próximas etapas

Você criou com êxito um novo serviço de dados que expõe um feed OData baseado no banco de dados de exemplo Northwind e que você habilitou o acesso ao feed para clientes que têm permissões no aplicativo Web ASP.NET. Em seguida, você iniciará o serviço de dados do Visual Studio e acessará o feed OData enviando solicitações HTTP GET por meio de um navegador da Web:

> [!div class="nextstepaction"]
> [Acessar o serviço de um navegador da Web](accessing-the-service-from-a-web-browser-wcf-data-services-quickstart.md)

## <a name="see-also"></a>Veja também

- [Ferramentas de Modelo de Dados de Entidade de ADO.NET](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb399249(v=vs.100))
