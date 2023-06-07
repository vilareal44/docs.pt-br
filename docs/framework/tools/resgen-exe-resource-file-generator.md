---
title: Resgen.exe (Gerador de Arquivo de Recurso)
description: Use Resgen.exe, o gerador de arquivo de recurso. Converter arquivos de texto (. txt,. restext) e formato de recurso XML (. resx) em binários de tempo de execução CLR incorporáveis (. Resources).
ms.date: 03/30/2017
helpviewer_keywords:
- resource files, .resources files
- resource files, .resx files
- resx files (resource files)
- .resources files
- files, converting
- Resource File Generator
- .resx files
- Resgen.exe
- resource files, converting
- converting files, Resource File Generator
- binary resources files
- embedding files in runtime binary executable
ms.assetid: 8ef159de-b660-4bec-9213-c3fbc4d1c6f4
ms.openlocfilehash: f51ee6c8537abafc82017f3cc29d734e939a254f
ms.sourcegitcommit: b4f8849c47c1a7145eb26ce68bc9f9976e0dbec3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87517224"
---
# <a name="resgenexe-resource-file-generator"></a>Resgen.exe (Gerador de Arquivo de Recurso)
O Gerador de Arquivos de Recurso (Resgen.exe) converte arquivos de texto (.txt ou .restext) e arquivos de recurso com base em XML (.resx) em arquivos binários do Common Language Runtime (.resources) que podem ser inseridos em um executável binário do tempo de execução ou em um assembly satélite. (Consulte [Criando arquivos de recurso](../resources/creating-resource-files-for-desktop-apps.md).)  
  
 Resgen.exe é um utilitário de conversão de recursos de finalidade geral que realiza as seguintes tarefas:  
  
- Converte arquivos .txt ou .restext em arquivos .resources ou .resx. (O formato de arquivos .restext é idêntico ao formato de arquivos .txt. No entanto, a extensão .restext ajuda a identificar arquivos de texto que contêm definições de recurso mais facilmente.)  
  
- Converte arquivos .resources em arquivos de texto ou .resx.  
  
- Converte arquivos .resx em arquivos de texto ou .resources.  
  
- Extrai os recursos de cadeia de caracteres de um assembly para um arquivo. resw que é adequado para uso em um aplicativo da loja do Windows 8. x.  
  
- Cria uma classe fortemente tipada que fornece acesso a recursos nomeados individuais e à instância de <xref:System.Resources.ResourceManager>.  
  
 Se Resgen.exe falhar por algum motivo, o valor de retorno será –1.  
  
 Para obter ajuda com Resgen.exe, você pode usar o comando a seguir, sem opções especificadas, para exibir a sintaxe de comando e as opções para Resgen.exe:  
  
```console  
resgen  
```  
  
 Também é possível usar a opção `/?`:  
  
```console  
resgen /?  
```  
  
 Se você usar Resgen.exe para gerar arquivos. Resources binários, poderá usar um compilador de linguagem para inserir os arquivos binários em assemblies executáveis, ou você pode usar o [vinculador de assembly (Al.exe)](al-exe-assembly-linker.md) para compilá-los em assemblies satélite.  
  
 Essa ferramenta é instalada automaticamente com o Visual Studio. Para executar a ferramenta, use o Prompt de Comando do Desenvolvedor para Visual Studio (ou o Prompt de Comando do Visual Studio no Windows 7). Para obter mais informações, consulte [Prompts de Comando](developer-command-prompt-for-vs.md).  
  
 No prompt de comando, digite o seguinte:  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
