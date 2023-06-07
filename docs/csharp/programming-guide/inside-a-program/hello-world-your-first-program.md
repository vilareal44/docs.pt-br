---
title: Olá, Mundo--seu primeiro programa usando o Visual Studio no Windows ou o guia de programação do Mac-C#
description: O Visual Studio é um ambiente de desenvolvimento profissional com muitos recursos para o desenvolvimento do .NET. Use o Visual Studio para criar uma versão em C# do Olá, Mundo!
ms.date: 09/12/2019
f1_keywords:
- cs.program
- vs.csharp.startpage.firstapplication
helpviewer_keywords:
- examples [C#], Hello World
- Hello World example [C#]
ms.assetid: 6493182a-b0b6-4539-a719-518a168cb730
ms.openlocfilehash: b78937b8ba1c7d4718bfb6252dac705945336d2a
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87301821"
---
# <a name="hello-world----your-first-program"></a>Olá, Mundo--seu primeiro programa

Neste artigo, você usará o Visual Studio para criar o "Olá, Mundo!" tradicional programa. O Visual Studio é um IDE (ambiente de desenvolvimento integrado) profissional com muitos recursos projetados para o desenvolvimento do .NET. Você usará apenas alguns dos recursos do Visual Studio para criar esse programa. Para saber mais sobre o Visual Studio, consulte [introdução com o Visual C#](/visualstudio/ide/quickstart-csharp-console).

[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]

## <a name="create-a-new-application"></a>Crie um novo aplicativo

<!-- markdownlint-disable MD025 -->

# <a name="windows"></a>[Windows](#tab/windows)

Inicie o Visual Studio. Você verá a imagem a seguir no Windows:

![Tela de boas-vindas do Visual Studio no Windows](./media/hello-world-your-first-program/visual-studio-windows-start-screen.png)

Selecione **criar um novo projeto** no canto inferior direito da imagem. O Visual Studio exibe a caixa de diálogo **novo projeto** :

![Tela novo projeto do Visual Studio no Windows](./media/hello-world-your-first-program/visual-studio-windows-new-project.png)

> [!NOTE]
> Se esta for a primeira vez que você iniciou o Visual Studio, a lista de **modelos de projetos recentes** estará vazia.

Na caixa de diálogo novo projeto, escolha "aplicativo de console (.NET Core)" e, em seguida, pressione **Avançar**. Dê um nome ao seu projeto, como "HelloWorld", depois pressione **criar**.

O Visual Studio abre seu projeto. Já é um "Olá, Mundo!" básico . Pressione `Ctrl`  +  `F5` para executar o projeto. O Visual Studio cria seu projeto, convertendo o código-fonte em um executável. Em seguida, ele inicia uma janela de comando que executa o novo aplicativo. Você deve ver o seguinte texto na janela:

```console
Hello World!

C:\Program Files\dotnet\dotnet.exe (process 11964) exited with code 0.
Press any key to close this window . . .
```

Pressione uma tecla para fechar a janela.

# <a name="macos"></a>[macOS](#tab/macos)

Iniciar Visual Studio para Mac. Você verá a imagem a seguir no Mac:

![Tela de boas-vindas do Visual Studio no Mac](./media/hello-world-your-first-program/visual-studio-mac-start-screen.png)

> [!NOTE]
> Se esta for a primeira vez que você iniciou Visual Studio para Mac, a lista **projetos recentes** estará vazia.

Selecione **novo** no canto superior direito da imagem. Visual Studio para Mac exibe a caixa de diálogo **novo projeto** :

![Tela novo projeto do Visual Studio no Mac](./media/hello-world-your-first-program/visual-studio-mac-new-project.png)

Na caixa de diálogo novo projeto, escolha ".NET Core" e "aplicativo de console" e, em seguida, pressione **Avançar**. Você precisará selecionar a estrutura de destino. O padrão é bem, então pressione Avançar. Dê um nome ao seu projeto, como "HelloWorld", depois pressione **criar**. Você pode usar o local do projeto padrão. Não adicione este projeto ao controle do código-fonte.

Visual Studio para Mac abre seu projeto. Já é um "Olá, Mundo!" básico . Pressione `Ctrl`  +  `Fn`  +  `F5` para executar o projeto. Visual Studio para Mac compila seu projeto, convertendo o código-fonte em um executável. Em seguida, ele inicia uma janela de comando que executa o novo aplicativo. Você deve ver o seguinte texto na janela:

```console
Hello World!

Press any key to close this window . . .
```

Pressione uma tecla para encerrar a sessão.

---

## <a name="elements-of-a-c-program"></a>Elementos de um programa em C#

Vamos examinar as partes importantes deste programa. A primeira linha contém um comentário. Os caracteres `//` convertem o restante da linha em um comentário.

[!code-csharp[csProgGuide#32](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/progGuide.cs#32)]

Você também pode comentar um bloco de texto, colocando-o entre os caracteres `/*` e `*/`. Isso é mostrado no exemplo a seguir.

[!code-csharp[csProgGuide#33](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/progGuide.cs#33)]

Um aplicativo de console do C# deve conter um método `Main`, no qual o controle começa e termina. O método `Main` é o local em que você cria objetos e executa outros métodos.

O método `Main` é um método [estático](../../language-reference/keywords/static.md) que reside dentro de uma classe ou um struct. No exemplo de "Hello World!" anterior, ele reside em uma classe chamada `Hello`. Você pode declarar o método `Main` de uma das seguintes maneiras:

- Ele pode retornar `void`. Isso significa que o programa não retorna um valor.

[!code-csharp[csProgGuideMain#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideMain/CS/Class3.cs#12)]

- Ele também pode retornar um inteiro. O inteiro é o **código de saída** para seu aplicativo.

[!code-csharp[csProgGuideMain#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideMain/CS/Class3.cs#13)]

- Com qualquer um dos tipos de retorno, ele pode receber argumentos.

[!code-csharp[csProgGuideMain#19](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideMain/CS/Class3.cs#19)]

-ou-

[!code-csharp[csProgGuideMain#18](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideMain/CS/Class3.cs#18)]

O parâmetro do método `Main`, `args`, é um matriz `string` que contém os argumentos de linha de comando usados para invocar o programa.

Para obter mais informações sobre como usar argumentos de linha de comando, consulte os exemplos nos [argumentos principal () e de linha de comando](../main-and-command-args/index.md).

## <a name="input-and-output"></a>Entrada e saída

Os programas em C# geralmente usam os serviços de entrada/saída fornecidos pela biblioteca de tempo de execução do .NET. A instrução `System.Console.WriteLine("Hello World!");` usa o método <xref:System.Console.WriteLine%2A>. Esse é um dos métodos de saída da classe <xref:System.Console> na biblioteca em tempo de execução. Ele exibe seu parâmetro de cadeia de caracteres no fluxo de saída padrão, seguido por uma nova linha. Outros métodos <xref:System.Console> estão disponíveis para operações de saída e entrada diferentes. Se você incluir a diretiva `using System;` no início do programa, poderá usar diretamente as classes e métodos <xref:System> sem qualificá-los completamente. Por exemplo, você pode chamar `Console.WriteLine` em vez de `System.Console.WriteLine`:

[!code-csharp[csProgGuide#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/using.cs#1)]

[!code-csharp[csProgGuide#23](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/progGuide.cs#23)]

Para saber mais sobre os métodos de entrada/saída, veja <xref:System.IO>.

## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [Exemplos e tutoriais](../../../samples-and-tutorials/index.md)
- [Argumentos Main () e de linha de comando](../main-and-command-args/index.md)
- [Guia de Introdução ao Visual C#](/visualstudio/ide/quickstart-csharp-console)
