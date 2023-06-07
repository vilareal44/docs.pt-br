---
title: Ferramenta de Definição de Esquema XML (Xsd.exe)
description: O gerador de serializador XML cria um assembly de serialização XML para tipos em um assembly especificado, o que melhora o desempenho de inicialização do XmlSerializer.
ms.date: 03/30/2017
ms.assetid: a6e6e65c-347f-4494-9457-653bf29baac2
ms.openlocfilehash: 0275ecfebd427feb104013024654d4a0bc98748a
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "84288973"
---
# <a name="xml-schema-definition-tool-xsdexe"></a>Ferramenta de Definição de Esquema XML (Xsd.exe)

A Ferramenta de Definição de Esquema XML (Xsd.exe) gera um esquema XML ou classes de common language runtime de arquivos XDR, XML e XSD files, ou de classes em um assembly de tempo de execução.

A ferramenta de definição de esquema XML (XSD. exe) geralmente pode ser encontrada no seguinte caminho: \
_C: \\ arquivos de programas (x86) \\ Microsoft SDKs \\ Windows \\ {version} \\ Bint \\ {version} ferramentas\\_

## <a name="syntax"></a>Sintaxe

Execute a ferramenta na linha de comando.

```console
xsd file.xdr [-outputdir:directory][/parameters:file.xml]
xsd file.xml [-outputdir:directory] [/parameters:file.xml]
xsd file.xsd {/classes | /dataset} [/element:element]
             [/enableLinqDataSet] [/language:language]
                          [/namespace:namespace] [-outputdir:directory] [URI:uri]
                          [/parameters:file.xml]
xsd {file.dll | file.exe} [-outputdir:directory] [/type:typename [...]][/parameters:file.xml]
```
  
> [!TIP]
> Para que as ferramentas de .NET Framework funcionem corretamente, você deve definir as `Path` `Include` variáveis de ambiente, e `Lib` corretamente. Defina essas variáveis de ambiente executando SDKVars. bat, que está localizado no \<SDK> diretório \v2.0\Bin. SDKVars.bat deve ser executado em todo shell de comando.

## <a name="argument"></a>Argumento

|Argumento|Descrição|
|--------------|-----------------|
|*arquivo. extensão*|Especifica o arquivo de entrada para ser convertido. Você deve especificar a extensão como um dos seguintes:. XDR,. xml,. xsd,. dll ou. exe.<br /><br /> Se você especificar um arquivo de esquema XDR (extensão .xdr), o Xsd.exe converterá o esquema XDR em um esquema XSD. O arquivo de saída tem o mesmo nome que o esquema XDR, mas com a extensão .xsd.<br /><br /> Se você especificar um arquivo XML (extensão .xml), o Xsd.exe deduzirá um esquema dos dados no arquivo e produzirá um esquema XSD. O arquivo de saída tem o mesmo nome que o arquivo XML, mas com a extensão .xsd.<br /><br /> Se você especificar um arquivo de esquema XML (extensão .xsd), o Xsd.exe gerará o código de origem para objetos em runtime que correspondem ao esquema XML.<br /><br /> Se você especificar um arquivo de assembly de runtime (extensão .exe ou .dll), o Xsd.exe gerará esquemas para um ou mais tipos nesse assembly. Você pode usar a opção `/type` para especificar os tipos para os quais gerar esquemas. Os esquemas de saída são nomeados schema0.xsd, schema1.xsd e assim por diante. O Xsd.exe produzirá vários esquemas somente se os determinados tipos especificarem um namespace usando o atributo personalizado `XMLRoot`.|

## <a name="general-options"></a>Opções gerais

