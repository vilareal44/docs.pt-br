---
title: Depurar um aplicativo de console do .NET Core usando Visual Studio Code
description: Saiba como depurar um aplicativo de console do .NET Core usando Visual Studio Code.
ms.date: 05/26/2020
ms.openlocfilehash: 40e9b114df1bd12fb05bfb773781d6009d087a06
ms.sourcegitcommit: 1cbd77da54405ea7dba343ac0334fb03237d25d2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84702121"
---
# <a name="tutorial-debug-a-net-core-console-application-using-visual-studio-code"></a>Tutorial: Depurar um aplicativo de console do .NET Core usando Visual Studio Code

Este tutorial apresenta as ferramentas de depuração disponíveis no Visual Studio Code para trabalhar com aplicativos .NET Core.

## <a name="prerequisites"></a>Pré-requisitos

- Este tutorial funciona com o aplicativo de console que você cria em [criar um aplicativo de console do .NET Core no Visual Studio Code](with-visual-studio-code.md).

## <a name="use-debug-build-configuration"></a>Usar configuração de compilação de depuração

*Debug* e *Release* são as configurações de compilação internas do .NET Core. Você usa a configuração de compilação de depuração para depuração e a configuração de versão para a distribuição de versão final.

Na configuração de depuração, um programa é compilado com informações de depuração simbólicas completas e sem otimização. A otimização complica a depuração, porque a relação entre o código fonte e as instruções geradas é mais complexa. A configuração de versão de um programa não tem informações de depuração simbólicas e é totalmente otimizada.

Por padrão, Visual Studio Code configurações de inicialização usam a configuração de compilação de depuração, portanto, você não precisa alterá-la antes da depuração.

1. Inicie o Visual Studio Code.

1. Abra a pasta do projeto que você criou em [criar um aplicativo de console do .NET Core no Visual Studio Code](with-visual-studio-code.md).

## <a name="set-a-breakpoint"></a>Definir um ponto de interrupção

Um *ponto de interrupção* interrompe temporariamente a execução do aplicativo antes de a linha com o ponto de interrupção ser executada.

1. Abra o arquivo *Program.cs* .

1. Defina um *ponto de interrupção* na linha que exibe o nome, a data e a hora clicando na margem esquerda da janela de código. A margem esquerda está à esquerda dos números de linha. Outras maneiras de definir um ponto de interrupção são pressionando <kbd>F9</kbd> ou selecionando **executar**  >  **alternância de ponto de interrupção** no menu enquanto a linha de código está selecionada.

   Visual Studio Code indica a linha na qual o ponto de interrupção é definido exibindo um ponto vermelho na margem esquerda.

   :::image type="content" source="media/debugging-with-visual-studio-code/breakpoint-set.png" alt-text="Conjunto de pontos de interrupção":::

## <a name="set-up-for-terminal-input"></a>Configurar para entrada de terminal

O ponto de interrupção está localizado após uma `Console.ReadLine` chamada de método. O **console de depuração** não aceita a entrada de terminal para um programa em execução. Para lidar com a entrada de terminal durante a depuração, você pode usar o terminal integrado (um dos Visual Studio Code Windows) ou um terminal externo. Para este tutorial, você usa o terminal integrado.

1. Abra *. vscode/launch.jsem*.

1. Altere a `console` configuração para `integratedTerminal` .

   De:

   ```
   "console": "internalConsole",
   ```

   Para:

   ```
   "console": "integratedTerminal",
   ```

1. Salve suas alterações.

## <a name="start-debugging"></a>Iniciar a depuração

1. Abra a exibição de depuração selecionando o ícone de depuração no menu do lado esquerdo.

   :::image type="content" source="media/debugging-with-visual-studio-code/select-debug-pane.png" alt-text="Abrir a guia Depurar no Visual Studio Code":::

1. Selecione a seta verde na parte superior do painel, ao lado de **inicialização do .NET Core (console)**. Outra maneira de iniciar o programa no modo de depuração é escolhendo **executar**  >  **Iniciar Depuração** no menu.

   :::image type="content" source="media/debugging-with-visual-studio-code/start-debugging.png" alt-text="Iniciar Depuração":::

1. Selecione a guia **terminal** para ver o "Qual é seu nome?" avisar que o programa é exibido antes de aguardar uma resposta.

   :::image type="content" source="media/debugging-with-visual-studio-code/select-terminal.png" alt-text="Selecione a guia terminal":::

