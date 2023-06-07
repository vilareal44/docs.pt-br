---
title: Criar arquivos de recurso para aplicativos .NET
description: Crie arquivos de recursos para aplicativos .NET. Crie arquivos de texto com recursos de cadeia de caracteres, XML ou arquivos binários programaticamente, ou arquivos XML com dados de cadeia de caracteres, imagem ou objeto.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- resource files, .resources files
- .resources files
- application resources, creating files
- resource files, creating
ms.assetid: 6c5ad891-66a0-4e7a-adcf-f41863ba6d8d
ms.openlocfilehash: 4730a14e499c75176d7ba7c8378626070d5211e9
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86865171"
---
# <a name="create-resource-files-for-net-apps"></a>Criar arquivos de recurso para aplicativos .NET

Você pode incluir recursos, como cadeias de caracteres, imagens ou dados de objetos em arquivos de recursos para torná-los facilmente disponíveis no seu aplicativo. O .NET Framework tem cinco maneiras de criar arquivos de recursos:

- Crie um arquivo de texto que contenha recursos de cadeia de caracteres. Você pode usar o [Gerador de Arquivos de Recurso (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md) para converter o arquivo de texto em um arquivo de recursos binário (. resources). Em seguida, você pode inserir o arquivo de recurso binário em um executável de aplicativo ou uma biblioteca de aplicativos usando um compilador de linguagem, ou você pode incorporá-lo em um assembly satélite usando o [vinculador de assembly (Al.exe)](../tools/al-exe-assembly-linker.md). Para obter mais informações, consulte a secção [Recursos em Arquivos de Texto](creating-resource-files-for-desktop-apps.md#TextFiles).

- Criar um arquivo de recurso XML (.resx) que contenha a cadeia de caracteres, imagens ou dados de objeto. Você pode usar o [Gerador de Arquivos de Recurso (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md) para converter o arquivo. resx em um arquivo de recursos binário (.resources). Em seguida, pode inserir o arquivo de recurso binário em um aplicativo executável ou em uma biblioteca de aplicativos usando um compilador de linguagem, ou você pode inseri-la em um assembly satélite usando o [Assembly Linker (Al.exe)](../tools/al-exe-assembly-linker.md). Para obter mais informações, confira a secção [Recursos em Arquivos .resx](creating-resource-files-for-desktop-apps.md#ResxFiles).

- Crie um arquivo de recurso XML (.resx) com programação usando os tipos no namespace <xref:System.Resources>. Pode criar um arquivo .resx, enumerar os seus recursos e recuperar os recursos específicos pelo seu nome. Para obter mais informações, confira [Trabalhando com arquivos .resx de forma programática](working-with-resx-files-programmatically.md).

- Criar um arquivo de recurso binário (.resources) de forma programática. Em seguida, pode inserir o arquivo em um aplicativo executável ou em uma biblioteca de aplicativos usando um compilador de linguagem, ou você pode inseri-la em um assembly satélite usando o [Assembly Linker (Al.exe)](../tools/al-exe-assembly-linker.md). Para obter mais informações, confira a secção [Recursos em Arquivos .resources](creating-resource-files-for-desktop-apps.md#ResourcesFiles).

- Use o [Visual Studio](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link) para criar um arquivo de recursos e incluí-lo em seu projeto. O Visual Studio fornece um editor de recursos que lhe permite adicionar, excluir e modificar recursos. No momento da compilação, o arquivo de recurso é automaticamente convertido em um arquivo binário .resources e inserido em um assembly de aplicativo ou em um assembly satélite. Para obter mais informações, confira a secção [Arquivos de Recurso no Visual Studio](creating-resource-files-for-desktop-apps.md#VSResFiles).

<a name="TextFiles"></a>
## <a name="resources-in-text-files"></a>Recursos em arquivos de texto

Você pode usar arquivos de texto (.txt ou .restext) para armazenar apenas os recursos de cadeia de caracteres. Para recursos que não são cadeia de caracteres, use arquivos .resx ou crie-os por meio de programação. Os arquivos de texto que contêm recursos de cadeia de caracteres têm o seguinte formato:

```text
# This is an optional comment.
name = value

; This is another optional comment.
name = value

; The following supports conditional compilation if X is defined.
#ifdef X
name1=value1
name2=value2
#endif

# The following supports conditional compilation if Y is undefined.
#if !Y
name1=value1
name2=value2
#endif
```

 O formato do arquivo de recursos de .txt e dos arquivos .restext é idêntico. A extensão de arquivo .restext serve apenas para tornar os arquivos de texto imediatamente identificáveis como arquivos de recurso baseados em texto.

 Os recursos de cadeia de caracteres são exibidos como pares *nome/valor*, em que *nome* é uma cadeia que identifica o recurso e *valor* é a cadeia de recurso que é retornada quando passa o *nome* para um método de recuperação de recursos, como <xref:System.Resources.ResourceManager.GetString%2A?displayProperty=nameWithType>. O *nome* e o *valor* devem ser separados por um sinal de igual (=). Por exemplo:

```text
FileMenuName=File
EditMenuName=Edit
ViewMenuName=View
HelpMenuName=Help
```

> [!CAUTION]
> Não use arquivos de recurso para armazenar senhas, informações confidenciais ou dados privados.

 Cadeias de caracteres vazias (ou seja, um recurso cujo valor é <xref:System.String.Empty?displayProperty=nameWithType>) são permitidas em arquivos de texto. Por exemplo:

```text
EmptyString=
```

 A partir do .NET Framework 4,5 e em todas as versões do .NET Core, os arquivos de texto dão suporte à compilação condicional com as `#ifdef` *construções Symbol*... `#endif` e `#if !` *Symbol*.. `#endif` .. Então pode usar o alternador `/define` com o [Gerador de Arquivos de Recurso (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md) para definir os símbolos. Cada recurso requer seu próprio `#ifdef` *símbolo*... `#endif` ou a `#if !` construção *Symbol*... `#endif` . Se você usar uma instrução `#ifdef` e estiver definido o *símbolo*, o recurso associado é incluído no arquivo .resources; caso contrário, ele não será incluído. Se você usar uma instrução `#if !` e não estiver definido o *símbolo*, o recurso associado é incluído no arquivo .resources; caso contrário, ele não será incluído.

 Os comentários são opcionais em arquivos de texto e são precedidos por um ponto e vírgula (;) ou por um sinal de cerquilha (#) no início de uma linha. As linhas que contêm comentários podem ser colocadas em qualquer lugar no arquivo. Os comentários não são incluídos em um arquivo .resources compilado que é criado usando o [Gerador de Arquivos de Recurso (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md).

 Todas as linhas em branco nos arquivos de texto são consideradas como espaços em branco e são ignoradas.

 O exemplo a seguir define dois recursos de cadeia de caracteres denominados `OKButton` e `CancelButton`.

```text
#Define resources for buttons in the user interface.
OKButton=OK
CancelButton=Cancel
```

 Se o arquivo de texto contiver ocorrências duplicadas de *nome*, o [Gerador de Arquivo de Recursos (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md) exibe um aviso e ignora o segundo nome.

 o *valor* não pode conter novos caracteres de linha, mas você pode usar caracteres de escape em estilo de linguagem C, como `\n` para representar uma nova linha e `\t` para representar uma guia. Você também pode incluir um caractere de barra invertida se ele tiver escape (por exemplo, " \\ \\ "). Além disso, é permitida uma cadeia de caracteres vazia.

 Salve os recursos no formato de arquivo de texto usando a codificação UTF-8 ou UTF-16 em uma ordem de byte little-endian ou big endian. No entanto, o [Gerador de Arquivos de Recurso (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md), que converte um arquivo .txt em um arquivo .resources, por padrão trata-os como sendo UTF-8. Se quiser que o Resgen.exe reconheça um arquivo codificado com UTF-16, deve incluir uma marca de ordem de byte Unicode (U + FEFF) no início do arquivo.

 Para incorporar um arquivo de recurso no formato de texto em um assembly do .NET, deve converter o arquivo em um arquivo de recursos binário (.resources) usando o [Gerador de Arquivo de Recursos (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md). Em seguida, pode inserir o arquivo .resources em um assembly do .NET usando um compilador de linguagem, ou pode inseri-lo em um assembly satélite, usando o [Assembly Linker (Al.exe)](../tools/al-exe-assembly-linker.md).

 O exemplo seguinte usa um arquivo de recursos no formato de texto chamado GreetingResources.txt para um aplicativo de console simples "Hello World". O arquivo de texto define duas cadeias de caracteres, `prompt` e `greeting` isso solicita que o usuário insira seu nome e exiba uma saudação.

```text
# GreetingResources.txt
# A resource file in text format for a "Hello World" application.
#
# Initial prompt to the user.
prompt=Enter your name:
# Format string to display the result.
greeting=Hello, {0}!
```

O arquivo de texto é convertido em um arquivo .resources usando o seguinte comando:

```console
resgen GreetingResources.txt
```

 O exemplo a seguir mostra o código-fonte para um aplicativo de console que usa o arquivo .resources para exibir mensagens para o usuário.

 [!code-csharp[Conceptual.Resources.TextFiles#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.textfiles/cs/greeting.cs#1)]
 [!code-vb[Conceptual.Resources.TextFiles#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.textfiles/vb/greeting.vb#1)]

 Se você estiver usando o Visual Basic e o arquivo de código-fonte se chamar Greeting.vb, o comando a seguir cria um arquivo executável que inclui o arquivo .resources incorporado:

```console
vbc greeting.vb -resource:GreetingResources.resources
```

 Se você estiver usando o C# e o arquivo de código-fonte se chamar Greeting.cs, o comando a seguir cria um arquivo executável que inclui o arquivo .resources incorporado:

```console
csc greeting.cs -resource:GreetingResources.resources
```

<a name="ResxFiles"></a>
## <a name="resources-in-resx-files"></a>Recursos em arquivos .resx
 Ao contrário dos arquivos de texto, que só podem armazenar recursos de cadeia de caracteres, os arquivos de recurso XML (.resx) podem armazenar cadeias de caracteres, dados binários como imagens, ícones e clipes de áudio e objetos de programação. Um arquivo .resx contém um cabeçalho padrão que descrevem o formato das entradas dos recursos e especifica informações de controle de versão para o XML usado para analisar os dados. Os dados do arquivo de recursos seguem o cabeçalho XML. Cada item de dados consiste em um par nome/valor que está contido numa marca `data`. O seu atributo `name` define o nome do recurso e a marca `value` aninhada contém o valor do recurso. Para dados de cadeia de caracteres, a marca `value` contém a cadeia de caracteres.

 Por exemplo, a seguinte marca `data` define um recurso de cadeia de caracteres chamado `prompt`, cujo valor é "Digite seu nome:".

```xml
<data name="prompt" xml:space="preserve">
  <value>Enter your name:</value>
</data>
```

> [!WARNING]
> Não use arquivos de recurso para armazenar senhas, informações confidenciais ou dados privados.

 Para objetos de recurso, a marca de **dados** inclui um atributo `type` que indica o tipo de dados do recurso. Para os objetos que consistem em dados binários, a marca `data` também inclui um atributo `mimetype`, que indica o tipo de dados binários `base64`.

> [!NOTE]
> Todos os arquivos .resx usam um formatador de serialização binária para gerar e analisar os dados binários de um tipo especificado. Como resultado, um arquivo .resx pode se tornar inválido se formato da serialização binária de um objeto for alterada de um modo incompatível.

 O exemplo a seguir mostra uma parte de um arquivo .resx que inclui um recurso <xref:System.Int32> e uma imagem de bitmap.

```xml
<data name="i1" type="System.Int32, mscorlib">
  <value>20</value>
</data>

<data name="flag" type="System.Drawing.Bitmap, System.Drawing,
    Version=1.0.5000.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a"
    mimetype="application/x-microsoft.net.object.bytearray.base64">
  <value>
    AAEAAAD/////AQAAAAAAAAAMAgAAADtTeX…
  </value>
</data>
```

> [!IMPORTANT]
> Como os arquivos .resx devem consistir em um XML bem formado num formato predefinido, não recomendamos que trabalhe manualmente com arquivos .resx, particularmente quando esses arquivos .resx contêm recursos além de cadeiras de caracteres. Em vez disso, o [Visual Studio](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link) fornece uma interface transparente para criar e manipular arquivos .resx. Para obter mais informações, confira a secção [Arquivos de Recurso no Visual Studio](creating-resource-files-for-desktop-apps.md#VSResFiles). Também pode criar e manipular arquivos .resx programaticamente. Para obter mais informações, confira [Trabalhando com arquivos .resx de forma programática](working-with-resx-files-programmatically.md).

<a name="ResourcesFiles"></a>
## <a name="resources-in-resources-files"></a>Recursos em arquivos .resources

Você pode usar a classe <xref:System.Resources.ResourceWriter?displayProperty=nameWithType> para criar um arquivo de recursos binário (.resources) com programação diretamente no código. Você também pode usar o [Gerador de Arquivos de Recurso (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md) para criar um arquivo .resources de um arquivo de texto ou um arquivo. resx. O arquivo .resources pode conter dados binários (matrizes de bytes) e dados de objeto além dos dados de cadeia de caracteres. Criando um arquivo .resources programaticamente requer as seguintes etapas:

1. Crie um objeto <xref:System.Resources.ResourceWriter> com um nome de arquivo exclusivo. Faça isso especificando um nome de arquivo ou um fluxo de arquivos para um construtor da classe <xref:System.Resources.ResourceWriter>.

2. Chame uma das sobrecargas do método <xref:System.Resources.ResourceWriter.AddResource%2A?displayProperty=nameWithType> para cada recursos nomeado a ser adicionado ao arquivo. O recurso pode ser uma cadeia de caracteres, um objeto ou um conjunto de dados binários (uma matriz de bytes).

3. Chame o método <xref:System.Resources.ResourceWriter.Close%2A?displayProperty=nameWithType> para gravar os recursos no arquivo e feche o objeto <xref:System.Resources.ResourceWriter>.

> [!NOTE]
> Não use arquivos de recurso para armazenar senhas, informações confidenciais ou dados privados.

 O exemplo a seguir cria um arquivo .resources chamado CarResources.resources que armazena seis cadeias de caracteres, um ícone e dois objetos definidos pelo aplicativo (dois objetos `Automobile`). A `Automobile` classe, que é definida e instanciada no exemplo, é marcada com o <xref:System.SerializableAttribute> atributo, que permite que ele seja persistido pelo formatador de serialização binária.

 [!code-csharp[Conceptual.Resources.Resources#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.resources/cs/resources1.cs#1)]
 [!code-vb[Conceptual.Resources.Resources#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.resources/vb/resources1.vb#1)]

 Depois de criar o arquivo .resources, você pode inseri-lo em um executável ou biblioteca de tempo de execução ao incluir o alternador do compilador de linguagem `/resource` ou incorporá-lo em um assembly satélite usando o [Assembly Linker (Al.exe)](../tools/al-exe-assembly-linker.md).

<a name="VSResFiles"></a>
## <a name="resource-files-in-visual-studio"></a>Arquivos de recurso no Visual Studio

Quando você adiciona um arquivo de recurso ao seu projeto do [Visual Studio](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link), o Visual Studio cria um arquivo .resx no diretório do projeto. O Visual Studio fornece editores de recursos que permitem adicionar cadeias de caracteres, imagens e objetos binários. Porque os editores são projetados para lidar apenas com dados estáticos, não podem ser usados para armazenar objetos de programação. Você deve gravar os dados de objeto para um arquivo .resx ou para um arquivo .resources por meio de programação. Para saber mais, confira [Como trabalhar com arquivos .resx de forma programática](working-with-resx-files-programmatically.md) e a seção [Recursos em arquivos .resources](creating-resource-files-for-desktop-apps.md#ResourcesFiles).

Se você estiver adicionando recursos localizados, conceda a eles o mesmo nome de arquivo raiz que o arquivo de recurso principal. Você também deve designar a cultura no nome do arquivo. Por exemplo, se você adicionar um arquivo de recurso denominado Resources.resx, também pode criar arquivos de recursos chamados Resources.en-US.resx e fr Resources.fr-FR.resx para armazenar recursos para as culturas do Inglês (Estados Unidos) e Francês (França), respetivamente. Também deve designar a cultura padrão do seu aplicativo. Essa é a cultura em que os recursos são usados se não existirem recursos localizados encontrados para uma determinada cultura. Para especificar a cultura padrão, no Solution Explorer no Visual Studio, clique com botão direito no nome do projeto, aponte para Aplicativo, clique em **Informações de Assembly** e selecione a idioma/cultura apropriada na lista de **Idioma Neutro**.

No momento da compilação, o Visual Studio converte primeiro os arquivos .resx em um projeto para os arquivos de recurso binários (.resources) e armazena-os num subdiretório do diretório *obj* do projeto. O Visual Studio incorpora quaisquer arquivos de recurso que não contenham recursos localizados no assembly principal que é gerado pelo projeto. Se os arquivos de recurso contém os recursos localizados, o Visual Studio insere-os em assemblies satélites separados para cada cultura localizada. Em seguida, ele armazena cada assembly satélite em um diretório cujo nome corresponde à cultura localizada. Por exemplo, os recursos localizados do Inglês (Estados Unidos) são armazenados em um assembly satélite no subdiretório en-US.

## <a name="see-also"></a>Veja também

- <xref:System.Resources>
- [Recursos em aplicativos da área de trabalho](index.md)
- [Empacotando e implantando recursos](packaging-and-deploying-resources-in-desktop-apps.md)
