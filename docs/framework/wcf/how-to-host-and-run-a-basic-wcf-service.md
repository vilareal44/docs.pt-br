---
title: 'Tutorial: hospedar e executar um serviço de Windows Communication Foundation básico'
description: Saiba como hospedar um serviço WCF em um aplicativo de console como parte de uma série de artigos que ajudam você a começar a criar um aplicativo WCF.
ms.date: 03/19/2019
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF services [WCF]
- WCF services [WCF], running
ms.assetid: 31774d36-923b-4e2d-812e-aa190127266f
ms.openlocfilehash: 5318991087e71430523681d601d3b38c4513027b
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246123"
---
# <a name="tutorial-host-and-run-a-basic-windows-communication-foundation-service"></a>Tutorial: hospedar e executar um serviço de Windows Communication Foundation básico

Este tutorial descreve a terceira das cinco tarefas necessárias para criar um aplicativo de Windows Communication Foundation básico (WCF). Para obter uma visão geral dos tutoriais, consulte [tutorial: introdução aos aplicativos Windows Communication Foundation](getting-started-tutorial.md).

A próxima tarefa para criar um aplicativo WCF é hospedar um serviço WCF em um aplicativo de console. Um serviço WCF expõe um ou mais *pontos de extremidade*, cada um dos quais expõe uma ou mais operações de serviço. Um ponto de extremidade de serviço especifica as seguintes informações:

- Um endereço onde você pode encontrar o serviço.
- Uma associação que contém as informações que descrevem como um cliente deve se comunicar com o serviço.
- Um contrato que define a funcionalidade que o serviço fornece aos seus clientes.

Neste tutorial, você aprenderá como:
> [!div class="checklist"]
>
> - Criar e configurar um projeto de aplicativo de console para hospedar um serviço WCF.
> - Adicione o código para hospedar o serviço WCF.
> - Atualize o arquivo de configuração.
> - Inicie o serviço WCF e verifique se ele está em execução.

## <a name="create-and-configure-a-console-app-project-for-hosting-the-service"></a>Criar e configurar um projeto de aplicativo de console para hospedar o serviço

1. Criar um projeto de aplicativo de console no Visual Studio:

    1. No menu **arquivo** , selecione **abrir**  >  **projeto/solução** e navegue até a solução **gettingstarted** que você criou anteriormente (*gettingstarted. sln*). Selecione **Abrir**.

    2. No menu **Exibir** , selecione **Gerenciador de soluções**.

    3. Na janela **Gerenciador de soluções** , selecione a solução **gettingstarted** (nó superior) e, em seguida, selecione **Adicionar**  >  **novo projeto** no menu de atalho.

    4. Na janela **Adicionar novo projeto** , no lado esquerdo, selecione a categoria **área de trabalho do Windows** em **Visual C#** ou **Visual Basic**.

    5. Selecione o modelo **aplicativo de console (.NET Framework)** e digite *GettingStartedHost* para o **nome**. Selecione **OK**.

2. Adicione uma referência no projeto **GettingStartedHost** ao projeto **GettingStartedLib** :

    1. Na janela **Gerenciador de soluções** , selecione a pasta **referências** no projeto **GettingStartedHost** e, em seguida, selecione **Adicionar referência** no menu de atalho.

    2. Na caixa de diálogo **Adicionar referência** , em **projetos** no lado esquerdo da janela, selecione **solução**.

    3. Selecione **GettingStartedLib** na seção central da janela e, em seguida, selecione **OK**.

       Essa ação torna os tipos definidos no projeto **GettingStartedLib** disponíveis para o projeto **GettingStartedHost** .

3. Adicione uma referência no projeto **GettingStartedHost** ao <xref:System.ServiceModel> assembly:

    1. Na janela **Gerenciador de soluções** , selecione a pasta **referências** no projeto **GettingStartedHost** e, em seguida, selecione **Adicionar referência** no menu de atalho.

    2. Na janela **Adicionar referência** , em **assemblies** no lado esquerdo da janela, selecione **estrutura**.

    3. Selecione **System. ServiceModel**e, em seguida, selecione **OK**.

    4. Salve a solução selecionando **arquivo**  >  **salvar tudo**.

## <a name="add-code-to-host-the-service"></a>Adicionar código para hospedar o serviço

Para hospedar o serviço, você adiciona código para realizar as seguintes etapas:

1. Crie um URI para o endereço base.
2. Crie uma instância de classe para hospedar o serviço.
3. Criar um ponto de extremidade de serviço.
4. Ative a troca de metadados.
5. Abra o host de serviço para escutar mensagens de entrada.
  
Faça as alterações a seguir ao código:

