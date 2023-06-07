---
title: 'Tutorial: escrever seu primeiro analisador e correção de código'
description: Este tutorial fornece instruções passo a passo para criar um analisador e correção de código usando o SDK do .NET Compiler (APIs do Roslyn).
ms.date: 08/01/2018
ms.custom: mvc
ms.openlocfilehash: e79907f364939462b7d0d5814c4752be23bcfdf3
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381587"
---
# <a name="tutorial-write-your-first-analyzer-and-code-fix"></a>Tutorial: escrever seu primeiro analisador e correção de código

O SDK da .NET Compiler Platform fornece as ferramentas necessárias para criar avisos personalizados destinados ao C# ou código do Visual Basic. Seu **analisador** contém código que reconhece as violações da sua regra. Sua **correção de código** contém o código que corrige a violação. As regras que você implementar podem ser qualquer coisa, incluindo estrutura do código, estilo de codificação, convenções de nomenclatura e muito mais. O .NET Compiler Platform fornece a estrutura para executar análise conforme os desenvolvedores escrevem o código, bem como todos os recursos de interface do usuário do Visual Studio para corrigir o código: mostrar rabiscos no editor, popular a Lista de Erros do Visual Studio, criar as sugestões da "lâmpada" e mostrar a visualização avançada das correções sugeridas.

Neste tutorial, você explorará a criação de um **analisador** e uma **correção de código** que o acompanha, usando as APIs do Roslyn. Um analisador é uma maneira de executar a análise de código-fonte e relatar um problema para o usuário. Opcionalmente, um analisador também pode fornecer uma correção de código que representa uma modificação no código-fonte do usuário. Este tutorial cria um analisador que localiza as declarações de variável local que poderiam ser declaradas usando o modificador `const`, mas não o são. A correção de código anexa modifica essas declarações para adicionar o modificador `const`.

## <a name="prerequisites"></a>Pré-requisitos

> [!NOTE]
> O modelo atual do **analisador do Visual Studio com o código Fix (.net Standard)** tem um bug conhecido e deve ser corrigido no Visual Studio 2019 versão 16,7. Os projetos no modelo não serão compilados, a menos que sejam feitas as seguintes alterações:
>
> 1. Selecione **ferramentas**  >  **Opções**  >  pacote**Gerenciador de pacotes NuGet**  >  **fontes**
>    - Selecione o botão de adição para adicionar uma nova fonte:
>    - Defina a **origem** como `https://dotnet.myget.org/F/roslyn-analyzers/api/v3/index.json` e selecione **Atualizar**
> 1. No **Gerenciador de soluções**, clique com o botão direito do mouse no projeto **MakeConst. vsix** e selecione **Editar arquivo de projeto**
>    - Atualize o `<AssemblyName>` nó para adicionar o `.Visx` sufixo:
>      - `<AssemblyName>MakeConst.Vsix</AssemblyName>`
>    - Atualize o `<ProjectReference>` nó na linha 41 para alterar o `TargetFramework` valor:
>      - `<ProjectReference Update="@(ProjectReference)" AdditionalProperties="TargetFramework=netstandard2.0" />`
> 1. Atualize o arquivo *MakeConstUnitTests.cs* , no projeto *MakeConst. Test* :
>    - Altere a linha 9 para o seguinte, observe a alteração do namespace:
>      - `using Verify = Microsoft.CodeAnalysis.CSharp.Testing.MSTest.CodeFixVerifier<`
>    - Altere a linha 24 para o seguinte método:
>      - `await Verify.VerifyAnalyzerAsync(test);`
>    - Altere a linha 62 para o seguinte método:
>      - `await Verify.VerifyCodeFixAsync(test, expected, fixtest);`

