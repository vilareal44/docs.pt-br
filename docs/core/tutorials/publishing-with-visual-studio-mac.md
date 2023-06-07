---
title: Publicar um aplicativo de console do .NET Core usando Visual Studio para Mac
description: A publicação cria o conjunto de arquivos necessários para executar um aplicativo .NET Core.
ms.date: 06/08/2020
ms.openlocfilehash: 67762481d3a56b8473e643f71b8df909b6e54fc6
ms.sourcegitcommit: 1cbd77da54405ea7dba343ac0334fb03237d25d2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84713661"
---
# <a name="tutorial-publish-a-net-core-console-application-using-visual-studio-for-mac"></a>Tutorial: publicar um aplicativo de console do .NET Core usando Visual Studio para Mac

Este tutorial mostra como publicar um aplicativo de console para que outros usuários possam executá-lo. A publicação cria o conjunto de arquivos necessários para executar seu aplicativo. Para implantar os arquivos, copie-os para o computador de destino.

## <a name="prerequisites"></a>Pré-requisitos

- Este tutorial funciona com o aplicativo de console que você cria em [criar um aplicativo de console do .NET Core no Visual Studio para Mac](with-visual-studio-mac.md).

## <a name="publish-the-app"></a>Publicar o aplicativo

1. Iniciar Visual Studio para Mac.

1. Abra o projeto HelloWorld que você criou em [criar um aplicativo de console do .NET Core no Visual Studio para Mac](with-visual-studio-mac.md).

1. Certifique-se de que o Visual Studio esteja compilando a versão de lançamento de seu aplicativo. Se necessário, altere a configuração de build na barra de ferramentas de **Depuração** para **Lançamento**.

   :::image type="content" source="media/publishing-with-visual-studio-mac/toolbar-release.png" alt-text="Barra de ferramentas do Visual Studio com build de versão selecionado":::

1. No menu principal, escolha **criar**  >  **publicar na pasta...**.

   :::image type="content" source="media/publishing-with-visual-studio-mac/publish-context-menu.png" alt-text="Menu de contexto Publicar do Visual Studio":::

1. Na caixa de diálogo **publicar na pasta** , selecione **publicar**.

   :::image type="content" source="media/publishing-with-visual-studio-mac/publish-to-folder-dialog.png" alt-text="Caixa de diálogo Publicar na pasta do Visual Studio":::

   A pasta de publicação é aberta, mostrando os arquivos que foram criados.

   :::image type="content" source="media/publishing-with-visual-studio-mac/publish-folder.png" alt-text="Publicar pasta":::

1. Selecione o ícone de engrenagem e selecione **Copiar "publicar" como nome de caminho** no menu de contexto.

   :::image type="content" source="media/publishing-with-visual-studio-mac/copy-path.png" alt-text="Copiar caminho para a pasta de publicação":::

## <a name="inspect-the-files"></a>Inspecionar os arquivos

O processo de publicação cria uma implantação dependente de estrutura, que é um tipo de implantação em que o aplicativo publicado é executado em um computador que tem o tempo de execução do .NET Core instalado. Os usuários podem executar o aplicativo publicado executando o `dotnet HelloWorld.dll` comando em um prompt de comando.

Como mostra a imagem anterior, a saída publicada inclui os seguintes arquivos:

* *HelloWorld.deps.json*

  Este é o arquivo de dependências de tempo de execução do aplicativo. Ele define os componentes do .NET Core e as bibliotecas (incluindo a biblioteca de vínculo dinâmico que contém seu aplicativo) necessárias para executar o aplicativo. Para obter mais informações, consulte [arquivos de configuração de tempo de execução](https://github.com/dotnet/cli/blob/85ca206d84633d658d7363894c4ea9d59e515c1a/Documentation/specs/runtime-configuration-file.md).

* *HelloWorld.dll*

   Esta é a versão de [implantação dependente de estrutura](../deploying/deploy-with-cli.md#framework-dependent-deployment) do aplicativo. Para executar essa biblioteca de vínculo dinâmico, digite `dotnet HelloWorld.dll` em um prompt de comando. Esse método de execução do aplicativo funciona em qualquer plataforma que tenha o tempo de execução do .NET Core instalado.

* *HelloWorld.pdb* (opcional para implantação)

   Este é o arquivo de símbolos de depuração. Não é necessário implantar esse arquivo juntamente com seu aplicativo, embora você deva salvá-lo caso precise depurar a versão publicada do seu aplicativo.

* *HelloWorld.runtimeconfig.json*

   Este é o arquivo de configuração de tempo de execução do aplicativo. Identifica a versão do .NET Core com base na qual o aplicativo foi criado para ser executado. Você também pode adicionar opções de configuração a ela. Para obter mais informações, consulte [definições de configuração de tempo de execução do .NET Core](../run-time-config/index.md#runtimeconfigjson).

## <a name="run-the-published-app"></a>Executar o aplicativo publicado

1. Abra um terminal e navegue até a pasta de *publicação* . Para fazer isso, insira `cd` e cole o caminho que você copiou anteriormente. Por exemplo:

   ```
   cd ~/Projects/HelloWorld/HelloWorld/bin/Release/netcoreapp3.1/publish/
   ```

1. Execute o aplicativo usando o `dotnet` comando:

   1. Insira `dotnet HelloWorld.dll` e pressione <kbd>Enter</kbd>.

   1. Insira um nome em resposta ao prompt e pressione qualquer tecla para sair.

## <a name="additional-resources"></a>Recursos adicionais

- [Implantação de aplicativo .NET Core](../deploying/index.md)

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você publicou um aplicativo de console. No próximo tutorial, você criará uma biblioteca de classes.

> [!div class="nextstepaction"]
> [Criar uma biblioteca de .NET Standard no Visual Studio para Mac](library-with-visual-studio-mac.md)
