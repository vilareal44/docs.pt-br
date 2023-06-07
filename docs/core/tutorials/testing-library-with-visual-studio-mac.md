---
title: Testar um .NET Standard biblioteca de classes com o .NET Core usando Visual Studio para Mac
description: Crie um projeto de teste de unidade para uma biblioteca de classes do .NET Core. Verifique se uma biblioteca de classes do .NET Core funciona corretamente com testes de unidade.
ms.date: 06/08/2020
ms.openlocfilehash: a183049623df44cbb8c4abd47ce6e78d91adae12
ms.sourcegitcommit: 1cbd77da54405ea7dba343ac0334fb03237d25d2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84713605"
---
# <a name="test-a-net-standard-class-library-with-net-core-using-visual-studio"></a>Testar um .NET Standard biblioteca de classes com o .NET Core usando o Visual Studio

Este tutorial mostra como automatizar o teste de unidade adicionando um projeto de teste a uma solução.

## <a name="prerequisites"></a>Pré-requisitos

- Este tutorial funciona com a solução que você cria em [criar uma .net Standard biblioteca no Visual Studio para Mac](library-with-visual-studio-mac.md).

## <a name="create-a-unit-test-project"></a>Crie um projeto de teste de unidade

As unidade de teste fornecem testes de software automatizados durante o desenvolvimento e a publicação. [MSTest](https://github.com/Microsoft/testfx-docs) é uma das três estruturas de teste que você pode escolher. Os outros são [xUnit](https://xunit.net/) e [NUnit](https://nunit.org/).

1. Iniciar Visual Studio para Mac.

1. Abra a `ClassLibraryProjects` solução que você criou em [criar uma .net Standard biblioteca no Visual Studio para Mac](library-with-visual-studio-mac.md).

1. No painel de **solução** , <kbd>pressione CTRL +</kbd>clique na `ClassLibraryProjects` solução e selecione **Adicionar**  >  **novo projeto**.

1. Na caixa de diálogo **novo projeto** , selecione **testes** no nó **da Web e do console** . Selecione o **projeto MSTest** seguido por **Avançar**.

   :::image type="content" source="media/testing-library-with-visual-studio-mac/visual-studio-mac-unit-test-project.png" alt-text="Caixa de diálogo novo projeto do Visual Studio Mac criando projeto de teste":::

1. Selecione **.NET Core 3,1**. Nomeie o novo projeto como "StringLibraryTest" e selecione **criar**.

   :::image type="content" source="media/testing-library-with-visual-studio-mac/visual-studio-mac-new-project-name.png" alt-text="Caixa de diálogo Novo Projeto do Visual Studio para Mac fornecendo um nome de projeto":::

   O Visual Studio cria um arquivo de classe com o seguinte código:

   ```csharp
   using Microsoft.VisualStudio.TestTools.UnitTesting;

   namespace StringLibraryTest
   {
       [TestClass]
       public class UnitTest1
       {
           [TestMethod]
           public void TestMethod1()
           {
           }
       }
   }
   ```

   O código-fonte criado pelo modelo de teste de unidade faz o seguinte:

   - Ele importa o namespace <xref:Microsoft.VisualStudio.TestTools.UnitTesting?displayProperty=nameWithType>, que contém os tipos usados para o teste de unidade.
   - Ele aplica o atributo<xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestClassAttribute> à classe `UnitTest1`.
   - Ele aplica o <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute> atributo a `TestMethod1` .

   Cada método marcado com [[TestMethod]](xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute) em uma classe de teste marcada com [[TestClass]](xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestClassAttribute) é executado automaticamente quando o teste de unidade é executado.

## <a name="add-a-project-reference"></a>Adicionar uma referência ao projeto

Para que o projeto de teste funcione com a `StringLibrary` classe, adicione uma referência ao `StringLibrary` projeto.

1. No painel de **solução** , <kbd>pressione CTRL</kbd>e clique em **dependências** em **StringLibraryTest**. Selecione **Adicionar referência** no menu de contexto.

1. Na caixa de diálogo **referências** , selecione o projeto **StringLibrary** . Selecione **OK**.

      :::image type="content" source="media/testing-library-with-visual-studio-mac/visual-studio-mac-edit-references.png" alt-text="Caixa de diálogo Editar Referências do Visual Studio para Mac":::

## <a name="add-and-run-unit-test-methods"></a>Adicionar e executar métodos de teste de unidade

Quando o Visual Studio executa um teste de unidade, ele executa cada método marcado com o <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute> atributo em uma classe marcada com o <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestClassAttribute> atributo. Um método de teste termina quando a primeira falha é encontrada ou quando todos os testes contidos no método foram bem-sucedidos.

Os testes mais comuns chamam membros da classe <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert>. Muitos métodos assert incluem pelo menos dois parâmetros, um deles é o resultado esperado do teste, e o outro é o resultado real do teste. Alguns dos `Assert` métodos chamados mais frequentemente da classe são mostrados na tabela a seguir:

| Métodos assert     | Função |
| ------------------ | -------- |
| `Assert.AreEqual`  | Verifica se os dois valores ou objetos são iguais. A declaração falhará se os valores ou objetos não forem iguais. |
| `Assert.AreSame`   | Verifica se duas variáveis de objeto se referem ao mesmo objeto. A assert falhará se as variáveis se referirem a objetos diferentes. |
| `Assert.IsFalse`   | Verifica se uma condição é `false`. O assert falhará se a condição for `true`. |
| `Assert.IsNotNull` | Verifica se um objeto não está `null` . A assert falhará se o objeto for `null`. |

Você também pode usar o <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert.ThrowsException%2A?displayProperty=nameWithType> método em um método de teste para indicar o tipo de exceção que ele deve lançar. O teste falhará se a exceção especificada não for lançada.

Ao testar o método `StringLibrary.StartsWithUpper`, você quer fornecer um número de cadeias de caracteres que comecem com um caractere maiúsculo. Você espera que o método retorne `true` nesses casos, para que possa chamar o método <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert.IsTrue%2A?displayProperty=nameWithType>. Da mesma forma, você deseja fornecer um número de cadeias de caracteres que comecem com algo diferente de um caractere maiúsculo. Você espera que o método retorne `false` nesses casos, para que possa chamar o método <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert.IsFalse%2A?displayProperty=nameWithType>.

Como o método de biblioteca lida com cadeias de caracteres, você também deseja certificar-se de que ele manipula com êxito uma [cadeia de caracteres vazia ( `String.Empty` )](xref:System.String.Empty), uma cadeia de caracteres válida que não tem caracteres e cujo número <xref:System.String.Length> é 0 e uma `null` cadeia de caracteres que não foi inicializada. Você pode chamar `StartsWithUpper` diretamente como um método estático e passar um único <xref:System.String> argumento. Ou você pode chamar `StartsWithUpper` como um método de extensão em uma `string` variável atribuída a `null` .

Você definirá três métodos, cada um deles chamará um <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert> método para cada elemento em uma matriz de cadeia de caracteres. Você chamará uma sobrecarga de método que permite especificar uma mensagem de erro a ser exibida em caso de falha de teste. A mensagem identifica a cadeia de caracteres que causou a falha.

Para criar os métodos de teste:

1. Abra o arquivo *UnitTest1.cs* e substitua o código pelo código a seguir:

   :::code language="csharp" source="./snippets/library-with-visual-studio/csharp/StringLibraryTest/UnitTest1.cs":::

   O teste de caracteres maiúsculos no `TestStartsWithUpper` método inclui a letra maiúscula grega alfa (u + 0391) e a letra maiúscula cirílica em (u + 041C). O teste de caracteres minúsculos no `TestDoesNotStartWithUpper` método inclui a letra grega pequena alfa (u + 03B1) e a letra cirílica minúscula Ghe (u + 0433).

1. Na barra de menus, selecione **arquivo**  >  **salvar como**. Na caixa de diálogo, verifique se a **codificação** está definida como **Unicode (UTF-8)**.

   :::image type="content" source="media/testing-library-with-visual-studio-mac/save-file-as-dialog.png" alt-text="Caixa de diálogo Salvar arquivo como do Visual Studio":::

1. Quando for perguntado se deseja substituir o arquivo existente, selecione **substituir**.

   Se você não salvar seu código-fonte como um arquivo codificado em UTF8, o Visual Studio poderá salvá-lo como um arquivo ASCII. Quando isso acontece, o tempo de execução não decodifica com precisão os caracteres UTF8 fora do intervalo ASCII, e os resultados de teste não estarão corretos.

1. Abra o painel **Testes de Unidade** no lado direito da tela. Selecione **Exibir**  >  **testes** no menu.

1. Clique no ícone **Dock** para manter o painel aberto.

   :::image type="content" source="media/testing-library-with-visual-studio-mac/visual-studio-mac-unit-test-dock-icon.png" alt-text="Ícone de encaixe do painel Testes de Unidade do Visual Studio para Mac":::

1. Clique no botão **Executar Tudo**.

   Todos os testes serão aprovados.

   :::image type="content" source="media/testing-library-with-visual-studio-mac/visual-studio-mac-unit-test-pass.png" alt-text="Visual Studio para Mac aprovações de teste esperadas":::

## <a name="handle-test-failures"></a>Lidar com falhas de teste

Se você estiver fazendo o TDD (desenvolvimento controlado por teste), você escreverá testes primeiro e eles falharão na primeira vez em que forem executados. Em seguida, você adiciona código ao aplicativo que torna o teste com sucesso. Para este tutorial, você criou o teste depois de gravar o código do aplicativo que ele valida, para que você não tenha visto o teste falhar. Para validar que um teste falha quando você espera que ele falhe, adicione um valor inválido para a entrada de teste.

1. Modifique a matriz `words` no método `TestDoesNotStartWithUpper` para incluir a cadeia de caracteres “Error”. Não é necessário salvar o arquivo porque o Visual Studio salva automaticamente os arquivos abertos quando uma solução é criada para executar testes.

   ```csharp
   string[] words = { "alphabet", "Error", "zebra", "abc", "αυτοκινητοβιομηχανία", "государство",
                      "1234", ".", ";", " " };
   ```

1. Execute os testes novamente.

   Desta vez, a janela **Test Explorer** indica que dois testes foram bem-sucedidos e um deles falhou.

   :::image type="content" source="media/testing-library-with-visual-studio-mac/failed-test-window.png" alt-text="Janela Gerenciador de Testes com testes com falha":::

1. <kbd>Ctrl</kbd>-clique no teste com falha, `TestDoesNotStartWithUpper` e selecione **Mostrar painel de resultados** no menu de contexto.

   O painel de **resultados** exibe a mensagem produzida pelo Assert: "Assert. IsFalse falhou. Esperado para 'Error': false, real: True". Devido à falha, nenhuma cadeia de caracteres na matriz após o "erro" foi testada.

   :::image type="content" source="media/testing-library-with-visual-studio-mac/visual-studio-mac-unit-test-failure.png" alt-text="Janela do Gerenciador de testes mostrando a falha de asserção IsFalse":::

1. Remova a cadeia de caracteres "Error" que você adicionou na etapa 1. Execute novamente o teste e os testes são aprovados.

## <a name="test-the-release-version-of-the-library"></a>Testar a versão de lançamento da biblioteca

Agora que todos os testes passaram durante a execução da compilação de depuração da biblioteca, execute o testa um tempo adicional em relação à compilação da versão da biblioteca. Vários fatores, incluindo as otimizações do compilador, podem produzir um comportamento diferente entre as compilações de Depuração e Lançamento.

Para testar a compilação de Lançamento:

1. Na barra de ferramentas do Visual Studio, altere a configuração de compilação de **Depurar** para **Lançamento**.

   :::image type="content" source="media/testing-library-with-visual-studio-mac/visual-studio-toolbar-release.png" alt-text="Barra de ferramentas do Visual Studio com build de versão realçado":::

1. No painel de **solução** , <kbd>pressione CTRL +</kbd>clique no projeto **StringLibrary** e selecione **Compilar** no menu de contexto para recompilar a biblioteca.

   :::image type="content" source="media/testing-library-with-visual-studio-mac/build-library-context-menu.png" alt-text="Menu de contexto de StringLibrary com comando de build":::

1. Execute os testes de unidade novamente.

   Os testes são aprovados.

## <a name="debug-tests"></a>Depurar testes

Você pode usar o mesmo processo mostrado no [tutorial: Depurar um aplicativo de console do .NET Core usando Visual Studio para Mac](debugging-with-visual-studio-mac.md) para depurar código usando seu projeto de teste de unidade. Em vez de iniciar o projeto de aplicativo de demonstração, <kbd>pressione CTRL</kbd>e clique no projeto **StringLibraryTests** e selecione **Iniciar Depuração de projeto** no menu de contexto. O Visual Studio inicia o projeto de teste com o depurador anexado. A execução será interrompida em qualquer ponto de interrupção que você adicionou ao projeto de teste ou ao código de biblioteca subjacente.

## <a name="additional-resources"></a>Recursos adicionais

* [Teste de unidade no .NET Core e no .NET Standard](../testing/index.md)

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você testou uma unidade de biblioteca de classes. Você pode tornar a biblioteca disponível para outras pessoas publicando-a no [NuGet](https://nuget.org) como um pacote. Para saber como, siga um tutorial do NuGet:

> [!div class="nextstepaction"]
> [Criar e publicar um pacote (CLI do dotnet)](/nuget/quickstart/create-and-publish-a-package-using-the-dotnet-cli)

Se você publicar uma biblioteca como um pacote NuGet, outras pessoas poderão instalá-la e usá-la. Para saber como, siga um tutorial do NuGet:

> [!div class="nextstepaction"]
> [Instalar e usar um pacote no Visual Studio para Mac](/nuget/quickstart/install-and-use-a-package-in-visual-studio-mac)

Uma biblioteca não precisa ser distribuída como um pacote. Ele pode ser agrupado com um aplicativo de console que o utiliza. Para saber como publicar um aplicativo de console, consulte o tutorial anterior nesta série:

> [!div class="nextstepaction"]
> [Publicar um aplicativo de console do .NET Core com o Visual Studio para Mac](publishing-with-visual-studio-mac.md)
