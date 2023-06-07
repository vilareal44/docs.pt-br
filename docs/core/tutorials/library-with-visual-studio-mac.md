---
title: Criar uma biblioteca de classes de .NET Standard usando Visual Studio para Mac
description: Saiba como criar uma biblioteca de classes de .NET Standard usando Visual Studio para Mac.
ms.date: 06/08/2020
ms.openlocfilehash: 8e1e4ca3bc1b12d889b847d80318f3d6cd1bbe46
ms.sourcegitcommit: 3492dafceb5d4183b6b0d2f3bdf4a1abc4d5ed8c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/16/2020
ms.locfileid: "86416000"
---
# <a name="tutorial-create-a-net-standard-library-using-visual-studio-for-mac"></a>Tutorial: criar uma biblioteca de .NET Standard usando Visual Studio para Mac

Neste tutorial, você cria uma biblioteca de classes que contém um único método de manipulação de cadeia de caracteres. Implemente-o como um [método de extensão](../../csharp/programming-guide/classes-and-structs/extension-methods.md) para que você possa chamá-lo como se fosse um membro da <xref:System.String> classe.

Uma *biblioteca de classes* define tipos e métodos que são chamados por um aplicativo. Uma biblioteca de classes que tem como alvo .NET Standard 2,1 pode ser usada por um aplicativo que se destina a qualquer implementação .NET que ofereça suporte à versão 2,1 do .NET Standard. Ao concluir a biblioteca de classes, você pode distribuí-la como um componente de terceiros ou como um componente agrupado com um ou mais aplicativos.

