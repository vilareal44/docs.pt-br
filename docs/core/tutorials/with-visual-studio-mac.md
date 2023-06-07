---
title: Criar um aplicativo de console do .NET Core usando Visual Studio para Mac
description: Saiba como criar um aplicativo de console do .NET Core usando Visual Studio para Mac.
ms.date: 06/02/2020
ms.openlocfilehash: 9cab838eaab2c59d8a0270267514f57acb7c60fb
ms.sourcegitcommit: 1eae045421d9ea2bfc82aaccfa5b1ff1b8c9e0e4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84811665"
---
# <a name="tutorial-create-a-net-core-console-application-using-visual-studio-for-mac"></a>Tutorial: criar um aplicativo de console do .NET Core usando Visual Studio para Mac

Este tutorial mostra como criar e executar um aplicativo de console do .NET Core usando Visual Studio para Mac.

> [!NOTE]
> Seus comentários são muito importantes. Há duas maneiras de enviar comentários à equipe de desenvolvimento no Visual Studio para Mac:
>
> * Em Visual Studio para Mac, selecione **ajuda**para  >  **relatar um problema** no menu ou **relatar um problema** na tela de boas-vindas, que abrirá uma janela para o arquivamento de um relatório de bug. Você pode acompanhar seus comentários no portal [Developer Community (Comunidade do Desenvolvedor)](https://developercommunity.visualstudio.com/spaces/8/index.html).
> * Para fazer uma sugestão, selecione **ajuda**  >  **fornecer uma sugestão** no menu ou **forneça uma sugestão** na tela de boas-vindas, que levará você para a [página da Web do Visual Studio para Mac Developer Community](https://developercommunity.visualstudio.com/content/idea/post.html?space=41).

## <a name="prerequisites"></a>Pré-requisitos

* [Visual Studio para Mac versão 8,6 ou posterior](https://visualstudio.microsoft.com/vs/mac/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link). Selecione a opção para instalar o .NET Core. A instalação do Xamarin é opcional para o desenvolvimento do .NET Core. Para saber mais, consulte os recursos a seguir:

  * [Tutorial: instalar o Visual Studio para Mac](/visualstudio/mac/installation).
  * [Versões do MacOS com suporte](../install/dependencies.md?pivots=os-macos).
  * [Versões do .NET Core com suporte pelo Visual Studio para Mac](/visualstudio/mac/net-core-support).

## <a name="create-the-app"></a>Criar o aplicativo

Crie um projeto de aplicativo de console do .NET Core chamado "HelloWorld".

1. Iniciar Visual Studio para Mac.

1. Selecione **novo** na janela iniciar.

   :::image type="content" source="media/with-visual-studio-mac/visual-studio-mac-new-project.png" alt-text="O botão Novo na tela de Boas-vindas do Visual Studio para Mac":::

1. Na caixa de diálogo **novo projeto** , selecione **aplicativo** no nó **Web e console** . Selecione o modelo de **aplicativo de console** e selecione **Avançar**.

   :::image type="content" source="media/with-visual-studio-mac/visual-studio-mac-new-dialog.png" alt-text="Lista de modelos de novo projeto":::

1. Na lista **suspensa estrutura de destino** da caixa de diálogo **configurar seu novo aplicativo de console** , selecione **.NET Core 3,1**e selecione **Avançar**.

   :::image type="content" source="media/with-visual-studio-mac/target-framework.png" alt-text="Selecionar estrutura de destino":::

1. Digite "HelloWorld" para o **nome do projeto**e selecione **criar**.

   :::image type="content" source="media/with-visual-studio-mac/visual-studio-mac-new-options.png" alt-text="Configurar a caixa de diálogo Novo Aplicativo de Console":::

O modelo cria um simples aplicativo “Olá, Mundo”. Ele chama o <xref:System.Console.WriteLine(System.String)?displayProperty=nameWithType> método para exibir "Olá, mundo!" na janela do terminal.

O código de modelo define uma classe, `Program` , com um único método, `Main` , que usa uma <xref:System.String> matriz como um argumento:

```csharp
using System;

namespace HelloWorld
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

`Main` é o ponto de entrada do aplicativo, o método que é chamado automaticamente pelo runtime quando ele inicia o aplicativo. Todos os argumentos de linha de comando fornecidos quando o aplicativo é iniciado estão disponíveis na `args` matriz.

## <a name="run-the-app"></a>Executar o aplicativo

1. Pressione <kbd>⌥</kbd><kbd>⌘</kbd><kbd>↵</kbd> (<kbd>option</kbd> + <kbd>comando</kbd>Option + <kbd>Enter</kbd>) para executar o aplicativo sem depuração.

   :::image type="content" source="media/with-visual-studio-mac/visual-studio-mac-output.png" alt-text="O terminal mostra Olá, Mundo!":::

1. Feche a janela do **terminal** .

## <a name="enhance-the-app"></a>Aprimorar o aplicativo

Aprimore o aplicativo para solicitar ao usuário seu nome e exibi-lo junto com a data e hora.

1. No *Program.cs*, substitua o conteúdo do `Main` método, que é a linha que chama `Console.WriteLine` , com o seguinte código:

   :::code language="csharp" source="./snippets/with-visual-studio/csharp/Program.cs" id="MainMethod":::

   Esse código exibe "Qual é o seu nome?" na janela do console e aguarda até que o usuário insira uma cadeia de caracteres seguida pela tecla <kbd>Enter</kbd> . Ele armazena essa cadeia de caracteres em uma variável chamada `name` . Ele também recupera o valor da propriedade <xref:System.DateTime.Now?displayProperty=nameWithType>, que contém a hora local atual e o atribui a uma variável chamada `date`. Por fim, ele exibe esses valores na janela do console.

   O `\n` representa um caractere de nova linha.

   O cifrão ( `$` ) na frente de uma cadeia de caracteres permite que você coloque expressões como nomes de variáveis entre chaves na cadeia de caracteres. O valor da expressão é inserido na cadeia de caracteres no lugar da expressão. Essa sintaxe é conhecida como [cadeias de caracteres interpoladas](../../csharp/language-reference/tokens/interpolated.md).

1. Pressione <kbd>⌥</kbd><kbd>⌘</kbd><kbd>↵</kbd> (<kbd>option</kbd> + <kbd>comando</kbd>Option + <kbd>Enter</kbd>) para executar o aplicativo.

1. Responda ao prompt digitando um nome e pressionando <kbd>Enter</kbd>.

   :::image type="content" source="media/with-visual-studio-mac/hello-world-update.png" alt-text="Terminal mostra a saída de programa modificada":::

1. Feche o terminal.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você criou um aplicativo de console do .NET Core. No próximo tutorial, você depurará o aplicativo.

> [!div class="nextstepaction"]
> [Depurar um aplicativo de console do .NET Core no Visual Studio](debugging-with-visual-studio-mac.md)
