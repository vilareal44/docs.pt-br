---
title: Depurar um aplicativo de console do .NET Core usando Visual Studio para Mac
description: Saiba como depurar um aplicativo de console do .NET Core usando o Mac do Visual Studio.
ms.date: 06/08/2020
ms.openlocfilehash: 7e2a25266fab40b5ef1d0a38b8bbf06a6843746b
ms.sourcegitcommit: 3492dafceb5d4183b6b0d2f3bdf4a1abc4d5ed8c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/16/2020
ms.locfileid: "86416012"
---
# <a name="tutorial-debug-a-net-core-console-application-using-visual-studio-for-mac"></a>Tutorial: Depurar um aplicativo de console do .NET Core usando Visual Studio para Mac

Este tutorial apresenta as ferramentas de depuração disponíveis no Visual Studio para Mac.

## <a name="prerequisites"></a>Pré-requisitos

- Este tutorial funciona com o aplicativo de console que você cria em [criar um aplicativo de console do .NET Core no Visual Studio para Mac](with-visual-studio-mac.md).

## <a name="use-debug-build-configuration"></a>Usar configuração de compilação de depuração

*Debug* e *Release* são as configurações de compilação internas do Visual Studio. Você usa a configuração de compilação de depuração para depuração e a configuração de versão para a distribuição de versão final.

Na configuração de depuração, um programa é compilado com informações de depuração simbólicas completas e sem otimização. A otimização complica a depuração, porque a relação entre o código fonte e as instruções geradas é mais complexa. A configuração de versão de um programa não tem informações de depuração simbólicas e é totalmente otimizada.

Por padrão, o Visual Studio usa a configuração de compilação de depuração, portanto, você não precisa alterá-la antes da depuração.

1. Iniciar Visual Studio para Mac.

1. Abra o projeto que você criou em [criar um aplicativo de console do .NET Core no Visual Studio para Mac](with-visual-studio-mac.md).

   A configuração de build atual é mostrada na barra de ferramentas. A imagem da barra de ferramentas a seguir mostra que o Visual Studio está configurado para compilar a versão de depuração do aplicativo:

   :::image type="content" source="media/debugging-with-visual-studio-mac/visual-studio-toolbar-debug.png" alt-text="Barra de ferramentas do Visual Studio com depuração realçada":::

## <a name="set-a-breakpoint"></a>Definir um ponto de interrupção

Um *ponto de interrupção* interrompe temporariamente a execução do aplicativo antes de a linha com o ponto de interrupção ser executada.

1. Defina um ponto de interrupção na linha que exibe o nome, a data e a hora. Para fazer isso, coloque o cursor na linha de código e pressione <kbd>⌘</kbd> <kbd>\\</kbd> (<kbd>comando</kbd> + <kbd>\\</kbd> ). Outra maneira de definir um ponto de interrupção é selecionando **executar**  >  **alternância de ponto de interrupção** no menu.

   O Visual Studio indica a linha na qual o ponto de interrupção é definido, realçando-o e exibindo um ponto vermelho na margem esquerda.

   :::image type="content" source="media/debugging-with-visual-studio-mac/set-breakpoint-in-editor.png" alt-text="Janela Programa do Visual Studio com o ponto de interrupção definido":::

1. Pressione <kbd>⌘</kbd><kbd>↵</kbd> (<kbd>comando</kbd> + <kbd>Enter</kbd>) para iniciar o programa no modo de depuração. Outra maneira de iniciar a depuração é escolhendo **executar**  >  **Iniciar Depuração** no menu.

1. Insira uma cadeia de caracteres na janela do terminal quando o programa solicitar um nome e pressione <kbd>Enter</kbd>.

1. A execução do programa é interrompida quando atinge o ponto de interrupção, antes que o `Console.WriteLine` método seja executado.

   :::image type="content" source="media/debugging-with-visual-studio-mac/breakpoint-hit.png" alt-text="Captura de tela de um ponto de interrupção no Visual Studio":::

## <a name="use-the-immediate-window"></a>Usar a janela imediata

A janela **imediata** permite interagir com o aplicativo que você está depurando. Você pode alterar o valor de variáveis interativamente para ver como ele afeta seu programa.

1. Se a janela **imediata** não estiver visível, exiba-a escolhendo **Exibir**  >  **painéis de depuração**  >  **imediatamente**.

1. Insira `name = "Gracie"` na janela **imediata** e pressione <kbd>Enter</kbd>.

