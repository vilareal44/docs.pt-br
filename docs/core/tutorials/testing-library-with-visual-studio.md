---
title: Testar um .NET Standard biblioteca de classes com o .NET Core usando o Visual Studio
description: Crie um projeto de teste de unidade para uma biblioteca de classes do .NET Core. Verifique se uma biblioteca de classes do .NET Core funciona corretamente com testes de unidade.
ms.date: 06/08/2020
dev_langs:
- csharp
- vb
ms.custom: vs-dotnet
ms.openlocfilehash: f20b089fd22794d5aaeff34502e960fe41a565e1
ms.sourcegitcommit: 1cbd77da54405ea7dba343ac0334fb03237d25d2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84700963"
---
# <a name="tutorial-test-a-net-standard-class-library-with-net-core-using-visual-studio"></a>Tutorial: testar uma biblioteca de classes de .NET Standard com o .NET Core usando o Visual Studio

Este tutorial mostra como automatizar o teste de unidade adicionando um projeto de teste a uma solução.

## <a name="prerequisites"></a>Pré-requisitos

- Este tutorial funciona com a solução que você cria em [criar uma .net Standard biblioteca no Visual Studio](library-with-visual-studio.md).

## <a name="create-a-unit-test-project"></a>Crie um projeto de teste de unidade

