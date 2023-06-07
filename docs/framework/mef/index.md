---
title: MEF (Managed Extensibility Framework)
description: Explore o Managed Extensibility Framework (MEF), que permite que os desenvolvedores de aplicativos descubram e usem extensões sem configuração no .NET 4 ou superior.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Managed Extensibility Framework, overview
- MEF, overview
ms.assetid: 6c61b4ec-c6df-4651-80f1-4854f8b14dde
ms.openlocfilehash: 00ed48f2202d4c04039ac264b1fe71474a02432e
ms.sourcegitcommit: 97ce5363efa88179dd76e09de0103a500ca9b659
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2020
ms.locfileid: "86281245"
---
# <a name="managed-extensibility-framework-mef"></a>MEF (Managed Extensibility Framework)

Este tópico fornece uma visão geral da Managed Extensibility Framework introduzida no .NET Framework 4.

## <a name="what-is-mef"></a>O que é a MEF?

O Managed Extensibility Framework ou MEF é uma biblioteca para a criação de aplicativos leves e extensíveis. Ele permite que os desenvolvedores de aplicativos descubram e usem extensões sem nenhuma configuração necessária. Isso também permite aos desenvolvedores de extensão encapsular o código facilmente e evitar dependências rígidas frágeis. A MEF não só permite que extensões sejam reutilizadas em aplicativos, mas também entre aplicativos.

## <a name="the-problem-of-extensibility"></a>O problema da extensibilidade

Imagine que você é arquiteto de um grande aplicativo que deve fornecer suporte para extensibilidade. Seu aplicativo deve incluir um número possivelmente grande de componentes menores, sendo responsável por sua criação e execução.

O método mais simples para o problema é incluem os componentes como código-fonte no seu aplicativo e chamá-los diretamente do código. Isso tem várias desvantagens óbvias. Mais importante delas, não é possível adicionar novos componentes sem modificar o código-fonte, uma restrição que pode ser aceitável em, por exemplo, um aplicativo Web, mas não funciona em um aplicativo cliente. Igualmente problemático, você não terá acesso ao código-fonte dos componentes, pois eles podem ser desenvolvidos por terceiros, pelo mesmo motivo que você não pode permitir que eles acessem o seu.

Uma abordagem um pouco mais sofisticada seria fornecer um ponto de extensão ou interface para permitir a separação entre o aplicativo e seus componentes. Com esse modelo, você pode fornecer uma interface que um componente pode implementar e uma API para habilitá-lo a interagir com seu aplicativo. Isso resolve o problema da necessidade de acesso ao código-fonte, mas ainda traz suas próprias dificuldades.

Como o aplicativo não tem qualquer capacidade para descobrir componentes por conta própria, ele deve ainda ser explicitamente informado de quais componentes estão disponíveis e quais devem ser carregados. Normalmente, isso é feito registrando explicitamente os componentes disponíveis em um arquivo de configuração. Isso significa que garantir que os componentes estão corretos se torna um problema de manutenção, especialmente se for o usuário final e não o desenvolvedor que deverá fazer a atualização.

Além disso, os componentes são incapazes de se comunicar entre si, exceto por meio de canais rigidamente definidos do aplicativo em si. Se o arquiteto do aplicativo não tiver previsto a necessidade de uma comunicação específica, ela geralmente será impossível.

Por fim, os desenvolvedores de componentes devem aceitar uma dependência forte no assembly que contém a interface implementada. Isso torna difícil para um componente a ser usado em mais de um aplicativo e também pode criar problemas quando você cria uma estrutura de testes para componentes.

## <a name="what-mef-provides"></a>O que o MEF fornece

Em vez desse registro explícito de componentes disponíveis, o MEF oferece uma maneira de descobri-los implicitamente, através de *composição*. Um componente do MEF, chamado de *peça*, especifica declarativamente ambas as suas dependências (conhecidas como *importações*) e quais recursos (conhecidos como *exportações*) ele disponibiliza. Quando uma peça é criada, o mecanismo de composição do MEF atende às suas importações com o que está disponível de outras peças.

Essa abordagem resolve os problemas abordados na seção anterior. Como as peças do MEF especificam de forma declarativa seus recursos, elas são descobertas no runtime, o que significa que um aplicativo usar as peças embutidas em código sem referências ou arquivos de configuração frágeis. O MEF permite que os aplicativos descubram e examinem as peças por seus metadados, sem instanciá-las ou até mesmo carregar seus assemblies. Como resultado, não é necessário especificar cuidadosamente quando e como as extensões devem ser carregadas.

