---
title: 'Tutorial: criar um cliente Windows Communication Foundation'
description: Saiba como criar um cliente Recuperando metadados de um serviço WCF como parte de uma série de artigos que ajudam você a começar a criar um aplicativo WCF.
ms.date: 03/19/2019
helpviewer_keywords:
- clients [WCF], running
- WCF clients [WCF], running
ms.assetid: a67884cc-1c4b-416b-8c96-5c954099f19f
ms.openlocfilehash: d5a2a2b5175c9e34eaf1dbaff20ac57f34117c4e
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246318"
---
# <a name="tutorial-create-a-windows-communication-foundation-client"></a>Tutorial: criar um cliente Windows Communication Foundation

Este tutorial descreve a quarta de cinco tarefas necessárias para criar um aplicativo de Windows Communication Foundation básico (WCF). Para obter uma visão geral dos tutoriais, consulte [tutorial: introdução aos aplicativos Windows Communication Foundation](getting-started-tutorial.md).

A próxima tarefa para criar um aplicativo WCF é criar um cliente Recuperando metadados de um serviço WCF. Você usa o Visual Studio para adicionar uma referência de serviço, que obtém os metadados do ponto de extremidade MEX do serviço. Em seguida, o Visual Studio gera um arquivo de código-fonte gerenciado para um proxy de cliente no idioma escolhido. Ele também cria um arquivo de configuração de cliente (*App.config*). Esse arquivo permite que o aplicativo cliente se conecte ao serviço em um ponto de extremidade.

> [!NOTE]
> Se você chamar um serviço WCF de um projeto de biblioteca de classes no Visual Studio, use o recurso **Adicionar referência de serviço** para gerar automaticamente um proxy e um arquivo de configuração associado. No entanto, como os projetos de biblioteca de classes não usam esse arquivo de configuração, você precisa adicionar as configurações no arquivo de configuração gerado ao arquivo de *App.config* para o executável que chama a biblioteca de classes.