resgen  [-define:symbol1[,symbol2,...]] [/useSourcePath] filename.extension  | /compile filename.extension... [outputFilename.extension] [/r:assembly] [/str:lang[,namespace[,class[,file]]] [/publicclass]]
```  
  
```console  
resgen filename.extension [outputDirectory]  
```  
  
## <a name="parameters"></a>Parâmetros  
  
|Parâmetro ou opção|Descrição|  
|-------------------------|-----------------|  
|`/define:` *symbol1*[, *symbol2*,...]|Do .NET Framework 4.5 em diante, dá suporte à compilação condicional em arquivos de recurso com base em texto (.txt ou .restext). Se *symbol* corresponder a um símbolo incluído no arquivo de texto de entrada dentro de um constructo `#ifdef`, o recurso da cadeia de caracteres associado estará incluído no arquivo .resources. Se o arquivo de texto de entrada incluir uma declaração `#if !` com um símbolo não definido pela opção `/define`, o recurso da cadeia de caracteres associado será incluído no arquivo de recursos.<br /><br /> `/define` será ignorado se for usado com arquivos que não sejam de texto. Os símbolos diferenciam maiúsculas de minúsculas.<br /><br /> Para obter mais informações sobre essa opção, consulte [Compilando Recursos Condicionalmente](#Conditional) mais à frente neste tópico.|  
|`useSourcePath`|Especifica que o diretório atual do arquivo de entrada deve ser usado para resolver caminhos de arquivo relativos.|  
|`/compile`|Permite especificar vários arquivos .resx ou de texto para conversão em vários arquivos .resources em uma única operação em massa. Se não especificar essa opção, você poderá especificar apenas um argumento de arquivo de entrada. Os arquivos de saída são *filename*.resources.<br /><br /> Essa opção não pode ser usada com a opção `/str:`.<br /><br /> Para obter mais informações sobre essa opção, consulte [Compilando ou Convertendo Vários Arquivos](#Multiple) mais à frente neste tópico.|  
|`/r:` `assembly`|Metadados de referência do assembly especificado. Eles são usados durante a conversão de arquivos .resx e permitem que Resgen.exe serialize ou desserialize recursos de objeto. Isso é semelhante às opções `/reference:` ou `/r:` para os compiladores do C# e do Visual Basic.|  
|`filename.extension`|Especifica o nome do arquivo de entrada para conversão. Se você estiver usando o primeiro, a sintaxe da linha de comando mais longa apresentada antes dessa tabela, `extension` deverá ser um dos seguintes:<br /><br /> .txt ou .restext<br /> Um arquivo de texto para conversão em um arquivo .resources ou .resx. Os arquivos de texto só podem conter recursos de cadeia de caracteres. Para obter informações sobre o formato do arquivo, consulte a seção "Recursos em arquivos de texto" de [Criando arquivos de recurso](../resources/creating-resource-files-for-desktop-apps.md).<br /><br /> .resx<br /> Um arquivo de recurso com base em XML para converter um arquivo .resources ou de texto (.txt ou .restext).<br /><br /> .resources<br /> Um arquivo de recurso binário para converter um arquivo .resx ou de texto (.txt ou .restext).<br /><br /> Se você estiver usando o segundo, a sintaxe da linha de comando mais curta apresentada antes dessa tabela, `extension` deverá ser o seguinte:<br /><br /> .exe ou .dll<br /> Um assembly .NET Framework (executável ou biblioteca) cujos recursos de cadeia de caracteres devem ser extraídos para um arquivo. resw para uso no desenvolvimento de aplicativos da loja do Windows 8. x.|  
|`outputFilename.extension`|Especifica o nome e o tipo do arquivo de recurso a ser criado.<br /><br /> Este argumento é opcional durante a conversão de um arquivo .txt, .restext ou .resx em um arquivo .resources. Se você não especificar `outputFilename`, Resgen.exe acrescentará uma extensão .resources à entrada `filename` e gravará o arquivo no diretório que contém `filename,extension`.<br /><br /> O argumento `outputFilename.extension` é obrigatório durante a conversão de um arquivo .resources. Especifique um nome de arquivo com a extensão .resx ao converter um arquivo .resources em um arquivo de recurso com base no XML. Especifique um nome de arquivo com a extensão .txt ou .restext ao converter um arquivo .resources em um arquivo de texto. Você só deverá converter um arquivo .resources em um arquivo .txt quando o arquivo .resources contiver apenas valores de cadeia de caracteres.|  
|`outputDirectory`|Para aplicativos da loja do Windows 8. x, especifica o diretório no qual um arquivo. resw que contém os recursos de cadeia de caracteres no `filename.extension` será gravado. `outputDirectory` já deve existir.|  
|`/str:` `language[,namespace[,classname[,filename]]]`|Cria um arquivo de classe de recurso fortemente tipado na linguagem de programação especificada na opção `language`. `language` pode consistir em um dos seguintes literais:<br /><br /> – Para o C#: `c#`, `cs` ou `csharp`.<br />– Para o Visual Basic: `vb` ou `visualbasic`.<br />– Para o VBScript: `vbs` ou `vbscript`.<br />– Para o C++: `c++`, `mc` ou `cpp`.<br />– Para o JavaScript: `js`, `jscript` ou `javascript`.<br /><br /> A opção `namespace` especifica o namespace padrão do projeto, a opção `classname` especifica o nome da classe gerada e a opção `filename` especifica o nome do arquivo de classe.<br /><br /> Como a opção `/str:` permite apenas um arquivo de entrada, ela não pode ser usada com a opção `/compile`.<br /><br /> Se `namespace` for especificado, mas `classname` não for, o nome da classe será derivado do nome do arquivo de saída (por exemplo, sublinhados são substituídos por pontos finais). Assim, os recursos fortemente tipados talvez não funcionem corretamente. Para evitar isso, especifique o nome da classe e o nome do arquivo de saída.<br /><br /> Para obter mais informações sobre essa opção, consulte [Gerando uma Classe de Recurso Fortemente Tipado](#Strong) mais à frente neste tópico.|  
|`/publicClass`|Cria uma classe de recurso fortemente tipada como uma classe pública. Por padrão, a classe de recurso é `internal` no C# e `Friend` no Visual Basic.<br /><br /> Essa opção será ignorada se a opção `/str:` não for usada.|  
  
## <a name="resgenexe-and-resource-file-types"></a>Resgen.exe e Tipos de Arquivo de Recurso  
 Para que Resgen.exe converta arquivos de recursos, de texto e .resx com êxito, eles devem seguir o formato correto.  
  
### <a name="text-txt-and-restext-files"></a>Arquivos de Texto (.txt e .restext)  
 Os arquivos de texto (.txt ou .restext) só podem conter recursos de cadeia de caracteres. Os recursos de cadeia de caracteres serão úteis se você estiver gravando um aplicativo que deve ter cadeias de caracteres convertidas em várias linguagens. Por exemplo, é possível regionalizar facilmente cadeias de caracteres de menu usando o recurso de cadeia de caracteres apropriado. Resgen.exe lê arquivos de texto que contêm pares de nome/valor, em que o nome é uma cadeia de caracteres que descreve o recurso e o valor é a própria cadeia de caracteres de recursos.  
  
> [!NOTE]
> Para obter informações sobre o formato de arquivos .txt e .restext, consulte a seção "Recursos em arquivos de texto" de [Criando arquivos de recurso](../resources/creating-resource-files-for-desktop-apps.md).  
  
 Um arquivo de texto que contém recursos deve ser salvo com codificação UTF-8 ou Unicode (UTF-16), a menos que ele contenha apenas caracteres no intervalo Latim Básico (para U+007F). Resgen.exe remove caracteres ANSI estendidos ao processar um arquivo de texto salvo usando a codificação ANSI.  
  
 Resgen.exe verifica se há nomes de recurso duplicados no arquivo de texto. Se o arquivo de texto contiver nomes de recurso duplicados, Resgen.exe emitirá um aviso e ignorará o segundo valor.  
  
### <a name="resx-files"></a>Arquivos .resx  
 O formato do arquivo de recurso .resx consiste em entradas XML. É possível especificar recursos de cadeia de caracteres dentro dessas entradas XML, como você faria em arquivos de texto. Uma vantagem principal dos arquivos .resx em relação aos arquivos de texto é que também é possível especificar ou inserir objetos. Ao exibir um arquivo .resx, você pode consultar o formulário binário de um objeto inserido (por exemplo, uma imagem) quando essas informações binárias fazem parte do manifesto de recurso. Assim como acontece com arquivos de texto, é possível abrir um arquivo .resx com um editor de texto (como Bloco de Notas ou Microsoft Word), gravar, analisar, e manipular seu conteúdo. Observe que isso exige um bom conhecimento de marcas XML e da estrutura do arquivo .resx. Para obter informações sobre o formato do arquivo, consulte a seção "Recursos em arquivos .resx" de [Criando arquivos de recurso](../resources/creating-resource-files-for-desktop-apps.md).  
  
 Para criar um arquivo .resources que contém objetos de não cadeia de caracteres inseridos, você deve usar Resgen.exe para converter um arquivo .resx contendo objetos ou adicionar os recursos de objeto ao arquivo diretamente do código chamando os métodos fornecidos pela classe <xref:System.Resources.ResourceWriter>.  
  
 Se o arquivo .resx ou .resources contiver objetos e você usar Resgen.exe para convertê-lo em um arquivo de texto, todos os recursos de cadeia de caracteres serão convertidos corretamente, mas os tipos de dados dos objetos de não cadeia de caracteres também serão gravados no arquivo como cadeias de caracteres. Você perderá os objetos inseridos na conversão, e Resgen.exe relatará que ocorreu um erro durante a recuperação dos recursos.  
  
### <a name="converting-between-resources-file-types"></a>Convertendo Entre Tipos de Arquivo de Recursos  
 Durante a conversão entre tipos de arquivo de recurso diferentes, Resgen.exe talvez não seja capaz de realizar a conversão ou pode perder informações sobre recursos específicos, dependendo dos tipos de arquivo de origem e de destino. A tabela a seguir especifica os tipos de conversões bem-sucedidos durante a conversão de um tipo de arquivo de recurso em outro.  
  
|Converter de|Em arquivo de texto|Em arquivo .resx|Em arquivo .resw|Em arquivo .resources|  
|------------------|------------------|-------------------|-------------------|------------------------|  
|Arquivo de texto (.txt ou .restext)|--|Sem problemas|Sem suporte|Sem problemas|  
|arquivo .resx|Haverá falha na conversão se o arquivo contiver recursos que não sejam de cadeia de caracteres (incluindo links de arquivo)|--|Sem suporte|Sem problemas|  
|arquivo .resources|Haverá falha na conversão se o arquivo contiver recursos que não sejam de cadeia de caracteres (incluindo links de arquivo)|Sem problemas|Sem suporte|--|  
|assembly .exe ou .dll|Sem suporte|Sem suporte|Somente recursos de cadeia de caracteres (incluindo nomes de caminho) são reconhecidos como recursos|Sem suporte|  
  
## <a name="performing-specific-resgenexe-tasks"></a>Realizando Tarefas de Resgen.exe Específicas  
 É possível usar Resgen.exe de várias formas: compilar um arquivo de recurso com base em texto ou XML em um arquivo binário, converter entre formatos de arquivo de recurso e gerar uma classe que encapsule a funcionalidade <xref:System.Resources.ResourceManager> e dá acesso aos recursos. Esta seção fornece informações detalhadas sobre cada tarefa:  
  
- [Compilando Recursos em um Arquivo Binário](resgen-exe-resource-file-generator.md#Compiling)  
  
- [Convertendo entre tipos de arquivo de recurso](resgen-exe-resource-file-generator.md#Convert)  
  
- [Compilando ou Convertendo Vários Arquivos](resgen-exe-resource-file-generator.md#Multiple)  
  
- [Exportando recursos para um arquivo .resw](resgen-exe-resource-file-generator.md#Exporting)  
  
- [Compilando Recursos Condicionalmente](resgen-exe-resource-file-generator.md#Conditional)  
  
- [Gerando uma Classe de Recurso Fortemente Tipado](resgen-exe-resource-file-generator.md#Strong)  
  
<a name="Compiling"></a>
### <a name="compiling-resources-into-a-binary-file"></a>Compilando Recursos em um Arquivo Binário  
 O uso mais comum de Resgen.exe é compilar um arquivo de recurso com base em texto (um arquivo .txt ou .restext) ou um arquivo de recurso com base em XML (um arquivo .resx) em um arquivo .resources binário. Em seguida, o arquivo de saída pode ser inserido em um assembly principal por um compilador de linguagem ou em um assembly satélite pelo [Assembly Linker (AL.exe)](al-exe-assembly-linker.md).  
  
 A sintaxe para compilar um arquivo de recurso é:  
  
```console  
resgen inputFilename [outputFilename]
```  
  
 em que os parâmetros são:  
  
 `inputFilename`  
 O nome do arquivo, incluindo a extensão, do arquivo de recurso para compilação. Resgen.exe só compila arquivos com extensões de .txt, .restext ou .resx.  
  
 `outputFilename`  
 O nome do arquivo de saída. Se você omitir `outputFilename`, Resgen.exe criará um arquivo .resources com o nome do arquivo raiz de `inputFilename` no mesmo diretório de `inputFilename`. Se `outputFilename` incluir um caminho do diretório, o diretório deverá existir.  
  
 Você fornece um namespace totalmente qualificado para o arquivo .resources especificando-o no nome do arquivo e separando-o do nome de arquivo raiz por um ponto final. Por exemplo, se `outputFilename` for `MyCompany.Libraries.Strings.resources`, o namespace será MyCompany.Libraries.  
  
 O comando a seguir lê os pares de nome/valor em Resources.txt e grava um arquivo .resources binário chamado Resources.resources. Como não é especificado explicitamente, o nome do arquivo de saída recebe o mesmo nome do arquivo de entrada por padrão.  
  
```console  
resgen Resources.txt
```  
  
 O comando a seguir lê os pares de nome/valor em Resources.restext e grava um arquivo de recursos binário chamado StringResources.resources.  
  
```console  
resgen Resources.restext StringResources.resources  
```  
  
 O comando a seguir lê um arquivo de entrada com base em XML chamado Resources.resx e grava um arquivo .resources binário chamado Resources.resources.  
  
```console  
resgen Resources.resx Resources.resources  
```  
  
<a name="Convert"></a>
### <a name="converting-between-resource-file-types"></a>Convertendo Entre Tipos de Arquivo de Recurso  
 Além de compilar arquivos de recurso com base em texto ou XML em arquivos .resources binários, Resgen.exe pode converter qualquer tipo de arquivo compatível em qualquer outro tipo de arquivo compatível. Isso significa que ele pode realizar as seguintes conversões:  
  
- Arquivos .txt e .restext em arquivos .resx.  
  
- Arquivos .resx em arquivos .txt e .restext.  
  
- Arquivos .resources em arquivos .txt e .restext.  
  
- Arquivos .resources em arquivos .resx.  
  
 A sintaxe é igual à mostrada na seção anterior.  
  
 Além disso, você pode usar Resgen.exe para converter recursos incorporados em um assembly de .NET Framework em um arquivo. resw Tor aplicativos da loja do Windows 8. x.  
  
 O comando a seguir lê um arquivo de recursos binário Resources.resources e grava um arquivo de saída com base em XML chamado Resources.resx.  
  
```console  
resgen Resources.resources Resources.resx  
```  
  
 O comando a seguir lê um arquivo de recursos com base em texto chamado StringResources.txt e grava um arquivo de recursos com base em XML chamado LibraryResources.resx. Além de conter recursos de cadeia de caracteres, o arquivo .resx também poderia ser usado para armazenar recursos que não sejam de cadeia de caracteres.  
  
```console  
resgen StringResources.txt LibraryResources.resx  
```  
  
 Os dois comandos a seguir leem um arquivo de recursos com base em XML chamado Resources.resx e gravam arquivos de texto chamados Resources.txt e Resources.restext. Se o arquivo .resx contiver objetos inseridos, eles não serão convertidos precisamente nos arquivos de texto.  
  
```console  
resgen Resources.resx Resources.txt  
resgen Resources.resx Resources.restext  
```  
  
<a name="Multiple"></a>
### <a name="compiling-or-converting-multiple-files"></a>Compilando ou Convertendo Vários Arquivos  
 É possível usar a opção `/compile` para converter uma lista de arquivos de recurso de um formato em outro em uma única operação. A sintaxe do é:  
  
```console  
resgen /compile filename.extension [filename.extension...]  
```  
  
 O comando a seguir compila três arquivos, StringResources.txt, TableResources.resw e ImageResources.resw, em arquivos .resources separados chamados StringResources.resources, TableResources.resources e ImageResources.resources.  
  
```console  
resgen /compile StringResources.txt TableResources.resx ImageResources.resx  
```  
  
<a name="Exporting"></a>
### <a name="exporting-resources-to-a-resw-file"></a>Exportando Recursos para um Arquivo .resw  
 Se você estiver desenvolvendo um aplicativo de armazenamento do Windows 8. x, talvez queira usar recursos de um aplicativo de área de trabalho existente. No entanto, os dois tipos de aplicativos dão suporte a formatos de arquivo diferentes. Em aplicativos da área de trabalho, recursos em arquivos de texto (.txt ou .restext) ou .resx são compilados em arquivos .resources binários. Nos aplicativos da loja do Windows 8. x, os arquivos. resw são compilados em arquivos de índice de recurso de pacote binário (PRI). Você pode usar Resgen.exe para preencher essa lacuna extraindo recursos de um executável ou de um assembly satélite e gravando-os em um ou mais arquivos. resw que podem ser usados ao desenvolver um aplicativo da loja do Windows 8. x.  
  
> [!IMPORTANT]
> O Visual Studio manipula automaticamente todas as conversões necessárias para incorporar os recursos em uma biblioteca portátil em um aplicativo da loja do Windows 8. x. Usar Resgen.exe diretamente para converter os recursos em um assembly para o formato de arquivo. resw é de interesse apenas para os desenvolvedores que desejam desenvolver um aplicativo da loja do Windows 8. x fora do Visual Studio.  
  
 A sintaxe para gerar arquivos .resw com base em um assembly é:  
  
```console  
resgen filename.extension  [outputDirectory]  
```  
  
 em que os parâmetros são:  
  
 `filename.extension`  
 O nome de um assembly do .NET Framework (um executável ou um .DLL). Se o arquivo não contiver recursos, Resgen.exe não criará arquivos.  
  
 `outputDirectory`  
 O diretório existente no qual os arquivos .resw devem ser gravados. Se `outputDirectory` for omitido, os arquivos .resw serão gravados no diretório atual. Resgen.exe cria um arquivo .resw para cada arquivo .resources no assembly. O nome do arquivo raiz do arquivo .resw é igual ao nome da raiz do arquivo .resources.  
  
 O seguinte comando cria um arquivo .resw no diretório Win8Resources de cada arquivo .resources inserido em MyApp.exe:  
  
```console  
resgen MyApp.exe Win8Resources  
```  
  
<a name="Conditional"></a>
### <a name="conditionally-compiling-resources"></a>Compilando Recursos Condicionalmente  
 Do .NET Framework 4.5 em diante, o Resgen.exe dá suporte à compilação condicional de recursos de cadeia de caracteres em arquivos de texto (.txt e .restext). Isso permite usar um arquivo de recurso com base em texto único em várias configurações de compilação.  
  
 Em um arquivo .txt ou .restext, você usa o constructo `#ifdef`...`#endif` para incluir um recurso no arquivo .resources binário se um símbolo é definido e usa o constructo `#if !`... `#endif` para incluir um recurso se um símbolo não é definido. No momento da compilação, você define símbolos usando a opção `/define:` seguido de uma lista delimitada por vírgulas de símbolos. A comparação diferencia maiúsculas de minúsculas; o uso de maiúsculas e minúsculas nos símbolos definidos por `/define` deve corresponder ao uso de maiúsculas e minúsculas de símbolos nos arquivos de texto que serão compilados.  
  
 Por exemplo, o arquivo a seguir chamado UIResources.rext inclui um recurso de cadeia de caracteres chamado `AppTitle` que pode utilizar um dos três valores, dependendo da possibilidade dos símbolos chamados `PRODUCTION`, `CONSULT` ou `RETAIL` estarem definidos.  
  
```text
#ifdef PRODUCTION  
AppTitle=My Software Company Project Manager
#endif  
#ifdef CONSULT  
AppTitle=My Consulting Company Project Manager  
#endif  
#ifdef RETAIL  
AppTitle=My Retail Store Project Manager  
#endif  
FileMenuName=File  
```  
  
 Em seguida, o arquivo pode ser compilado em um arquivo .resources binário com o seguinte comando:  
  
```console  
resgen /define:CONSULT UIResources.restext  
```  
  
 Isso produz um arquivo .resources que contém dois recursos de cadeia de caracteres. O valor do recurso `AppTitle` é "My Consulting Company Project Manager".  
  
<a name="Strong"></a>
### <a name="generating-a-strongly-typed-resource-class"></a>Gerando uma Classe de Recurso Fortemente Tipado  
 Resgen.exe dá suporte a recursos fortemente tipados, que encapsula o acesso a recursos criando classes que contêm um conjunto de propriedades somente leitura estáticas. Isso oferece uma alternativa à chamada dos métodos da classe <xref:System.Resources.ResourceManager> diretamente para recuperar recursos. É possível habilitar o suporte ao recursos fortemente tipado usando-se a opção `/str` em Resgen.exe, que encapsula a funcionalidade da classe <xref:System.Resources.Tools.StronglyTypedResourceBuilder>. Quando você especifica a opção `/str`, a saída de Resgen.exe é uma classe que contém propriedades fortemente tipadas que correspondem aos recursos referenciados no parâmetro de entrada. Esta classe dá acesso somente leitura fortemente tipado aos recursos disponíveis no arquivo processado.  
  
 A sintaxe para criar um recurso fortemente tipado é:  
  
```console  
resgen inputFilename [outputFilename] /str:language[,namespace,[classname[,filename]]] [/publicClass]  
```  
  
 Os parâmetros e as opções são:  
  
 `inputFilename`  
 O nome do arquivo, incluindo a extensão, do arquivo de recurso para o qual uma classe de recurso fortemente tipada é gerada. O arquivo pode ser um arquivo com base em texto, em XML ou .resources binário; ele pode ter uma extensão .txt, .restext, .resw ou .resources.  
  
 `outputFilename`  
 O nome do arquivo de saída. Se `outputFilename` incluir um caminho do diretório, o diretório deverá existir. Se você omitir `outputFilename`, Resgen.exe criará um arquivo .resources com o nome do arquivo raiz de `inputFilename` no mesmo diretório de `inputFilename`.  
  
 `outputFilename` pode ser um arquivo com base em texto, em XML ou .resources binário. Se a extensão de arquivo `outputFilename` for diferente da extensão de arquivo de `inputFilename`, Resgen.exe realizará a conversão do arquivo.  
  
 Se `inputFilename` for um arquivo .resources, Resgen.exe copiará o arquivo .resources se `outputFilename` também for um arquivo .resources. Se `outputFilename` for omitido, Resgen.exe substituirá `inputFilename` por um arquivo .resources idêntico.  
  
 *linguagem*  
 O idioma no qual gerar o código-fonte para a classe de recurso fortemente tipado. Os valores possíveis são `cs`, `C#` e `csharp` para código C#, `vb` e `visualbasic` para código Visual Basic, `vbs` e `vbscript` para código VBScript e `c++`, `mc` e `cpp` para código C++.  
  
 *namespace*  
 O namespace que contém a classe de recurso fortemente tipada. O arquivo .resources e a classe de recurso devem ter o mesmo namespace. Para obter informações sobre como especificar o namespace no `outputFilename`, consulte [Compilando recursos em um arquivo binário](resgen-exe-resource-file-generator.md#Compiling). Se *namespace* for omitido, a classe de recurso não estará contida em um namespace.  
  
 *ClassName*  
 O nome da classe de recurso fortemente tipado. Isso deve corresponder ao nome da raiz do arquivo .resources. Por exemplo, se Resgen.exe gerar um arquivo .resources chamado MyCompany.Libraries.Strings.resources, o nome da classe de recurso fortemente tipado será Strings. Se *classname* for omitido, a classe gerada será derivada do nome da raiz de `outputFilename`. Se `outputFilename` for omitido, a classe gerada será derivada do nome da raiz de `inputFilename`.  
  
 *classname* não pode conter caracteres inválidos como espaços inseridos. Se *ClassName* contiver espaços inseridos, ou se *ClassName* for gerado por padrão de *inputFilename*e *inputFilename* contiver espaços inseridos, Resgen.exe substituirá todos os caracteres inválidos por um sublinhado ( \_ ).  
  
 *nome do arquivo*  
 O nome do arquivo de classe.  
  
 `/publicclass`  
 Torna pública a classe de recurso fortemente tipada em vez de `internal` (no C#) ou `Friend` (no Visual Basic). Isso permite que os recursos sejam acessados fora do assembly no qual estão inseridos.  
  
> [!IMPORTANT]
> Quando você cria uma classe de recurso fortemente tipada, o nome do arquivo .resources deve corresponder ao namespace e ao nome da classe do código gerado. No entanto, Resgen.exe permite especificar opções que produzam um arquivo .resources que tenha um nome incompatível. Para contornar esse comportamento, renomeie o arquivo de saída depois de ter sido gerado.  
  
 A classe de recurso fortemente tipada tem os seguintes membros:  
  
- Um construtor sem parâmetros, que pode ser usado para instanciar a classe de recursos fortemente tipados.  
  
- Uma propriedade `static` (C#) ou `Shared` (Visual Basic) e a propriedade `ResourceManager` somente leitura, que retorna a instância <xref:System.Resources.ResourceManager> que gerencia o recurso fortemente tipado.  
  
- Uma propriedade `Culture` estática, que permite definir a cultura usada na recuperação de recurso. Por padrão, o valor é `null`, o que significa que a cultura da interface do usuário atual é usada.  
  
- Uma propriedade `static` (C#) ou `Shared` (Visual Basic) e a propriedade somente leitura para cada recurso no arquivo .resources. O nome da propriedade é o nome do recurso. -  
  
 Por exemplo, o comando a seguir compila um arquivo de recurso chamado StringResources.txt em StringResources.resources e gera uma classe chamada `StringResources` em um arquivo de código-fonte do Visual Basic chamado StringResources.vb que pode ser usado para acessar o Gerenciador de Recursos.  
  
```console  
resgen StringResources.txt /str:vb,,StringResources
```  
  
## <a name="see-also"></a>Consulte também

- [Ferramentas](index.md)
- [Recursos em aplicativos da área de trabalho](../resources/index.md)
- [Criação de arquivos de recurso](../resources/creating-resource-files-for-desktop-apps.md)
- [Al.exe (vinculador de assembly)](al-exe-assembly-linker.md)
- [Prompts de comando](developer-command-prompt-for-vs.md)
