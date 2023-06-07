---
ms.openlocfilehash: 4bc060f991501b81db0cb7c521e55996a2b5e91e
ms.sourcegitcommit: 97ce5363efa88179dd76e09de0103a500ca9b659
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2020
ms.locfileid: "86281285"
---
### <a name="behavior-change-for-vector2lerp-and-vector4lerp"></a>Alteração de comportamento para vector2. Lerp e Vector4. Lerp

A implementação de <xref:System.Numerics.Vector2.Lerp(System.Numerics.Vector2,System.Numerics.Vector2,System.Single)?displayProperty=nameWithType> e <xref:System.Numerics.Vector4.Lerp(System.Numerics.Vector4,System.Numerics.Vector4,System.Single)?displayProperty=nameWithType> mudou para uma conta correta para um erro de arredondamento de ponto flutuante.

#### <a name="change-description"></a>Descrição da alteração

Anteriormente, <xref:System.Numerics.Vector2.Lerp(System.Numerics.Vector2,System.Numerics.Vector2,System.Single)?displayProperty=nameWithType> e <xref:System.Numerics.Vector4.Lerp(System.Numerics.Vector4,System.Numerics.Vector4,System.Single)?displayProperty=nameWithType> foram implementados como `value1 + (value2 - value1) * amount` . No entanto, devido a um erro de arredondamento de ponto flutuante, esse algoritmo nem sempre retorna `value2` quando `amount` é `1.0f` .

No .NET 5,0 e posterior, a implementação usa o mesmo algoritmo que <xref:System.Numerics.Vector3.Lerp(System.Numerics.Vector3,System.Numerics.Vector3,System.Single)?displayProperty=nameWithType> , que é `(value1 * (1.0f - amount)) + (value2 * amount)` . Esse algoritmo conta corretamente para o erro de arredondamento. Agora, quando `amount` é `1.0f` , o resultado é precisamente `value2` . O algoritmo atualizado também permite que o algoritmo seja otimizado livremente usando <xref:System.MathF.FusedMultiplyAdd%2A?displayProperty=nameWithType> quando ele está disponível.

#### <a name="version-introduced"></a>Versão introduzida

5,0 Preview 7

#### <a name="recommended-action"></a>Ação recomendada

Nenhuma ação é necessária. No entanto, se você quiser manter o comportamento antigo, poderá implementar sua própria `Lerp` função que usa o algoritmo anterior do `value1 + (value2 - value1) * amount` .

#### <a name="category"></a>Categoria

Bibliotecas principais do .NET

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Numerics.Vector2.Lerp(System.Numerics.Vector2,System.Numerics.Vector2,System.Single)?displayProperty=fullName>
- <xref:System.Numerics.Vector4.Lerp(System.Numerics.Vector4,System.Numerics.Vector4,System.Single)?displayProperty=fullName>

<!--

#### Affected APIs

- `M:System.Numerics.Vector2.Lerp(System.Numerics.Vector2,System.Numerics.Vector2,System.Single)`
- `M:System.Numerics.Vector4.Lerp(System.Numerics.Vector4,System.Numerics.Vector4,System.Single)`

-->
