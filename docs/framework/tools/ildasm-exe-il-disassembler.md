---
title: Ildasm.exe (IL Disassembler)
description: Use Ildasm.exe (desmontador de IL), que usa um arquivo PE (executável portátil) contendo o código de IL (linguagem intermediária) e torna um arquivo de texto para Ilasm.exe.
ms.date: 03/30/2017
helpviewer_keywords:
- PE files, MSIL Disassembler
- portable executable files, MSIL Disassembler
- Ildasm.exe
- MSIL Disassembler
- text files produced by MSIL Disassembler
- disassembling file for MSIL Assembler input
ms.assetid: db27f6b2-f1ec-499e-be3a-7eecf95ca42b
ms.openlocfilehash: 6f2611488e7f653783cab833ad47131978bf74aa
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87166834"
---
# <a name="ildasmexe-il-disassembler"></a>Ildasm.exe (IL Disassembler)

O desmontador de IL é uma ferramenta complementar para o IL assembler (*Ilasm.exe*). *Ildasm.exe* usa um arquivo executável portátil (PE) que contém o código de Il (linguagem intermediária) e cria um arquivo de texto adequado como entrada para *Ilasm.exe*.

Essa ferramenta é instalada automaticamente com o Visual Studio. Para executar a ferramenta, use o Prompt de Comando do Desenvolvedor para Visual Studio (ou o Prompt de Comando do Visual Studio no Windows 7). Para obter mais informações, consulte [Prompts de Comando](developer-command-prompt-for-vs.md).

No prompt de comando, digite o seguinte:

## <a name="syntax"></a>Sintaxe

```console
ildasm [options] [PEfilename] [options]
```

## <a name="parameters"></a>parâmetros

As opções a seguir estão disponíveis para arquivos *. exe*, *. dll*, *. obj*, *. lib*e *. winmd* .

| Opção | Descrição |
| ------ | ----------- |
|**/out =**`filename`|Cria um arquivo de saída com o `filename` especificado, em vez de exibir os resultados em uma interface gráfica do usuário.|
|**/rtf**|Produz saída em formato rich text. Inválido com a opção **/text**.|
|**/Text**|Exibe os resultados na janela de console, e não em uma interface gráfica do usuário ou como um arquivo de saída.|
|**/html**|Produz saída em formato HTML. Válido apenas com a opção **/output**.|
|**/?**|Exibe a sintaxe de comando e as opções para a ferramenta.|

As opções adicionais a seguir estão disponíveis para arquivos *. exe*, *. dll*e *. winmd* .

| Opção | Descrição |
| ------ | ----------- |
|**/bytes**|Mostra bytes reais, em formato hexadecimal, como comentários de instrução.|
|**/caverbal**|Produz blobs de atributo personalizado em forma verbal. O padrão é a forma binária.|
|**/linenum**|Inclua referências para linhas de origem.|
|**/nobar**|Suprime a janela pop-up do indicador de andamento da desmontagem.|
|**/noca**|Suprime a saída de atributos personalizados.|
|**/Project**|Exibe metadados da maneira como são exibidos para o código gerenciado, e não da maneira como são exibidos no Windows Runtime nativo. Se `PEfilename` não for um arquivo de metadados do Windows (*. winmd*), essa opção não terá nenhum efeito. Consulte [Suporte do .NET Framework para aplicativos da Windows Store e Windows Runtime](../../standard/cross-platform/support-for-windows-store-apps-and-windows-runtime.md).|
|**/pubonly**|Desmonta apenas tipos e membros públicos. Equivalente a **/visibility:PUB**.|
|**/quoteallnames**|Inclui todos os nomes entre aspas simples.|
|**/raweh**|Mostra cláusulas de tratamento de exceções na forma bruta.|
|**/Source**|Mostra linhas de origem como comentários.|
|**/tokens**|Mostra tokens de metadados de classes e membros.|
|**/visibility:** `vis`[+`vis`...]|Desmonta apenas tipos ou membros com a visibilidade especificada. A seguir estão os valores válidos para `vis`:<br /><br /> **PUB** — Público<br /><br /> **PRI** — Privado<br /><br /> **FAM** — Família<br /><br /> **ASM** — Assembly<br /><br /> **FAA** — Família e Assembly<br /><br /> **FOA** — Família ou Assembly<br /><br /> **PSC** — Escopo Privado<br /><br /> Para definições desses modificadores de visibilidade, consulte <xref:System.Reflection.MethodAttributes> e <xref:System.Reflection.TypeAttributes>.|

As opções a seguir são válidas para arquivos *. exe*, *. dll*e *. winmd* somente para saída de arquivo ou console.

