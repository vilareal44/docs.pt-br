---
title: 'Tutorial: instalar e usar uma ferramenta global do .NET Core'
description: Saiba como instalar e usar uma ferramenta .NET como uma ferramenta global.
ms.topic: tutorial
ms.date: 02/12/2020
ms.openlocfilehash: 28e34a4e5a0344e314c5d23228c1af5839db991c
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88062763"
---
# <a name="tutorial-install-and-use-a-net-core-global-tool-using-the-net-core-cli"></a>Tutorial: instalar e usar uma ferramenta global do .NET Core usando o CLI do .NET Core

**Este artigo aplica-se a:** ✔️ SDK do .net Core 2,1 e versões posteriores

Este tutorial ensina como instalar e usar uma ferramenta global. Você usa uma ferramenta que você cria no [primeiro tutorial desta série](global-tools-how-to-create.md).

## <a name="prerequisites"></a>Pré-requisitos

* Conclua o [primeiro tutorial desta série](global-tools-how-to-create.md).

## <a name="use-the-tool-as-a-global-tool"></a>Usar a ferramenta como uma ferramenta global

1. Instale a ferramenta do pacote executando o comando [dotnet ferramenta de instalação](dotnet-tool-install.md) na pasta *Microsoft. botsay* Project:

   ```dotnetcli
   dotnet tool install --global --add-source ./nupkg microsoft.botsay
   ```

   O `--global` parâmetro informa ao CLI do .NET Core para instalar os binários de ferramenta em um local padrão que é adicionado automaticamente à variável de ambiente Path.

   O `--add-source` parâmetro informa ao CLI do .NET Core para usar temporariamente o diretório *./nupkg* como um feed de origem adicional para pacotes NuGet. Você deu ao seu pacote um nome exclusivo para certificar-se de que ele só será encontrado no diretório *./nupkg* , não no site do NuGet.org.

   A saída mostra o comando usado para chamar a ferramenta e a versão instalada:

   ```console
   You can invoke the tool using the following command: botsay
   Tool 'microsoft.botsay' (version '1.0.0') was successfully installed.
   ```

1. Invoque a ferramenta:

   ```console
   botsay hello from the bot
   ```

   > [!NOTE]
   > Se esse comando falhar, talvez seja necessário abrir um novo terminal para atualizar o caminho.

1. Remova a ferramenta executando o comando [dotnet ferramenta de desinstalação](dotnet-tool-uninstall.md) :

   ```dotnetcli
   dotnet tool uninstall -g microsoft.botsay
   ```

## <a name="use-the-tool-as-a-global-tool-installed-in-a-custom-location"></a>Usar a ferramenta como uma ferramenta global instalada em um local personalizado

1. Instale a ferramenta do pacote.

   No Windows:

   ```dotnetcli
   dotnet tool install --tool-path c:\dotnet-tools --add-source ./nupkg microsoft.botsay
   ```

   No Linux ou macOS:

   ```dotnetcli
   dotnet tool install --tool-path ~/bin --add-source ./nupkg microsoft.botsay
   ```

   O `--tool-path` parâmetro informa ao CLI do .NET Core para instalar os binários de ferramenta no local especificado. Se o diretório não existir, ele será criado. Esse diretório não é adicionado automaticamente à variável de ambiente PATH.

   A saída mostra o comando usado para chamar a ferramenta e a versão instalada:

   ```console
   You can invoke the tool using the following command: botsay
   Tool 'microsoft.botsay' (version '1.0.0') was successfully installed.
   ```

1. Invoque a ferramenta:

   No Windows:

   ```console
   c:\dotnet-tools\botsay hello from the bot
   ```

   No Linux ou macOS:

   ```console
   ~/bin/botsay hello from the bot
   ```

1. Remova a ferramenta executando o comando [dotnet ferramenta de desinstalação](dotnet-tool-uninstall.md) :

   No Windows:

   ```dotnetcli
   dotnet tool uninstall --tool-path c:\dotnet-tools microsoft.botsay
   ```

   No Linux ou macOS:

   ```dotnetcli
   dotnet tool uninstall --tool-path ~/bin microsoft.botsay
   ```

## <a name="troubleshoot"></a>Solucionar problemas

Se você receber uma mensagem de erro ao seguir o tutorial, consulte [solucionar problemas de uso da ferramenta .NET Core](troubleshoot-usage-issues.md).

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você instalou e usou uma ferramenta como uma ferramenta global. Para instalar e usar a mesma ferramenta como uma ferramenta local, avance para o próximo tutorial.

> [!div class="nextstepaction"]
> [Instalar e usar ferramentas locais](local-tools-how-to-use.md)
