---
title: Publicar um aplicativo de console do .NET Core usando Visual Studio Code
description: A publicação cria o conjunto de arquivos necessários para executar um aplicativo .NET Core.
ms.date: 07/04/2020
ms.openlocfilehash: 8fd9975e8a88704b9dea45b40127c8dc03f7d09f
ms.sourcegitcommit: 0edbeb66d71b8df10fcb374cfca4d731b58ccdb2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86051877"
---
# <a name="tutorial-publish-a-net-core-console-application-using-visual-studio-code"></a>Tutorial: publicar um aplicativo de console do .NET Core usando Visual Studio Code

Este tutorial mostra como publicar um aplicativo de console para que outros usuários possam executá-lo. A publicação cria o conjunto de arquivos necessários para executar um aplicativo. Para implantar os arquivos, copie-os para o computador de destino.

O CLI do .NET Core é usado para publicar o aplicativo, para que você possa seguir este tutorial com um editor de código diferente de Visual Studio Code se preferir.

## <a name="prerequisites"></a>Pré-requisitos

- Este tutorial funciona com o aplicativo de console que você cria em [criar um aplicativo de console do .NET Core no Visual Studio Code](with-visual-studio-code.md).

## <a name="publish-the-app"></a>Publicar o aplicativo

1. Inicie o Visual Studio Code.

1. Abra a pasta do projeto *HelloWorld* que você criou em [criar um aplicativo de console do .net Core no Visual Studio Code](with-visual-studio-code.md).

1. Escolha **Exibir**  >  **terminal** no menu principal.

   O terminal é aberto na pasta *HelloWorld* .

1. Execute o comando a seguir:

   ```dotnetcli
   dotnet publish --configuration Release
   ```

   A configuração de compilação padrão é *debug*, portanto, esse comando especifica a configuração da compilação da *versão* . A saída da configuração de Build de versão tem informações de depuração simbólicas mínimas e é totalmente otimizada.

   A saída do comando é semelhante ao exemplo a seguir:

   ```
   Microsoft (R) Build Engine version 16.6.0+5ff7b0c9e for .NET Core
   Copyright (C) Microsoft Corporation. All rights reserved.

   Determining projects to restore...
   All projects are up-to-date for restore.
   HelloWorld -> C:\Projects\HelloWorld\bin\Release\netcoreapp3.1\HelloWorld.dll
   HelloWorld -> C:\Projects\HelloWorld\bin\Release\netcoreapp3.1\publish\
   ```

## <a name="inspect-the-files"></a>Inspecionar os arquivos

Por padrão, o processo de publicação cria uma implantação dependente de estrutura, que é um tipo de implantação em que o aplicativo publicado é executado em um computador que tem o tempo de execução do .NET Core instalado. Para executar o aplicativo publicado, você pode usar o arquivo executável ou executar o `dotnet HelloWorld.dll` comando em um prompt de comando.

Nas etapas a seguir, você examinará os arquivos criados pelo processo de publicação.

1. Selecione o **Gerenciador** na barra de navegação à esquerda.

1. Expanda *bin/Release/netcoreapp 3.1/publicar*.

   :::image type="content" source="media/publishing-with-visual-studio-code/published-files-output.png" alt-text="Explorer mostrando arquivos publicados":::

   Como mostra a imagem, a saída publicada inclui os seguintes arquivos:

   * *HelloWorld.deps.json*

      Este é o arquivo de dependências de tempo de execução do aplicativo. Ele define os componentes do .NET Core e as bibliotecas (incluindo a biblioteca de vínculo dinâmico que contém seu aplicativo) necessárias para executar o aplicativo. Para obter mais informações, consulte [arquivos de configuração de tempo de execução](https://github.com/dotnet/cli/blob/85ca206d84633d658d7363894c4ea9d59e515c1a/Documentation/specs/runtime-configuration-file.md).

   * *HelloWorld.dll*

      Esta é a versão de [implantação dependente de estrutura](../deploying/deploy-with-cli.md#framework-dependent-deployment) do aplicativo. Para executar essa biblioteca de vínculo dinâmico, digite `dotnet HelloWorld.dll` em um prompt de comando. Esse método de execução do aplicativo funciona em qualquer plataforma que tenha o tempo de execução do .NET Core instalado.

   * *HelloWorld.exe* (*HelloWorld* no Linux, não criado no MacOS).

      Esta é a versão [executável dependente de estrutura](../deploying/deploy-with-cli.md#framework-dependent-executable) do aplicativo. O arquivo é específico do sistema operacional.

   * *HelloWorld.pdb* (opcional para implantação)

      Este é o arquivo de símbolos de depuração. Não é necessário implantar esse arquivo juntamente com seu aplicativo, embora você deva salvá-lo caso precise depurar a versão publicada do seu aplicativo.

   * *HelloWorld.runtimeconfig.json*

      Este é o arquivo de configuração de tempo de execução do aplicativo. Identifica a versão do .NET Core com base na qual o aplicativo foi criado para ser executado. Você também pode adicionar opções de configuração a ela. Para obter mais informações, consulte [definições de configuração de tempo de execução do .NET Core](../run-time-config/index.md#runtimeconfigjson).

## <a name="run-the-published-app"></a>Executar o aplicativo publicado

1. No **Gerenciador**, clique com o botão direito do mouse na pasta de *publicação* (<kbd>Ctrl</kbd>-clique em MacOS) e selecione **abrir no terminal**.

   :::image type="content" source="media/publishing-with-visual-studio-code/open-in-terminal.png" alt-text="Menu de contexto mostrando abrir no terminal":::

1. No Windows ou Linux, execute o aplicativo usando o executável.

   1. No Windows, digite `.\HelloWorld.exe` e pressione <kbd>Enter</kbd>.

   1. No Linux, digite `./HelloWorld` e pressione <kbd>Enter</kbd>.

   1. Insira um nome em resposta ao prompt e pressione qualquer tecla para sair.

1. Em qualquer plataforma, execute o aplicativo usando o [`dotnet`](../tools/dotnet.md) comando:

   1. Insira `dotnet HelloWorld.dll` e pressione <kbd>Enter</kbd>.

   1. Insira um nome em resposta ao prompt e pressione qualquer tecla para sair.

## <a name="additional-resources"></a>Recursos adicionais

- [Implantação de aplicativo .NET Core](../deploying/index.md)

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você publicou um aplicativo de console. No próximo tutorial, você criará uma biblioteca de classes.

> [!div class="nextstepaction"]
> [Criar uma biblioteca de .NET Standard no Visual Studio Code](library-with-visual-studio-code.md)