1. Insira uma cadeia de caracteres na janela do **terminal** em resposta à solicitação de um nome e pressione <kbd>Enter</kbd>.

   A execução do programa para quando ele atinge o ponto de interrupção e antes de o método `Console.WriteLine` ser executado. A seção **locais** da janela **variáveis** exibe os valores das variáveis que são definidas no método em execução no momento.

   :::image type="content" source="media/debugging-with-visual-studio-code/breakpoint-hit.png" alt-text="Impacto de ponto de interrupção, mostrando locais":::

## <a name="use-the-debug-console"></a>Usar o console de depuração

A janela do **console de depuração** permite interagir com o aplicativo que você está depurando. Você pode alterar o valor de variáveis para ver como ele afeta seu programa.

1. Selecione a guia **console de depuração** .

1. Digite `name = "Gracie"` no prompt na parte inferior da janela do **console de depuração** e pressione a tecla <kbd>Enter</kbd> .

   :::image type="content" source="media/debugging-with-visual-studio-code/change-variable-values.png" alt-text="Alterar valores de variáveis":::

1. Digite `date = DateTime.Parse("2019-11-16T17:25:00Z").ToUniversalTime()` na parte inferior da janela do **console de depuração** e pressione a tecla <kbd>Enter</kbd> .

   A janela **variáveis** exibe os novos valores das `name` variáveis e `date` .

1. Continue a execução do programa selecionando o botão **continuar** na barra de ferramentas. Outra maneira de continuar é pressionando <kbd>F5</kbd>.

   :::image type="content" source="media/debugging-with-visual-studio-code/continue-debugging.png" alt-text="Continuar a depuração":::

1. Selecione a guia **terminal** novamente.

   Os valores exibidos na janela do console correspondem às alterações feitas no **console de depuração**.

   :::image type="content" source="media/debugging-with-visual-studio-code/changed-variable-values.png" alt-text="Terminal mostrando os valores inseridos":::

1. Pressione qualquer tecla para sair do aplicativo e parar a depuração.

## <a name="set-a-conditional-breakpoint"></a>Definir um ponto de interrupção condicional

O programa exibe a cadeia de caracteres que o usuário insere. O que acontecerá se o usuário não inserir nada? Você pode testá-lo com um recurso de depuração útil chamado de *ponto de interrupção condicional*.

1. Clique com o botão direito do mouse em (<kbd>Ctrl</kbd>-clique em MacOS) no ponto vermelho que representa os pontos de interrupção. No menu de contexto, selecione **Editar ponto de interrupção** para abrir uma caixa de diálogo que permite inserir uma expressão condicional.

   :::image type="content" source="media/debugging-with-visual-studio-code/breakpoint-context-menu.png" alt-text="Menu de contexto do ponto de interrupção":::

1. Selecione `Expression` na lista suspensa, insira a seguinte expressão condicional e pressione <kbd>Enter</kbd>.

   ```csharp
   String.IsNullOrEmpty(name)
   ```

   :::image type="content" source="media/debugging-with-visual-studio-code/conditional-expression.png" alt-text="Inserir uma expressão condicional":::

   Cada vez que o ponto de interrupção for atingido, o depurador chamará o `String.IsNullOrEmpty(name)` método e interromperá essa linha somente se a chamada do método retornar `true` .

   Em vez de uma expressão condicional, você pode especificar uma *contagem de ocorrências*, que interrompe a execução do programa antes que uma instrução seja executada um número especificado de vezes. Outra opção é especificar uma *condição de filtro*, que interrompe a execução do programa com base nesses atributos como um identificador de thread, um nome de processo ou um nome de thread.

1. Inicie o programa com a depuração pressionando <kbd>F5</kbd>.

1. Na guia **terminal** , pressione a tecla <kbd>Enter</kbd> quando for solicitado a inserir seu nome.

   Como a condição especificada ( `name` is `null` ou <xref:System.String.Empty?displayProperty=nameWithType> ) foi satisfeita, a execução do programa é interrompida quando atinge o ponto de interrupção e antes da execução do `Console.WriteLine` método.

   A janela **variáveis** mostra que o valor da `name` variável é `""` , ou <xref:System.String.Empty?displayProperty=nameWithType> .

1. Confirme se o valor é uma cadeia de caracteres vazia digitando a seguinte instrução no prompt do **console de depuração** e pressionando <kbd>Enter</kbd>. O resultado é `true`.

   ```csharp
   name == String.Empty
   ```

   :::image type="content" source="media/debugging-with-visual-studio-code/expression-in-debug-console.png" alt-text="Console de depuração retornando um valor true após a execução da instrução":::

1. Selecione o botão **Continuar** na barra de ferramentas para continuar a execução do programa.

1. Selecione a guia **terminal** e pressione qualquer tecla para sair do programa e parar a depuração.

