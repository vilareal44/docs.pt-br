---
title: Formatos de caminho de arquivo em sistemas Windows
description: Neste artigo, saiba mais sobre formatos de caminho de arquivo em sistemas Windows, como caminhos de DOS tradicionais, caminhos de dispositivo DOS e caminhos UNC (Convenção de nomenclatura universal).
ms.date: 06/06/2019
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- I/O, long paths
- long paths
- path formats, Windows
ms.openlocfilehash: 5eb9d5127dffd2e80349352ad7a4b57f8848d56b
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87165796"
---
# <a name="file-path-formats-on-windows-systems"></a>Formatos de caminho de arquivo em sistemas Windows

Membros de muitos dos tipos no namespace <xref:System.IO> incluem um parâmetro `path` que permite que você especifique um caminho absoluto ou relativo para um recurso do sistema de arquivos. Em seguida, esse caminho é passado para as [APIs do sistema de arquivos do Windows](/windows/desktop/fileio/file-systems). Este tópico discute os formatos de caminhos de arquivo que podem ser usados em sistemas do Windows.

## <a name="traditional-dos-paths"></a>Caminhos DOS tradicionais

Um caminho DOS padrão pode consistir em três componentes:

- Um volume ou letra da unidade seguidos pelo separador de volume (`:`).
- Um nome de diretório. O [caractere separador de diretório](<xref:System.IO.Path.DirectorySeparatorChar>) separa subdiretórios dentro da hierarquia aninhada do diretório.
- Um nome de arquivo opcional. O [caractere separador de diretório](<xref:System.IO.Path.DirectorySeparatorChar>) separa o caminho do arquivo e o nome do arquivo.

Se todos os três componentes estiverem presentes, o caminho será absoluto. Se nenhum volume ou letra da unidade for especificado e o nome do diretório começar com o [caractere separador de diretório](<xref:System.IO.Path.DirectorySeparatorChar>), o caminho será relativo na raiz da unidade atual. Caso contrário, o caminho será relativo ao diretório atual. A tabela a seguir mostra alguns possíveis caminhos de arquivo e diretório.

|Caminho  |Descrição  |
| -- | -- |
| `C:\Documents\Newsletters\Summer2018.pdf` | Um caminho de arquivo absoluto da raiz da unidade C: |
| `\Program Files\Custom Utilities\StringFinder.exe` | Um caminho absoluto da raiz da unidade atual. |
| `2018\January.xlsx` | Um caminho relativo para um arquivo em um subdiretório do diretório atual. |
| `..\Publications\TravelBrochure.pdf` | Um caminho relativo para um arquivo em um diretório par do diretório atual. |
| `C:\Projects\apilibrary\apilibrary.sln` | Um caminho absoluto para um arquivo na raiz da unidade C: |
| `C:Projects\apilibrary\apilibrary.sln` | Um caminho relativo do diretório atual da unidade C:. |

> [!IMPORTANT]
> Observe a diferença entre os últimos dois caminhos. Ambos especificam o especificador de volume opcional (C: em ambos os casos), mas o primeiro começa com a raiz do volume especificado, e o segundo não. Assim, o primeiro é um caminho absoluto do diretório raiz da unidade C:, ao passo que o segundo é um caminho relativo do diretório atual da unidade C:. Uso do segundo formulário quando o primeiro é uma fonte comum de bugs que envolvem caminhos de arquivo do Windows.

É possível determinar se um caminho de arquivo é totalmente qualificado (ou seja, se o caminho é independente do diretório atual e não se altera quando o diretório atual é alterado) chamando o método <xref:System.IO.Path.IsPathFullyQualified%2A?displayProperty=nameWthType>. Esse tipo de caminho poderá incluir segmentos de diretório relativo (`.` e `..`) e ainda ser totalmente qualificado se o caminho resolvido sempre apontar para o mesmo local.

O exemplo a seguir ilustra a diferença entre caminhos absolutos e relativos. Ele assume que o diretório D:\FY2018\ existe e que você não definiu nenhum diretório atual para D:\ no prompt de comando antes de executar o exemplo.