1. Abra o arquivo **Program.cs** ou **Module1. vb** no projeto **GettingStartedHost** e substitua seu código pelo código a seguir:

    ```csharp
    using System;
    using System.ServiceModel;
    using System.ServiceModel.Description;
    using GettingStartedLib;

    namespace GettingStartedHost
    {
        class Program
        {
            static void Main(string[] args)
            {
                // Step 1: Create a URI to serve as the base address.
                Uri baseAddress = new Uri("http://localhost:8000/GettingStarted/");

                // Step 2: Create a ServiceHost instance.
                ServiceHost selfHost = new ServiceHost(typeof(CalculatorService), baseAddress);

                try
                {
                    // Step 3: Add a service endpoint.
                    selfHost.AddServiceEndpoint(typeof(ICalculator), new WSHttpBinding(), "CalculatorService");

                    // Step 4: Enable metadata exchange.
                    ServiceMetadataBehavior smb = new ServiceMetadataBehavior();
                    smb.HttpGetEnabled = true;
                    selfHost.Description.Behaviors.Add(smb);

                    // Step 5: Start the service.
                    selfHost.Open();
                    Console.WriteLine("The service is ready.");

                    // Close the ServiceHost to stop the service.
                    Console.WriteLine("Press <Enter> to terminate the service.");
                    Console.WriteLine();
                    Console.ReadLine();
                    selfHost.Close();
                }
                catch (CommunicationException ce)
                {
                    Console.WriteLine("An exception occurred: {0}", ce.Message);
                    selfHost.Abort();
                }
            }
        }
    }
    ```

    ```vb
    Imports System.ServiceModel
    Imports System.ServiceModel.Description
    Imports GettingStartedLib.GettingStartedLib

    Module Service

        Class Program
            Shared Sub Main()
                ' Step 1: Create a URI to serve as the base address.
                Dim baseAddress As New Uri("http://localhost:8000/GettingStarted/")

                ' Step 2: Create a ServiceHost instance.
                Dim selfHost As New ServiceHost(GetType(CalculatorService), baseAddress)
               Try

                    ' Step 3: Add a service endpoint.
                    selfHost.AddServiceEndpoint( _
                        GetType(ICalculator), _
                        New WSHttpBinding(), _
                        "CalculatorService")

                    ' Step 4: Enable metadata exchange.
                    Dim smb As New ServiceMetadataBehavior()
                    smb.HttpGetEnabled = True
                    selfHost.Description.Behaviors.Add(smb)

                    ' Step 5: Start the service.
                    selfHost.Open()
                    Console.WriteLine("The service is ready.")

                    ' Close the ServiceHost to stop the service.
                    Console.WriteLine("Press <Enter> to terminate the service.")
                    Console.WriteLine()
                    Console.ReadLine()
                    selfHost.Close()

                Catch ce As CommunicationException
                    Console.WriteLine("An exception occurred: {0}", ce.Message)
                    selfHost.Abort()
                End Try
            End Sub
        End Class

    End Module
    ```

    Para obter informações sobre como esse código funciona, consulte [etapas do programa de Hospedagem de serviço](#service-hosting-program-steps).

2. Atualize as propriedades do projeto:

   1. Na janela **Gerenciador de soluções** , selecione a pasta **GettingStartedHost** e, em seguida, selecione **Propriedades** no menu de atalho.

   2. Na página Propriedades do **GettingStartedHost** , selecione a guia **aplicativo** :

      - Para projetos C#, selecione **GettingStartedHost. Program** na lista **objeto de inicialização** .

      - Para projetos Visual Basic, selecione **Service. Program** na lista **objeto de inicialização** .

   3. No menu **arquivo** , selecione **salvar tudo**.

## <a name="verify-the-service-is-working"></a>Verifique se o serviço está funcionando

1. Compile a solução e, em seguida, execute o aplicativo de console **GettingStartedHost** de dentro do Visual Studio.

    O serviço deve ser executado com privilégios de administrador. Como você abriu o Visual Studio com privilégios de administrador, ao executar o **GettingStartedHost** no Visual Studio, o aplicativo também é executado com privilégios de administrador. Como alternativa, você pode abrir um novo prompt de comando como administrador (selecione **mais**  >  **Executar como administrador** no menu de atalho) e execute **GettingStartedHost.exe** dentro dele.

2. Abra um navegador da Web e navegue até a página do serviço em `http://localhost:8000/GettingStarted/CalculatorService` .

   > [!NOTE]
   > Serviços como este exigem a permissão adequada para registrar endereços HTTP no computador para escuta. As contas de administrador têm essa permissão, mas as contas de não administrador devem ter permissão para namespaces de HTTP. Para obter mais informações sobre como configurar reservas de namespace, confira [Configurando HTTP e HTTPS](feature-details/configuring-http-and-https.md).

## <a name="service-hosting-program-steps"></a>Etapas do programa de Hospedagem de serviço

As etapas no código que você adicionou para hospedar o serviço são descritas da seguinte maneira:

- **Etapa 1**: Crie uma instância da `Uri` classe para manter o endereço base do serviço. Uma URL que contém um endereço base tem um URI opcional que identifica um serviço. O endereço base é formatado da seguinte maneira: `<transport>://<machine-name or domain><:optional port #>/<optional URI segment>` . O endereço base para o serviço de calculadora usa o transporte HTTP, localhost, a porta 8000 e o segmento URI, GettingStarted.

- **Etapa 2**: criar uma instância da <xref:System.ServiceModel.ServiceHost> classe, que você usa para hospedar o serviço. O construtor usa dois parâmetros: o tipo da classe que implementa o contrato de serviço e o endereço base do serviço.

- **Etapa 3**: criar uma <xref:System.ServiceModel.Description.ServiceEndpoint> instância. Um ponto de extremidade de serviço é composto de um endereço, uma associação e um contrato de serviço. O <xref:System.ServiceModel.Description.ServiceEndpoint> Construtor é composto pelo tipo de interface do contrato de serviço, uma associação e um endereço. O contrato de serviço é `ICalculator`, que você definiu e implementou no tipo de serviço. A associação para este exemplo é <xref:System.ServiceModel.WSHttpBinding> , que é uma ligação interna e se conecta a pontos de extremidade que estão em conformidade com as especificações WS-*. Para obter mais informações sobre associações do WCF, consulte [visão geral de associações do WCF](bindings-overview.md). Você acrescenta o endereço ao endereço base para identificar o ponto de extremidade. O código especifica o endereço como CalculatorService e o endereço totalmente qualificado para o ponto de extremidade como `http://localhost:8000/GettingStarted/CalculatorService` .

    > [!IMPORTANT]
    > Para .NET Framework versão 4 e posterior, adicionar um ponto de extremidade de serviço é opcional. Para essas versões, se você não adicionar seu código ou configuração, o WCF adicionará um ponto de extremidade padrão para cada combinação de endereço base e contrato implementado pelo serviço. Para obter mais informações sobre pontos de extremidade padrão, consulte [especificando um endereço de ponto de extremidades](specifying-an-endpoint-address.md). Para obter mais informações sobre pontos de extremidade padrão, associações e comportamentos, consulte [configuração simplificada](simplified-configuration.md) e [configuração simplificada para serviços WCF](samples/simplified-configuration-for-wcf-services.md).

- **Etapa 4**: habilitar a troca de metadados. Os clientes usam a troca de metadados para gerar proxies para chamar as operações de serviço. Para habilitar a troca de metadados, crie uma <xref:System.ServiceModel.Description.ServiceMetadataBehavior> instância, defina sua <xref:System.ServiceModel.Description.ServiceMetadataBehavior.HttpGetEnabled> propriedade como `true` e adicione o `ServiceMetadataBehavior` objeto à <xref:System.ServiceModel.Description.ServiceDescription.Behaviors%2A> coleção da <xref:System.ServiceModel.ServiceHost> instância.

- **Etapa 5**: abrir <xref:System.ServiceModel.ServiceHost> para escutar mensagens de entrada. O aplicativo aguarda até que você pressione **Enter**. Depois que o aplicativo instancia <xref:System.ServiceModel.ServiceHost> , ele executa um bloco try/catch. Para obter mais informações sobre a captura segura de exceções lançadas pelo <xref:System.ServiceModel.ServiceHost> , consulte [usar fechar e anular para liberar recursos do cliente WCF](samples/use-close-abort-release-wcf-client-resources.md).

> [!IMPORTANT]
> Quando você adiciona uma biblioteca de serviços WCF, o Visual Studio a hospeda para você se depurá-la iniciando um host de serviço. Para evitar conflitos, você pode impedir que o Visual Studio hospede a biblioteca de serviços do WCF.
>
> 1. Selecione o projeto **GettingStartedLib** em **Gerenciador de soluções** e escolha **Propriedades** no menu de atalho.
> 2. Selecione **Opções do WCF** e desmarque **Iniciar host de serviço WCF ao depurar outro projeto na mesma solução**.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu a:
> [!div class="checklist"]
>
> - Criar e configurar um projeto de aplicativo de console para hospedar um serviço WCF.
> - Adicione o código para hospedar o serviço WCF.
> - Atualize o arquivo de configuração.
> - Inicie o serviço WCF e verifique se ele está em execução.

Avance para o próximo tutorial para aprender a criar um cliente WCF.

> [!div class="nextstepaction"]
> [Tutorial: criar um cliente WCF](how-to-create-a-wcf-client.md)
