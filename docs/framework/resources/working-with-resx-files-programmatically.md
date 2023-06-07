---
title: Trabalhando com arquivos .resx de forma programática
description: Crie ou recupere dados de arquivos de recurso XML (. resx) programaticamente usando tipos e membros no namespace System. Resources da biblioteca de classes .NET.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- resource files, .resx files
- .resx files
ms.assetid: 168f941a-2b84-43f8-933f-cf4a8548d824
ms.openlocfilehash: 519ca099b65710b6eb4251e1a9419e965ee69f93
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87166154"
---
# <a name="work-with-resx-files-programmatically"></a>Trabalhar com arquivos .resx de forma programática

Como arquivos de recurso XML (.resx) devem conter XML bem definido, incluindo um cabeçalho que deve seguir um esquema específico seguido por dados em pares nome/valor, você pode concluir que criar esses arquivo manualmente é algo propenso a erros. Como alternativa, você pode criar arquivos .resx com programação usando tipos e membros da biblioteca de classes .NET. Você também pode usar a biblioteca de classes .NET para recuperar os recursos que são armazenados em arquivos .resx. Este artigo explica como você pode usar os tipos e membros no <xref:System.Resources> namespace para trabalhar com arquivos. resx.

Este artigo discute como trabalhar com arquivos XML (. resx) que contêm recursos. Para obter informações sobre como trabalhar com arquivos de recursos binários que foram inseridos em assemblies, consulte <xref:System.Resources.ResourceManager> .

> [!WARNING]
> Também há outras maneiras de trabalhar com arquivos .resx além de programação. Quando você adiciona um arquivo de recurso a um projeto do [Visual Studio](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link) , o Visual Studio fornece uma interface para criar e manter um arquivo. resx e converte automaticamente o arquivo. resx em um arquivo. Resources no momento da compilação. Você também pode usar um editor de texto para manipular um arquivo .resx diretamente. No entanto, para evitar danificar o arquivo, tenha cuidado para não modificar nenhuma informação binária armazenada nele.

## <a name="create-a-resx-file"></a>Criar um arquivo .resx

Você pode usar a classe <xref:System.Resources.ResXResourceWriter?displayProperty=nameWithType> para criar um arquivo .resx com programação seguindo estas etapas:

1. Crie uma instância de um objeto <xref:System.Resources.ResXResourceWriter> chamando o método <xref:System.Resources.ResXResourceWriter.%23ctor%28System.String%29> e fornecendo o nome do arquivo .resx. O nome do arquivo deve incluir a extensão .resx. Se você criar uma instância do objeto <xref:System.Resources.ResXResourceWriter> em um bloco `using`, não será necessário chamar explicitamente o método <xref:System.Resources.ResXResourceWriter.Close%2A?displayProperty=nameWithType> na etapa 3.

2. Chame o método <xref:System.Resources.ResXResourceWriter.AddResource%2A?displayProperty=nameWithType> para cada recurso que você deseja adicionar ao arquivo. Use as sobrecargas desse método para adicionar cadeia de caracteres, objetos e dados binários (matriz de bytes). Se o recurso for um objeto, ele deverá ser serializável.

3. Chame o método <xref:System.Resources.ResXResourceWriter.Close%2A?displayProperty=nameWithType> para gerar o arquivo de recurso e liberar todos os recursos. Se o objeto <xref:System.Resources.ResXResourceWriter> foi criado em um bloco `using`, os recursos são gravados para o arquivo .resx e os recursos usados pelo objeto <xref:System.Resources.ResXResourceWriter> são liberados ao final do bloco `using`.

O arquivo .resx resultante tem o cabeçalho apropriado e uma marca `data` para cada recurso adicionado pelo método <xref:System.Resources.ResXResourceWriter.AddResource%2A?displayProperty=nameWithType>.

> [!WARNING]
> Não use arquivos de recurso para armazenar senhas, informações confidenciais ou dados privados.

O exemplo a seguir cria um arquivo .resx chamado CarResources.resx que armazena seis cadeias de caracteres, um ícone e dois objetos definidos pelo aplicativo (dois objetos `Automobile`). A `Automobile` classe, que é definida e instanciada no exemplo, é marcada com o <xref:System.SerializableAttribute> atributo.