|Opção|Descrição|
|------------|-----------------|
|**/h \[\]**|Exibe sintaxe de comando e opções para a ferramenta.|
|**/o \[ utputdir \] :**_diretório_|Especifica o diretório para arquivos de saída. Esse argumento pode aparecer somente uma vez. O padrão é o diretório atual.|
|**/?**|Exibe sintaxe de comando e opções para a ferramenta.|
|**/p \[ arameters \] :**_File. xml_|As opções de leitura para vários modos de operação do arquivo .xml especificado. A forma curta é `/p:`. Para obter mais informações, consulte a seção [comentários](#remarks) .|

## <a name="xsd-file-options"></a>Opções de arquivo XSD
 Você deve especificar somente uma das seguintes opções para arquivos .xsd.

|Opção|Descrição|
|------------|-----------------|
|**/c \[ Lasses\]**|Gera classes que correspondem ao esquema especificado. Para ler dados XML em um objeto, use o método <xref:System.Xml.Serialization.XmlSerializer.Deserialize%2A?displayProperty=nameWithType>.|
|**/d \[ ados\]**|Gera uma classe derivada de <xref:System.Data.DataSet> que corresponde ao esquema especificado. Para ler dados XML na classe derivada, use o método <xref:System.Data.DataSet.ReadXml%2A?displayProperty=nameWithType>.|

 Você também pode especificar qualquer uma das seguintes opções para arquivos .xsd.

|Opção|Descrição|
|------------|-----------------|
|**/e \[ lementar \] :**_Element_|Especifica o elemento no esquema para o qual gerar código. Por padrão, todos os elementos são tipados. Você pode especificar esse argumento mais de uma vez.|
|**/enableDataBinding**|Implementa a interface <xref:System.ComponentModel.INotifyPropertyChanged> em todos os tipos gerados para habilitar a associação de dados. A forma curta é `/edb`.|
|**/enableLinqDataSet**|(Forma abreviada: `/eld` .) Especifica que o DataSet gerado pode ser consultado em relação ao uso de LINQ to DataSet. Essa opção é usada quando a opção /dataset também está especificada. Para obter mais informações, consulte [Visão geral do LINQ to DataSet](../../framework/data/adonet/linq-to-dataset-overview.md) e [Consultando DataSets tipados](../../framework/data/adonet/querying-typed-datasets.md). Para obter informações gerais sobre como usar o LINQ, consulte [LINQ (consulta integrada à linguagem)-C#](../../csharp/programming-guide/concepts/linq/index.md) ou [LINQ (consulta integrada à linguagem)-Visual Basic](../../visual-basic/programming-guide/concepts/linq/index.md).|
|**/f \[ ields\]**|Gera campos em vez de propriedades. Por padrão, as propriedades são geradas.|
|**/l \[ uagem \] :**_idioma_|Especifica a linguagem de programação a ser usada. Escolha `CS` (C#, que é o padrão), `VB` (Visual Basic), `JS` (JScript), ou `VJS` (Visual J#). Você também pode especificar um nome totalmente qualificado para uma classe implementando <xref:System.CodeDom.Compiler.CodeDomProvider?displayProperty=nameWithType>|
|**/n \[ amespace \] :**_namespace_|Especifica o namespace de runtime para os tipos gerados. O namespace padrão é `Schemas`.|
|**/nologo**|Suprime o banner.|
|**/Order**|Gera identificadores de pedido explícito em todos os membros de partícula.|
|**/o \[ UT \] :**_DirectoryName_|Especifica o diretório de saída no qual colocar os arquivos. O padrão é o diretório atual.|
|**/u \[ ri \] :**_URI_|Especifica o URI para os elementos no esquema para o qual gerar código. Esse URI, se houver, aplica-se a todos os elementos especificados com a opção `/element`.|

## <a name="dll-and-exe-file-options"></a>Opções de arquivo DLL e EXE

|Opção|Descrição|
|------------|-----------------|
|**/t \[ tipo \] :**_TypeName_|Especifica o nome do tipo para o qual criar um esquema. Você pode especificar vários argumentos de tipo. Se *typename* não especificar um namespace, o Xsd.exe corresponderá todos os tipos no assembly com o tipo especificado. Se *typename* especificar um namespace, somente esse tipo terá uma correspondência. Se *typename* terminar com um caractere de asterisco (\*), a ferramenta corresponderá todos os tipos que começam com a cadeia de caracteres antes do \*. Se você omitir a opção `/type`, o Xsd.exe gera esquemas para todos os tipos no assembly.|

## <a name="remarks"></a>Comentários

A tabela a seguir mostra as operações que o Xsd.exe realiza.

| | |
|------------|-----------------|
|XDR to XSD|Gera um esquema XML de um arquivo de esquema de dados XML reduzidos. XDR é um formato anterior do esquema baseado em XML.|
|XML to XSD|Gera um esquema XML de um arquivo XML.|
|XSD to DataSet|Gera classes <xref:System.Data.DataSet> de common language runtime de um arquivo de esquema XSD. As classes geradas fornecem um modelo de objeto ideal para os dados XML regulares.|
|XSD para classes|Gera classes de runtime de um arquivo de esquema XSD. As classes geradas podem ser usadas em conjunto com <xref:System.Xml.Serialization.XmlSerializer?displayProperty=nameWithType> para ler e escrever código XML que segue o esquema.|
|Classes para XSD| Gera um esquema XML de um tipo ou tipos em um arquivo de assembly de runtime. O esquema gerado define o formato XML usado pelo <xref:System.Xml.Serialization.XmlSerializer> .|

 O Xsd.exe somente permite que você manipule esquemas de XML que seguem a linguagem XSD (linguagem de definição de esquema XML) proposta pelo World Wide Web Consortium (W3C). Para obter mais informações sobre a proposta de definição de esquema XML ou o padrão XML, consulte <https://w3.org> .

## <a name="setting-options-with-an-xml-file"></a>Configurando opções com um arquivo XML

Ao usar a opção `/parameters`, você pode especificar um único arquivo XML que define várias opções. As opções que você define dependem de como você está usando a ferramenta XSD.exe. As escolhas incluem gerar esquemas, arquivos de código ou arquivos de código que incluem recursos de `DataSet`. Por exemplo, você pode definir o elemento `<assembly>` para o nome de um executável (.exe) ou arquivo de biblioteca de tipos (.dll) ao gerar um esquema, mas não ao gerar um arquivo de código. O XML a seguir mostra como usar o elemento `<generateSchemas>` com um executável especificado:

```xml
<!-- This is in a file named GenerateSchemas.xml. -->
<xsd xmlns='http://microsoft.com/dotnet/tools/xsd/'>
<generateSchemas>
   <assembly>ConsoleApplication1.exe</assembly>
</generateSchemas>
</xsd>
```

Se o XML anterior estiver contido em um arquivo chamado GenerateSchemas. xml, use a `/parameters` opção digitando o seguinte em um prompt de comando e pressionando **Enter**:

```console
 xsd /p:GenerateSchemas.xml
```

Por outro lado, se você estiver gerando um esquema para um único tipo localizado no assembly, poderá usar o seguinte XML:

```xml
<!-- This is in a file named GenerateSchemaFromType.xml. -->
<xsd xmlns='http://microsoft.com/dotnet/tools/xsd/'>
<generateSchemas>
   <type>IDItems</type>
</generateSchemas>
</xsd>
```

Mas para usar o código precedente, você também deverá fornecer o nome do assembly no prompt de comando. Insira o seguinte em um prompt de comando (supondo que o arquivo XML é denominado GenerateSchemaFromType. xml):

```console
xsd /p:GenerateSchemaFromType.xml ConsoleApplication1.exe
```

Você deve especificar somente uma das seguintes opções para o elemento `<generateSchemas>`.

|Elemento|Descrição|
|-------------|-----------------|
|\<assembly>|Especifica um assembly do qual gerar o esquema.|
|\<type>|Especifica um tipo encontrado em um assembly para o qual gerar um esquema.|
|\<xml>|Especifica um arquivo XML para o qual gerar um esquema.|
|\<xdr>|Especifica um arquivo XDR para o qual gerar um esquema.|

Para gerar um arquivo de código, use o elemento `<generateClasses>`. O exemplo a seguir gera um arquivo de código. Observe que dois atributos também são mostrados e permitem definir a linguagem de programação e o namespace do arquivo gerado.

```xml
<xsd xmlns='http://microsoft.com/dotnet/tools/xsd/'>
<generateClasses language='VB' namespace='Microsoft.Serialization.Examples'/>
</xsd>
<!-- You must supply an .xsd file when typing in the command line.-->
<!-- For example: xsd /p:genClasses mySchema.xsd -->
```

 As opções que você pode definir para o elemento `<generateClasses>` incluem o seguinte.

|Elemento|Descrição|
|-------------|-----------------|
|\<element>|Especifica um elemento no arquivo .xsd para o qual gerar código.|
|\<schemaImporterExtensions>|Especifica um tipo derivado de uma classe <xref:System.Xml.Serialization.Advanced.SchemaImporterExtension>.|
|\<schema>|Especifica um arquivo de esquema XML para o qual gerar um código. Vários arquivos de esquema XML podem ser especificados usando vários \<schema> elementos.|

A tabela a seguir mostra os atributos que também podem ser usados com o elemento `<generateClasses>`.

|Atributo|Descrição|
|---------------|-----------------|
|Linguagem|Especifica a linguagem de programação a ser usada. Escolha `CS` (C#, o padrão), `VB` (Visual Basic), `JS` (JScript), ou `VJS` (Visual J#). Você também pode especificar um nome totalmente qualificado para uma classe que implementa <xref:System.CodeDom.Compiler.CodeDomProvider>|
|namespace|Especifica o namespace para o código gerado. O namespace deve estar em conformidade com os padrões CLR (por exemplo, sem caracteres de espaço ou barra invertida).|
|opções|Um dos seguintes valores: `none`, `properties` (gera propriedades em vez de campos públicos), `order` ou `enableDataBinding` (consulte as opções `/order` e `/enableDataBinding` na seção anterior Opções de arquivo XSD).|

 Você também pode controlar como o código do `DataSet` é gerado usando o elemento `<generateDataSet>`. O XML a seguir especifica que o código gerado usa `DataSet` estruturas (como a <xref:System.Data.DataTable> classe) para criar Visual Basic código para um elemento especificado. As estruturas de DataSet geradas darão suporte a consultas LINQ.

 ```xml
 <xsd xmlns='http://microsoft.com/dotnet/tools/xsd/'>
     <generateDataSet language='VB' namespace='Microsoft.Serialization.Examples' enableLinqDataSet='true'>
     </generateDataSet>
 </xsd>
```

As opções que você pode definir para o elemento `<generateDataSet>` incluem o seguinte.

|Elemento|Descrição|
|-------------|-----------------|
|\<schema>|Especifica um arquivo de esquema XML para o qual gerar um código. Vários arquivos de esquema XML podem ser especificados usando vários \<schema> elementos.|

 A tabela a seguir mostra os atributos que podem ser usados com o elemento `<generateDataSet>`.

|Atributo|Descrição|
|---------------|-----------------|
|enableLinqDataSet|Especifica que o DataSet gerado pode ser consultado usando LINQ to DataSet. O valor padrão é false.|
|Linguagem|Especifica a linguagem de programação a ser usada. Escolha `CS` (C#, o padrão), `VB` (Visual Basic), `JS` (JScript), ou `VJS` (Visual J#). Você também pode especificar um nome totalmente qualificado para uma classe que implementa <xref:System.CodeDom.Compiler.CodeDomProvider>|
|namespace|Especifica o namespace para o código gerado. O namespace deve estar em conformidade com os padrões CLR (por exemplo, sem caracteres de espaço ou barra invertida).|

 Há atributos que você pode definir no elemento `<xsd>` de nível superior. Essas opções podem ser usadas com qualquer um dos elementos filho (`<generateSchemas>`, `<generateClasses>` ou `<generateDataSet>`). O código XML a seguir gera código para um elemento denominado "IDItems" no diretório de saída denominado "MyOutputDirectory".

```xml
<xsd xmlns='http://microsoft.com/dotnet/tools/xsd/' output='MyOutputDirectory'>
<generateClasses>
    <element>IDItems</element>
</generateClasses>
</xsd>
```

A tabela a seguir mostra os atributos que também podem ser usados com o elemento `<xsd>`.

|Atributo|Descrição|
|---------------|-----------------|
|output|O nome de um diretório onde o esquema ou arquivo de código gerado será colocado.|
|nologo|Suprime o banner. Definir como `true` ou `false`.|
|ajuda|Exibe sintaxe de comando e opções para a ferramenta. Definir como `true` ou `false`.|

## <a name="examples"></a>Exemplos
 O comando a seguir gera um esquema XML de `myFile.xdr` e salva-o no diretório atual.

```console
xsd myFile.xdr
```

O comando a seguir gera um esquema XML de `myFile.xml` e salva-o no diretório especificado.

```console
xsd myFile.xml /outputdir:myOutputDir
```

O comando a seguir gera um conjunto de dados que corresponde ao esquema especificado na linguagem C# e salva-o como `XSDSchemaFile.cs` no diretório atual.

```console
xsd /dataset /language:CS XSDSchemaFile.xsd
```

O comando a seguir gera esquemas XML para todos os tipos no assembly `myAssembly.dll` e salva-os como `schema0.xsd` no diretório atual.

```console
xsd myAssembly.dll
```

## <a name="see-also"></a>Confira também

- <xref:System.Data.DataSet>
- <xref:System.Xml.Serialization.XmlSerializer?displayProperty=nameWithType>
- [Ferramentas](../../framework/tools/index.md)
- [Prompts de comando](../../framework/tools/developer-command-prompt-for-vs.md)
- [LINQ para visão geral do DataSet](../../framework/data/adonet/linq-to-dataset-overview.md)
- [Consultando DataSets tipados](../../framework/data/adonet/querying-typed-datasets.md)
- [LINQ (consulta integrada à linguagem) (C#)](../../csharp/programming-guide/concepts/linq/index.md)
- [LINQ (consulta integrada à linguagem) (Visual Basic)](../../visual-basic/programming-guide/concepts/linq/index.md)