Além de suas exportações fornecidas, uma peça pode especificar suas importações, será preenchida por outras peças. Isso não somente torna a comunicação entre peças possível, mas muito mais fácil, e permite faturamento correto do código. Por exemplo, serviços comuns a muitos componentes podem ser faturados em uma peça separada e facilmente modificados ou substituídos.

Como o modelo de MEF não exige nenhuma dependência em um assembly de aplicativo específico, ele permite que extensões sejam reusadas de aplicativo para aplicativo. Isso também facilita desenvolver um agente de teste, independente do aplicativo, para testar os componentes de extensão.

Um aplicativo extensível escrito com o MEF declara uma importação que pode ser preenchida por componentes de extensão e também pode declarar exportações para expor serviços de aplicativo para extensões. Cada componente de extensão declara uma exportação e também pode declarar importações. Dessa forma, os próprios componentes de extensão são automaticamente extensíveis.

## <a name="where-mef-is-available"></a>Onde o MEF está disponível

O MEF é parte integrante do .NET Framework 4 e está disponível em qualquer lugar em que o .NET Framework é usado. Você pode usar o MEF em seus aplicativos clientes, seja usando o Windows Forms, WPF ou qualquer outra tecnologia ou em aplicativos de servidores que usam o ASP.NET.

## <a name="mef-and-maf"></a>MEF e MAF

Versões anteriores do .NET Framework introduziram o MAF (Managed Add-in Framework), projetado para permitir que aplicativos isolem e gerenciem extensões. O foco do MAF é em um nível ligeiramente mais elevado que o MEF, concentrando-se no isolamento de extensão e carregamento e descarregamento do assembly, enquanto o foco do MEF é a descoberta, extensibilidade e portabilidade. As duas estruturas interoperam sem problemas entre si e um único aplicativo pode aproveitar ambas.

## <a name="simplecalculator-an-example-application"></a>SimpleCalculator: um aplicativo de exemplo

A maneira mais simples de ver o que o MEF é criar um aplicativo MEF simples. Neste exemplo, você criará uma calculadora muito simples chamada SimpleCalculator. A meta da SimpleCalculator é criar um aplicativo de console que aceite comandos aritméticos básicos, no formato "5+3" ou "6-2" e retorne as respostas corretas. Usando MEF, você poderá adicionar novos operadores sem alterar o código do aplicativo.