1. Desmarque o ponto de interrupção clicando no pontos na margem esquerda da janela de código. Outras maneiras de limpar um ponto de interrupção são pressionando <kbd>F9</kbd> ou escolhendo **executar > alternar ponto de interrupção** no menu enquanto a linha de código está selecionada.

1. Se você receber um aviso de que a condição de ponto de interrupção será perdida, selecione **remover ponto de interrupção**.

## <a name="step-through-a-program"></a>Percorrer um programa

Visual Studio Code também permite que você percorra linha por linha por meio de um programa e monitore sua execução. Normalmente, você definiria um ponto de interrupção e seguirá o fluxo do programa por meio de uma pequena parte do código do programa. Como esse programa é pequeno, você pode percorrer todo o programa.

1. Defina um ponto de interrupção na chave de abertura do `Main` método.

1. Pressione <kbd>F5</kbd> para iniciar a depuração.

   Visual Studio Code realça a linha do ponto de interrupção.

   Neste ponto, a janela **variáveis** mostra que a `args` matriz está vazia e `name` `date` tem valores padrão.

1. Selecione **executar**  >  **etapa em** ou pressione <kbd>F11</kbd>.

   :::image type="content" source="media/debugging-with-visual-studio-code/step-into.png" alt-text="Botão de depuração":::

   Visual Studio Code realça a próxima linha.

1. Selecione **executar**  >  **etapa em** ou pressione <kbd>F11</kbd>.

   Visual Studio Code executa o `Console.WriteLine` para o prompt de nome e realça a próxima linha de execução. A próxima linha é o `Console.ReadLine` para o `name` . A janela **variáveis** é inalterada e a guia **terminal** mostra o "Qual é seu nome?" aviso.

1. Selecione **executar**  >  **etapa em** ou pressione <kbd>F11</kbd>.

   O Visual Studio realça a `name` atribuição de variável. A janela **variáveis** mostra que `name` ainda é `null` .

1. Responda ao prompt inserindo uma cadeia de caracteres na guia terminal e pressionando <kbd>Enter</kbd>.

   A guia **terminal** pode não exibir a cadeia de caracteres que você inserir enquanto estiver inserindo, mas o <xref:System.Console.ReadLine%2A?displayProperty=nameWithType> método capturará sua entrada.

1. Selecione **executar**  >  **etapa em** ou pressione <kbd>F11</kbd>.

   Visual Studio Code realça a `date` atribuição de variável. A janela **variáveis** mostra o valor retornado pela chamada para o <xref:System.Console.ReadLine%2A?displayProperty=nameWithType> método. A guia **terminal** exibe a cadeia de caracteres que você inseriu no prompt.

1. Selecione **executar**  >  **etapa em** ou pressione <kbd>F11</kbd>.

   A janela **variáveis** mostra o valor da `date` variável após a atribuição da <xref:System.DateTime.Now?displayProperty=nameWithType> propriedade.

1. Selecione **executar**  >  **etapa em** ou pressione <kbd>F11</kbd>.

   Visual Studio Code chama o <xref:System.Console.WriteLine(System.String,System.Object,System.Object)?displayProperty=nameWithType> método. A janela do console exibe a cadeia de caracteres formatada.

1. Selecione **executar**  >  **etapa para sair** ou pressione <kbd>Shift</kbd> + <kbd>F11</kbd>.

   :::image type="content" source="media/debugging-with-visual-studio-code/step-out.png" alt-text="Botão de depuração":::

1. Selecione a guia **terminal** .

   O terminal exibe "Pressione qualquer tecla para sair..."

1. Pressione qualquer tecla para sair do programa.

## <a name="use-release-build-configuration"></a>Usar a configuração de Build de versão

Depois de testar a versão de depuração do seu aplicativo, você também deve compilar e testar a versão de lançamento. A versão de lançamento incorpora otimizações de compilador que podem afetar o comportamento de um aplicativo. Por exemplo, as otimizações do compilador que são projetadas para melhorar o desempenho podem criar condições de corrida em aplicativos multissegmentados.

Para compilar e testar a versão de lançamento do seu aplicativo de console, abra o **terminal** e execute o seguinte comando:

```dotnetcli
dotnet run --configuration Release
```

## <a name="additional-resources"></a>Recursos adicionais

* [Depurar no Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging)

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você usou Visual Studio Code ferramentas de depuração. No próximo tutorial, você publica uma versão implantável do aplicativo.

> [!div class="nextstepaction"]
> [Publicar um aplicativo de console do .NET Core com o Visual Studio Code](publishing-with-visual-studio-code.md)