| Opção | Descrição |
| ------ | ----------- |
|**/All**|Especifica uma combinação das opções **/header**, **/bytes**, **/stats**, **/classlist** e **/tokens**.|
|**/classlist**|Inclui uma lista de classes definidas no módulo.|
|**/forward**|Usa declaração da classe de encaminhamento.|
|**/headers**|Inclui informações de cabeçalho do arquivo na saída.|
|**/Item:** `class` [**::** `member` [`(sig`]]|Desmonta o seguinte, dependendo do argumento fornecido:<br /><br /> –   Desmonta o `class` especificado.<br />–   Desmonta o `member` especificado da `class`.<br />–   Desmonta o `member` da `class` com a assinatura especificada `sig`. O formato de `sig` é:<br />     [`instance`] `returnType`(`parameterType1`, `parameterType2`, …, `parameterTypeN`)<br />     **Observação** No .NET Framework versões 1,0 e 1,1, `sig` deve ser seguido de um parêntese de fechamento: `(sig)` . Desde o .NET Framework 2.0, o parêntese de fechamento deve ser omitido: `(sig`.|
|**/noil**|Suprime saída de código do assembly IL.|
|**/stats.**|Inclui estatísticas na imagem.|
|**/typelist**|Produz a lista completa de tipos, para preservar a ordenação de tipos em uma viagem de ida e volta.|
|**/unicode**|Usa codificação Unicode para a saída.|
|**/utf8**|Usa codificação UTF-8 para a saída. ANSI é o padrão.|

As opções a seguir são válidas para arquivos *. exe*, *. dll*, *. obj*, *. lib*e *. winmd* somente para saída de arquivo ou console.

| Opção | Descrição |
| ------ | ----------- |
|**/Metadata**[= `specifier` ]|Mostra metadados, em que `specifier` é:<br /><br /> **MDHEADER** — Mostra as informações de cabeçalho dos metadados e os tamanhos.<br /><br /> **HEX** — Mostra informações em hexadecimal, bem como em palavras.<br /><br /> **CSV** — Mostra as contagens de registros e os tamanhos de heap.<br /><br /> **UNREX** — Mostra externos não resolvidos.<br /><br /> **SCHEMA** — Mostra as informações de cabeçalho dos metadados e de esquema.<br /><br /> **RAW** — Mostra as tabelas de metadados brutos.<br /><br /> **HEAPS** — Mostra os heaps brutos.<br /><br /> **VALIDATE** — Valida a consistência dos metadados.<br /><br /> É possível especificar **/metadata** várias vezes, com valores diferentes para `specifier`.|

As opções a seguir são válidas para arquivos *. lib* somente para saída de arquivo ou console.

| Opção | Descrição |
| ------ | ----------- |
|**/objectfile**=`filename`|Mostre os metadados de um único arquivo de objeto na biblioteca especificada.|

> [!NOTE]
> Todas as opções para *Ildasm.exe* não diferenciam maiúsculas de minúsculas e são reconhecidas pelas três primeiras letras. Por exemplo, **/quo** é equivalente a **/quoteallnames**. As opções que especificam argumentos aceitam dois-pontos (:) ou um sinal de igualdade (=) como o separador entre a opção e o argumento. Por exemplo, **/output:** *filename* é equivalente a **/output=** *filename*.

## <a name="remarks"></a>Comentários

*Ildasm.exe* só funciona em arquivos PE no disco. Ele não funciona em arquivos instalados no cache de assembly global.

O arquivo de texto produzido por *Ildasm.exe* pode ser usado como entrada para o Assembler IL (*Ilasm.exe*). Isso é útil, por exemplo, durante a compilação do código em uma linguagem de programação que não dá suporte a todos os atributos de metadados do runtime. Depois de compilar o código e executar sua saída por meio de *Ildasm.exe*, o arquivo de texto Il resultante pode ser editado manualmente para adicionar os atributos ausentes. Em seguida, é possível executar esse arquivo de texto por meio do IL Assembler para produzir um arquivo executável final.

> [!NOTE]
> Atualmente, não é possível usar essa técnica com arquivos PE que contenham código nativo inserido (por exemplo, arquivos PE produzidos por Visual C++).  

É possível usar a GUI padrão no IL Disassembler para exibir os metadados e o código desmontado de qualquer arquivo PE existente em uma exibição de árvore hierárquica. Para usar a GUI, digite **ildasm** na linha de comando sem fornecer o argumento *PEfilename* ou nenhuma opção. No menu **arquivo** , você pode navegar até o arquivo PE que você deseja carregar em *Ildasm.exe*. Para salvar os metadados e o código desmontado exibido para o PE selecionado, selecione o comando **Despejo** no menu **Arquivo**. Para salvar apenas o modo de exibição de árvore hierárquica, selecione o comando **Despejar Modo de Exibição de Árvore** no menu **Arquivo**. Para obter um guia detalhado para carregar um arquivo em *Ildasm.exe* e interpretar a saída, confira o Tutorial de *Ildasm.exe*, localizado na pasta Exemplos que acompanha o SDK do Windows.

Se você fornecer *Ildasm.exe* com um argumento *PEfilename* que contém recursos incorporados, a ferramenta produzirá vários arquivos de saída: um arquivo de texto que contém o código de Il e, para cada recurso gerenciado inserido, um arquivo. resources produzido usando o nome do recurso dos metadados. Se um recurso não gerenciado for inserido em *PEfilename*, um arquivo. res será produzido usando o nome de arquivo especificado para a saída de Il pela opção **/output** .

