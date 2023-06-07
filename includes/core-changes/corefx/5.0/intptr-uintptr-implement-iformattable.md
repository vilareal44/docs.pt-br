---
ms.openlocfilehash: 3d29848e12d496d2d53c204e00ef8604831495e1
ms.sourcegitcommit: 1e6439ec4d5889fc08cf3bfb4dac2b91931eb827
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/08/2020
ms.locfileid: "88024687"
---
### <a name="intptr-and-uintptr-implement-iformattable"></a>IntPtr e UIntPtr implementam IFormattable

<xref:System.IntPtr>e <xref:System.UIntPtr> agora implemente <xref:System.IFormattable> . As funções que verificam o <xref:System.IFormattable> suporte agora podem retornar resultados diferentes para esses tipos, pois eles podem passar um especificador de formato e uma cultura.

#### <a name="change-description"></a>Descrição da alteração

Em versões anteriores do .NET, <xref:System.IntPtr> e <xref:System.UIntPtr> não implemente <xref:System.IFormattable> . As funções que verificam <xref:System.IFormattable> podem retornar a apenas chamar <xref:System.IntPtr.ToString%2A?displayProperty=nameWithType> ou <xref:System.UIntPtr.ToString%2A?displayProperty=nameWithType> , o que significa que especificadores de formato e culturas não são respeitados.

No .NET 5,0 e versões posteriores, <xref:System.IntPtr> e <xref:System.UIntPtr> implemente <xref:System.IFormattable> . As funções que verificam o <xref:System.IFormattable> suporte agora podem retornar resultados diferentes para esses tipos, pois eles podem passar um especificador de formato e uma cultura.

Essa alteração afeta cenários como cadeias de caracteres interpoladas e <xref:System.Console.WriteLine%2A?displayProperty=nameWithType> , entre outros.

#### <a name="reason-for-change"></a>Motivo da alteração

<xref:System.IntPtr>e <xref:System.UIntPtr> agora tem suporte a idiomas em C# por meio das `nint` `nuint` palavras-chave e. Os tipos de suporte foram atualizados para fornecer paridade próxima (quando possível) com a funcionalidade exposta por outros tipos primitivos, como <xref:System.Int32?displayProperty=nameWithType> .

#### <a name="version-introduced"></a>Versão introduzida

5,0 versão prévia 4

#### <a name="recommended-action"></a>Ação recomendada

Se você não quiser que um especificador de formato ou cultura personalizada seja usado ao exibir valores desses tipos, você pode chamar o <xref:System.IntPtr.ToString?displayProperty=nameWithType> e <xref:System.UIntPtr.ToString?displayProperty=nameWithType> sobrecargas de `ToString()` .

#### <a name="category"></a>Categoria

Bibliotecas principais do .NET

#### <a name="affected-apis"></a>APIs afetadas

Não detectável via análise de API.

<!--

#### Affected APIs

Not detectable via API analysis.

-->
