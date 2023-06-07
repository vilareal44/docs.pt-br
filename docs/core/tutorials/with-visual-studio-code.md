---
title: Criar um aplicativo de console do .NET Core usando Visual Studio Code
description: Saiba como criar um aplicativo de console do .NET Core usando Visual Studio Code e o CLI do .NET Core.
ms.date: 05/22/2020
ms.openlocfilehash: 466a1353b574711a73570428569b58eab7ad8135
ms.sourcegitcommit: 1eae045421d9ea2bfc82aaccfa5b1ff1b8c9e0e4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84811694"
---
# <a name="tutorial-create-a-net-core-console-application-using-visual-studio-code"></a>Tutorial: criar um aplicativo de console do .NET Core usando Visual Studio Code

Este tutorial mostra como criar e executar um aplicativo de console do .NET Core usando Visual Studio Code e o CLI do .NET Core. Tarefas de projeto, como criar, compilar e executar um projeto, são feitas usando o CLI do .NET Core. Você pode seguir este tutorial com um editor de código diferente e executar comandos em um terminal, se preferir.

## <a name="prerequisites"></a>Pré-requisitos

1. [Visual Studio Code](https://code.visualstudio.com/) com a [extensão C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) instalada. Para obter informações sobre como instalar extensões em Visual Studio Code, consulte [vs Code Marketplace de extensão](https://code.visualstudio.com/docs/editor/extension-gallery).
2. O [SDK do .NET Core 3,1 ou posterior](https://dotnet.microsoft.com/download)

## <a name="create-the-app"></a>Criar o aplicativo

Crie um projeto de aplicativo de console do .NET Core chamado "HelloWorld".

1. Inicie o Visual Studio Code.

1. Selecione **arquivo**  >  **abrir pasta** (**arquivo**  >  **aberto...** no MacOS) no menu principal.

1. Na caixa de diálogo **abrir pasta** , crie uma pasta *HelloWorld* e clique em **Selecionar pasta** (**aberta** no MacOS).

   O nome da pasta torna-se o nome do projeto e o nome do namespace por padrão. Você adicionará código posteriormente no tutorial que assume que o namespace do projeto é `HelloWorld` .

1. Abra o **terminal** no Visual Studio Code selecionando **Exibir**  >  **terminal** no menu principal.

   O **terminal** é aberto com o prompt de comando na pasta *HelloWorld* .

1. No **terminal**, digite o seguinte comando:

   ```dotnetcli
   dotnet new console
   ```

O modelo cria um simples aplicativo “Olá, Mundo”. Ele chama o <xref:System.Console.WriteLine(System.String)?displayProperty=nameWithType> método para exibir "Olá, mundo!" na janela do console.

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

`Main` é o ponto de entrada do aplicativo, o método que é chamado automaticamente pelo runtime quando ele inicia o aplicativo. Quaisquer argumentos de linha de comando fornecidos quando o aplicativo for iniciado estão disponíveis na matriz *args*.

## <a name="run-the-app"></a>Executar o aplicativo

Execute o seguinte comando no **terminal**:

```dotnetcli
dotnet run
```

O programa exibe "Olá, Mundo!" e termina.

![O comando de execução dotnet](media/with-visual-studio-code/dotnet-run-command.png)

## <a name="enhance-the-app"></a>Aprimorar o aplicativo

Aprimore o aplicativo para solicitar ao usuário seu nome e exibi-lo junto com a data e hora.

1. Abra *Program.cs* clicando nele.

   Na primeira vez que um arquivo do C# é aberto no Visual Studio Code, o [OmniSharp](https://www.omnisharp.net/) é carregado no editor.

   ![Abra o arquivo Program.cs](media/with-visual-studio-code/open-program-cs.png)

1. Selecione **Sim** quando Visual Studio Code solicitar que você adicione os ativos ausentes para compilar e depurar seu aplicativo.

   ![Prompt para ativos ausentes](media/with-visual-studio-code/missing-assets.png)

1. Substitua o conteúdo do `Main` método em *Program.cs*, que é a linha que chama `Console.WriteLine` , com o seguinte código:

   :::code language="csharp" source="./snippets/with-visual-studio/csharp/Program.cs" id="MainMethod":::

   Esse código exibe "Qual é o seu nome?" na janela do console e aguarda até que o usuário insira uma cadeia de caracteres seguida pela tecla <kbd>Enter</kbd> . Ele armazena essa cadeia de caracteres em uma variável chamada `name` . Ele também recupera o valor da propriedade <xref:System.DateTime.Now?displayProperty=nameWithType>, que contém a hora local atual e o atribui a uma variável chamada `date`. Por fim, ele exibe esses valores na janela do console.

   O `\n` representa um caractere de nova linha.

   O cifrão ( `$` ) na frente de uma cadeia de caracteres permite que você coloque expressões como nomes de variáveis entre chaves na cadeia de caracteres. O valor da expressão é inserido na cadeia de caracteres no lugar da expressão. Essa sintaxe é conhecida como [cadeias de caracteres interpoladas](../../csharp/language-reference/tokens/interpolated.md).

1. Salve suas alterações.

   > [!IMPORTANT]
   > No Visual Studio Code, você precisa salvar explicitamente as alterações. Ao contrário do Visual Studio, as alterações de arquivo não são salvas automaticamente quando você compila e executa um aplicativo.

1. Execute o programa novamente:

   ```dotnetcli
   dotnet run
   ```

1. Responda ao prompt digitando um nome e pressionando a tecla <kbd>Enter</kbd> .

   :::image type="content" source="media/debugging-with-visual-studio-code/run-modified-program.png" alt-text="Janela do terminal com saída de programa modificada":::

1. Pressione qualquer tecla para sair do programa.

## <a name="additional-resources"></a>Recursos adicionais

- [Configurar o Visual Studio Code](https://code.visualstudio.com/docs/setup/setup-overview)

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você criou um aplicativo de console do .NET Core. No próximo tutorial, você depurará o aplicativo.

> [!div class="nextstepaction"]
> [Depurar um aplicativo de console do .NET Core usando Visual Studio Code](debugging-with-visual-studio-code.md)