- [Visual Studio 2017](https://visualstudio.microsoft.com/vs/older-downloads/#visual-studio-2017-and-other-products)
- [Visual Studio 2019](https://www.visualstudio.com/downloads)

Você precisará instalar o **SDK do .net Compiler Platform** por meio do instalador do Visual Studio:

[!INCLUDE[interactive-note](~/includes/roslyn-installation.md)]

Há várias etapas para criar e validar o analisador:

1. Crie a solução.
1. Registre o nome e a descrição do analisador.
1. Relate os avisos e recomendações do analisador.
1. Implemente a correção de código para aceitar as recomendações.
1. Melhore a análise por meio de testes de unidade.

## <a name="explore-the-analyzer-template"></a>Explorar o modelo do analisador

O analisador relata ao usuário quaisquer declarações de variável local que podem ser convertidas em constantes locais. Por exemplo, considere o seguinte código:

```csharp
int x = 0;
Console.WriteLine(x);
```

No código acima, um valor constante é atribuído a `x`, que nunca é modificado. Ele pode ser declarado usando o modificador `const`:

```csharp
const int x = 0;
Console.WriteLine(x);
```

A análise para determinar se uma variável pode ser tornada constante está envolvida, exigindo análise sintática, análise constante da expressão de inicializador e também análise de fluxo de dados, para garantir que nunca ocorram gravações na variável. O .NET Compiler Platform fornece APIs que facilitam essa análise. A primeira etapa é criar um novo em projeto de **Analisador com correção de código** do C#.

- No Visual Studio, escolha **Arquivo > Novo > Projeto...** para exibir a caixa de diálogo Novo Projeto.
- Em **Visual C# > Extensibilidade**, escolha **Analisador com correção de código (.NET Standard)**.
- Nomeie seu projeto como "**MakeConst**" e clique em OK.

O analisador com modelo de correção de código cria três projetos: um contém o analisador e a correção de código, o segundo é um projeto de teste de unidade e o terceiro é o projeto VSIX. O projeto de inicialização padrão é o projeto VSIX. Pressione <kbd>F5</kbd> para iniciar o projeto VSIX. Isso inicia uma segunda instância do Visual Studio que tenha carregado o seu novo analisador.

> [!TIP]
> Quando você executa seu analisador, você pode iniciar uma segunda cópia do Visual Studio. Essa segunda cópia usa um hive do Registro diferente para armazenar configurações. Isso lhe permite diferenciar as configurações visuais em duas cópias do Visual Studio. Você pode escolher um tema diferente para a execução experimental do Visual Studio. Além disso, não use perfil móvel de suas configurações nem faça logon na conta do Visual Studio usando a execução experimental do Visual Studio. Isso mantém as diferenças entre as configurações.

Na segunda instância do Visual Studio que você acabou de iniciar, crie um novo projeto de aplicativo de console em C# (o .NET Core ou .NET Framework projeto funcionará, os analisadores funcionam no nível de origem). Passe o mouse sobre o token com um sublinhado ondulado e o texto de aviso fornecido por um analisador é exibido.

O modelo cria um analisador que relata um aviso em cada declaração de tipo em que o nome do tipo contém letras minúsculas, conforme mostrado na figura a seguir:

![Analisador de aviso de relatórios](media/how-to-write-csharp-analyzer-code-fix/report-warning.png)

O modelo também fornece uma correção de código que altera qualquer nome de tipo que contenha caracteres de letras minúsculas, deixando-o com todas as letras maiúsculas. Você pode clicar na lâmpada exibida com o aviso para ver as alterações sugeridas. Aceitar as alterações sugeridas atualiza o nome do tipo e todas as referências para esse tipo na solução. Agora que você já viu o analisador inicial em ação, feche a segunda instância do Visual Studio e retorne ao projeto do analisador.

Você não precisa iniciar uma segunda cópia do Visual Studio e criar um novo código para testar cada alteração em seu analisador. O modelo também cria um projeto de teste de unidade para você. Esse projeto contém dois testes. `TestMethod1` mostra o formato típico de um teste que analisa o código sem disparar um diagnóstico. `TestMethod2` mostra o formato de um teste que dispara um diagnóstico e, em seguida, aplica uma correção de código sugerida. Conforme você cria o analisador e a correção de código, você escreve testes para estruturas de código diferentes para verificar seu trabalho. Testes de unidade para os analisadores são muito mais rápidos do que testá-los de forma interativa com o Visual Studio.

> [!TIP]
> Testes de unidade de analisador são uma excelente ferramenta quando você sabe quais constructos de código devem e não devem disparar seu analisador. Carregar o analisador em outra cópia do Visual Studio é uma excelente ferramenta para explorar e encontrar constructos nos quais você talvez não tenha pensado ainda.

## <a name="create-analyzer-registrations"></a>Criar registros do analisador

O modelo cria a classe inicial `DiagnosticAnalyzer`, no arquivo **MakeConstAnalyzer.cs**. Esse analisador inicial mostra duas propriedades importantes de cada analisador.

- Cada analisador de diagnóstico deve fornecer um atributo `[DiagnosticAnalyzer]` que descreve a linguagem em que opera.
- Cada analisador de diagnóstico deve derivar da classe <xref:Microsoft.CodeAnalysis.Diagnostics.DiagnosticAnalyzer>.

O modelo também mostra os recursos básicos que fazem parte de qualquer analisador:

1. Registrar ações. As ações representam alterações de código que devem disparar o analisador para examinar se há violações de código. Quando o Visual Studio detecta as edições de código que correspondem a uma ação registrada, ele chama o método registrado do analisador.
1. Criar diagnósticos. Quando o analisador detecta uma violação, ele cria um objeto de diagnóstico que o Visual Studio usa para notificar o usuário sobre a violação.

Registrar ações na substituição do método <xref:Microsoft.CodeAnalysis.Diagnostics.DiagnosticAnalyzer.Initialize(Microsoft.CodeAnalysis.Diagnostics.AnalysisContext)?displayProperty=nameWithType>. Neste tutorial, você visitará **nós de sintaxe** em busca de declarações locais e verá quais delas têm valores constantes. Se houver possibilidade de uma declaração ser constante, seu analisador criará e relatará um diagnóstico.

A primeira etapa é atualizar as constantes de registro e o método `Initialize`, de modo que essas constantes indiquem seu analisador "Make Const". A maioria das constantes de cadeia de caracteres é definida no arquivo de recurso de cadeia de caracteres. Você deve seguir essa prática para uma localização mais fácil. Abra o arquivo **Resources.resx** para o projeto do analisador **MakeConst**. Isso exibe o editor de recursos. Atualize os recursos de cadeia de caracteres da seguinte maneira:

- Altere `AnalyzerTitle` para "A variável pode ser tornada constante".
- Altere `AnalyzerMessageFormat` para "Pode ser tornada constante".
- Altere `AnalyzerDescription` para "Tornar Constante".

Além disso, lembre-se de alterar a lista suspensa **Modificador de Acesso** para `public`. Isso facilita o uso dessas constantes em testes de unidade. Quando você terminar, o editor de recursos deverá aparecer como mostrado na figura a seguir:

![Atualizar recursos de cadeia de caracteres](media/how-to-write-csharp-analyzer-code-fix/update-string-resources.png)

As alterações restantes estão no arquivo do analisador. Abra **MakeConstAnalyzer.cs** no Visual Studio. Altere a ação registrada de uma que age em símbolos para uma que age sobre a sintaxe. No método `MakeConstAnalyzerAnalyzer.Initialize`, localize a linha que registra a ação em símbolos:

```csharp
context.RegisterSymbolAction(AnalyzeSymbol, SymbolKind.NamedType);
```

Substitua-a com a seguinte linha:

[!code-csharp[Register the node action](~/samples/snippets/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst/MakeConstAnalyzer.cs#RegisterNodeAction "Register a node action")]

Após essa alteração, você poderá excluir o método `AnalyzeSymbol`. Este analisador examina <xref:Microsoft.CodeAnalysis.CSharp.SyntaxKind.LocalDeclarationStatement?displayProperty=nameWithType>, e não instruções <xref:Microsoft.CodeAnalysis.SymbolKind.NamedType?displayProperty=nameWithType>. Observe que `AnalyzeNode` tem rabiscos vermelhos sob ele. O código apenas que você acaba de adicionar referencia um método `AnalyzeNode` que não foi declarado. Declare esse método usando o seguinte código:

```csharp
private void AnalyzeNode(SyntaxNodeAnalysisContext context)
{
}
```

Altere o `Category` para "Usage" em **MakeConstAnalyzer.cs**, conforme mostrado no código a seguir:

```csharp
private const string Category = "Usage";
```

## <a name="find-local-declarations-that-could-be-const"></a>Localize as declarações locais que podem ser constantes

É hora de escrever a primeira versão do método `AnalyzeNode`. Ele deve procurar uma única declaração local que poderia ser `const` mas não é, algo semelhante ao seguinte código:

```csharp
int x = 0;
Console.WriteLine(x);
```

A primeira etapa é encontrar declarações locais. Adicione o seguinte código a `AnalyzeNode` em **MakeConstAnalyzer.cs**:

```csharp
var localDeclaration = (LocalDeclarationStatementSyntax)context.Node;
```

Essa conversão sempre terá êxito porque seu analisador fez o registro para alterações unicamente a declarações locais. Nenhum outro tipo de nó dispara uma chamada para seu método `AnalyzeNode`. Em seguida, verifique a declaração para quaisquer modificadores `const`. Se você encontrá-los, retorne imediatamente. O código a seguir procura por quaisquer modificadores `const` na declaração local:

```csharp
// make sure the declaration isn't already const:
if (localDeclaration.Modifiers.Any(SyntaxKind.ConstKeyword))
{
    return;
}
```

Por fim, você precisa verificar que a variável pode ser `const`. Isso significa assegurar que ela nunca seja atribuída após ser inicializada.

Você executará alguma análise semântica usando o <xref:Microsoft.CodeAnalysis.Diagnostics.SyntaxNodeAnalysisContext>. Você usa o argumento `context` para determinar se a declaração de variável local pode ser tornada `const`. Um <xref:Microsoft.CodeAnalysis.SemanticModel?displayProperty=nameWithType> representa todas as informações semânticas em um único arquivo de origem. Você pode aprender mais no artigo que aborda [modelos semânticos](../work-with-semantics.md). Você usará o <xref:Microsoft.CodeAnalysis.SemanticModel?displayProperty=nameWithType> para realizar a análise de fluxo de dados na instrução de declaração local. Em seguida, você usa os resultados dessa análise de fluxo de dados para garantir que a variável local não seja escrita com um novo valor em nenhum outro lugar. Chame o método de extensão <xref:Microsoft.CodeAnalysis.ModelExtensions.GetDeclaredSymbol%2A> para recuperar o <xref:Microsoft.CodeAnalysis.ILocalSymbol> para a variável e verifique se ele não está contido na coleção <xref:Microsoft.CodeAnalysis.DataFlowAnalysis.WrittenOutside%2A?displayProperty=nameWithType> da análise de fluxo de dados. Adicione o seguinte código ao final do método `AnalyzeNode`:

```csharp
// Perform data flow analysis on the local declaration.
var dataFlowAnalysis = context.SemanticModel.AnalyzeDataFlow(localDeclaration);

// Retrieve the local symbol for each variable in the local declaration
// and ensure that it is not written outside of the data flow analysis region.
var variable = localDeclaration.Declaration.Variables.Single();
var variableSymbol = context.SemanticModel.GetDeclaredSymbol(variable);
if (dataFlowAnalysis.WrittenOutside.Contains(variableSymbol))
{
    return;
}
```

O código recém-adicionado garante que a variável não seja modificada e pode, portanto, ser tornada `const`. É hora de gerar o diagnóstico. Adicione o código a seguir como a última linha em `AnalyzeNode`:

```csharp
context.ReportDiagnostic(Diagnostic.Create(Rule, context.Node.GetLocation()));
```

Você pode verificar seu andamento pressionando <kbd>F5</kbd> para executar o analisador. Você pode carregar o aplicativo de console que você criou anteriormente e, em seguida, adicionar o seguinte código de teste:

```csharp
int x = 0;
Console.WriteLine(x);
```

A lâmpada deve aparecer e o analisador deve relatar um diagnóstico. No entanto, a lâmpada ainda usa a correção de código gerada por modelo, e diz a você que ela pode ser colocada em letras maiúsculas. A próxima seção explica como escrever a correção de código.

## <a name="write-the-code-fix"></a>Escrever a correção de código

Um analisador pode fornecer uma ou mais correções de código. Uma correção de código define uma edição que resolve o problema relatado. Para o analisador que você criou, você pode fornecer uma correção de código que insere a palavra-chave const:

```csharp
const int x = 0;
Console.WriteLine(x);
```

O usuário escolhe-a da lâmpada da interface do usuário no editor e do Visual Studio altera o código.

Abra o arquivo **MakeConstCodeFixProvider.cs** adicionado pelo modelo.  Essa correção de código já está conectada à ID de Diagnóstico produzida pelo analisador de diagnóstico, mas ela ainda não implementa a transformação de código correta. Primeiro você deve remover parte do código de modelo. Altere a cadeia de caracteres do título para "Tornar constante":

[!code-csharp[Update the CodeFix title](~/samples/snippets/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst/MakeConstCodeFixProvider.cs#CodeFixTitle "Update the CodeFix title")]

Em seguida, exclua o método `MakeUppercaseAsync`. Ele não se aplica mais.

Todos os provedores de correção de código derivam de <xref:Microsoft.CodeAnalysis.CodeFixes.CodeFixProvider>. Todos eles substituem <xref:Microsoft.CodeAnalysis.CodeFixes.CodeFixProvider.RegisterCodeFixesAsync(Microsoft.CodeAnalysis.CodeFixes.CodeFixContext)?displayProperty=nameWithType> para relatar as correções de código disponíveis. Em `RegisterCodeFixesAsync`, altere o tipo de nó ancestral pelo qual você está pesquisando para um <xref:Microsoft.CodeAnalysis.CSharp.Syntax.LocalDeclarationStatementSyntax> para corresponder ao diagnóstico:

[!code-csharp[Find local declaration node](~/samples/snippets/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst/MakeConstCodeFixProvider.cs#FindDeclarationNode  "Find the local declaration node that raised the diagnostic")]

Em seguida, altere a última linha para registrar uma correção de código. A correção criará um novo documento resultante da adição do modificador `const` para uma declaração existente:

[!code-csharp[Register the new code fix](~/samples/snippets/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst/MakeConstCodeFixProvider.cs#RegisterCodeFix  "Register the new code fix")]

Você observará rabiscos vermelhos no código que você acabou de adicionar no símbolo `MakeConstAsync`. Adicione uma declaração para `MakeConstAsync` semelhante ao seguinte código:

```csharp
private async Task<Document> MakeConstAsync(Document document,
   LocalDeclarationStatementSyntax localDeclaration,
   CancellationToken cancellationToken)
{
}
```

Seu novo método `MakeConstAsync` transformará o <xref:Microsoft.CodeAnalysis.Document> que representa o arquivo de origem do usuário em um novo <xref:Microsoft.CodeAnalysis.Document> que agora contém uma declaração `const`.

Você cria um novo token de palavra-chave `const` a ser inserido no início da instrução de declaração. Tenha cuidado para remover qualquer desafio à esquerda do primeiro token de instrução de declaração e anexe-o ao token `const`. Adicione o seguinte código ao método `MakeConstAsync`:

[!code-csharp[Create a new const keyword token](~/samples/snippets/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst/MakeConstCodeFixProvider.cs#CreateConstToken  "Create the new const keyword token")]

Em seguida, adicione o token `const` à declaração usando o seguinte código:

```csharp
// Insert the const token into the modifiers list, creating a new modifiers list.
var newModifiers = trimmedLocal.Modifiers.Insert(0, constToken);
// Produce the new local declaration.
var newLocal = trimmedLocal
    .WithModifiers(newModifiers)
    .WithDeclaration(localDeclaration.Declaration);
```

Em seguida, formate a nova declaração de acordo com regras de formatação de C#. Formatar de suas alterações para corresponderem ao código existente cria uma experiência melhor. Adicione a instrução a seguir imediatamente após o código existente:

[!code-csharp[Format the new declaration](~/samples/snippets/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst/MakeConstCodeFixProvider.cs#FormatLocal  "Format the new declaration")]

Um novo namespace é necessário para esse código. Adicione a seguinte `using` diretiva à parte superior do arquivo:

```csharp
using Microsoft.CodeAnalysis.Formatting;
```

A etapa final é fazer a edição. Há três etapas para esse processo:

1. Obter um identificador para o documento existente.
1. Criar um novo documento, substituindo a declaração existente pela nova declaração.
1. Retornar o novo documento.

Adicione o seguinte código ao final do método `MakeConstAsync`:

[!code-csharp[replace the declaration](~/samples/snippets/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst/MakeConstCodeFixProvider.cs#ReplaceDocument  "Generate a new document by replacing the declaration")]

A correção de código está pronta para ser experimentada.  Pressione <kbd>F5</kbd> para executar o projeto do analisador em uma segunda instância do Visual Studio. Na segunda instância do Visual Studio, crie um novo projeto de Aplicativo de Console em C# e adicione algumas declarações de variável local inicializadas com valores constantes para o método Main. Você verá que elas são relatadas como avisos, conforme mostrado a seguir.

![Pode fazer avisos constantes](media/how-to-write-csharp-analyzer-code-fix/make-const-warning.png)

Você fez muito progresso. Há rabiscos sob as declarações que podem ser tornados `const`. Mas ainda há trabalho a fazer. Isso funciona bem se você adicionar `const` às declarações começando com `i`, depois `j` e, por fim, `k`. Mas se você adicionar o modificador `const` em uma ordem diferente, começando com `k`, seu analisador criará erros: `k` não pode ser declarado como `const`, a menos que `i` e `j` já sejam ambos `const`. Você tem que fazer mais análise para assegurar que lida com as diferentes maneiras em que variáveis podem ser declaradas e inicializadas.

## <a name="build-data-driven-tests"></a>Criar testes controlados por dados

Seu analisador e correção de código trabalham em um caso simples de uma única declaração que pode ser tornada const. Há várias instruções de declaração possíveis em que essa implementação comete erros. Você tratará desses casos trabalhando com a biblioteca de teste de unidade gravada pelo modelo. Isso é muito mais rápido do que abrir repetidamente uma segunda cópia do Visual Studio.

Abra o arquivo **MakeConstUnitTests.cs** no projeto de teste de unidade. O modelo criou dois testes que seguem os dois padrões comuns para um analisador e o teste de unidade de correção de código. `TestMethod1` mostra o padrão para um teste que garante que o analisador não relata um diagnóstico quando não deve fazê-lo. `TestMethod2` mostra o padrão para relatar um diagnóstico e executar a correção de código.

O código para quase todos os testes para o seu analisador segue um destes dois padrões. Para a primeira etapa, você pode refazer esses testes como testes controlados por dados. Em seguida, será fácil criar novos testes adicionando novas constantes de cadeia de caracteres para representar diferentes entradas de teste.

Para obter eficiência, a primeira etapa é refatorar os dois testes em testes controlados por dados. Em seguida, você só precisa definir algumas constantes de cadeia de caracteres para cada novo teste. Enquanto você estiver Refatorando, renomeie os dois métodos para melhores nomes. Substitua `TestMethod1` com este teste, que garante que nenhum diagnóstico seja gerado:

```csharp
[DataTestMethod]
[DataRow("")]
public void WhenTestCodeIsValidNoDiagnosticIsTriggered(string testCode)
{
    VerifyCSharpDiagnostic(testCode);
}
```

Você pode criar uma nova linha de dados para este teste, definindo qualquer fragmento de código que não faça com que o diagnóstico dispare um aviso. Essa sobrecarga de `VerifyCSharpDiagnostic` passa quando não há nenhum diagnóstico disparado para o fragmento de código-fonte.

Em seguida, substitua `TestMethod2` com esse teste que garante que um diagnóstico é gerado e uma correção de código aplicada ao fragmento de código-fonte:

```csharp
[DataTestMethod]
[DataRow(LocalIntCouldBeConstant, LocalIntCouldBeConstantFixed, 10, 13)]
public void WhenDiagnosticIsRaisedFixUpdatesCode(
    string test,
    string fixTest,
    int line,
    int column)
{
    var expected = new DiagnosticResult
    {
        Id = MakeConstAnalyzer.DiagnosticId,
        Message = new LocalizableResourceString(nameof(MakeConst.Resources.AnalyzerMessageFormat), MakeConst.Resources.ResourceManager, typeof(MakeConst.Resources)).ToString(),
        Severity = DiagnosticSeverity.Warning,
        Locations =
            new[] {
                    new DiagnosticResultLocation("Test0.cs", line, column)
                }
    };

    VerifyCSharpDiagnostic(test, expected);

    VerifyCSharpFix(test, fixTest);
}
```

O código anterior também fez algumas alterações no código que cria o resultado de diagnóstico esperado. Ele usa as constantes públicas registradas no analisador `MakeConst`. Além disso, ele usa duas constantes de cadeia de caracteres para a fonte de entrada e fonte fixa. Adicione as seguintes constantes de cadeia de caracteres à classe `UnitTest`:

[!code-csharp[string constants for fix test](~/samples/snippets/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#FirstFixTest "string constants for fix test")]

Execute esses dois testes para certificar-se de sua aprovação. No Visual Studio, abra o **Gerenciador de Testes** selecionando **Teste** > **Windows** > **Gerenciador de Testes**. Em seguida, selecione o link **executar tudo** .

## <a name="create-tests-for-valid-declarations"></a>Criar testes para declarações válidas

Como regra geral, os analisadores devem sair assim que possível, fazendo o mínimo de trabalho. O Visual Studio chama analisadores registrados conforme o usuário edita o código. A capacidade de resposta é um requisito fundamental. Há vários casos de teste para o código que não deverão gerar o diagnóstico. O analisador já gerencia um desses testes, o caso em que uma variável é atribuída após ser inicializada. Adicione a constante de cadeia de caracteres a seguir para seus testes para representar esse caso:

[!code-csharp[variable assigned](~/samples/snippets/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#VariableAssigned "a variable that is assigned after being initialized won't raise the diagnostic")]

Em seguida, adicione uma linha de dados para este teste, conforme mostrado no snippet de código a seguir:

```csharp
[DataTestMethod]
[DataRow(""),
 DataRow(VariableAssigned)]
public void WhenTestCodeIsValidNoDiagnosticIsTriggered(string testCode)
```

Esse teste é aprovado também. Em seguida, adicione as constantes para as condições que você ainda não manipulou ainda:

- Declarações que já são `const`, porque elas já são const:

   [!code-csharp[already const declaration](~/samples/snippets/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#AlreadyConst "a declaration that is already const should not raise the diagnostic")]

- Declarações que não têm nenhum inicializador, porque não há nenhum valor a ser usado:

   [!code-csharp[declarations that have no initializer](~/samples/snippets/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#NoInitializer "a declaration that has no initializer should not raise the diagnostic")]

- Declarações em que o inicializador não é uma constante, porque elas não podem ser constantes de tempo de compilação:

   [!code-csharp[declarations where the initializer isn't const](~/samples/snippets/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#InitializerNotConstant "a declaration where the initializer is not a compile-time constant should not raise the diagnostic")]

Isso pode ser ainda mais complicado, porque o C# permite várias declarações como uma instrução. Considere a seguinte constante de cadeia de caracteres de caso de teste:

[!code-csharp[multiple initializers](~/samples/snippets/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#MultipleInitializers "A declaration can be made constant only if all variables in that statement can be made constant")]

A variável `i` pode ser tornada constante, mas o mesmo não se aplica à variável `j`. Portanto, essa instrução não pode ser tornada uma declaração const. Adicione as declarações `DataRow` para todos esses testes:

```csharp
[DataTestMethod]
[DataRow(""),
    DataRow(VariableAssigned),
    DataRow(AlreadyConst),
    DataRow(NoInitializer),
    DataRow(InitializerNotConstant),
    DataRow(MultipleInitializers)]
public void WhenTestCodeIsValidNoDiagnosticIsTriggered(string testCode)
```

Execute os testes novamente e você verá esses novos casos de teste falharem.

## <a name="update-your-analyzer-to-ignore-correct-declarations"></a>Atualize seu analisador para ignorar as declarações corretas

Você precisa de algumas melhorias no método `AnalyzeNode` do analisador para filtrar o código que corresponde a essas condições. Elas são todas condições relacionadas, portanto, alterações semelhantes corrigirão todas essas condições. Faça as alterações a seguir em `AnalyzeNode`:

- A análise semântica examinou uma única declaração de variável. Esse código deve estar em um loop de `foreach` que examina todas as variáveis declaradas na mesma instrução.
- Cada variável declarada precisa ter um inicializador.
- O inicializador de cada variável declarada precisa ser uma constante de tempo de compilação.

No seu método `AnalyzeNode`, substitua a análise semântica original:

```csharp
// Perform data flow analysis on the local declaration.
var dataFlowAnalysis = context.SemanticModel.AnalyzeDataFlow(localDeclaration);

// Retrieve the local symbol for each variable in the local declaration
// and ensure that it is not written outside of the data flow analysis region.
var variable = localDeclaration.Declaration.Variables.Single();
var variableSymbol = context.SemanticModel.GetDeclaredSymbol(variable);
if (dataFlowAnalysis.WrittenOutside.Contains(variableSymbol))
{
    return;
}
```

com o snippet de código a seguir:

```csharp
// Ensure that all variables in the local declaration have initializers that
// are assigned with constant values.
foreach (var variable in localDeclaration.Declaration.Variables)
{
    var initializer = variable.Initializer;
    if (initializer == null)
    {
        return;
    }

    var constantValue = context.SemanticModel.GetConstantValue(initializer.Value);
    if (!constantValue.HasValue)
    {
        return;
    }
}

// Perform data flow analysis on the local declaration.
var dataFlowAnalysis = context.SemanticModel.AnalyzeDataFlow(localDeclaration);

foreach (var variable in localDeclaration.Declaration.Variables)
{
    // Retrieve the local symbol for each variable in the local declaration
    // and ensure that it is not written outside of the data flow analysis region.
    var variableSymbol = context.SemanticModel.GetDeclaredSymbol(variable);
    if (dataFlowAnalysis.WrittenOutside.Contains(variableSymbol))
    {
        return;
    }
}
```

O primeiro loop de `foreach` examina cada declaração de variável usando a análise sintática. A primeira verificação garante que a variável tenha um inicializador. A segunda verificação garante que o inicializador seja uma constante. O segundo loop tem a análise semântica original. As verificações semânticas estão em um loop separado porque ele tem um impacto maior no desempenho. Execute os testes novamente e você deverá ver todos eles serem aprovados.

## <a name="add-the-final-polish"></a>Adicionar o final polonês

Você está quase lá. Há mais algumas condições com as quais o seu analisador deve lidar. Enquanto o usuário está escrevendo código, o Visual Studio chama os analisadores. Muitas vezes o analisador será chamado para código que não é compilado. O método `AnalyzeNode` do analisador de diagnóstico não verifica para ver se o valor da constante é conversível para o tipo de variável. Assim, a implementação atual converterá facilmente uma declaração incorreta, tal como int i = "abc", em uma constante local. Adicione uma constante de cadeia de caracteres de origem para essa condição:

[!code-csharp[Mismatched types don't raise diagnostics](~/samples/snippets/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#DeclarationIsInvalid "When the variable type and the constant type don't match, there's no diagnostic")]

Além disso, os tipos de referência não são tratados corretamente. O único valor de constante permitido para um tipo de referência é `null`, exceto no caso de <xref:System.String?displayProperty=nameWithType>, que permite literais de cadeia de caracteres. Em outras palavras, `const string s = "abc"` é legal, mas `const object s = "abc"` não é. Este snippet de código verifica essa condição:

[!code-csharp[Reference types don't raise diagnostics](~/samples/snippets/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#DeclarationIsntString "When the variable type is a reference type other than string, there's no diagnostic")]

Para ser criterioso, você precisará adicionar outro teste para verificar se pode criar uma declaração de constante para uma cadeia de caracteres. O snippet de código a seguir define o código que gera o diagnóstico e o código após a aplicação da correção:

[!code-csharp[string reference types raise diagnostics](~/samples/snippets/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#ConstantIsString "When the variable type is string, it can be constant")]

Por fim, se uma variável é declarada com a palavra-chave `var`, a correção de código faz a coisa errada e gera uma declaração `const var`, que não é compatível com a linguagem C#. Para corrigir esse bug, a correção de código deve substituir a palavra-chave `var` pelo nome do tipo inferido:

[!code-csharp[var references need to use the inferred types](~/samples/snippets/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#VarDeclarations "Declarations made using var must have the type replaced with the inferred type")]

Essas alterações atualizam as declarações de linha de dados para ambos os testes. O código a seguir mostra esses testes com todos os atributos de linha de dados:

[!code-csharp[The finished tests](~/samples/snippets/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#FinishedTests "The finished tests for the make const analyzer")]

Felizmente, todos os erros acima podem ser resolvidos usando as mesmas técnicas que você acabou de aprender.

Para corrigir o primeiro bug, primeiro abra **DiagnosticAnalyzer.cs** e localize o loop foreach em que cada um dos inicializadores de declaração local é verificado para garantir que valores constantes sejam atribuídos a eles. Imediatamente _antes_ do primeiro loop foreach, chame `context.SemanticModel.GetTypeInfo()` para recuperar informações detalhadas sobre o tipo declarado da declaração local:

```csharp
var variableTypeName = localDeclaration.Declaration.Type;
var variableType = context.SemanticModel.GetTypeInfo(variableTypeName).ConvertedType;
```

Em seguida, dentro do loop `foreach`, verifique cada inicializador para garantir que ele pode ser convertido no tipo de variável. Adicione a seguinte verificação depois de garantir que o inicializador é uma constante:

```csharp
// Ensure that the initializer value can be converted to the type of the
// local declaration without a user-defined conversion.
var conversion = context.SemanticModel.ClassifyConversion(initializer.Value, variableType);
if (!conversion.Exists || conversion.IsUserDefined)
{
    return;
}
```

A próxima alteração é realizada com base na última. Antes da chave de fechamento do primeiro loop foreach, adicione o código a seguir para verificar o tipo da declaração de local quando a constante é uma cadeia de caracteres ou valor nulo.

```csharp
// Special cases:
//  * If the constant value is a string, the type of the local declaration
//    must be System.String.
//  * If the constant value is null, the type of the local declaration must
//    be a reference type.
if (constantValue.Value is string)
{
    if (variableType.SpecialType != SpecialType.System_String)
    {
        return;
    }
}
else if (variableType.IsReferenceType && constantValue.Value != null)
{
    return;
}
```

Você deve escrever um pouco mais de código no provedor de correção de código para substituir a `var` palavra-chave pelo nome de tipo correto. Retorne ao **CodeFixProvider.cs**. O código que você adicionará realizará as seguintes etapas:

- Verifique se a declaração é uma declaração `var` e se afirmativo:
- Crie um novo tipo para o tipo inferido.
- Certifique-se de que a declaração de tipo não é um alias. Em caso afirmativo, é legal declarar `const var`.
- Certifique-se de que `var` não é um nome de tipo neste programa. (Em caso afirmativo, `const var` é legal).
- Simplificar o nome completo do tipo

Isso soa como muito código. Mas não é. Substitua a linha que declara e inicializa `newLocal` com o código a seguir. Ele é colocado imediatamente após a inicialização de `newModifiers`:

[!code-csharp[Replace Var designations](~/samples/snippets/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst/MakeConstCodeFixProvider.cs#ReplaceVar "Replace a var designation with the explicit type")]

Você precisará adicionar uma `using` diretiva para usar o <xref:Microsoft.CodeAnalysis.Simplification.Simplifier> tipo:

```csharp
using Microsoft.CodeAnalysis.Simplification;
```

Execute seus testes, que devem todos ser aprovados. Dê parabéns a si mesmo, executando seu analisador concluído. Pressione <kbd>Ctrl</kbd> + <kbd>F5</kbd> para executar o projeto do analisador em uma segunda instância do Visual Studio com a extensão de visualização do Roslyn carregada.

- Na segunda instância do Visual Studio, crie um novo projeto de Aplicativo de Console de C# e adicione `int x = "abc";` ao método Main. Graças à primeira correção de bug, nenhum aviso deve ser relatado para esta declaração de variável local (embora haja um erro do compilador, conforme esperado).
- Em seguida, adicione `object s = "abc";` ao método Main. Devido à segunda correção de bug, nenhum aviso deve ser relatado.
- Por fim, adicione outra variável local que usa a palavra-chave `var`. Você verá que um aviso é relatado e uma sugestão é exibida abaixo e a esquerda.
- Mova o cursor do editor sobre o sublinhado ondulado e pressione <kbd>Ctrl</kbd> + <kbd>.</kbd>. para exibir a correção de código sugerida. Ao selecionar a correção de código, observe que a `var` palavra-chave agora é manipulada corretamente.

Por fim, adicione o seguinte código:

```csharp
int i = 2;
int j = 32;
int k = i + j;
```

Após essas alterações, você obtém linhas onduladas vermelhas apenas nas duas primeiras variáveis. Adicione `const` para ambos `i` e `j`, e você receberá um novo aviso em `k` porque ele agora pode ser `const`.

Parabéns! Você criou sua primeira extensão do .NET Compiler Platform que executa análise de código com o sistema em funcionamento para detectar um problema e fornece uma correção rápida para corrigi-lo. Ao longo do caminho, você aprendeu muitas das APIs de código que fazem parte do SDK do .NET Compiler Platform (APIs do Roslyn). Você pode verificar seu trabalho comparando-o à [amostra concluída](https://github.com/dotnet/samples/tree/master/csharp/roslyn-sdk/Tutorials/MakeConst) em nosso repositório GitHub de exemplos.

## <a name="other-resources"></a>Outros recursos

- [Introdução à análise de sintaxe](../get-started/syntax-analysis.md)
- [Introdução à análise semântica](../get-started/semantic-analysis.md)