> [!NOTE]
> *Ildasm.exe* mostra apenas descrições de metadados para arquivos de entrada *. obj* e *. lib* . O código IL desses tipos de arquivo não é desmontado.

Você pode executar *Ildasm.exe* sobre an.exe ou arquivo *. dll* para determinar se o arquivo é gerenciado. Se o arquivo não for gerenciado, a ferramenta exibirá uma mensagem informando que o arquivo não tem cabeçalho de Common Language Runtime válido e não pode ser desmontado. Se o arquivo for gerenciado, a ferramenta será executada com êxito.

## <a name="version-information"></a>Informações sobre versão

Do .NET Framework 4.5 em diante, o *Ildasm.exe* identifica um BLOB (objeto binário grande) de marshal não reconhecido exibindo o conteúdo binário bruto. Por exemplo, o código a seguir mostra como um BLOB de marshaling gerado por um programa C# é exibido:

```csharp
public void Test([MarshalAs((short)70)] int test) { }
```

```il
// IL from Ildasm.exe output
.method public hidebysig instance void Test(int32  marshal({ 46 }) test) cil managed
```

Do .NET Framework 4.5 em diante, o *Ildasm.exe* exibe atributos aplicados a implementações de interface, conforme é mostrado no seguinte trecho da saída de *Ildasm.exe*:

```il
.class public auto ansi beforefieldinit MyClass
  extends [mscorlib]System.Object
  implements IMyInterface
  {
    .interfaceimpl type IMyInterface
    .custom instance void
      [mscorlib]System.Diagnostics.DebuggerNonUserCodeAttribute::.ctor() = ( 01 00 00 00 )
      …
```

## <a name="examples"></a>Exemplos

O comando a seguir faz com que os metadados e o código desmontado do arquivo PE `MyHello.exe` sejam exibidos no *Ildasm.exe* GUI padrão.

```console
ildasm myHello.exe
```

O comando a seguir desmonta o arquivo `MyFile.exe` e armazena o texto do IL Assembler resultante no arquivo *MyFile.il*.

```console
ildasm MyFile.exe /output:MyFile.il
```

O comando a seguir desmonta o arquivo `MyFile.exe` e exibe o texto do IL Assembler resultante na janela do console.

```console
ildasm MyFile.exe /text
```

Se o arquivo `MyApp.exe` contiver recursos gerenciados não gerenciados inseridos, o comando a seguir produzirá quatro arquivos: *MyApp.il*, *MyApp.res*, *Icons.resources* e *Message.resources*:

```console
ildasm MyApp.exe /output:MyApp.il
```

O comando a seguir desmonta o método `MyMethod` dentro da classe `MyClass` em `MyFile.exe` e exibe a saída na janela do console.

```console
ildasm /item:MyClass::MyMethod MyFile.exe /text
```

No exemplo anterior, talvez haja vários métodos chamados `MyMethod` com assinaturas diferentes. O comando a seguir desmonta o método de instância `MyMethod` com o tipo de retorno de **void** e os tipos de parâmetro **int32** e **string**.

```console
ildasm /item:"MyClass::MyMethod(instance void(int32,string)" MyFile.exe /text
```

> [!NOTE]
> No .NET Framework versões 1.0 e 1.1, o parêntese à esquerda que segue o nome do método deve ser acompanhado de um parêntese à direita após a assinatura: `MyMethod(instance void(int32))`. Desde o .NET Framework 2.0, o parêntese de fechamento deve ser omitido: `MyMethod(instance void(int32)`.

Para recuperar um método `static` (método `Shared` no Visual Basic), omita a palavra-chave `instance`. Tipos de classe que não são tipos primitivos como `int32` e `string` devem incluir o namespace e ser precedidos pela palavra-chave `class`. Os tipos externos devem ser precedidos pelo nome da biblioteca entre colchetes. O comando a seguir desmonta um método estático chamado `MyMethod` que tem um parâmetro de tipo <xref:System.AppDomain> e tem um tipo de retorno de <xref:System.AppDomain>.

```console
ildasm /item:"MyClass::MyMethod(class [mscorlib]System.AppDomain(class [mscorlib]System.AppDomain)" MyFile.exe /text
```

Um tipo aninhado deve ser precedido por sua classe de contenção, delimitada por uma barra. Por exemplo, se a classe `MyNamespace.MyClass` contiver uma classe aninhada chamada `NestedClass`, a classe aninhada será identificada da seguinte forma: `class MyNamespace.MyClass/NestedClass`.

## <a name="see-also"></a>Confira também

- [Ferramentas](index.md)
- [Ilasm.exe (assembler IL)](ilasm-exe-il-assembler.md)
- [Processo de execução gerenciada](../../standard/managed-execution-process.md)
- [Prompts de comando](developer-command-prompt-for-vs.md)