1. Insira `date = date.AddDays(1)` na janela **imediata** e pressione <kbd>Enter</kbd>.

   A janela **imediato** exibe o novo valor da variável de cadeia de caracteres e as propriedades do <xref:System.DateTime> valor.

   :::image type="content" source="media/debugging-with-visual-studio-mac/immediate-window.png" alt-text="Janela imediata no Visual Studio":::

   A janela **Locais** exibe os valores das variáveis que são definidas no método que está sendo executado no momento. Os valores das variáveis que você acabou de alterar são atualizados na janela **locais** .

   :::image type="content" source="media/debugging-with-visual-studio-mac/locals-window.png" alt-text="Janela locais no Visual Studio":::

1. Pressione <kbd>⌘</kbd><kbd>↵</kbd> (<kbd>comando</kbd> + <kbd>Enter</kbd>) para continuar a depuração.

   Os valores exibidos no terminal correspondem às alterações feitas na janela **imediata** .

   Se você não vir o terminal, selecione **terminal-HelloWorld** na barra de navegação inferior.

   :::image type="content" source="media/debugging-with-visual-studio-mac/terminal-hello-world.png" alt-text="Olá, Mundo de terminal na barra de navegação inferior":::

1. Pressione qualquer tecla para sair do programa.

1. Feche a janela do terminal.

## <a name="set-a-conditional-breakpoint"></a>Definir um ponto de interrupção condicional

O programa exibe uma cadeia de caracteres que o usuário insere. O que acontecerá se o usuário não inserir nada? Você pode testá-lo com um recurso de depuração útil chamado de *ponto de interrupção condicional*.

1. <kbd>Ctrl</kbd>-clique no ponto vermelho que representa o pontos de interrupção. No menu de contexto, selecione **Editar ponto de interrupção**.

1. Na caixa de diálogo **Editar ponto de interrupção** , insira o seguinte código no campo a seguir **e a condição a seguir é verdadeira**e selecione **aplicar**.

   ```csharp
   String.IsNullOrEmpty(name)
   ```

   :::image type="content" source="media/debugging-with-visual-studio-mac/breakpoint-settings.png" alt-text="Editor mostrando o painel de configurações de ponto de interrupção":::

   Cada vez que o ponto de interrupção for atingido, o depurador chamará o `String.IsNullOrEmpty(name)` método e interromperá essa linha somente se a chamada do método retornar `true` .

   Em vez de uma expressão condicional, você pode especificar uma *contagem de ocorrências*, que interrompe a execução do programa antes que uma instrução seja executada um número especificado de vezes.

1. Pressione <kbd>⌘</kbd><kbd>↵</kbd> (<kbd>comando</kbd> + <kbd>Enter</kbd>) para iniciar a depuração.

1. Na janela do terminal, pressione <kbd>Enter</kbd> quando for solicitado a inserir seu nome.

   Como a condição que você especificou ( `name` é `null` ou <xref:System.String.Empty?displayProperty=nameWithType> ) foi satisfeita, a execução do programa é interrompida quando atinge o ponto de interrupção.

1. Selecione a janela **locais** , que mostra os valores das variáveis que são locais para o método atualmente em execução. Nesse caso, `Main` é o método atualmente em execução. Observe que o valor da `name` variável é `""` , ou seja, <xref:System.String.Empty?displayProperty=nameWithType> .

1. Você também pode ver que o valor é uma cadeia de caracteres vazia inserindo o `name` nome da variável na janela **imediata** e pressionando <kbd>Enter</kbd>.

   :::image type="content" source="media/debugging-with-visual-studio-mac/immediate-window-output.png" alt-text="A janela imediata que mostra o nome é uma cadeia de caracteres vazia":::

1. Pressione <kbd>⌘</kbd><kbd>↵</kbd> (<kbd>comando</kbd> + <kbd>Enter</kbd>) para continuar a depuração.

1. Na janela do terminal, pressione qualquer tecla para sair do programa.

1. Feche a janela do terminal.

1. Limpe o ponto de interrupção clicando no Red Dot na margem esquerda da janela de código. Outra maneira de limpar um ponto de interrupção é escolhendo **executar > alternar ponto de interrupção** enquanto a linha de código está selecionada.

## <a name="step-through-a-program"></a>Percorrer um programa

O Visual Studio também permite percorrer linha por linha de um programa e monitorar sua execução. Normalmente, você definiria um ponto de interrupção e seguirá o fluxo do programa por meio de uma pequena parte do código do programa. Como esse programa é pequeno, você pode percorrer todo o programa.