Para baixar o código completo deste exemplo, consulte o [exemplo SimpleCalculator (Visual Basic)](https://docs.microsoft.com/samples/dotnet/samples/simple-calculator-vb/).

> [!NOTE]
> A finalidade da SimpleCalculator é demonstrar os conceitos e a sintaxe do MEF, em vez de fornecer necessariamente um cenário realista para seu uso. Muitos dos aplicativos que mais se beneficiariam da potência do MEF são mais complexos que a SimpleCalculator. Para obter exemplos mais abrangentes, consulte o [Managed Extensibility Framework](https://github.com/MicrosoftArchive/mef) no GitHub.

- Para começar, no Visual Studio, crie um novo projeto de aplicativo de Console e dê a ele o nome `SimpleCalculator`.

- Adicione uma referência ao `System.ComponentModel.Composition` assembly, onde o MEF reside.

- Abra *Module1. vb* ou *Program.cs* e adicione `Imports` `using` instruções or para `System.ComponentModel.Composition` e `System.ComponentModel.Composition.Hosting` . Esses dois namespaces contêm tipos MEF que você precisará para desenvolver um aplicativo extensível.

- Se você estiver usando o Visual Basic, adicione a palavra-chave `Public` à linha que declara o módulo `Module1`.

## <a name="composition-container-and-catalogs"></a>Contêiner de composição e catálogos

O núcleo do modelo de composição de MEF é o *contêiner de composição*, que contém todas as peças disponíveis e executa a composição. Composição e a correspondência de importações e exportações. O tipo mais comum de contêiner de composição é <xref:System.ComponentModel.Composition.Hosting.CompositionContainer>, e você o usará na SimpleCalculator.

Se você estiver usando Visual Basic, adicione uma classe pública chamada `Program` no *Module1. vb*.

Adicione a seguinte linha à `Program` classe no *Module1. vb* ou *Program.cs*:

```vb
Dim _container As CompositionContainer
```

```csharp
private CompositionContainer _container;
```

Para descobrir as peças disponíveis para ele, os contêineres de composição usam um *catálogo*. Um catálogo é um objeto que disponibiliza as peças descobertas em alguma origem. O MEF fornece catálogos para descobrir peças de um tipo fornecido, um assembly ou de um diretório. Os desenvolvedores de aplicativos podem criar facilmente novos catálogos para descobrir as peças de outras fontes, como um serviço Web.

Adicione o seguinte construtor à classe `Program`:

```vb
Public Sub New()
    ' An aggregate catalog that combines multiple catalogs.
     Dim catalog = New AggregateCatalog()

    ' Adds all the parts found in the same assembly as the Program class.
    catalog.Catalogs.Add(New AssemblyCatalog(GetType(Program).Assembly))

    ' Create the CompositionContainer with the parts in the catalog.
    _container = New CompositionContainer(catalog)

    ' Fill the imports of this object.
    Try
        _container.ComposeParts(Me)
    Catch ex As CompositionException
        Console.WriteLine(ex.ToString)
    End Try
End Sub
```

```csharp
private Program()
{
    // An aggregate catalog that combines multiple catalogs.
    var catalog = new AggregateCatalog();
    // Adds all the parts found in the same assembly as the Program class.
    catalog.Catalogs.Add(new AssemblyCatalog(typeof(Program).Assembly));

    // Create the CompositionContainer with the parts in the catalog.
    _container = new CompositionContainer(catalog);

    // Fill the imports of this object.
    try
    {
        this._container.ComposeParts(this);
    }
    catch (CompositionException compositionException)
    {
        Console.WriteLine(compositionException.ToString());
   }
}
```

A chamada para <xref:System.ComponentModel.Composition.AttributedModelServices.ComposeParts%2A> instrui o contêiner de composição a compor um conjunto específico de peças, neste caso a instância atual do `Program`. Neste ponto, no entanto, nada acontecerá, pois o `Program` tem não importações a preencher.

## <a name="imports-and-exports-with-attributes"></a>Importações e exportações com atributos

Primeiro, o `Program` importa uma calculadora. Isso separa questões de interface do usuário, tal como a entrada e a saída do console que entrará em `Program`, da lógica da calculadora.

Adicione o código a seguir à classe `Program`:

```vb
<Import(GetType(ICalculator))>
Public Property calculator As ICalculator
```

```csharp
[Import(typeof(ICalculator))]
public ICalculator calculator;
```

Observe que a declaração do objeto `calculator` não é incomum, mas sim que está decorada com o atributo <xref:System.ComponentModel.Composition.ImportAttribute>. Esse atributo declara algo como uma importação; ou seja, ele será preenchido pelo mecanismo de composição quando o objeto for composto.

Cada importação tem um *contrato*, que determina com quais exportações ele corresponderá. O contrato pode ser uma cadeia de caracteres especificada explicitamente ou ele pode ser automaticamente gerado pelo MEF por meio de um determinado tipo, neste caso, a interface `ICalculator`. Qualquer exportação declarada com um contrato correspondente atender a esta importação. Observe que, enquanto o tipo do objeto `calculator` é na verdade `ICalculator`, isso não é necessário. O contrato é independente do tipo do objeto de importação. (Nesse caso, você poderia deixar o `typeof(ICalculator)`. O MEF automaticamente assumirá que o contrato se baseia no tipo da importação, a menos que seja especificado explicitamente.)

Adicione essa interface simples ao módulo ou ao namespace `SimpleCalculator`:

```vb
Public Interface ICalculator
    Function Calculate(input As String) As String
End Interface
```

```csharp
public interface ICalculator
{
    String Calculate(String input);
}
```

Agora que você definiu o `ICalculator`, precisará de uma classe que o implemente. Adicione a seguinte classe ao módulo ou ao namespace `SimpleCalculator`:

```vb
<Export(GetType(ICalculator))>
Public Class MySimpleCalculator
   Implements ICalculator

End Class
```

```csharp
[Export(typeof(ICalculator))]
class MySimpleCalculator : ICalculator
{

}
```

Aqui está a exportação que corresponde à importação em `Program`. Para que a exportação corresponda à importação, a exportação deverá ter o mesmo contrato. Exportando sob um contrato com base em `typeof(MySimpleCalculator)` produziria uma incompatibilidade e a importação não seria preenchida. O contrato deve ter correspondência exata.

Uma vez que o contêiner de composição será populado com todas as peças disponíveis nesse assembly, a peça `MySimpleCalculator` estará disponível. Quando o construtor do `Program` executa a composição do objeto `Program`, sua importação será preenchida por um objeto `MySimpleCalculator`, que será criado para este fim.

A camada de interface do usuário (`Program`) não precisa saber de mais nada. Portanto, você pode preencher o resto da lógica de interface do usuário no método `Main`.

Adicione o seguinte código ao método `Main`:

```vb
Sub Main()
    ' Composition is performed in the constructor.
    Dim p As New Program()
    Dim s As String
    Console.WriteLine("Enter Command:")
    While (True)
        s = Console.ReadLine()
        Console.WriteLine(p.calculator.Calculate(s))
    End While
End Sub
```

```csharp
static void Main(string[] args)
{
    // Composition is performed in the constructor.
    var p = new Program();
    string s;
    Console.WriteLine("Enter Command:");
    while (true)
    {
        s = Console.ReadLine();
        Console.WriteLine(p.calculator.Calculate(s));
    }
}
```

 Esse código simplesmente lê uma linha de entrada e chama a função `Calculate` do `ICalculator` no resultado, que ele grava de volta ao console. Este é todo o código necessário no `Program`. Todo o resto do trabalho ocorrerá nas peças.

## <a name="further-imports-and-importmany"></a>Outras Importações e ImportMany

Para que a SimpleCalculator seja extensível, ela precisa importar uma lista de operações. Um atributo <xref:System.ComponentModel.Composition.ImportAttribute> ordinário é preenchido por um e somente um <xref:System.ComponentModel.Composition.ExportAttribute>. Se houver mais de uma disponível, o mecanismo de composição gerará um erro. Para criar uma importação pode ser preenchida por qualquer número de exportações, você pode usar o atributo <xref:System.ComponentModel.Composition.ImportManyAttribute>.

Adicione as seguintes propriedades de operações à classe `MySimpleCalculator`:

```vb
<ImportMany()>
Public Property operations As IEnumerable(Of Lazy(Of IOperation, IOperationData))
```

```csharp
[ImportMany]
IEnumerable<Lazy<IOperation, IOperationData>> operations;
```

<xref:System.Lazy%602> é um tipo é fornecido pelo MEF para manter indiretas referências a exportações. Aqui, além do próprio objeto exportado, você também obtém os *metadados da exportação* ou informações que descrevem o objeto exportado. Cada <xref:System.Lazy%602> contém um objeto `IOperation`, que representa uma operação real, e um objeto `IOperationData`, que representa seus metadados.

Adicione as interfaces simples a seguir ao módulo ou ao namespace `SimpleCalculator`:

```vb
Public Interface IOperation
    Function Operate(left As Integer, right As Integer) As Integer
End Interface

Public Interface IOperationData
    ReadOnly Property Symbol As Char
End Interface
```

```csharp
public interface IOperation
{
     int Operate(int left, int right);
}

public interface IOperationData
{
    Char Symbol { get; }
}
```

 Nesse caso, os metadados de cada operação são o símbolo que representa essa operação, como +,-, \* e assim por diante. Para disponibilizar a operação de adição, adicione a seguinte classe ao módulo ou ao namespace `SimpleCalculator`:

```vb
<Export(GetType(IOperation))>
<ExportMetadata("Symbol", "+"c)>
Public Class Add
    Implements IOperation

    Public Function Operate(left As Integer, right As Integer) As Integer Implements IOperation.Operate
        Return left + right
    End Function
End Class
```

```csharp
[Export(typeof(IOperation))]
[ExportMetadata("Symbol", '+')]
class Add: IOperation
{
    public int Operate(int left, int right)
    {
        return left + right;
    }
}
```

O atributo <xref:System.ComponentModel.Composition.ExportAttribute> funciona como antes. O atributo <xref:System.ComponentModel.Composition.ExportMetadataAttribute> anexa metadados na forma de um par de nome-valor à exportação. Enquanto a classe `Add` implementa `IOperation`, uma classe que implementa `IOperationData` não é definida explicitamente. Em vez disso, uma classe é implicitamente criada pelo MEF com propriedades baseadas nos nomes dos metadados fornecidos. (Isso é uma das várias maneiras de acessar os metadados no MEF.)

A composição no MEF é *recursiva*. Você compõe explicitamente o objeto `Program`, que importou um `ICalculator` que acabou sendo do tipo `MySimpleCalculator`. O `MySimpleCalculator`, por sua vez, importa um conjunto de objetos `IOperation` e essa importação será preenchida quando `MySimpleCalculator` é criado, ao mesmo tempo que as importações do `Program`. Se a classe `Add` declarar outra importação, ela também deverá ser preenchida, e assim por diante. Quaisquer importações não preenchidas resultam em um erro de composição. (É possível, no entanto, declarar importações como opcional ou atribuir valores padrão.)

## <a name="calculator-logic"></a>Lógica da Calculadora

Com as peças no lugar, falta apenas a própria lógica da calculadora. Adicione o seguinte código na classe `MySimpleCalculator` para implementar o método `Calculate`:

```vb
Public Function Calculate(input As String) As String Implements ICalculator.Calculate
    Dim left, right As Integer
    Dim operation As Char
    ' Finds the operator.
    Dim fn = FindFirstNonDigit(input)
    If fn < 0 Then
        Return "Could not parse command."
    End If
    operation = input(fn)
    Try
        ' Separate out the operands.
        left = Integer.Parse(input.Substring(0, fn))
        right = Integer.Parse(input.Substring(fn + 1))
    Catch ex As Exception
        Return "Could not parse command."
    End Try
    For Each i As Lazy(Of IOperation, IOperationData) In operations
        If i.Metadata.symbol = operation Then
            Return i.Value.Operate(left, right).ToString()
        End If
    Next
    Return "Operation not found!"
End Function
```

```csharp
public String Calculate(string input)
{
    int left;
    int right;
    char operation;
    // Finds the operator.
    int fn = FindFirstNonDigit(input);
    if (fn < 0) return "Could not parse command.";

    try
    {
        // Separate out the operands.
        left = int.Parse(input.Substring(0, fn));
        right = int.Parse(input.Substring(fn + 1));
    }
    catch
    {
        return "Could not parse command.";
    }

    operation = input[fn];

    foreach (Lazy<IOperation, IOperationData> i in operations)
    {
        if (i.Metadata.Symbol.Equals(operation)) return i.Value.Operate(left, right).ToString();
    }
    return "Operation Not Found!";
}
```

As etapas iniciais analisam a cadeia de caracteres de entrada em operandos esquerdo e direito e um caractere de operador. No loop `foreach`, cada membro da coleção `operations` é examinado. Esses objetos são do tipo <xref:System.Lazy%602> e seus valores de metadados e o objeto exportado o podem ser acessados com as propriedades <xref:System.Lazy%602.Metadata%2A> e <xref:System.Lazy%601.Value%2A>, respectivamente. Nesse caso, se a propriedade `Symbol` do objeto `IOperationData` for correspondente, a calculadora chamará o método `Operate` do objeto `IOperation` e retornará o resultado.

Para concluir a calculadora, também é necessário um método auxiliar que retorna a posição do primeiro caractere não dígito em uma cadeia de caracteres. Adicione o seguinte método auxiliar para a classe `MySimpleCalculator`:

```vb
Private Function FindFirstNonDigit(s As String) As Integer
    For i = 0 To s.Length - 1
        If Not Char.IsDigit(s(i)) Then Return i
    Next
    Return -1
End Function
```

```csharp
private int FindFirstNonDigit(string s)
{
    for (int i = 0; i < s.Length; i++)
    {
        if (!Char.IsDigit(s[i])) return i;
    }
    return -1;
}
```

Agora você poderá compilar e executar o projeto. No Visual Basic, certifique-se de adicionar a palavra-chave `Public` ao `Module1`. Na janela do console, digite uma operação de adição, como "5+3", e a calculadora retorna os resultados. Qualquer outro operador resulta na mensagem "Operação Não Encontrada!" .

## <a name="extending-simplecalculator-using-a-new-class"></a>Estendendo SimpleCalculator usando uma nova classe

Agora que a calculadora funciona, adicionar uma nova operação é muito fácil. Adicione a seguinte classe ao módulo ou ao namespace `SimpleCalculator`:

```vb
<Export(GetType(IOperation))>
<ExportMetadata("Symbol", "-"c)>
Public Class Subtract
    Implements IOperation

    Public Function Operate(left As Integer, right As Integer) As Integer Implements IOperation.Operate
        Return left - right
    End Function
End Class
```

```csharp
[Export(typeof(IOperation))]
[ExportMetadata("Symbol", '-')]
class Subtract : IOperation
{
    public int Operate(int left, int right)
    {
        return left - right;
    }
}
```

Compile e execute o projeto. Digite uma operação de subtração, como "5-3". A calculadora agora dá suporte à subtração, além da adição.

## <a name="extending-simplecalculator-using-a-new-assembly"></a>Estendendo SimpleCalculator usando um novo assembly

Adicionar classes ao código-fonte é bastante simples, mas o MEF oferece a capacidade de procurar peças fora de uma fonte do aplicativo. Para demonstrar isso, você precisará modificar a SimpleCalculator para pesquisar um diretório, bem como seu próprio assembly, por peças, adicionando um <xref:System.ComponentModel.Composition.Hosting.DirectoryCatalog>.

Adicione um novo diretório chamado `Extensions` ao projeto da SimpleCalculator. Certifique-se de adicioná-lo no nível do projeto e não no nível da solução. Em seguida, adicione um novo projeto de Biblioteca de Classes à solução, chamado `ExtendedOperations`. O novo projeto será compilado em um assembly separado.

Abra o designer de propriedades do projeto para o projeto ExtendedOperations e clique na guia **Compilar** ou **Compilar** . Altere o caminho de **saída da compilação** ou o **caminho de saída** para apontar para o diretório de extensões no diretório do projeto SimpleCalculator (*.. \SimpleCalculator\Extensions \\ *).

 No *Module1. vb* ou *Program.cs*, adicione a seguinte linha ao `Program` Construtor:

```vb
catalog.Catalogs.Add(New DirectoryCatalog("C:\SimpleCalculator\SimpleCalculator\Extensions"))
```

```csharp
catalog.Catalogs.Add(new DirectoryCatalog("C:\\SimpleCalculator\\SimpleCalculator\\Extensions"));
```

Substitua o caminho de exemplo pelo caminho para o diretório de Extensions. (Esse caminho absoluto destina-se apenas a fins de depuração. Em um aplicativo de produção, você usaria um caminho relativo.) <xref:System.ComponentModel.Composition.Hosting.DirectoryCatalog>Agora, o adicionará todas as partes encontradas em quaisquer assemblies no diretório de extensões ao contêiner de composição.

No projeto ExtendedOperations, adicione referências à SimpleCalculator e ao System.ComponentModel.Composition. No arquivo de classe ExtendedOperations, adicione um `Imports` ou uma instrução `using` para o System.ComponentModel.Composition. No Visual Basic, adicione também uma instrução `Imports` para a SimpleCalculator. Em seguida, adicione a seguinte classe ao arquivo da classe ExtendedOperations:

```vb
<Export(GetType(SimpleCalculator.IOperation))>
<ExportMetadata("Symbol", "%"c)>
Public Class Modulo
    Implements IOperation

    Public Function Operate(left As Integer, right As Integer) As Integer Implements IOperation.Operate
        Return left Mod right
    End Function
End Class
```

```csharp
[Export(typeof(SimpleCalculator.IOperation))]
[ExportMetadata("Symbol", '%')]
public class Mod : SimpleCalculator.IOperation
{
    public int Operate(int left, int right)
    {
        return left % right;
    }
}
```

Observe que para o contrato corresponder, o atributo <xref:System.ComponentModel.Composition.ExportAttribute> deve ter o mesmo tipo de <xref:System.ComponentModel.Composition.ImportAttribute>.

Compile e execute o projeto. Teste o novo operador Mod (%).

## <a name="conclusion"></a>Conclusão

Este tópico abordou os conceitos básicos do MEF.

- Peças, catálogos e o contêiner de composição

     Peças e o contêiner de composição são pilares essenciais de um aplicativo MEF. Uma peça é qualquer objeto que importa ou exporta um valor, até e incluindo a si mesmo. Um catálogo fornece um conjunto de peças de uma fonte específica. O contêiner de composição usa as peças fornecidas por um catálogo para executar a composição, a associação de importações a exportações.

- Importações e exportações

     Importações e exportações são o modo pelo qual os componentes se comunicam. Com uma importação, o componente especifica uma necessidade por um determinado valor ou objeto e com uma exportação ele especifica a disponibilidade de um valor. Cada importação é compatível com uma lista dos exportações por meio do seu contrato.

## <a name="next-steps"></a>Próximas etapas

Para baixar o código completo deste exemplo, consulte o [exemplo SimpleCalculator (Visual Basic)](https://docs.microsoft.com/samples/dotnet/samples/simple-calculator-vb/).

 Para obter mais informações e exemplos de código, confira [Managed Extensibility Framework](https://github.com/MicrosoftArchive/mef). Para obter uma lista dos tipos de MEF, confira o namespace <xref:System.ComponentModel.Composition?displayProperty=nameWithType>.
