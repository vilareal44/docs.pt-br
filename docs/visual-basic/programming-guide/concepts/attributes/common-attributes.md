---
title: Atributos comuns
ms.date: 07/20/2015
ms.assetid: 11fe4894-1bf9-4525-a36b-cddcd3a5d22b
ms.openlocfilehash: 57ef8f103d64a51d896f46d2889d78ec99ff3223
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400713"
---
# <a name="common-attributes-visual-basic"></a>Atributos comuns (Visual Basic)

Este tópico descreve os atributos que são mais comumente usados em Visual Basic programas.

- [Atributos globais](#Global)

- [Atributo obsoleto](#Obsolete)

- [Atributo condicional](#Conditional)

- [Atributos de informações do chamador](#CallerInfo)

- [Atributos de Visual Basic](#VB)

## <a name="global-attributes"></a><a name="Global"></a>Atributos globais

A maioria dos atributos são aplicados aos elementos específicos de linguagem, como classes ou métodos. No entanto, alguns atributos são globais. Eles se aplicam a um assembly inteiro ou módulo. Por exemplo, o atributo <xref:System.Reflection.AssemblyVersionAttribute> pode ser usado para inserir informações de versão em um assembly, desta maneira:

```vb
<Assembly: AssemblyVersion("1.0.0.0")>
```

Os atributos globais aparecem no código-fonte após quaisquer instruções de nível superior `Imports` e antes de qualquer tipo, módulo ou declaração de namespace. Os atributos globais podem aparecer em vários arquivos de origem, mas os arquivos devem ser compilados em uma única passagem de compilação. Para projetos de Visual Basic, os atributos globais geralmente são colocados no arquivo AssemblyInfo. vb (o arquivo é criado automaticamente quando você cria um projeto no Visual Studio).

Os atributos de assembly são valores que fornecem informações sobre um assembly. Eles se enquadram nas seguintes categorias:

- Atributos de identidade do assembly

- Atributos informativos

- Atributos de manifesto do assembly

### <a name="assembly-identity-attributes"></a>Atributos de Identidade do Assembly

Três atributos (com um nome forte, se aplicável) determinam a identidade de um assembly: nome, versão e cultura. Esses atributos formam o nome completo do assembly e são necessários ao fazer referência a ele no código. Você pode definir a versão e a cultura de um assembly, usando atributos. No entanto, o valor do nome é definido pelo compilador, pelo IDE do Visual Studio na [caixa de diálogo de Informações do Assembly](/visualstudio/ide/reference/assembly-information-dialog-box) ou pelo Assembly Linker (Al.exe) quando o assembly é criado, com base no arquivo que contém o manifesto do assembly. O atributo <xref:System.Reflection.AssemblyFlagsAttribute> especifica se várias cópias do assembly podem coexistir.

A tabela a seguir mostra os atributos de identidade.

|Atributo|Finalidade|
|---------------|-------------|
|<xref:System.Reflection.AssemblyName>|Descreve completamente a identidade de um assembly.|
|<xref:System.Reflection.AssemblyVersionAttribute>|Especifica a versão de um assembly.|
|<xref:System.Reflection.AssemblyCultureAttribute>|Especifica a qual cultura o assembly dá suporte.|
|<xref:System.Reflection.AssemblyFlagsAttribute>|Especifica se um assembly dá suporte à execução lado a lado no mesmo computador, no mesmo processo ou no mesmo domínio do aplicativo.|

### <a name="informational-attributes"></a>Atributos Informativos

Você pode usar atributos informativos para fornecer informações adicionais corporativas ou de produto para um assembly. A tabela a seguir mostra os atributos informativos definidos no namespace <xref:System.Reflection?displayProperty=nameWithType>.

|Atributo|Finalidade|
|---------------|-------------|
|<xref:System.Reflection.AssemblyProductAttribute>|Define um atributo personalizado que especifica um nome de produto para um manifesto do assembly.|
|<xref:System.Reflection.AssemblyTrademarkAttribute>|Define um atributo personalizado que especifica uma marca registrada para um manifesto do assembly.|
|<xref:System.Reflection.AssemblyInformationalVersionAttribute>|Define um atributo personalizado que especifica uma versão informativa para um manifesto do assembly.|
|<xref:System.Reflection.AssemblyCompanyAttribute>|Define um atributo personalizado que especifica um nome de empresa para um manifesto do assembly.|
|<xref:System.Reflection.AssemblyCopyrightAttribute>|Define um atributo personalizado que especifica os direitos autorais para um manifesto do assembly.|
|<xref:System.Reflection.AssemblyFileVersionAttribute>|Instrui o compilador a usar um número de versão específico para o recurso de versão de arquivo do Win32.|
|<xref:System.CLSCompliantAttribute>|Indica se o assembly está em conformidade com a CLS (Common Language Specification).|

### <a name="assembly-manifest-attributes"></a>Atributos de Manifesto do Assembly

Você pode usar atributos de manifesto do assembly para fornecer informações no manifesto do assembly. Isso inclui título, descrição, alias padrão e configuração. A tabela a seguir mostra os atributos de manifesto do assembly definidos no namespace <xref:System.Reflection?displayProperty=nameWithType>.

|Atributo|Finalidade|
|---------------|-------------|
|<xref:System.Reflection.AssemblyTitleAttribute>|Define um atributo personalizado que especifica um título de assembly para um manifesto do assembly.|
|<xref:System.Reflection.AssemblyDescriptionAttribute>|Define um atributo personalizado que especifica uma descrição de assembly para um manifesto do assembly.|
|<xref:System.Reflection.AssemblyConfigurationAttribute>|Define um atributo personalizado que especifica uma configuração de assembly (como comercial ou de depuração) para um manifesto do assembly. assembly.|
|<xref:System.Reflection.AssemblyDefaultAliasAttribute>|Define um alias amigável padrão para um manifesto do assembly|

## <a name="obsolete-attribute"></a><a name="Obsolete"></a>Atributo obsoleto

O atributo `Obsolete` marca uma entidade programa como não recomendada para uso. Cada uso de uma entidade marcada como obsoleta gerará subsequentemente um aviso ou erro, dependendo de como o atributo é configurado. Por exemplo:

```vb
<System.Obsolete("use class B")>
Class A
    Sub Method()
    End Sub
End Class

Class B
    <System.Obsolete("use NewMethod", True)>
    Sub OldMethod()
    End Sub

    Sub NewMethod()
    End Sub
End Class
```

Neste exemplo o atributo `Obsolete` é aplicado à classe `A` e ao método `B.OldMethod`. Como o segundo argumento do construtor de atributo aplicado a `B.OldMethod` está definido como `true`, esse método causará um erro do compilador, enquanto que ao usar a classe `A`, produzirá apenas um aviso. Chamar `B.NewMethod`, no entanto, não produz aviso nem erro.

A cadeia de caracteres fornecida como o primeiro argumento para o construtor de atributo será exibida como parte do aviso ou erro. Por exemplo, ao usá-lo com as definições anteriores, o código a seguir gera um erro e dois avisos:

```vb
' Generates 2 warnings:
' Dim a As New A
' Generate no errors or warnings:

Dim b As New B
b.NewMethod()

' Generates an error, terminating compilation:
' b.OldMethod()
```

São gerados dois avisos para a classe `A`: um para a declaração da referência de classe e outro para o construtor de classe.

O atributo `Obsolete` pode ser usado sem argumentos, mas é recomendável incluir uma explicação de por que o item está obsoleto e o que deve ser usado no lugar dele.

O atributo `Obsolete` é um atributo de uso único e pode ser aplicado a qualquer entidade que permite atributos. `Obsolete` é um alias para <xref:System.ObsoleteAttribute>.

## <a name="conditional-attribute"></a><a name="Conditional"></a> Atributo condicional

O atributo `Conditional` torna a execução de um método dependente de um identificador de pré-processamento. O atributo `Conditional` é um alias para <xref:System.Diagnostics.ConditionalAttribute> e pode ser aplicado a um método ou uma classe de atributo.

Neste exemplo, `Conditional` é aplicado a um método para habilitar ou desabilitar a exibição de informações de diagnóstico específicas do programa:

```vb
#Const TRACE_ON = True
Imports System.Diagnostics

Module TestConditionalAttribute
    Public Class Trace
        <Conditional("TRACE_ON")>
        Public Shared Sub Msg(ByVal msg As String)
            Console.WriteLine(msg)
        End Sub

    End Class

    Sub Main()
        Trace.Msg("Now in Main...")
        Console.WriteLine("Done.")
    End Sub
End Module
```

Se o identificador `TRACE_ON` não estiver definido, nenhuma saída de rastreamento será exibida.

O atributo `Conditional` é frequentemente usado com o identificador `DEBUG` para habilitar recursos de rastreamento e de registro em log para builds de depuração, mas não em builds de versão, dessa maneira:

```vb
<Conditional("DEBUG")>
Shared Sub DebugMethod()

End Sub
```

Quando um método marcado como condicional é chamado, a presença ou ausência do símbolo de pré-processamento especificado determina se a chamada será incluída ou omitida. Se o símbolo estiver definido, a chamada será incluída, caso contrário, a chamada será omitida. O uso de `Conditional` é uma alternativa mais limpa, mais elegante e menos propensa a erros para incluir métodos dentro de blocos `#if…#endif`, dessa maneira:

```vb
#If DEBUG Then
    Sub ConditionalMethod()
    End Sub
#End If
```

Um método condicional deve ser um método em uma declaração de classe ou de struct e não deve ter um valor retornado.

### <a name="using-multiple-identifiers"></a>Usando vários identificadores

Se um método tem vários atributos `Conditional`, uma chamada para o método será incluída se pelo menos um dos símbolos condicionais for definido (em outras palavras, os símbolos são logicamente vinculados através do uso do operador OR). Neste exemplo, a presença de `A` ou `B` resultará em uma chamada de método:

```vb
<Conditional("A"), Conditional("B")>
Shared Sub DoIfAorB()

End Sub
```

Para alcançar o efeito da vinculação lógica de símbolos usando o operador AND, você pode definir métodos condicionais em série. Por exemplo, o segundo método abaixo será executado somente se `A` e `B` estiverem definidos:

```vb
<Conditional("A")>
Shared Sub DoIfA()
    DoIfAandB()
End Sub

<Conditional("B")>
Shared Sub DoIfAandB()
    ' Code to execute when both A and B are defined...
End Sub
```

### <a name="using-conditional-with-attribute-classes"></a>Usando condicional com classes de atributos

O atributo `Conditional` também pode ser aplicado a uma definição de classe de atributos. Neste exemplo, o atributo personalizado `Documentation` só adicionará informações aos metadados se DEBUG estiver definido.

```vb
<Conditional("DEBUG")>
Public Class Documentation
    Inherits System.Attribute
    Private text As String
    Sub New(ByVal doc_text As String)
        text = doc_text
    End Sub
End Class

Class SampleClass
    ' This attribute will only be included if DEBUG is defined.
    <Documentation("This method displays an integer.")>
    Shared Sub DoWork(ByVal i As Integer)
        System.Console.WriteLine(i)
    End Sub
End Class
```

## <a name="caller-info-attributes"></a><a name="CallerInfo"></a>Atributos de informações do chamador

Ao usar atributos de informações do chamador, você pode obter informações sobre o chamador de um método. Você pode obter o caminho do arquivo do código-fonte, o número de linha no código-fonte e o nome do membro do chamador.

Para obter informações do chamador do membro, você usa os atributos que são aplicados aos parâmetros opcionais. Cada parâmetro opcional especifica um valor padrão. A tabela a seguir lista os atributos de informações do chamador que são definidos no namespace de <xref:System.Runtime.CompilerServices?displayProperty=nameWithType>:

|Atributo|Descrição|Type|
|---|---|---|
|<xref:System.Runtime.CompilerServices.CallerFilePathAttribute>|O caminho completo do arquivo de origem que contém o chamador. Esse é o caminho em tempo de compilação.|`String`|
|<xref:System.Runtime.CompilerServices.CallerLineNumberAttribute>|Número de linha no arquivo de origem do qual o método é chamado.|`Integer`|
|<xref:System.Runtime.CompilerServices.CallerMemberNameAttribute>|Nome do método ou nome da propriedade do chamador. Para obter mais informações, consulte [informações do chamador (Visual Basic)](../caller-information.md).|`String`|

Para obter mais informações sobre os atributos de informações do chamador, consulte [informações do chamador (Visual Basic)](../caller-information.md).

## <a name="visual-basic-attributes"></a><a name="VB"></a>Atributos de Visual Basic

A tabela a seguir lista os atributos que são específicos para Visual Basic.

|Atributo|Finalidade|
|---------------|-------------|
|<xref:Microsoft.VisualBasic.ComClassAttribute>|Indica ao compilador que a classe deve ser exposta como um objeto COM.|
|<xref:Microsoft.VisualBasic.HideModuleNameAttribute>|Permite que os membros do módulo sejam acessados usando apenas a qualificação necessária para o módulo.|
|<xref:Microsoft.VisualBasic.VBFixedStringAttribute>|Especifica o tamanho de uma cadeia de caracteres de comprimento fixo em uma estrutura para uso com funções de entrada e saída de arquivo.|
|<xref:Microsoft.VisualBasic.VBFixedArrayAttribute>|Especifica o tamanho de uma matriz fixa em uma estrutura para uso com funções de entrada e saída de arquivo.|

### <a name="comclassattribute"></a>COMClassAttribute

Use `COMClassAttribute` o para simplificar o processo de criação de componentes com do Visual Basic. Os objetos COM são consideravelmente diferentes dos assemblies de .NET Framework e, sem `COMClassAttribute` , você precisa seguir várias etapas para gerar um objeto com de Visual Basic. Para classes marcadas com `COMClassAttribute` , o compilador executa muitas dessas etapas automaticamente.

### <a name="hidemodulenameattribute"></a>HideModuleNameAttribute

Use `HideModuleNameAttribute` para permitir que os membros do módulo sejam acessados usando apenas a qualificação necessária para o módulo.

### <a name="vbfixedstringattribute"></a>VBFixedStringAttribute

Use `VBFixedStringAttribute` para forçar Visual Basic criar uma cadeia de caracteres de comprimento fixo. As cadeias de caracteres são de comprimento variável por padrão, e esse atributo é útil ao armazenar cadeias de caracteres em arquivos. O código a seguir demonstra isso:

```vb
Structure Worker
    ' The runtime uses VBFixedString to determine
    ' if the field should be written out as a fixed size.
    <VBFixedString(10)> Public LastName As String
    <VBFixedString(7)> Public Title As String
    <VBFixedString(2)> Public Rank As String
End Structure
```

### <a name="vbfixedarrayattribute"></a>VBFixedArrayAttribute

Use `VBFixedArrayAttribute` para declarar matrizes que são fixas em tamanho. Assim como Visual Basic cadeias de caracteres, as matrizes são de comprimento variável por padrão. Esse atributo é útil ao serializar ou gravar dados em arquivos.

## <a name="see-also"></a>Confira também

- <xref:System.Reflection>
- <xref:System.Attribute>
- [Guia de programação do Visual Basic](../../index.md)
- [Atributos](../../../../standard/attributes/index.md)
- [Reflexão (Visual Basic)](../reflection.md)
- [Acessando atributos usando reflexão (Visual Basic)](accessing-attributes-by-using-reflection.md)