1. Defina um ponto de interrupção na chave que marca o início do `Main` método (pressione o <kbd>comando</kbd> + <kbd>\\</kbd> ).

1. Pressione <kbd>⌘</kbd><kbd>↵</kbd> (<kbd>comando</kbd> + <kbd>Enter</kbd>) para iniciar a depuração.

   O Visual Studio é interrompido na linha com o ponto de interrupção.

1. Pressione <kbd>⇧</kbd><kbd>⌘</kbd><kbd>i</kbd> (<kbd>Shift</kbd> + <kbd>Command</kbd> + <kbd>i</kbd>) ou selecione **executar**  >  **etapa em** para avançar uma linha.

   O Visual Studio realça e exibe uma seta ao lado da próxima linha de execução.

   :::image type="content" source="media/debugging-with-visual-studio-mac/step-into-method.png" alt-text="Método Step Into do Visual Studio":::

   Neste ponto, a janela **locais** mostra que a `args` matriz está vazia e `name` `date` tem valores padrão. Além disso, o Visual Studio abriu um terminal em branco.

1. Pressione <kbd>⇧</kbd><kbd>⌘</kbd><kbd>i</kbd> (<kbd>Shift</kbd> + <kbd>Command</kbd> + <kbd>i</kbd>).

   O Visual Studio destaca a instrução que inclui a atribuição de variável `name`. A janela **locais** mostra que `name` é `null` , e o terminal exibe a cadeia de caracteres "Qual é seu nome?".

1. Responda ao prompt inserindo uma cadeia de caracteres na janela do console e pressionando <kbd>Enter</kbd>.

1. Pressione <kbd>⇧</kbd><kbd>⌘</kbd><kbd>i</kbd> (<kbd>Shift</kbd> + <kbd>Command</kbd> + <kbd>i</kbd>).

   O Visual Studio destaca a instrução que inclui a atribuição de variável `date`. A janela **locais** mostra o valor retornado pela chamada para o <xref:System.Console.ReadLine%2A?displayProperty=nameWithType> método. O terminal exibe a cadeia de caracteres que você inseriu no prompt.

1. Pressione <kbd>⇧</kbd><kbd>⌘</kbd><kbd>i</kbd> (<kbd>Shift</kbd> + <kbd>Command</kbd> + <kbd>i</kbd>).

   A janela **locais** mostra o valor da `date` variável após a atribuição da <xref:System.DateTime.Now?displayProperty=nameWithType> propriedade. O terminal não foi alterado.

1. Pressione <kbd>⇧</kbd><kbd>⌘</kbd><kbd>i</kbd> (<kbd>Shift</kbd> + <kbd>Command</kbd> + <kbd>i</kbd>).

   O Visual Studio chama o método <xref:System.Console.WriteLine(System.String,System.Object,System.Object)?displayProperty=nameWithType>. O terminal exibe a cadeia de caracteres formatada.

1. Pressione <kbd>⇧</kbd><kbd>⌘</kbd><kbd>u</kbd> (<kbd>Shift</kbd> + <kbd>Command</kbd> + <kbd>u</kbd>) ou selecione **executar**  >  **etapa para sair**.

   O terminal exibe uma mensagem e aguarda que você pressione uma tecla.

1. Pressione qualquer tecla para sair do programa.

## <a name="use-release-build-configuration"></a>Usar a configuração de Build de versão

Depois de testar a versão de depuração do seu aplicativo, você também deve compilar e testar a versão de lançamento. A versão de lançamento incorpora otimizações de compilador que podem afetar negativamente o comportamento de um aplicativo. Por exemplo, as otimizações do compilador que são projetadas para melhorar o desempenho podem criar condições de corrida em aplicativos multissegmentados.

Para compilar e testar a versão de lançamento do aplicativo de console, execute as seguintes etapas:

1. Altere a configuração de Build na barra de ferramentas de **depurar** para **versão**.

   :::image type="content" source="media/debugging-with-visual-studio-mac/visual-studio-toolbar-release.png" alt-text="barra de ferramentas padrão do Visual Studio com depuração realçada":::

1. Pressione <kbd>⌥</kbd><kbd>⌘</kbd><kbd>↵</kbd> (<kbd>option</kbd> + <kbd>comando</kbd>Option + <kbd>Enter</kbd>) para executar sem depuração.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você usou as ferramentas de depuração do Visual Studio. No próximo tutorial, você publica uma versão implantável do aplicativo.

> [!div class="nextstepaction"]
> [Publicar um aplicativo de console do .NET Core com o Visual Studio para Mac](publishing-with-visual-studio-mac.md)
