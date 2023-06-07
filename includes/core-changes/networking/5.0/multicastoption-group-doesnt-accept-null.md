---
ms.openlocfilehash: 88a0b7e04c7015ca3fa5abd8a6897dafcbe41c49
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88557953"
---
### <a name="multicastoptiongroup-doesnt-accept-a-null-value"></a>MulticastOption. Group não aceita um valor nulo

<xref:System.Net.Sockets.MulticastOption.Group?displayProperty=nameWithType> Não aceita mais um valor de `null` . Se você definir a propriedade como `null` , um <xref:System.ArgumentNullException> será gerado.

#### <a name="version-introduced"></a>Versão introduzida

5.0

#### <a name="change-description"></a>Descrição da alteração

Nas versões anteriores do .NET, você pode definir a <xref:System.Net.Sockets.MulticastOption.Group?displayProperty=nameWithType> propriedade como `null` . Se o <xref:System.Net.Sockets.MulticastOption> for passado posteriormente para <xref:System.Net.Sockets.Socket.SetSocketOption%2A?displayProperty=nameWithType> , o tempo de execução lançará um <xref:System.NullReferenceException> .

No .NET 5,0 e posterior, um <xref:System.ArgumentNullException> será gerado se você definir a propriedade como `null` .

#### <a name="reason-for-change"></a>Motivo da alteração

Para ser consistente com as diretrizes de design de estrutura, a propriedade foi atualizada para lançar um <xref:System.ArgumentNullException> se o valor for `null` .

#### <a name="recommended-action"></a>Ação recomendada

Certifique-se de que você não definiu <xref:System.Net.Sockets.MulticastOption.Group?displayProperty=nameWithType> como `null` .

#### <a name="category"></a>Categoria

Rede

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Net.Sockets.MulticastOption.Group?displayProperty=fullName>

<!--

#### Affected APIs

- `P:System.Net.Sockets.MulticastOption.Group`

-->