As unidade de teste fornecem testes de software automatizados durante o desenvolvimento e a publicação. [MSTest](https://github.com/Microsoft/testfx-docs) é uma das três estruturas de teste que você pode escolher. Os outros são [xUnit](https://xunit.net/) e [NUnit](https://nunit.org/).

1. Inicie o Visual Studio.

1. Abra a `ClassLibraryProjects` solução que você criou em [criar uma .net Standard biblioteca no Visual Studio](library-with-visual-studio.md).

1. Adicione um novo projeto de teste de unidade chamado "StringLibraryTest" à solução.

   1. Clique com o botão direito do mouse na solução em **Gerenciador de soluções** e selecione **Adicionar**  >  **novo projeto**.

   1. Na página **Adicionar um novo projeto** , digite **MSTest** na caixa de pesquisa. Escolha **C#** ou **Visual Basic** na lista idioma e, em seguida, escolha **todas as plataformas** na lista plataforma.

   1. Escolha o modelo de **projeto de teste MSTest (.NET Core)** e, em seguida, escolha **Avançar**.

   1. Na página **configurar seu novo projeto** , digite **StringLibraryTest** na caixa **nome do projeto** . Em seguida, escolha **Criar**.

1. O Visual Studio cria o projeto e abre o arquivo de classe na janela de código com o código a seguir. Se o idioma que você deseja usar não for mostrado, altere o seletor de idioma na parte superior da página.

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

   ```vb
   Imports Microsoft.VisualStudio.TestTools.UnitTesting

   Namespace StringLibraryTest
       <TestClass>
       Public Class UnitTest1
           <TestMethod>
           Sub TestSub()

           End Sub
       End Class
   End Namespace
   ```

   O código-fonte criado pelo modelo de teste de unidade faz o seguinte:

   - Ele importa o namespace <xref:Microsoft.VisualStudio.TestTools.UnitTesting?displayProperty=nameWithType>, que contém os tipos usados para o teste de unidade.
   - Ele aplica o atributo<xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestClassAttribute> à classe `UnitTest1`.
   - Ele aplica o <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute> atributo para definir `TestMethod1` em C# ou `TestSub` em Visual Basic.

   Cada método marcado com [[TestMethod]](xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute) em uma classe de teste marcada com [[TestClass]](xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestClassAttribute) é executado automaticamente quando o teste de unidade é executado.

## <a name="add-a-project-reference"></a>Adicionar uma referência ao projeto

Para que o projeto de teste funcione com a `StringLibrary` classe, adicione uma referência no projeto **StringLibraryTest** ao `StringLibrary` projeto.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no nó **dependências** do projeto **StringLibraryTest** e selecione **Adicionar referência de projeto** no menu de contexto.

1. No diálogo **Gerenciador de referências** , expanda o nó **projetos** e selecione a caixa ao lado de **StringLibrary**. Adicionar uma referência ao `StringLibrary` assembly permite que o compilador encontre métodos **StringLibrary** ao compilar o projeto **StringLibraryTest** .

1. Selecione **OK**.

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

1. Na janela de código *UnitTest1.cs* ou *UnitTest1. vb* , substitua o código pelo código a seguir:

   :::code language="csharp" source="./snippets/library-with-visual-studio/csharp/StringLibraryTest/UnitTest1.cs":::
   :::code language="vb" source="./snippets/library-with-visual-studio/vb/StringLibraryTest/UnitTest1.vb":::

   O teste de caracteres maiúsculos no `TestStartsWithUpper` método inclui a letra maiúscula grega alfa (u + 0391) e a letra maiúscula cirílica em (u + 041C). O teste de caracteres minúsculos no `TestDoesNotStartWithUpper` método inclui a letra grega pequena alfa (u + 03B1) e a letra cirílica minúscula Ghe (u + 0433).

1. Na barra de menus, selecione **arquivo**  >  **salvar UnitTest1.cs como** ou **arquivo**  >  **salvar UnitTest1. vb como**. Na caixa de diálogo **Salvar Arquivo Como**, selecione a seta ao lado do botão **Salvar** e selecione **Salvar com Codificação**.

   > [!div class="mx-imgBorder"]
   > ![Caixa de diálogo Salvar arquivo como do Visual Studio](./media/testing-library-with-visual-studio/save-file-as-dialog.png)

1. Na caixa de diálogo **Confirmar Salvar Como**, selecione o botão **Sim** para salvar o arquivo.

1. Na caixa de diálogo **Opções Avançadas de Salvamento**, selecione **Unicode (UTF-8 com assinatura) – página de código 65001** na lista suspensa **Codificação** e selecione **OK**.

   > [!div class="mx-imgBorder"]
   > ![Caixa de diálogo Opções Avançadas de Salvamento do Visual Studio](./media/testing-library-with-visual-studio/advanced-save-options.png)

   Se você não salvar seu código-fonte como um arquivo codificado em UTF8, o Visual Studio poderá salvá-lo como um arquivo ASCII. Quando isso acontece, o tempo de execução não decodifica com precisão os caracteres UTF8 fora do intervalo ASCII, e os resultados de teste não estarão corretos.

1. Na barra de menus, selecione **testar**  >  **executar todos os testes**. Se a janela **Gerenciador de testes** não abrir, abra-a escolhendo **Test**  >  **Test Explorer**. Os três testes estão listados na seção **Testes Aprovados**, e a seção **Resumo** relata o resultado da execução de teste.

   > [!div class="mx-imgBorder"]
   > ![Janela Gerenciador de Testes com testes aprovados](./media/testing-library-with-visual-studio/test-explorer-window.png)

## <a name="handle-test-failures"></a>Lidar com falhas de teste

Se você estiver fazendo o TDD (desenvolvimento controlado por teste), você escreverá testes primeiro e eles falharão na primeira vez em que forem executados. Em seguida, você adiciona código ao aplicativo que torna o teste com sucesso. Para este tutorial, você criou o teste depois de gravar o código do aplicativo que ele valida, para que você não tenha visto o teste falhar. Para validar que um teste falha quando você espera que ele falhe, adicione um valor inválido para a entrada de teste.

1. Modifique a matriz `words` no método `TestDoesNotStartWithUpper` para incluir a cadeia de caracteres “Error”. Não é necessário salvar o arquivo porque o Visual Studio salva automaticamente os arquivos abertos quando uma solução é criada para executar testes.

   ```csharp
   string[] words = { "alphabet", "Error", "zebra", "abc", "αυτοκινητοβιομηχανία", "государство",
                      "1234", ".", ";", " " };
   ```

   ```vb
   Dim words() As String = { "alphabet", "Error", "zebra", "abc", "αυτοκινητοβιομηχανία", "государство",
                      "1234", ".", ";", " " }

   ```

1. Execute o teste selecionando **testar**  >  **executar todos os testes** na barra de menus. A Janela **Gerenciador de Testes** indica que dois testes tiveram êxito e um falhou.

   > [!div class="mx-imgBorder"]
   > ![Janela Gerenciador de Testes com testes com falha](./media/testing-library-with-visual-studio/failed-test-window.png)

1. Selecione o teste com falha, `TestDoesNotStartWith` .

   A janela **Gerenciador de Testes** mostra a mensagem produzida pelo assert: "Assert.IsFalse falhou. Esperado para 'Error': false, real: True". Devido à falha, nenhuma cadeia de caracteres na matriz após o "erro" foi testada.

   > [!div class="mx-imgBorder"]
   > ![Janela do Gerenciador de testes mostrando a falha de asserção IsFalse](./media/testing-library-with-visual-studio/failed-test-detail.png)

1. Remova a cadeia de caracteres "Error" que você adicionou na etapa 1. Execute novamente o teste e os testes são aprovados.

## <a name="test-the-release-version-of-the-library"></a>Testar a versão de lançamento da biblioteca

Agora que todos os testes passaram durante a execução da compilação de depuração da biblioteca, execute o testa um tempo adicional em relação à compilação da versão da biblioteca. Vários fatores, incluindo as otimizações do compilador, podem produzir um comportamento diferente entre as compilações de Depuração e Lançamento.

Para testar a compilação de Lançamento:

1. Na barra de ferramentas do Visual Studio, altere a configuração de compilação de **Depurar** para **Lançamento**.

   > [!div class="mx-imgBorder"]
   > ![Barra de ferramentas do Visual Studio com build de versão realçado](./media/testing-library-with-visual-studio/visual-studio-toolbar-release.png)

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto **StringLibrary** e selecione **Compilar** no menu de contexto para recompilar a biblioteca.

   > [!div class="mx-imgBorder"]
   > ![Menu de contexto de StringLibrary com comando de build](./media/testing-library-with-visual-studio/build-library-context-menu.png)

1. Execute os testes de unidade escolhendo **testar executar**  >  **todos os testes** na barra de menus. Os testes são aprovados.

## <a name="additional-resources"></a>Recursos adicionais

* [Noções básicas do teste de unidade – Visual Studio](/visualstudio/test/unit-test-basics)
* [Teste de unidade no .NET Core e no .NET Standard](../testing/index.md)

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você testou uma unidade de biblioteca de classes. Você pode tornar a biblioteca disponível para outras pessoas publicando-a no [NuGet](https://nuget.org) como um pacote. Para saber como, siga um tutorial do NuGet:

> [!div class="nextstepaction"]
> [Criar e publicar um pacote NuGet usando o Visual Studio](/nuget/quickstart/create-and-publish-a-package-using-visual-studio?tabs=netcore-cli)

Se você publicar uma biblioteca como um pacote NuGet, outras pessoas poderão instalá-la e usá-la. Para saber como, siga um tutorial do NuGet:

> [!div class="nextstepaction"]
> [Instalar e usar um pacote no Visual Studio](/nuget/quickstart/install-and-use-a-package-in-visual-studio)

Uma biblioteca não precisa ser distribuída como um pacote. Ele pode ser agrupado com um aplicativo de console que o utiliza. Para saber como publicar um aplicativo de console, consulte o tutorial anterior nesta série:

> [!div class="nextstepaction"]
> [Publicar um aplicativo de console do .NET Core com o Visual Studio](publishing-with-visual-studio.md)
