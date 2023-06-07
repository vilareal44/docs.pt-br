---
title: Criar uma biblioteca de classes de .NET Standard usando o Visual Studio
description: Saiba como criar uma biblioteca de classes de .NET Standard usando o Visual Studio.
ms.date: 08/07/2020
dev_langs:
- csharp
- vb
ms.custom: vs-dotnet,contperfq1
ms.openlocfilehash: 595e93d8d8d22478c6770ddd4f70a0214653f5b9
ms.sourcegitcommit: d337df55f83325918cbbd095eb573400bea49064
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88187954"
---
# <a name="tutorial-create-a-net-standard-library-using-visual-studio"></a>Tutorial: criar uma biblioteca de .NET Standard usando o Visual Studio

Neste tutorial, você cria uma biblioteca de classes simples que contém um único método de manipulação de cadeia de caracteres.

Uma *biblioteca de classes* define tipos e métodos que são chamados por um aplicativo. Uma biblioteca de classes que tem como alvo .NET Standard 2,0 permite que sua biblioteca seja chamada por qualquer implementação do .NET que dê suporte a essa versão do .NET Standard.

Ao concluir a biblioteca de classes, você pode distribuí-la como um pacote NuGet ou como um componente agrupado com o aplicativo que a utiliza.

## <a name="prerequisites"></a>Pré-requisitos

- [Visual Studio 2019 versão 16,6 ou uma versão posterior](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) com a carga de trabalho de **desenvolvimento de plataforma cruzada do .NET Core** instalada. O SDK do .NET Core 3,1 é instalado automaticamente quando você seleciona essa carga de trabalho.

## <a name="create-a-solution"></a>Criar uma solução

Comece criando uma solução em branco para colocar o projeto de biblioteca de classes no. Uma solução do Visual Studio serve como um contêiner para um ou mais projetos. Você adicionará mais projetos relacionados à mesma solução.

Para criar a solução em branco:

1. Inicie o Visual Studio.

2. Na janela iniciar, escolha **criar um novo projeto**.

3. Na página **criar um novo projeto** , digite **solução** na caixa de pesquisa. Escolha o modelo de **solução em branco** e, em seguida, escolha **Avançar**.

   ![Modelo de solução em branco no Visual Studio](media/library-with-visual-studio/blank-solution.png)

4. Na página **configurar seu novo projeto** , digite **ClassLibraryProjects** na caixa **nome do projeto** . Em seguida, escolha **Criar**.

## <a name="create-a-class-library-project"></a>Criar um projeto de biblioteca de classes

1. Adicione um novo projeto de biblioteca de classe .NET Standard chamado "StringLibrary" à solução.

   1. Clique com o botão direito do mouse na solução em **Gerenciador de soluções** e selecione **Adicionar**  >  **novo projeto**.

   1. Na página **Adicionar um novo projeto** , insira **biblioteca** na caixa de pesquisa. Escolha **C#** ou **Visual Basic** na lista idioma e, em seguida, escolha **todas as plataformas** na lista plataforma. Escolha o modelo **biblioteca de classes (.net Standard)** e, em seguida, escolha **Avançar**.

   1. Na página **configurar seu novo projeto** , digite **StringLibrary** na caixa **nome do projeto** . Em seguida, escolha **criar**.

1. Verifique se a biblioteca tem como destino a versão correta do .NET Standard. Clique com o botão direito do mouse no projeto de biblioteca em **Gerenciador de soluções**e selecione **Propriedades**. A caixa de texto **estrutura de destino** mostra que o projeto tem como destino .net Standard 2,0.

   ![Propriedades do projeto da biblioteca de classes](./media/library-with-visual-studio/library-project-properties.png)

1. Se você estiver usando Visual Basic, desmarque o texto na caixa de texto **namespace raiz** .

   ![Propriedades do projeto da biblioteca de classes](./media/library-with-visual-studio/vb/library-project-properties.png)

   Para cada projeto, Visual Basic cria automaticamente um namespace que corresponde ao nome do projeto. Neste tutorial, você define um namespace de nível superior usando a [`namespace`](../../visual-basic/language-reference/statements/namespace-statement.md) palavra-chave no arquivo de código.

1. Substitua o código na janela de código para *Class1.cs*  ou *Class1. vb* pelo código a seguir e salve o arquivo. Se o idioma que você deseja usar não for mostrado, altere o seletor de idioma na parte superior da página.

   :::code language="csharp" source="./snippets/library-with-visual-studio/csharp/StringLibrary/Class1.cs":::
   :::code language="vb" source="./snippets/library-with-visual-studio/vb/StringLibrary/Class1.vb":::

   A biblioteca de classes, `UtilityLibraries.StringLibrary` , contém um método chamado `StartsWithUpper` . Esse método retorna um <xref:System.Boolean> valor que indica se a instância atual da cadeia de caracteres começa com um caractere maiúsculo. O padrão Unicode distingue caracteres maiúsculos de caracteres minúsculos. O método <xref:System.Char.IsUpper(System.Char)?displayProperty=nameWithType> retornará `true` se um caractere for maiúsculo.

   `StartsWithUpper` é implementado como um [método de extensão](../../csharp/programming-guide/classes-and-structs/extension-methods.md) para que você possa chamá-lo como se fosse um membro da <xref:System.String> classe.

