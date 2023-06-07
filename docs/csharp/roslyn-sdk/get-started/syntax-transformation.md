---
title: Introdução à transformação de sintaxe (APIs Roslyn)
description: Uma introdução pela travessia, consulta e percurso por árvores de sintaxe.
ms.date: 06/01/2018
ms.custom: mvc
ms.openlocfilehash: 5879dfd6ed0a5f6465829eec496d10cfcfd07362
ms.sourcegitcommit: 71b8f5a2108a0f1a4ef1d8d75c5b3e129ec5ca1e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84202116"
---
# <a name="get-started-with-syntax-transformation"></a>Introdução à transformação de sintaxe

Este tutorial baseia-se nos conceitos e técnicas explorados nos guias de início rápido [Introdução à análise de sintaxe](syntax-analysis.md) e [Introdução à análise semântica](semantic-analysis.md). Se ainda não o fez, você deve concluir as etapas rápidas antes de começar esta.

Neste início rápido, você explora técnicas para criar e transformar árvores de sintaxe. Em combinação com as técnicas que você aprendeu em guias de início rápido anteriores, você cria sua primeira refatoração de linha de comando!

[!INCLUDE[interactive-note](~/includes/roslyn-installation.md)]

## <a name="immutability-and-the-net-compiler-platform"></a>Imutabilidade e a plataforma de compiladores .NET

**Imutabilidade** é um princípio fundamental da plataforma de compiladores .NET. Estruturas de dados imutáveis ​​não podem ser alteradas depois de criadas. Estruturas de dados imutáveis ​​podem ser compartilhadas com segurança e analisadas por vários consumidores simultaneamente. Não há perigo de que um consumidor afete o outro de maneiras imprevisíveis. Seu analisador não precisa de bloqueios ou outras medidas de simultaneidade. Essa regra se aplica a árvores de sintaxe, compilações, símbolos, modelos semânticos e todas as outras estruturas de dados que você encontrar. Em vez de modificar as estruturas existentes, as APIs criam novos objetos com base nas diferenças especificadas para os antigos. Você aplica esse conceito a árvores de sintaxe para criar novas árvores usando transformações.

## <a name="create-and-transform-trees"></a>Criar e transformar árvores

Você escolhe uma das duas estratégias para transformações de sintaxe. Os **métodos de fábrica** são melhor usados ​​quando você está procurando por nós específicos para substituir ou locais específicos onde deseja inserir um novo código. **Regravadores** são a melhor opção quando você deseja examinar um projeto inteiro em busca dos padrões de código que deseja substituir.

### <a name="create-nodes-with-factory-methods"></a>Criar nós com métodos de fábrica

A primeira transformação de sintaxe demonstra os métodos de fábrica. Substitua uma instrução `using System.Collections;` por uma instrução `using System.Collections.Generic;`. Este exemplo demonstra como você cria objetos <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxNode?displayProperty=nameWithType> usando os métodos de fábrica <xref:Microsoft.CodeAnalysis.CSharp.SyntaxFactory?displayProperty=nameWithType>. Para cada tipo de **nó**, **token**ou **trivia**, há um método de fábrica que cria uma instância desse tipo. Você cria árvores de sintaxe compondo os nós hierarquicamente de baixa para cima. Em seguida, você transformará o programa existente substituindo nós existentes pela nova árvore que você criou.

Inicie o Visual Studio e crie um novo projeto de **Ferramenta de Análise de Código Autônoma** do C#. No Visual Studio, escolha **arquivo**  >  **novo**  >  **projeto** para exibir a caixa de diálogo novo projeto. Em extensibilidade do **Visual C#**,  >  **Extensibility** escolha uma ferramenta de **análise de código**autônoma. Este guia de início rápido tem dois projetos de exemplo, portanto, nomeie a solução **SyntaxTransformationQuickStart** e nomeie o projeto **ConstructionCS**. Clique em **OK**.

Este projeto usa os métodos de classe <xref:Microsoft.CodeAnalysis.CSharp.SyntaxFactory?displayProperty=nameWithType> para construir um <xref:Microsoft.CodeAnalysis.CSharp.Syntax.NameSyntax?displayProperty=nameWithType> representando o namespace `System.Collections.Generic`.

Adicione a seguinte diretiva using à parte superior do `Program.cs` .

