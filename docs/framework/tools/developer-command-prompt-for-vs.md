---
title: Prompt de Comando do Desenvolvedor para o Visual Studio
description: Aprenda a usar o Prompt de Comando do Desenvolvedor para o Visual Studio, que permite usar ferramentas .NET com mais facilidade. Ele define automaticamente variáveis de ambiente específicas.
ms.date: 01/05/2020
helpviewer_keywords:
- command prompt, Windows SDK
- Visual Studio command prompt
- command prompt, Visual Studio
- SDK command prompt
- tools [.NET Framework], setting environment variables
- environment variables, setting for tools
- developer command prompt
ms.assetid: 94fcf524-9045-4993-bfb2-e2d8bad44219
ms.openlocfilehash: 92416820f47cb778dfcc916b8626df4aa328814c
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87167176"
---
# <a name="developer-command-prompt-for-visual-studio"></a>Prompt de Comando do Desenvolvedor para o Visual Studio

O Prompt de Comando do Desenvolvedor para Visual Studio permite que você use .NET Framework ferramentas com mais facilidade. É um prompt de comando que define automaticamente variáveis de ambiente específicas. Depois de abrir Prompt de Comando do Desenvolvedor, você pode inserir os comandos para [.NET Framework ferramentas](index.md) como `ildasm` ou `clrver` .

## <a name="prerequisites"></a>Pré-requisitos

- [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)

## <a name="search-for-the-command-prompt-on-your-machine"></a>Pesquisar pelo prompt de comando em seu computador

Você pode ter vários prompts de comando, dependendo da versão do Visual Studio e de quaisquer SDKs e cargas de trabalho adicionais instalados. Se as etapas a seguir não funcionarem, você poderá tentar [localizar manualmente os arquivos em seu computador](#manually-locate-the-files-on-your-machine) ou [iniciar o prompt de comando de dentro do Visual Studio](#start-the-command-prompt-from-inside-visual-studio).

### <a name="windows-10"></a>Windows 10

1. Selecione **Iniciar** ![ a tecla do logotipo do Windows no teclado.](./media/developer-command-prompt-for-vs/windows-logo-key-graphic.png) e role até a letra **V**.

1. Expanda a pasta do **Visual Studio 2019** .

1. Escolha **prompt de comando do desenvolvedor para VS 2019** (ou o prompt de comando que você deseja usar).

   Como alternativa, você pode começar a digitar o nome do prompt de comando na caixa de pesquisa na barra de tarefas e escolher o resultado desejado, pois a lista de resultados começa a exibir as correspondências de pesquisa.

   ![GIF animado mostrando o comportamento de pesquisa no Windows 10](./media/developer-command-prompt-for-vs/windows10-search.gif)

### <a name="windows-81"></a>Windows 8.1

1. Vá para a tela **Inicial** pressionando a tecla do logotipo do Windows ![tecla do logotipo do Windows no teclado.](./media/developer-command-prompt-for-vs/windows-logo-key-graphic.png) no seu teclado, por exemplo.

1. Na tela **Iniciar** , pressione a **tecla CTRL** + **Tab** para abrir a lista de **aplicativos** e, em seguida, pressione **V**. Isso abre uma lista que inclui todos os prompts de comando instalados do Visual Studio.

1. Escolha **prompt de comando do desenvolvedor para VS 2019** (ou o prompt de comando que você deseja usar).

### <a name="windows-7"></a>Windows 7

1. Escolha **Iniciar** e expanda **todos os programas**.

1. Escolha o **Visual Studio 2019**  >  **Ferramentas do Visual Studio**  >  **prompt de comando do desenvolvedor para vs 2019**ou o prompt de comando que você deseja usar.

   ![Menu Iniciar do Windows 7 com o prompt de comando realçado](./media/developer-command-prompt-for-vs/windows7-menu.png)

Se você tiver outros SDKs instalados, como o [SDK do Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk) ou [versões anteriores](https://developer.microsoft.com/windows/downloads/sdk-archive), poderá ver prompts de comando adicionais. Consulte a documentação das ferramentas individuais para determinar qual versão do prompt de comando você deve usar.

## <a name="manually-locate-the-files-on-your-machine"></a>Localizar manualmente os arquivos em seu computador

Normalmente, os atalhos para os prompts de comando que você instalou são colocados na pasta do **menu iniciar** do Visual Studio, como no *%ProgramData%\Microsoft\Windows\Start Menu\Programs\Visual Studio 2019 \ Ferramentas do Visual Studio*. Mas se, por algum motivo, pesquisar o prompt de comando não produzir os resultados esperados, você poderá tentar localizar manualmente o atalho em seu computador. Tente pesquisar o nome do arquivo de prompt de comando, como *VsDevCmd.bat*ou vá para a pasta ferramentas, como *% ProgramFiles (x86)% \ Microsoft Visual Studio\2019\Community\Common7\Tools* (o caminho muda de acordo com sua versão, edição e local de instalação do Visual Studio).

## <a name="start-the-command-prompt-from-inside-visual-studio"></a>Iniciar o prompt de comando de dentro do Visual Studio

Para facilitar o acesso, você pode adicionar Prompt de Comando do Desenvolvedor, ou qualquer outro prompt de comando, ao menu ferramentas no Visual Studio. Para disponibilizar a ferramenta, adicione-a à lista de ferramentas externas. Siga estas etapas:

1. Abra o Visual Studio.

1. Na janela de início, escolha **Continuar sem código**.

1. Na barra de menus, escolha **ferramentas**  >  **Ferramentas externas**.

1. Na caixa de diálogo **Ferramentas Externas**, escolha o botão **Adicionar**. Uma nova entrada é exibida.

1. Insira um **Título** para o novo item de menu, como `Command Prompt`.

1. No campo **Comando**, especifique o arquivo que deseja iniciar, como `%comspec%` ou `C:\Windows\System32\cmd.exe`.

1. No campo **argumentos** , especifique onde encontrar o prompt de comando específico que você deseja usar, como `/k "C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\Common7\Tools\VsDevCmd.bat"` . Esse comando inicia o Prompt de Comando do Desenvolvedor instalado com a Comunidade do Visual Studio 2019. Altere este valor de acordo com a sua versão, edição e local de instalação do Visual Studio.

1. No campo **diretório inicial** , especifique o diretório no qual o prompt de comando será iniciado. Escolha um valor, como **diretório do projeto** , selecionando a seta ao lado do campo.

1. Clique no botão **OK**.

   ![Caixa de diálogo Ferramentas externas com os valores de campo preenchidos.](./media/developer-command-prompt-for-vs/add-external-tool.png)

   O novo item de menu foi adicionado e você pode acessar o prompt de comando no menu Ferramentas.

   ![Item de menu do prompt de comando no Visual Studio](./media/developer-command-prompt-for-vs/command-prompt-vs-menu.png)

## <a name="see-also"></a>Confira também

- [Ferramentas do .NET Framework](index.md)
- [Gerenciando ferramentas externas](/visualstudio/ide/managing-external-tools)
- [Usar o conjunto de ferramentas do Microsoft C++ na linha de comando](/cpp/build/building-on-the-command-line)