[!code-csharp[Conceptual.Resources.ResX#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.resx/cs/create1.cs#1)]
[!code-vb[Conceptual.Resources.ResX#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.resx/vb/create1.vb#1)]

> [!TIP]
> Você também pode usar o [Visual Studio](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link) para criar arquivos .resx. No tempo de compilação, o Visual Studio usa o [Gerador de Arquivo de Recurso (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md) para converter o arquivo .resx em um arquivo de recurso binário (.resources), além de inseri-lo em um assembly de aplicativo ou um assembly satélite.

Não é possível inserir um arquivo .resx em um executável do runtime ou compilá-lo em um assembly satélite. Você deve converter seu arquivo .resx em um arquivo de recurso binário (.resources) usando o [Gerador de Arquivo de Recurso (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md). O arquivo .resources resultante poderá então ser inserido em um assembly de aplicativo ou um assembly satélite. Para obter mais informações, consulte [Criando arquivos de recursos](creating-resource-files-for-desktop-apps.md).

## <a name="enumerate-resources"></a>Enumerar recursos
 Em alguns casos, pode ser útil recuperar todos os recursos, em vez de um recurso específico, de um arquivo .resx. Para fazer isso, você pode usar a classe <xref:System.Resources.ResXResourceReader?displayProperty=nameWithType>, que fornece um enumerador para todos os recursos no arquivo .resx. A classe <xref:System.Resources.ResXResourceReader?displayProperty=nameWithType> implementa <xref:System.Collections.IDictionaryEnumerator>, que retorna um objeto <xref:System.Collections.DictionaryEntry> que representa um recurso específico para cada iteração do loop. Sua propriedade <xref:System.Collections.DictionaryEntry.Key%2A?displayProperty=nameWithType> retorna a chave do recurso e sua <xref:System.Collections.DictionaryEntry.Value%2A?displayProperty=nameWithType> propriedade retorna o valor do recurso.

 O exemplo a seguir cria um objeto <xref:System.Resources.ResXResourceReader> para o arquivo CarResources.resx criado no exemplo anterior e itera por meio do arquivo de recurso. Ele adiciona os dois objetos `Automobile` que são definidos no arquivo de recurso para um objeto <xref:System.Collections.Generic.List%601?displayProperty=nameWithType> e adiciona cinco das seis cadeias de caracteres a um objeto <xref:System.Collections.SortedList>. Os valores do objeto <xref:System.Collections.SortedList> são convertidos em uma matriz de parâmetro, que é usada para exibir os títulos de coluna para o console. Os valores da propriedade `Automobile` também são exibidos no console.

 [!code-csharp[Conceptual.Resources.ResX#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.resx/cs/enumerate1.cs#2)]
 [!code-vb[Conceptual.Resources.ResX#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.resx/vb/enumerate1.vb#2)]

## <a name="retrieve-a-specific-resource"></a>Recuperar um recurso específico
 Além de enumerar os itens em um arquivo .resx, você pode recuperar um recurso específico por nome usando a classe <xref:System.Resources.ResXResourceSet?displayProperty=nameWithType>. O método <xref:System.Resources.ResourceSet.GetString%28System.String%29?displayProperty=nameWithType> recupera o valor de um recurso de cadeia de caracteres nomeado. O método <xref:System.Resources.ResourceSet.GetObject%28System.String%29?displayProperty=nameWithType> recupera o valor de um objeto nomeado ou de dados binários. O método retorna um objeto que deve ser convertido (em C# ou no Visual Basic) em um objeto do tipo apropriado.

 O exemplo a seguir recupera o ícone e a cadeia de caracteres de legenda de um formulário pelos nomes dos recursos. Ele também recupera objetos `Automobile` definido pelo aplicativo usados no exemplo anterior e exibe-os em um controle <xref:System.Windows.Forms.DataGridView>.

 [!code-csharp[Conceptual.Resources.ResX#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.resx/cs/retrieve1.cs#3)]
 [!code-vb[Conceptual.Resources.ResX#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.resx/vb/retrieve1.vb#3)]

## <a name="convert-resx-files-to-binary-resources-files"></a>Converter arquivos .resx em arquivos .resources binários
 Converter arquivos .resx em arquivos de recurso binário inserido (.resources) traz vantagens significativas. Embora os arquivos .resx sejam fáceis de ler e manter durante o desenvolvimento do aplicativo, eles raramente são incluídos em aplicativos acabados. Se eles forem distribuídos com um aplicativo, existirão como arquivos separados do executável do aplicativo e as bibliotecas que o acompanham. Por outro lado, arquivos .resources são inseridos no executável do aplicativo ou nos assemblies que os acompanham. Além disso, para aplicativos localizados, contar com arquivos .resx no tempo de execução coloca a responsabilidade de manipular o fallback de recurso nas mãos do desenvolvedor. Por outro lado, se um conjunto de assemblies de satélite que contêm arquivos .resources inseridos tiver sido criado, o Common Language Runtime manipula o processo de fallback de recurso.

 Para converter um arquivo .resx em um arquivo .resources, use o [Gerador de Arquivo de Recurso (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md), que tem a seguinte sintaxe básica:

 **Resgen.exe** *.resxFilename*

 O resultado é um arquivo de recurso binário que tem o mesmo nome de arquivo raiz que o arquivo .resx e uma extensão de arquivo .resources. Esse arquivo poderá ser compilado em um executável ou uma biblioteca no tempo de compilação. Se você estiver usando o compilador do Visual Basic, use a sintaxe a seguir para inserir um arquivo .resources no executável do aplicativo:

 **vbc** *filename* **.vb -resource:** *.resourcesFilename*

 Se você usar C#, a sintaxe será a seguinte:

 **csc** *filename* **.cs -resource:** *.resourcesFilename*

 O arquivo .resources também pode ser incorporado em um assembly satélite usando o [Vinculador de Assembly (AL.exe)](../tools/al-exe-assembly-linker.md), que tem a seguinte sintaxe básica:

 **al** *resourcesFilename* **-out:** *assemblyFilename*

## <a name="see-also"></a>Confira também

- [Criação de arquivos de recurso](creating-resource-files-for-desktop-apps.md)
- [Resgen.exe (gerador de arquivo de recurso)](../tools/resgen-exe-resource-file-generator.md)
- [Al.exe (vinculador de assembly)](../tools/al-exe-assembly-linker.md)