1. Na barra de menus, selecione **Compilar**compilar  >  **solução** para verificar se o projeto é compilado sem erros.

## <a name="add-a-console-app-to-the-solution"></a>Adicionar um aplicativo de console à solução

Adicione um aplicativo de console que usa a biblioteca de classes. O aplicativo solicitará que o usuário insira uma cadeia de caracteres e relate se a cadeia de caracteres começa com um caractere maiúsculo.

1. Adicione um novo aplicativo de console .NET Core chamado "ShowCase" à solução.

   1. Clique com o botão direito do mouse na solução em **Gerenciador de soluções** e selecione **Adicionar**  >  **novo projeto**.

   1. Na página **Adicionar um novo projeto** , insira **console** na caixa de pesquisa. Escolha **C#** ou **Visual Basic** na lista idioma e, em seguida, escolha **todas as plataformas** na lista plataforma.

   1. Escolha o modelo **aplicativo de console (.NET Core)** e, em seguida, escolha **Avançar**.

   1. Na página **configurar seu novo projeto** , insira **Showcase** na caixa **nome do projeto** . Em seguida, escolha **Criar**.

1. Na janela de código do arquivo *Program.cs* ou *Program. vb* , substitua todo o código pelo código a seguir.

   :::code language="csharp" source="./snippets/library-with-visual-studio/csharp/ShowCase/Program.cs":::
   :::code language="vb" source="./snippets/library-with-visual-studio/vb/ShowCase/Program.vb":::

   O código usa a variável `row` ​​para manter uma contagem do número de linhas de dados gravadas na janela do console. Sempre que for maior ou igual a 25, o código limpará a janela do console e exibirá uma mensagem para o usuário.

   O programa solicita que o usuário insira uma cadeia de caracteres. Ele indica se a cadeia de caracteres começa com um caractere maiúsculo. Se o usuário pressionar a tecla <kbd>Enter</kbd> sem inserir uma cadeia de caracteres, o aplicativo será encerrado e a janela do console será fechada.

## <a name="add-a-project-reference"></a>Adicionar uma referência ao projeto

Inicialmente, o novo projeto de aplicativo de console não tem acesso à biblioteca de classes. Para permitir que ele chame métodos na biblioteca de classes, crie uma referência de projeto para o projeto de biblioteca de classes.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no `ShowCase` nó **dependências** do projeto e selecione **Adicionar referência de projeto**.

   ![Adicionar menu de contexto de referência no Visual Studio](media/library-with-visual-studio/add-reference-context-menu.png)

1. Na caixa de diálogo **Gerenciador de referências** , selecione o projeto **StringLibrary** e selecione **OK**.

   ![Caixa de diálogo Gerenciador de referências com StringLibrary selecionado](media/library-with-visual-studio/manage-project-references.png)

## <a name="run-the-app"></a>Executar o aplicativo

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto **ShowCase** e selecione **Definir como Projeto de Inicialização** no menu de contexto.

   ![Menu de contexto do projeto do Visual Studio para definir o projeto de inicialização](media/library-with-visual-studio/set-startup-project-context-menu.png)

1. Pressione <kbd>Ctrl</kbd> + <kbd>F5</kbd> para compilar e executar o programa sem depuração.

   ![Barra de ferramentas do projeto do Visual Studio mostrando botão de depuração](media/library-with-visual-studio/visual-studio-project-toolbar.png)

1. Experimente o programa digitando cadeias de caracteres e pressionando <kbd>Enter</kbd>e pressione <kbd>Enter</kbd> para sair.

   :::image type="content" source="media/library-with-visual-studio/run-showcase.png" alt-text="Janela do console com o ShowCase em execução":::

## <a name="additional-resources"></a>Recursos adicionais

* [Desenvolver bibliotecas com o CLI do .NET Core](libraries.md)
* [.Net Standard versões e plataformas às quais eles dão suporte](../../standard/net-standard.md).

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você criou uma biblioteca de classes. No próximo tutorial, você aprenderá a testar unidade a biblioteca de classes.

> [!div class="nextstepaction"]
> [Teste de unidade de uma biblioteca de .NET Standard usando o Visual Studio](testing-library-with-visual-studio.md)

Ou você pode ignorar o teste de unidade automatizado e aprender a compartilhar a biblioteca criando um pacote NuGet:

> [!div class="nextstepaction"]
> [Criar e publicar um pacote usando o Visual Studio](/nuget/quickstart/create-and-publish-a-package-using-visual-studio)

Ou saiba como publicar um aplicativo de console. Se você publicar o aplicativo de console da solução criada neste tutorial, a biblioteca de classes o passará como um arquivo *. dll* .

> [!div class="nextstepaction"]
> [Publicar um aplicativo de console do .NET Core usando o Visual Studio](publishing-with-visual-studio.md)
