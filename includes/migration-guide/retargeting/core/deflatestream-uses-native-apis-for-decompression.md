---
ms.openlocfilehash: 7e42a294b151d07a6fdc61308cdf92df7a31a1d6
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614284"
---
### <a name="deflatestream-uses-native-apis-for-decompression"></a>O DeflateStream usa APIs nativas para descompactação

#### <a name="details"></a>Detalhes

Começando com o .NET Framework 4.7.2, a implementação da descompactação na classe `T:System.IO.Compression.DeflateStream` foi alterada para usar APIs nativas do Windows por padrão. Normalmente, isso resulta em uma melhoria significativa de desempenho. Todos os aplicativos .NET direcionados ao .NET Framework versão 4.7.2 ou superior usam a implementação nativo. Essa alteração pode resultar em algumas diferenças no comportamento, que incluem:

- As mensagens de exceção podem ser diferentes. No entanto, o tipo de exceção gerada permanece o mesmo.
- Algumas situações especiais, como não ter memória suficiente para concluir uma operação, podem ser tratadas de maneira diferente.
- Existem diferenças conhecidas para analisar o cabeçalho gzip (observação: somente o `GZipStream` definido para descompactação é afetado):
- As exceções durante a análise de cabeçalhos inválidos podem ser geradas em momentos diferentes.
- A implementação nativa impõe que os valores de alguns sinalizadores reservados dentro do cabeçalho gzip (ou seja, [FLG](http://www.zlib.org/rfc-gzip.html#header-trailer)) são definidos de acordo com a especificação, o que pode causar a geração de uma exceção em casos em que, anteriormente, valores inválidos eram ignorados.

#### <a name="suggestion"></a>Sugestão

Se a descompactação com as APIs nativas tiver prejudicado o comportamento do aplicativo, você poderá recusar esse recurso adicionando a opção `Switch.System.IO.Compression.DoNotUseNativeZipLibraryForDecompression` à seção `runtime` do arquivo app.config e configurando-a como `true`:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <runtime>
    <AppContextSwitchOverrides value="Switch.System.IO.Compression.DoNotUseNativeZipLibraryForDecompression=true" />
  </runtime>
</configuration>
```

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.7.2       |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.IO.Compression.DeflateStream?displayProperty=nameWithType>
- <xref:System.IO.Compression.GZipStream?displayProperty=nameWithType>
