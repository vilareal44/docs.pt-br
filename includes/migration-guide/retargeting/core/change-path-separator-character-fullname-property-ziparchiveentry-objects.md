---
ms.openlocfilehash: f87d0dbeda6094bd745f5b580772b1161f30d0d5
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86218242"
---
### <a name="change-in-path-separator-character-in-fullname-property-of-ziparchiveentry-objects"></a>Alteração no caractere separador de caminho na propriedade FullName de objetos ZipArchiveEntry

#### <a name="details"></a>Detalhes

Para aplicativos direcionados para o .NET Framework 4.6.1 e versões posteriores, o caractere separador de caminho mudou de uma barra invertida (" \\ ") para uma barra "/" ("/") na <xref:System.IO.Compression.ZipArchiveEntry.FullName> propriedade de <xref:System.IO.Compression.ZipArchiveEntry> objetos criados por sobrecargas do <xref:System.IO.Compression.ZipFile.CreateFromDirectory%2A> método. A alteração traz a implementação do .NET em conformidade com a seção 4.4.17.1 da [Especificação de formato de arquivo .ZIP](https://pkware.cachefly.net/webdocs/casestudies/APPNOTE.TXT) e permite que arquivamentos .ZIP sejam descompactados em sistemas não Windows.<br />A descompactação de um arquivo zip criado por um aplicativo direcionado a uma versão anterior do .NET Framework em sistemas operacionais não Windows, como o Macintosh, falha na preservação da estrutura de diretório. Por exemplo, no Macintosh, ela cria um conjunto de arquivos cujo nome de arquivo concatena o caminho do diretório com qualquer caractere de barra invertida ("\\") e o nome do arquivo. Consequentemente, a estrutura do diretório de arquivos descompactados não é preservada.

#### <a name="suggestion"></a>Sugestão

O impacto dessa alteração em. Os arquivos ZIP que são descompactados no sistema operacional Windows por APIs no <xref:System.IO?displayProperty=nameWithType> namespace .NET Framework devem ser mínimos, uma vez que essas APIs podem lidar diretamente com uma barra ("/") ou uma barra invertida (" \\ ") como o caractere separador de caminho.<br />Se essa alteração for indesejável, você poderá recusá-la adicionando um parâmetro de configuração à [`<runtime>`](~/docs/framework/configure-apps/file-schema/runtime/runtime-element.md) seção do arquivo de configuração do aplicativo. O exemplo a seguir mostra a seção `<runtime>` e a opção de recusa `Switch.System.IO.Compression.ZipFile.UseBackslash`:

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.IO.Compression.ZipFile.UseBackslash=true" />
</runtime>
```

Além disso, os aplicativos que se destinam a versões anteriores do .NET Framework mas que estão em execução no .NET Framework 4.6.1 e versões posteriores podem aceitar esse comportamento adicionando um parâmetro de configuração à [`<runtime>`](~/docs/framework/configure-apps/file-schema/runtime/runtime-element.md) seção do arquivo de configuração do aplicativo. Veja a seguir a seção `<runtime>` e a opção de aceitação `Switch.System.IO.Compression.ZipFile.UseBackslash`.

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.IO.Compression.ZipFile.UseBackslash=false" />
</runtime>
```

| Nome    | Valor       |
|:--------|:------------|
| Escopo   | Edge        |
| Versão | 4.6.1       |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.IO.Compression.ZipFile.CreateFromDirectory(System.String,System.String)?displayProperty=nameWithType>
- <xref:System.IO.Compression.ZipFile.CreateFromDirectory(System.String,System.String,System.IO.Compression.CompressionLevel,System.Boolean)?displayProperty=nameWithType>
- <xref:System.IO.Compression.ZipFile.CreateFromDirectory(System.String,System.String,System.IO.Compression.CompressionLevel,System.Boolean,System.Text.Encoding)?displayProperty=nameWithType>