[!code-csharp[import the SyntaxFactory class](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/ConstructionCS/Program.cs#StaticUsings "import the Syntax Factory class and the System.Console class")]

Você criará **nós de sintaxe de nome** para construir a árvore que representa a instrução `using System.Collections.Generic;`. <xref:Microsoft.CodeAnalysis.CSharp.Syntax.NameSyntax> é a classe base para quatro tipos de nomes que aparecem no C#. Você compõe esses quatro tipos de nomes para criar qualquer nome que possa aparecer na linguagem C#:

* <xref:Microsoft.CodeAnalysis.CSharp.Syntax.NameSyntax?displayProperty=nameWithType>, que representa nomes simples de identificadores únicos como `System` e `Microsoft`.
* <xref:Microsoft.CodeAnalysis.CSharp.Syntax.GenericNameSyntax?displayProperty=nameWithType>, que representa um tipo genérico ou nome de método, como `List<int>`.
* <xref:Microsoft.CodeAnalysis.CSharp.Syntax.QualifiedNameSyntax?displayProperty=nameWithType>,que representa um nome qualificado do formulário `<left-name>.<right-identifier-or-generic-name>`, como `System.IO`.
* <xref:Microsoft.CodeAnalysis.CSharp.Syntax.AliasQualifiedNameSyntax?displayProperty=nameWithType>, que representa um nome usando um alias externo de assembly como `LibraryV2::Foo`.

Você usa o método <xref:Microsoft.CodeAnalysis.CSharp.SyntaxFactory.IdentifierName(System.String)> para criar um nó <xref:Microsoft.CodeAnalysis.CSharp.Syntax.NameSyntax>. Adicione o seguinte código no seu método `Main` no `Program.cs`:

[!code-csharp[create the system identifier](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/ConstructionCS/Program.cs#CreateIdentifierName "Create and display the system name identifier")]

O código anterior cria um objeto <xref:Microsoft.CodeAnalysis.CSharp.Syntax.IdentifierNameSyntax> e o atribui à variável `name`. Muitas das APIs Roslyn retornam classes básicas para facilitar o trabalho com tipos relacionados. A variável `name`, um <xref:Microsoft.CodeAnalysis.CSharp.Syntax.NameSyntax>, pode ser reutilizada conforme você constrói o <xref:Microsoft.CodeAnalysis.CSharp.Syntax.QualifiedNameSyntax>. Não use inferência de tipo ao criar a amostra. Você automatizará essa etapa neste projeto.

Você criou o nome. Agora, é hora de criar mais nós na árvore criando um <xref:Microsoft.CodeAnalysis.CSharp.Syntax.QualifiedNameSyntax>. A nova árvore usa `name` como a esquerda do nome e um novo <xref:Microsoft.CodeAnalysis.CSharp.Syntax.IdentifierNameSyntax> para o namespace `Collections` como o lado direito do <xref:Microsoft.CodeAnalysis.CSharp.Syntax.QualifiedNameSyntax>. Adicione o seguinte código a `program.cs`:

[!code-csharp[create the collections identifier](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/ConstructionCS/Program.cs#CreateQualifiedIdentifierName "Build the System.Collections identifier")]

Execute o código novamente e confira os resultados. Você está construindo uma árvore de nós que representa o código. Você continuará este padrão para construir o <xref:Microsoft.CodeAnalysis.CSharp.Syntax.QualifiedNameSyntax> para o namespace `System.Collections.Generic`. Adicione o seguinte código a `Program.cs`:

[!code-csharp[create the full identifier](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/ConstructionCS/Program.cs#CreateFullNamespace "Build the System.Collections.Generic identifier")]

Execute o programa novamente para ver que você criou a árvore para o código a ser adicionado.

### <a name="create-a-modified-tree"></a>Criar uma árvore modificada

Você criou uma pequena árvore de sintaxe que contém uma instrução. As APIs para criar novos nós são a escolha certa para criar instruções únicas ou outros pequenos blocos de código. No entanto, para construir blocos maiores de código, você deve usar métodos que substituem nós ou inserem nós em uma árvore existente. Lembre-se de que as árvores de sintaxe são imutáveis. A **API de Sintaxe** não fornece nenhum mecanismo para modificar uma árvore de sintaxe existente após a construção. Em vez disso, fornece métodos que produzem novas árvores com base nas alterações existentes. Os métodos `With*` são definidos em classes concretas que derivam de <xref:Microsoft.CodeAnalysis.SyntaxNode> ou em métodos de extensão declarados na classe <xref:Microsoft.CodeAnalysis.SyntaxNodeExtensions>. Esses métodos criam um novo nó aplicando alterações nas propriedades filho de um nó existente. Além disso, o método de extensão <xref:Microsoft.CodeAnalysis.SyntaxNodeExtensions.ReplaceNode%2A> pode ser usado para substituir um nó descendente em uma subárvore. Esse método também atualiza o pai para apontar para o filho recém-criado e repete esse processo até a árvore inteira — um processo conhecido como _re-spinning_ da árvore.

O próximo passo é criar uma árvore que represente um programa inteiro (pequeno) e depois modificá-la. Adicione o seguinte código ao início da classe `Program`:

[!code-csharp[create a parse tree](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/ConstructionCS/Program.cs#DeclareSampleCode "Create a tree that represents a small program")]

> [!NOTE]
> O código de exemplo usa o namespace `System.Collections` e não o namespace `System.Collections.Generic`.

Em seguida, adicione o seguinte código à parte inferior do método `Main` para analisar o texto e criar uma árvore:

[!code-csharp[create a parse tree](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/ConstructionCS/Program.cs#CreateParseTree "Create a tree that represents a small program")]

Este exemplo usa o método <xref:Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax.WithName(Microsoft.CodeAnalysis.CSharp.Syntax.NameSyntax)?displayProperty=NameWithType> para substituir o nome em um nó <xref:Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax> pelo que foi construído no código anterior.

Crie um novo nó <xref:Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax> usando o método <xref:Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax.WithName(Microsoft.CodeAnalysis.CSharp.Syntax.NameSyntax)> para atualizar o nome `System.Collections` com o nome criado no código anterior. Adicione o seguinte código à parte inferior do método `Main`:

[!code-csharp[create a new subtree](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/ConstructionCS/Program.cs#BuildNewUsing "Create the subtree with the replaced namespace")]

Execute o programa e observe atentamente a saída. O `newusing` não foi colocado na árvore raiz. A árvore original não foi alterada.

Adicione o seguinte código usando o método de extensão <xref:Microsoft.CodeAnalysis.SyntaxNodeExtensions.ReplaceNode%2A> para criar uma nova árvore. A nova árvore é o resultado da substituição da importação existente pelo nó `newUsing` atualizado. Você atribui essa nova árvore à variável `root` ​​existente:

[!code-csharp[create a new root tree](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/ConstructionCS/Program.cs#TransformTree "Create the transformed root tree with the replaced namespace")]

Execute o programa novamente. Desta vez, a árvore importa corretamente o namespace `System.Collections.Generic`.

### <a name="transform-trees-using-syntaxrewriters"></a>Transformar árvores usando `SyntaxRewriters`

Os métodos `With*` e <xref:Microsoft.CodeAnalysis.SyntaxNodeExtensions.ReplaceNode%2A> fornecem meios convenientes para transformar ramos individuais de uma árvore de sintaxe. A classe <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxRewriter?displayProperty=nameWithType> realiza várias transformações em uma árvore de sintaxe. A classe <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxRewriter?displayProperty=nameWithType> é uma subclasse de <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxVisitor%601?displayProperty=nameWithType>. O <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxRewriter> aplica uma transformação a um tipo específico de <xref:Microsoft.CodeAnalysis.SyntaxNode>. Você pode aplicar transformações a vários tipos de objetos <xref:Microsoft.CodeAnalysis.SyntaxNode> sempre que eles aparecerem em uma árvore de sintaxe. O segundo projeto neste guia de início rápido cria uma refatoração de linha de comando que remove tipos explícitos em declarações de variáveis ​​locais em qualquer lugar em que a inferência de tipo possa ser usada.

Crie um novo projeto **de ferramenta de análise de código autônomo** em C#. No Visual Studio, clique com o botão direito do mouse no nó da solução `SyntaxTransformationQuickStart`. Escolha **Adicionar**  >  **novo projeto** para exibir a **caixa de diálogo novo projeto**. Em extensibilidade do **Visual C#**  >  **Extensibility**, escolha **ferramenta de análise de código autônoma**. Nomeie seu projeto como `TransformationCS` e clique em OK.

A primeira etapa é criar uma classe que deriva de <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxRewriter> para executar as transformações. Adicione um novo arquivo de classe ao projeto. No Visual Studio, escolha **projeto**  >  **Adicionar classe...**. No tipo de caixa de diálogo **Adicionar novo item** `TypeInferenceRewriter.cs` como o nome do arquivo.

Adicione o seguinte usando diretivas ao arquivo `TypeInferenceRewriter.cs`:

[!code-csharp[add necessary usings](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/TypeInferenceRewriter.cs#AddUsings "Add required usings")]

Em seguida, faça a classe `TypeInferenceRewriter` se estender à classe <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxRewriter>:

[!code-csharp[add base class](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/TypeInferenceRewriter.cs#BaseClass "Add base class")]

Adicione o seguinte código para declarar um campo somente leitura privado para conter um <xref:Microsoft.CodeAnalysis.SemanticModel> e inicializá-lo no construtor. Você precisará deste campo posteriormente para determinar onde a inferência de tipos pode ser usada:

[!code-csharp[initialize members](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/TypeInferenceRewriter.cs#Construction "Declare and initialize member variables")]

Substitua o método <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxRewriter.VisitLocalDeclarationStatement(Microsoft.CodeAnalysis.CSharp.Syntax.LocalDeclarationStatementSyntax)>:

```csharp
public override SyntaxNode VisitLocalDeclarationStatement(LocalDeclarationStatementSyntax node)
{

}
```

> [!NOTE]
> Muitas das APIs Roslyn declaram os tipos de retorno que são classes base dos tipos de runtime reais retornados. Em muitos cenários, um tipo de nó pode ser substituído inteiramente por outro tipo de nó, ou até mesmo removido. Neste exemplo, o método <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxRewriter.VisitLocalDeclarationStatement(Microsoft.CodeAnalysis.CSharp.Syntax.LocalDeclarationStatementSyntax)> retorna um <xref:Microsoft.CodeAnalysis.SyntaxNode>, em vez do tipo derivado de <xref:Microsoft.CodeAnalysis.CSharp.Syntax.LocalDeclarationStatementSyntax>. Este regravador retorna um novo nó <xref:Microsoft.CodeAnalysis.CSharp.Syntax.LocalDeclarationStatementSyntax> baseado no existente.

Este guia de início rápido lida com declarações de variáveis ​​locais. Você poderia estendê-lo para outras declarações, como loops `foreach`, loops `for`, expressões LINQ e expressões lambda. Além disso, este regravador só irá transformar as declarações da forma mais simples:

```csharp
Type variable = expression;
```

Se você quiser explorar por conta própria, considere estender a amostra finalizada para esses tipos de declarações de variáveis:

```csharp
// Multiple variables in a single declaration.
Type variable1 = expression1,
     variable2 = expression2;
// No initializer.
Type variable;
```

Adicione o seguinte código ao corpo do método `VisitLocalDeclarationStatement` para ignorar a reescrita dessas formas de declaração:

[!code-csharp[exclude other declarations](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/TypeInferenceRewriter.cs#Exclusions "Exclude variables declarations not processed by this sample")]

O método indica que nenhuma reescrita ocorre retornando o parâmetro `node` não modificado. Se nenhuma dessas expressões `if` for verdadeira, o nó representa uma possível declaração com inicialização. Adicione estas instruções para extrair o nome do tipo especificado na declaração e vinculá-lo usando o campo <xref:Microsoft.CodeAnalysis.SemanticModel> para obter um símbolo de tipo:

[!code-csharp[extract type name](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/TypeInferenceRewriter.cs#ExtractTypeSymbol "Extract the type name specified by the declaration")]

Agora, adicione esta instrução para associar a expressão inicializadora:

[!code-csharp[bind initializer](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/TypeInferenceRewriter.cs#BindInitializer "Bind the initializer expressions")]

Por fim, inclua a seguinte instrução `if` para substituir o nome do tipo existente pela palavra-chave `var`, se o tipo de expressão do inicializador corresponder ao tipo especificado:

[!code-csharp[ReplaceNode](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/TypeInferenceRewriter.cs#ReplaceNode "Replace the initializer node")]

A condicional é necessária porque a declaração pode converter a expressão inicializadora em uma classe ou interface base. Se este for o caso, os tipos à esquerda e à direita da atribuição não coincidem. Remover o tipo explícito nesses casos alteraria a semântica de um programa. `var` é especificado como um identificador em vez de uma palavra-chave porque `var` é uma palavra-chave contextual. As trivialidades inicial e final (espaço em branco) são transferidas do nome do tipo antigo para a palavra-chave `var` para manter espaço em branco vertical e recuo. É mais simples usar `ReplaceNode` em vez de `With*` para transformar o <xref:Microsoft.CodeAnalysis.CSharp.Syntax.LocalDeclarationStatementSyntax>, pois o nome do tipo é na verdade o neto da declaração.

Você concluiu o `TypeInferenceRewriter`. Agora, retorne ao arquivo `Program.cs` para finalizar o exemplo. Crie um teste <xref:Microsoft.CodeAnalysis.Compilation> e obtenha o <xref:Microsoft.CodeAnalysis.SemanticModel> dele. Use esse <xref:Microsoft.CodeAnalysis.SemanticModel> para testar seu `TypeInferenceRewriter`. Você realizará esta etapa por último. Enquanto isso, declare uma variável de espaço reservado representando sua compilação de teste:

[!code-csharp[DeclareCompilation](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/Program.cs#DeclareTestCompilation "Declare the test compilation")]

Após uma pausa, um erro será exibido informando que não existe método `CreateTestCompilation`. Pressione **Ctrl + Ponto** para abrir a lâmpada e pressione Enter para invocar o comando **Gerar Stub de Método**. Este comando irá gerar um stub de método para o método `CreateTestCompilation` na classe `Program`. Você voltará a preencher este método mais tarde:

![Gerar método C# a partir de uso](./media/syntax-transformation/generate-from-usage.png)

Grave o seguinte código para iterar sobre cada <xref:Microsoft.CodeAnalysis.SyntaxTree> no teste <xref:Microsoft.CodeAnalysis.Compilation>. Para cada um, inicialize um novo `TypeInferenceRewriter` com o <xref:Microsoft.CodeAnalysis.SemanticModel> para essa árvore:

[!code-csharp[IterateTrees](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/Program.cs#IterateTrees "Iterate all the source trees in the test compilation")]

Dentro da instrução `foreach` criada, adicione o seguinte código para executar a transformação em cada árvore de origem. Este código registra condicionalmente a nova árvore transformada se alguma edição tiver sido feita. Seu regravador só deve modificar uma árvore se encontrar uma ou mais declarações de variáveis ​​locais que poderiam ser simplificadas usando a inferência de tipos:

[!code-csharp[TransformTrees](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/Program.cs#TransformTrees "Transform and save any trees that are modified by the rewriter")]

Você deve ver rabiscos abaixo do código `File.WriteAllText`. Selecione a lâmpada e adicione a instrução `using System.IO;` necessária.

Você está quase lá! Há uma etapa à esquerda: Criando um teste <xref:Microsoft.CodeAnalysis.Compilation> . Como você não usou a inferência de tipos durante este guia de início rápido, este seria um caso de teste perfeito. Infelizmente, criar uma compilação a partir de um arquivo de projeto C# está além do escopo desta explicação passo a passo. Mas, felizmente, se você está seguindo as instruções cuidadosamente, há esperança. Substitua o conteúdo do método de `CreateTestCompilation` com o código a seguir. Ele cria uma compilação de teste que combina coincidentemente com o projeto descrito neste guia de início rápido:

[!code-csharp[CreateTestCompilation](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxTransformationQuickStart/TransformationCS/Program.cs#CreateTestCompilation "Create a test compilation using the code written for this quickstart.")]

Cruze os dedos e execute o projeto. No Visual Studio, escolha **depurar**  >  **Iniciar Depuração**. O Visual Studio exibirá um aviso dizendo que os arquivos em seu projeto foram alterados. Clique em "**Sim para Todos**" para recarregar os arquivos modificados. Examine-os para observar sua grandiosidade. Observe como o código parece mais limpo sem todos os especificadores de tipo explícitos e redundantes.

Parabéns! Você usou as **APIs do compilador** para criar sua própria refatoração que pesquisa todos os arquivos em um projeto C# para determinados padrões sintáticos, analisa a semântica do código-fonte que corresponde a esses padrões e os transforma. Agora você é oficialmente um autor de refatoração!