> [!NOTE]
> Seus comentários são muito importantes. Há duas maneiras de enviar comentários à equipe de desenvolvimento no Visual Studio para Mac:
>
> - Em Visual Studio para Mac, selecione **ajuda**  >  **relatar um problema** no menu ou **relatar um problema** na tela de boas-vindas, que abre uma janela para o arquivamento de um relatório de bug. Você pode acompanhar seus comentários no portal [Developer Community (Comunidade do Desenvolvedor)](https://developercommunity.visualstudio.com/spaces/41/index.html).
> - Para fazer uma sugestão, selecione **ajuda**  >  **fornecer uma sugestão** no menu ou **forneça uma sugestão** na tela de boas-vindas, que leva você para a [página da Web do Visual Studio para Mac Developer Community](https://developercommunity.visualstudio.com/content/idea/post.html?space=41).

## <a name="prerequisites"></a>Pré-requisitos

* [Instale Visual Studio para Mac versão 8,6 ou posterior](https://visualstudio.microsoft.com/vs/mac/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link). Selecione a opção para instalar o .NET Core. A instalação do Xamarin é opcional para o desenvolvimento do .NET Core. Para obter mais informações, consulte os seguintes recursos:

  * [Tutorial: instalar o Visual Studio para Mac](/visualstudio/mac/installation).
  * [Versões do MacOS com suporte](../install/dependencies.md?pivots=os-macos).
  * [Versões do .NET Core com suporte pelo Visual Studio para Mac](/visualstudio/mac/net-core-support).

## <a name="create-a-solution-with-a-class-library-project"></a>Criar uma solução com um projeto de biblioteca de classes

Uma solução do Visual Studio serve como um contêiner para um ou mais projetos. Crie uma solução e um projeto de biblioteca de classes na solução. Você adicionará mais projetos relacionados à mesma solução posteriormente.

1. Iniciar Visual Studio para Mac.

1. Na janela iniciar, selecione **novo projeto**.

1. Na caixa de diálogo **novo projeto** no nó **várias plataformas** , selecione **biblioteca**, em seguida, selecione o modelo de **biblioteca de .net Standard** e selecione **Avançar**.

   :::image type="content" source="media/library-with-visual-studio-mac/visual-studio-mac-new-project.png" alt-text="Caixa de diálogo novo projeto":::

1. Na caixa de diálogo **configurar sua nova biblioteca de .net Standard** , escolha ".net Standard 2,1" e selecione **Avançar**.

   :::image type="content" source="media/library-with-visual-studio-mac/choose-net-std-21.png" alt-text="Escolha .NET Standard 2,1":::

1. Nomeie o projeto "StringLibrary" e a solução "ClassLibraryProjects". Deixe **criar um diretório de projeto dentro do diretório de solução** selecionado. Selecione **Criar**.

   :::image type="content" source="media/library-with-visual-studio-mac/visual-studio-mac-new-project-options.png" alt-text="Opções da caixa de diálogo Novo projeto do Visual Studio para Mac":::

1. No menu principal, selecione **Exibir**  >  **painéis**  >  **solução**e selecione o ícone de encaixe para manter o pad aberto.

   :::image type="content" source="media/library-with-visual-studio-mac/solution-dock-icon.png" alt-text="Ícone de encaixe para o painel de solução":::

1. No painel de **solução** , expanda o `StringLibrary` nó para revelar o arquivo de classe fornecido pelo modelo, *Class1.cs*. <kbd>Ctrl</kbd>-clique no arquivo, selecione **renomear** no menu de contexto e renomeie o arquivo para *StringLibrary.cs*. Abra o arquivo e substitua o conteúdo pelo código a seguir:

   :::code language="csharp" source="./snippets/library-with-visual-studio/csharp/StringLibrary/Class1.cs":::

1. Pressione <kbd>⌘</kbd><kbd>S</kbd> (<kbd>Command</kbd> + <kbd>s</kbd>) para salvar o arquivo.

1. Selecione **Erros** na margem da parte inferior da janela do IDE para abrir o painel **Erros**. Selecione o botão **Compilar Saída**.

   :::image type="content" source="media/library-with-visual-studio-mac/visual-studio-mac-error-button.png" alt-text="Margem inferior do IDE do Visual Studio para Mac mostrando o botão Erros":::

1. Selecione **Compilar**  >  **compilar tudo** no menu.

   A solução é compilada. O painel de saída da compilação mostra que a compilação foi bem-sucedida.

   :::image type="content" source="media/library-with-visual-studio-mac/visual-studio-mac-build-panel.png" alt-text="Painel de saída do Build do Visual Studio do painel Erros com a mensagem Build bem-sucedido":::

## <a name="add-a-console-app-to-the-solution"></a>Adicionar um aplicativo de console à solução

Adicione um aplicativo de console que usa a biblioteca de classes. O aplicativo solicitará que o usuário insira uma cadeia de caracteres e relate se a cadeia de caracteres começa com um caractere maiúsculo.

1. No painel de **solução** , <kbd>pressione CTRL +</kbd>clique na `ClassLibraryProjects` solução. Adicione um novo projeto de **aplicativo de console** selecionando o modelo nos modelos de aplicativo **Web e de console**  >  **App** e selecione **Avançar**.

1. Selecione **.NET Core 3,1** como a **estrutura de destino** e selecione **Avançar**.

1. Nomeie a **demonstração**do projeto. Selecione **Criar** para criar o projeto na solução.

   :::image type="content" source="media/library-with-visual-studio-mac/add-showcase-project.png" alt-text="Adicionar projeto de demonstração":::

1. Abra o arquivo *Program.cs* . Substitua o código pelo código a seguir:

   :::code language="csharp" source="./snippets/library-with-visual-studio/csharp/ShowCase/Program.cs":::

   O programa solicita que o usuário insira uma cadeia de caracteres. Ele indica se a cadeia de caracteres começa com um caractere maiúsculo. Se o usuário pressionar a tecla <kbd>Enter</kbd> sem inserir uma cadeia de caracteres, o aplicativo será encerrado e a janela do console será fechada.

   O código usa a variável `row` ​​para manter uma contagem do número de linhas de dados gravadas na janela do console. Sempre que for maior ou igual a 25, o código limpará a janela do console e exibirá uma mensagem para o usuário.

## <a name="add-a-project-reference"></a>Adicionar uma referência ao projeto

Inicialmente, o novo projeto de aplicativo de console não tem acesso à biblioteca de classes. Para permitir que ele chame métodos na biblioteca de classes, crie uma referência de projeto para o projeto de biblioteca de classes.

1. No painel **soluções** , <kbd>pressione CTRL</kbd>e clique no nó **dependências** do novo projeto de **demonstração** . No menu de contexto, selecione **Adicionar referência**.

1. Na caixa de diálogo **referências** , selecione **StringLibrary** e selecione **OK**.

## <a name="run-the-app"></a>Executar o aplicativo

1. <kbd>Ctrl</kbd>-clique no projeto de demonstração e selecione **Executar projeto** no menu de contexto.

1. Experimente o programa digitando cadeias de caracteres e pressionando <kbd>Enter</kbd>e pressione <kbd>Enter</kbd> para sair.

   :::image type="content" source="media/library-with-visual-studio-mac/visual-studio-mac-console-window.png" alt-text="Janela do console do Visual Studio para Mac mostrando o aplicativo em execução":::

## <a name="additional-resources"></a>Recursos adicionais

* [Desenvolver bibliotecas com o CLI do .NET Core](libraries.md)
* [.Net Standard versões e plataformas às quais eles dão suporte](../../standard/net-standard.md).
* [Notas sobre a versão do Visual Studio 2019 para Mac](/visualstudio/releasenotes/vs2019-mac-relnotes)

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você criou uma solução e um projeto de biblioteca e adicionou um projeto de aplicativo de console que usa a biblioteca do. No próximo tutorial, você adicionará um projeto de teste de unidade à solução.

> [!div class="nextstepaction"]
> [Testar uma biblioteca de .NET Standard com o .NET Core usando Visual Studio para Mac](testing-library-with-visual-studio-mac.md)