> [!NOTE]
> Como alternativa, use a [ferramenta de utilitário de metadados ServiceModel](#servicemodel-metadata-utility-tool) em vez do Visual Studio para gerar a classe proxy e o arquivo de configuração.

O aplicativo cliente usa a classe proxy gerada para se comunicar com o serviço. Este procedimento é descrito em [tutorial: usar um cliente](how-to-use-a-wcf-client.md).

Neste tutorial, você aprenderá como:
> [!div class="checklist"]
>
> - Criar e configurar um projeto de aplicativo de console para o cliente WCF.
> - Adicione uma referência de serviço ao serviço WCF para gerar a classe proxy e os arquivos de configuração.

## <a name="create-a-windows-communication-foundation-client"></a>Criar um cliente Windows Communication Foundation

1. Criar um projeto de aplicativo de console no Visual Studio:

    1. No menu **arquivo** , selecione **abrir**  >  **projeto/solução** e navegue até a solução **gettingstarted** que você criou anteriormente (*gettingstarted. sln*). Selecione **Abrir**.

    2. No menu **Exibir** , selecione **Gerenciador de soluções**.

    3. Na janela **Gerenciador de soluções** , selecione a solução **gettingstarted** (nó superior) e, em seguida, selecione **Adicionar**  >  **novo projeto** no menu de atalho.

    4. Na janela **Adicionar novo projeto** , no lado esquerdo, selecione a categoria **área de trabalho do Windows** em **Visual C#** ou **Visual Basic**.

    5. Selecione o modelo **aplicativo de console (.NET Framework)** e digite *GettingStartedClient* para o **nome**. Selecione **OK**.

2. Adicione uma referência no projeto **GettingStartedClient** ao <xref:System.ServiceModel> assembly:

    1. Na janela **Gerenciador de soluções** , selecione a pasta **referências** no projeto **GettingStartedClient** e, em seguida, selecione **Adicionar referência** no menu de atalho.

    2. Na janela **Adicionar referência** , em **assemblies** no lado esquerdo da janela, selecione **estrutura**.

    3. Localize e selecione **System. ServiceModel**e, em seguida, escolha **OK**.

    4. Salve a solução selecionando **arquivo**  >  **salvar tudo**.

3. Adicione uma referência de serviço ao serviço de calculadora:

   1. Na janela **Gerenciador de soluções** , selecione a pasta **referências** no projeto **GettingStartedClient** e, em seguida, selecione **Adicionar referência de serviço** no menu de atalho.

   2. Na janela **Adicionar referência de serviço** , selecione **descobrir**.

      O serviço CalculatorService é iniciado e o Visual Studio o exibe na caixa **Serviços** .

   3. Selecione **CalculatorService** para expandi-lo e exibir os contratos de serviço implementados pelo serviço. Deixe o **namespace** padrão e escolha **OK**.

      O Visual Studio adiciona um novo item na pasta **Serviços conectados** no projeto **GettingStartedClient** .

### <a name="servicemodel-metadata-utility-tool"></a>Ferramenta de utilitário de metadados ServiceModel

Os exemplos a seguir mostram como opcionalmente usar a [ferramenta de utilitário de metadados ServiceModel (Svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md) para gerar o arquivo de classe de proxy. Essa ferramenta gera o arquivo de classe de proxy e o arquivo de *App.config* . Os exemplos a seguir mostram como gerar o proxy em C# e Visual Basic, respectivamente:

```shell
svcutil.exe /language:cs /out:generatedProxy.cs /config:app.config http://localhost:8000/GettingStarted/CalculatorService
```

```shell
svcutil.exe /language:vb /out:generatedProxy.vb /config:app.config http://localhost:8000/GettingStarted/CalculatorService
```

### <a name="client-configuration-file"></a>Arquivo de configuração de cliente

Depois de criar o cliente, o Visual Studio cria o arquivo de configuração **App.config** no projeto **GettingStartedClient** , que deve ser semelhante ao exemplo a seguir:

```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <startup>
            <!-- specifies the version of WCF to use-->
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.6.1" />
        </startup>
        <system.serviceModel>
            <bindings>
                <!-- Uses wsHttpBinding-->
                <wsHttpBinding>
                    <binding name="WSHttpBinding_ICalculator" />
                </wsHttpBinding>
            </bindings>
            <client>
                <!-- specifies the endpoint to use when calling the service -->
                <endpoint address="http://localhost:8000/GettingStarted/CalculatorService"
                    binding="wsHttpBinding" bindingConfiguration="WSHttpBinding_ICalculator"
                    contract="ServiceReference1.ICalculator" name="WSHttpBinding_ICalculator">
                    <identity>
                        <dns value="localhost" />
                    </identity>
                </endpoint>
            </client>
        </system.serviceModel>
    </configuration>
```

Na [\<system.serviceModel>](../configure-apps/file-schema/wcf/system-servicemodel.md) seção, observe o [\<endpoint>](../configure-apps/file-schema/wcf/endpoint-element.md) elemento. O elemento de ** &lt; ponto de extremidade &gt; ** define o ponto de extremidade que o cliente usa para acessar o serviço da seguinte maneira:

- Endereço: `http://localhost:8000/GettingStarted/CalculatorService` . O endereço do ponto de extremidade.
- Contrato de serviço: `ServiceReference1.ICalculator` . O contrato de serviço lida com a comunicação entre o cliente WCF e o serviço. O Visual Studio gerou esse contrato quando você usou sua função **Adicionar referência de serviço** . Basicamente, é uma cópia do contrato que você definiu no projeto GettingStartedLib.
- Associação: <xref:System.ServiceModel.WSHttpBinding> . A associação especifica HTTP como transporte, segurança interoperável e outros detalhes de configuração.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu a:
> [!div class="checklist"]
>
> - Criar e configurar um projeto de aplicativo de console para o cliente WCF.
> - Adicione uma referência de serviço ao serviço WCF para gerar a classe proxy e os arquivos de configuração para o aplicativo cliente.

Avance para o próximo tutorial para aprender a usar o cliente gerado.

> [!div class="nextstepaction"]
> [Tutorial: usar um cliente WCF](how-to-use-a-wcf-client.md)