[!code-csharp[absolute-and-relative-paths](~/samples/snippets/standard/io/file-names/cs/paths.cs)]
[!code-vb[absolute-and-relative-paths](~/samples/snippets/standard/io/file-names/vb/paths.vb)]

[!INCLUDE [localized code comments](../../../includes/code-comments-loc.md)]

## <a name="unc-paths"></a>Caminhos UNC

Os caminhos UNC (convenção de nomenclatura universal), usados para acessar recursos de rede, têm o seguinte formato:

- Um nome do host ou servidor, que é precedido por \\\\. O nome do servidor pode ser um nome de computador NetBIOS ou um endereço IP/FQDN (IPv4 e v6 são compatíveis).
- Um nome do compartilhamento, separado do nome do host por \\. Juntos, o servidor e o nome do compartilhamento compõem o volume.
- Um nome de diretório. O [caractere separador de diretório](<xref:System.IO.Path.DirectorySeparatorChar>) separa subdiretórios dentro da hierarquia aninhada do diretório.
- Um nome de arquivo opcional. O [caractere separador de diretório](<xref:System.IO.Path.DirectorySeparatorChar>) separa o caminho do arquivo e o nome do arquivo.

Veja alguns exemplos de caminhos UNC:

|Caminho  |Descrição  |
| -- | -- |
| `\\system07\C$\` | O diretório raiz da unidade C: em `system07`. |
| `\\Server2\Share\Test\Foo.txt` | O arquivo Foo.txt no diretório Teste do volume \\\\Server2\\Share.|

Caminhos UNC devem sempre ser totalmente qualificados. Podem incluir segmentos de diretório relativo (`.` e `..`), mas esses precisam ser parte de um caminho totalmente qualificado. É possível usar caminhos relativos somente mapeando um caminho UNC para uma letra da unidade.

## <a name="dos-device-paths"></a>Caminhos de dispositivo DOS

O sistema operacional Windows tem um modelo de objeto unificado que aponta para todos os recursos, incluindo arquivos. Esses caminhos de objeto podem ser acessados na janela do console e estão expostos à camada Win32 por meio de uma pasta especial de links simbólicos para as quais o DOS herdado e os caminhos UNC estão mapeados. Essa pasta especial é acessada pela sintaxe do caminho de dispositivo DOS, que é uma das opções a seguir:

`\\.\C:\Test\Foo.txt`
`\\?\C:\Test\Foo.txt`

Além de identificar uma unidade pela letra, você pode identificar um volume usando a GUID do volume. Ela assume o formato:

`\\.\Volume{b75e2c83-0000-0000-0000-602f00000000}\Test\Foo.txt`
`\\?\Volume{b75e2c83-0000-0000-0000-602f00000000}\Test\Foo.txt`

> [!NOTE]
> A sintaxe do caminho de dispositivo DOS é compatível com implementações do .NET executadas no Windows, a partir do .NET Core 1.1 e .NET Framework 4.6.2.

O caminho de dispositivo DOS tem os seguintes componentes:

- O especificador de caminho do dispositivo (`\\.\` ou `\\?\`), que identifica o caminho como um caminho de dispositivo DOS.

   > [!NOTE]
   > O `\\?\` é compatível com todas as versões do .NET Core e, no .NET Framework, a partir da versão 4.6.2.

- Um link simbólico para o objeto de dispositivo "real" (C: no caso de um nome de unidade ou Volume{b75e2c83-0000-0000-0000-602f00000000} no caso de um GUID de volume).

   O primeiro segmento do caminho de dispositivo DOS depois do especificador de caminho do dispositivo identifica o volume ou a unidade. Por exemplo, `\\?\C:\` e `\\.\BootPartition\`.

   Há um link específico para UNCs que é chamado, não surpreendentemente, `UNC`. Por exemplo:

  `\\.\UNC\Server\Share\Test\Foo.txt`
  `\\?\UNC\Server\Share\Test\Foo.txt`

    Para UNCs de dispositivo, a parte do servidor/compartilhamento forma o volume. Por exemplo, no `\\?\server1\e:\utilities\\filecomparer\`, a parte do servidor/compartilhamento é server1\utilities. Isso é importante ao chamar um método como <xref:System.IO.Path.GetFullPath(System.String,System.String)?displayProperty=nameWithType> com segmentos de diretório relativo. Não é possível navegar além desse volume.

Caminhos de dispositivo DOS são totalmente qualificados por definição. Os segmento de diretório relativo (`.` e `..`) não são permitidos. Os diretórios atuais nunca são inseridos no uso deles.

## <a name="example-ways-to-refer-to-the-same-file"></a>Exemplo: maneiras de se referir ao mesmo arquivo

O exemplo a seguir mostra algumas maneiras de se referir ao arquivo ao usar as APIs do namespace <xref:System.IO>. O exemplo cria uma instância de um objeto <xref:System.IO.FileInfo> e usa suas propriedades <xref:System.IO.FileInfo.Name> e <xref:System.IO.FileInfo.Length> para exibir o nome do arquivo e seu tamanho.

[!code-csharp[referring-to-the-same-file](~/samples/snippets/standard/io/file-names/cs/file-refs.cs)]
[!code-vb[referring-to-the-same-file](~/samples/snippets/standard/io/file-names/vb/file-refs.vb)]

## <a name="path-normalization"></a>Normalização de caminho

Quase todos os caminhos passados para APIs do Windows são normalizados. Durante a normalização, o Windows executa as seguintes etapas:

- Identifica o caminho.
- Aplica o diretório atual a caminhos (relativos) parcialmente qualificados.
- Canoniza os separadores de diretório e componente.
- Avalia os componentes do diretório relativo (`.` para o diretório atual e `..` para o diretório pai).
- Corta alguns caracteres.

Essa normalização ocorre de maneira implícita, mas é possível fazê-la explicitamente chamando o método <xref:System.IO.Path.GetFullPath%2A?displayProperty=nameWithType>, que encapsula uma chamada para a [função GetFullPathName()](/windows/desktop/api/fileapi/nf-fileapi-getfullpathnamea). Também é possível chamar a [função GetFullPathName()](/windows/desktop/api/fileapi/nf-fileapi-getfullpathnamea) do Windows diretamente usando P/Invoke.

### <a name="identify-the-path"></a>Identificar o caminho

O primeiro passo na normalização do caminho é identificar o tipo do caminho. Os caminhos são classificados em uma dentre algumas categorias:

- São caminhos de dispositivo, ou seja, começam com dois separadores e um ponto de interrogação ou ponto final (`\\?` ou `\\.`).
- São caminhos UNC, ou seja, começam com dois separadores sem um ponto de interrogação ou ponto final.
- São caminhos DOS totalmente qualificados, ou seja, começam com uma letra da unidade, um separador de volume e um separador de componente (`C:\`).
- Designam um dispositivo herdado (`CON`, `LPT1`).
- Estão relacionados com a raiz da unidade atual, ou seja, começam com um separador de componente único (`\`).
- Estão relacionados com o diretório atual de uma unidade especificada, ou seja, começam com uma letra da unidade, um separador de volume e sem um separador de componente (`C:`).
- Estão relacionados com o diretório atual, ou seja, começam com qualquer outra coisa (`temp\testfile.txt`).

O tipo do caminho determina se o diretório atual é aplicado de alguma maneira ou não. Também determina qual é a "raiz" do caminho.

### <a name="handle-legacy-devices"></a>Manipular dispositivos herdados

Se o caminho for um dispositivo DOS herdado como `CON`, `COM1` ou `LPT1`, será convertido em um caminho de dispositivo pelo `\\.\` precedente e retornado.

Um caminho que começa com um nome do dispositivo herdado sempre é interpretado como um dispositivo herdado pelo método <xref:System.IO.Path.GetFullPath(System.String)?displayProperty=nameWithType>. Por exemplo, o caminho de dispositivo DOS de `CON.TXT` é `\\.\CON`, e o caminho de dispositivo DOS de `COM1.TXT\file1.txt` é `\\.\COM1`.

### <a name="apply-the-current-directory"></a>Aplicar o diretório atual

Se o caminho não for totalmente qualificado, o Windows aplicará o diretório atual a ele. O diretório atual não foi aplicado a UNCs e caminhos de dispositivo. Nem a unidade total com o separador C:\\.

Se o caminho começar com um único separador de componente, a unidade do diretório atual será aplicada. Por exemplo, se o caminho do arquivo for `\utilities` e o diretório atual for `C:\temp\`, a normalização produzirá `C:\utilities`.

Se o caminho começar com uma letra da unidade, um separador de volume e sem um separador de componente, o último diretório atual definido do shell de comando para a unidade especificada será aplicado. Se o último diretório atual não tiver sido definido, a unidade isolada será aplicada. Por exemplo, se o caminho de arquivo for `D:sources`, o diretório atual for `C:\Documents\` e o último diretório atual na unidade D: for `D:\sources\`, o resultado será `D:\sources\sources`. Esses caminhos "relativos à unidade" são uma fonte comum de erros lógicos de script e programas. Assumir que um caminho iniciado com uma letra e dois-pontos não é relativo é claramente incorreto.

Se o caminho começar com algo diferente de um separador, a unidade e o diretório atuais serão aplicados. Por exemplo, se o caminho for `filecompare` e o diretório atual for `C:\utilities\`, o resultado será `C:\utilities\filecompare\`.

> [!IMPORTANT]
> Os caminhos relativos são perigosos em aplicativos multi-threaded (ou seja, a maioria), porque o diretório atual tem uma configuração por processo. Qualquer thread pode alterar o diretório atual a qualquer momento. A partir do .NET Core 2.1, é possível chamar o método <xref:System.IO.Path.GetFullPath(System.String,System.String)?displayProperty=nameWithType> para obter um caminho absoluto de um caminho relativo, bem como o caminho base (o diretório atual) que você precisa para resolvê-lo.

### <a name="canonicalize-separators"></a>Tornar os separadores canônicos

Todas as barras (`/`) são convertidas no separador padrão do Windows, a barra invertida (`\`). Se estiverem presentes, uma série de barras que segue as duas primeiras barras serão ocultadas e exibidas como uma barra só.

### <a name="evaluate-relative-components"></a>Avaliar componentes relativos

Conforme o caminho é processado, quaisquer componentes ou segmentos compostos de um ponto final ou ponto duplo (`.` ou `..`) serão avaliados:

- No caso do ponto final, o segmento atual será removido, pois se refere ao diretório atual.

- No caso do ponto duplo, o segmento atual e o segmento pai serão removidos, pois o ponto duplo se refere ao diretório pai.

   Os diretórios pais só serão removidos se não forem maiores que a raiz do caminho. A raiz do caminho depende do tipo do caminho. É a unidade (`C:\`) dos caminhos DOS, o servidor/compartilhamento de UNCs (`\\Server\Share`) e o prefixo do caminho de dispositivo dos caminhos de dispositivo (`\\?\` ou `\\.\`).

### <a name="trim-characters"></a>Caracteres de corte

Alguns outros caracteres são removidos durante a normalização junto com os separadores e segmentos relativos removidos anteriormente:

- Se um segmento terminar em ponto final, esse ponto final será removido. Um segmento com ponto final ou ponto duplo foi normalizado na etapa anterior. Um segmento com três ou mais pontos não será normalizado e, na verdade, é um nome de arquivo/diretório válido.

- Se o caminho não terminar em um separador, todos os pontos e espaços à direita (U+0020) serão removidos. Se o último segmento for simplesmente um ponto final ou duplo, será aplicada a regra de componentes relativos já mencionada.

   Essa regra significa que é possível criar um nome de diretório com um espaço à direita ao adicionar um separador à direita após o espaço.

   > [!IMPORTANT]
   > **Nunca** crie um diretório ou nome de arquivo com um espaço à direita. Os espaços à direita podem dificultar ou impossibilitar o acesso ao diretório e, normalmente, os aplicativos apresentam falha nas tentativas de tratar diretórios ou arquivos com nomes que contêm espaços à direita.

## <a name="skip-normalization"></a>Ignorar normalização

Normalmente, qualquer caminho passado para uma API do Windows é (efetivamente) passado para a [função GetFullPathName](/windows/desktop/api/fileapi/nf-fileapi-getfullpathnamea) e normalizado. Há uma exceção importante: um caminho de dispositivo que começa com um ponto de interrogação em vez de um ponto final. A menos que o caminho comece exatamente com `\\?\` (observe o uso da barra invertida canônica), ele é normalizado.

Por que ignorar a normalização? Existem três motivos principais:

1. Para ter acesso a caminhos normalmente não disponíveis, mas legais. Um arquivo ou diretório chamado `hidden.`, por exemplo, não pode ser acessado de outra maneira.

1. Para melhorar o desempenho ignorando a normalização, se você já tiver normalizado.

1. Somente no .NET Framework, ignorar a verificação `MAX_PATH` do tamanho do caminho para permitir caminhos com mais de 259 caracteres. A maioria das APIs permitem isso, com algumas exceções.

> [!NOTE]
> O .NET Core trata caminhos longos de maneira implícita e não executa uma verificação `MAX_PATH`. A verificação `MAX_PATH` se aplica somente ao .NET Framework.

Ignorar a normalização e as verificações de tamanho do caminho é a única diferença entre as duas sintaxes de caminho de dispositivo. Caso contrário, elas serão idênticas. Tenha cuidado ao ignorar a normalização, pois é fácil criar caminhos de difícil tratamento para aplicativos "normais".

Os caminhos que começam com `\\?\` ainda serão normalizados se você os passar explicitamente para a [função GetFullPathName](/windows/desktop/api/fileapi/nf-fileapi-getfullpathnamea).

Você pode passar caminhos com mais de `MAX_PATH` caracteres para [getfullpathname](/windows/desktop/api/fileapi/nf-fileapi-getfullpathnamea) sem `\\?\` . Ele dá suporte caminhos de tamanho arbitrário, até o tamanho máximo da cadeia de caracteres que o Windows consegue tratar.

## <a name="case-and-the-windows-file-system"></a>Maiúsculas, minúsculas e o sistema de arquivos do Windows

Uma peculiaridade do sistema de arquivos do Windows que usuários e desenvolvedores que não o utilizam consideram confusa é que os nomes do caminho e do diretório não diferenciam maiúsculas de minúsculas. Isto é, os nomes do caminho e do diretório refletem as cadeias de caracteres utilizadas no momento da criação. Por exemplo, a chamada de método

```csharp
Directory.Create("TeStDiReCtOrY");
```

```vb
Directory.Create("TeStDiReCtOrY")
```

cria um diretório chamado TeStDiReCtOrY. Se você renomear um diretório ou arquivo para alterar as maiúsculas e minúsculas, o nome do diretório ou do arquivo refletirá a cadeia de caracteres usadas ao renomeá-los. Por exemplo, o código a seguir renomeia o arquivo test.txt como Test.txt:

[!code-csharp[case-and-renaming](~/samples/snippets/standard/io/file-names/cs/rename.cs)]
[!code-vb[case-and-renaming](~/samples/snippets/standard/io/file-names/vb/rename.vb)]

No entanto, as comparações entre o nome do diretório e do arquivo não diferenciam maiúsculas e minúsculas. Se você pesquisar por um arquivo com o nome "test.txt", as APIs do sistema de arquivos do .NET ignorarão a comparação entre maiúsculas e minúsculas. "Test.txt", "TEST.TXT", "test.TXT" e qualquer outra combinação de letras maiúsculas e minúsculas corresponderá a "test.txt".
