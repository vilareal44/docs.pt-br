---
title: Opções interativas
description: Saiba mais sobre as opções de linha de comando com suporte pelo F# Interativo, fsi.exe.
ms.date: 08/15/2020
ms.openlocfilehash: da2251c1d2e57090ed926e501cebf3c53ac58052
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88558602"
---
# <a name="f-interactive-options"></a>Opções de F# Interativo

Este artigo descreve as opções de linha de comando com suporte pelo F# Interativo, `fsi.exe` . F# Interativo aceita muitas das mesmas opções de linha de comando que o compilador F #, mas também aceita algumas opções adicionais.

## <a name="use-f-interactive-for-scripting"></a>Usar F# Interativo para scripts

F# Interativo, `dotnet fsi` , pode ser iniciado interativamente ou pode ser iniciado na linha de comando para executar um script. A sintaxe de linha de comando é

```dotnetcli
dotnet fsi [options] [ script-file [arguments] ]
```

A extensão de arquivo para arquivos de script F # é `.fsx` .

## <a name="table-of-f-interactive-options"></a>Tabela de opções de F# Interativo

A tabela a seguir resume as opções com suporte pelo F# Interativo. Você pode definir essas opções na linha de comando ou por meio do IDE do Visual Studio. Para definir essas opções no IDE do Visual Studio, abra o menu **ferramentas** , selecione **Opções**, expanda o nó **ferramentas F #** e, em seguida, selecione **F# interativo**.

Quando as listas aparecem em F# Interativo argumentos de opção, os elementos de lista são separados por ponto e vírgula ( `;` ).

|Opção|Descrição|
|------|-----------|
|**--**|Usado para instruir F# Interativo a tratar os argumentos restantes como argumentos de linha de comando para o programa ou script F #, que você pode acessar no código usando a lista **FSI. CommandLineArgs**.|
|**--verificado**[ **+**&#124;**-** ]|O mesmo que a opção de compilador **fsc.exe** . Para obter mais informações, consulte [Opções do compilador](compiler-options.md).|
|**--página de código: &lt; int&gt;**|O mesmo que a opção de compilador **fsc.exe** . Para obter mais informações, consulte [Opções do compilador](compiler-options.md).|
|**--consolecolors**[ **+**&#124;**-** ]|Gera mensagens de aviso e de erro em cores.|
|**--crossoptimize**[ **+**&#124;**-** ]|Habilitar ou desabilitar otimizações de módulo cruzado.|
|**--depuração**[ **+**&#124;**-** ]<br /><br />**--debug:**[**full**&#124;**pdbonly**&#124;**portátil**&#124;**Embedded**]<br /><br />**-g**[ **+**&#124;**-** ]<br /><br />**-g:**[**full**&#124;**pdbonly**&#124;**portátil**&#124;**Embedded**]|O mesmo que a opção de compilador **fsc.exe** . Para obter mais informações, consulte [Opções do compilador](compiler-options.md).|
|**--definir: &lt; cadeia de caracteres&gt;**|O mesmo que a opção de compilador **fsc.exe** . Para obter mais informações, consulte [Opções do compilador](compiler-options.md).|
|**--determinístico**[ **+**&#124;**-** ]|Produz um assembly determinístico (incluindo o GUID de versão do módulo e o carimbo de data/hora).|
|**--exec**|Instrui o F # interativo a sair depois de carregar os arquivos ou executar o arquivo de script fornecido na linha de comando.|
|**--fullpaths**|O mesmo que a opção de compilador **fsc.exe** . Para obter mais informações, consulte [Opções do compilador](compiler-options.md).|
|**--GUI**[ **+**&#124;**-** ]|Habilita ou desabilita o loop de evento Windows Forms. O padrão é habilitado.|
|**--ajuda**<br /><br />**-?**|Usado para exibir a sintaxe de linha de comando e uma breve descrição de cada opção.|
|**--lib: &lt; pasta-lista&gt;**<br /><br />**-I: &lt; pasta-lista&gt;**|O mesmo que a opção de compilador **fsc.exe** . Para obter mais informações, consulte [Opções do compilador](compiler-options.md).|
|**--carregar: &lt; nome de arquivo&gt;**|Compila o código-fonte fornecido na inicialização e carrega as construções de F # compiladas na sessão.|
|**--mlcompatibility**|O mesmo que a opção de compilador **fsc.exe** . Para obter mais informações, consulte [Opções do compilador](compiler-options.md).|
|**--noframework**|O mesmo que a opção de compilador **fsc.exe** . Para obter mais informações, consulte [Opções do compilador](compiler-options.md)|
|**--nologo**|O mesmo que a opção de compilador **fsc.exe** . Para obter mais informações, consulte [Opções do compilador](compiler-options.md).|
|**--nowarn: &lt; lista de avisos&gt;**|O mesmo que a opção de compilador **fsc.exe** . Para obter mais informações, consulte [Opções do compilador](compiler-options.md).|
|**--otimizar**[ **+**&#124;**-** ]|O mesmo que a opção de compilador **fsc.exe** . Para obter mais informações, consulte [Opções do compilador](compiler-options.md).|
|**--preferreduilang: &lt; lang&gt;**| Especifica o nome de cultura do idioma de saída preferencial (por exemplo, es-ES, ja-JP). |
|**--Quiet**|Suprime a saída de F# Interativo para o fluxo **stdout** .|
|**--Cotações-depurar**|Especifica que as informações adicionais de depuração devem ser emitidas para expressões derivadas de literais de cotação F # e definições refletidas. As informações de depuração são adicionadas aos atributos personalizados de um nó de árvore de expressão F #. Consulte [Incitaçãos de código](code-quotations.md) e [expr. CustomAttributes](https://msdn.microsoft.com/library/eb89943f-5f5b-474e-b125-030ca412edb3).|
|**--ReadLine**[ **+**&#124;**-** ]|Habilitar ou desabilitar o preenchimento com Tab no modo interativo.|
|**--referência: &lt; nome de arquivo&gt;**<br /><br />**-r: &lt; nome de arquivo&gt;**|O mesmo que a opção de compilador **fsc.exe** . Para obter mais informações, consulte [Opções do compilador](compiler-options.md).|
|**--tailcalls**[ **+**&#124;**-** ]|Habilitar ou desabilitar o uso da instrução de IL final, que faz com que o quadro de pilha seja reutilizado para funções recursivas da parte final. Essa opção é habilitada por padrão.|
|**--targetprofile: &lt; cadeia de caracteres&gt;**|Especifica o perfil da estrutura de destino deste assembly. Os valores válidos são `mscorlib`, `netcore` ou `netstandard`. O padrão é `mscorlib`.|
|**--Use: &lt; nome de arquivo&gt;**|Instrui o intérprete a usar o arquivo fornecido na inicialização como entrada inicial.|
|**--utf8output**|O mesmo que a opção de compilador fsc.exe. Para obter mais informações, consulte [Opções do compilador](compiler-options.md).|
|**--WARN: &lt; nível de aviso&gt;**|O mesmo que a opção de compilador **fsc.exe** . Para obter mais informações, consulte [Opções do compilador](compiler-options.md).|
|**--warnaserror**[ **+**&#124;**-** ]|O mesmo que a opção de compilador **fsc.exe** . Para obter mais informações, consulte [Opções do compilador](compiler-options.md).|
|**--warnaserror**[ **+**&#124;**-** ]:** &lt; int-List &gt; **|O mesmo que a opção de compilador **fsc.exe** . Para obter mais informações, consulte [Opções do compilador](compiler-options.md).|

## <a name="f-interactive-structured-printing"></a>F# Interativo impressão estruturada

F# Interativo ( `dotnet fsi` ) usa uma versão estendida da [formatação de texto simples estruturado](plaintext-formatting.md) para relatar valores.

1. Todos os recursos de `%A` formatação de texto sem formatação têm suporte e alguns também são personalizáveis.

2. A impressão será colorida se houver suporte para cores no console de saída.

3. Um limite é colocado no comprimento das cadeias de caracteres mostradas, a menos que você avalie explicitamente essa cadeia de caracteres.

4. Um conjunto de configurações definíveis pelo usuário está disponível por meio do `fsi` objeto.

As configurações disponíveis para personalizar a impressão de texto sem formatação para os valores relatados são:

```fsharp
open System.Globalization

fsi.FormatProvider <- CultureInfo("de-DE")  // control the default culture for primitives

fsi.PrintWidth <- 120        // Control the width used for structured printing

fsi.PrintDepth <- 10         // Control the maximum depth of nested printing

fsi.PrintLength <- 10        // Control the length of lists and arrays

fsi.PrintSize <- 100         // Control the maximum overall object count

fsi.ShowProperties <- false  // Control whether properties of .NET objects are shown by default

fsi.ShowIEnumerable <- false // Control whether sequence values are expanded by default

fsi.ShowDeclarationValues <- false // Control whether values are shown for declaration outputs
```

### <a name="customize-with-addprinter-and-addprinttransformer"></a>Personalizar com `AddPrinter` e `AddPrintTransformer`

A impressão em F# Interativo saídas pode ser personalizada usando o `fsi.AddPrinter` e o `fsi.AddPrintTransformer` .
A primeira função fornece texto para substituir a impressão de um objeto. A segunda função retorna um objeto alternativo a ser exibido em vez disso. Por exemplo, considere o seguinte código F #:

```fsharp
open System

fsi.AddPrinter<DateTime>(fun dt -> dt.ToString("s"))

type DateAndLabel =
    { Date: DateTime
      Label: string  }

let newYearsDay1999 =
    { Date = DateTime(1999, 1, 1)
      Label = "New Year" }
```

Se você executar o exemplo no F# Interativo, ele produzirá com base na opção de formatação definida. Nesse caso, ele afeta a formatação de data e hora:

```console
type DateAndLabel =
  { Date: DateTime
    Label: string }
val newYearsDay1999 : DateAndLabel = { Date = 1999-01-01T00:00:00
                                       Label = "New Year" }
```

`fsi.AddPrintTransformer` pode ser usado para fornecer um objeto substituto para impressão:

```fsharp
type MyList(values: int list) =
    member _.Values = values

fsi.AddPrintTransformer(fun (x:MyList) -> box x.Values)

let x = MyList([1..10])
```

Isso gera:

```console
val x : MyList = [1; 2; 3; 4; 5; 6; 7; 8; 9; 10]
```

Se a função de transformador passou para `fsi.AddPrintTransformer` retornos `null` , o transformador de impressão é ignorado.
Isso pode ser usado para filtrar qualquer valor de entrada, começando com o tipo `obj` .  Por exemplo:

```fsharp
fsi.AddPrintTransformer(fun (x:obj) ->
    match x with
    | :? string as s when s = "beep" -> box ["quack"; "quack"; "quack"]
    | _ -> null)

let y = "beep"
```

Isso gera:

```console
val y : string = ["quack"; "quack"; "quack"]
```

## <a name="related-topics"></a>Tópicos relacionados

|Title|Descrição|
|-----|-----------|
|[Opções do compilador](compiler-options.md)|Descreve as opções de linha de comando disponíveis para o compilador F #, **fsc.exe**.|
