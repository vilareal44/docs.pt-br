---
ms.openlocfilehash: 43ebb37124b8efe077b9f81fe5351bd7db2c72f7
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87302694"
---
### <a name="vectort-always-throws-notsupportedexception-for-unsupported-types"></a>Vetor \<T> sempre gera NotSupportedException para tipos sem suporte

<xref:System.Numerics.Vector%601?displayProperty=nameWithType>Agora sempre gera um <xref:System.NotSupportedException> para parâmetros de tipo sem suporte.

#### <a name="change-description"></a>Descrição da alteração

Anteriormente, os membros de <xref:System.Numerics.Vector%601> nem sempre geraram um <xref:System.NotSupportedException> quando `T` era um [tipo sem suporte](#unsupported-types). A exceção nem sempre foi acionada devido a caminhos de código com suporte para aceleração de hardware. Por exemplo, `Vector<bool> + Vector<bool>` retornado `default` em vez de lançar uma exceção em plataformas que não têm aceleração de hardware, como ARM32. Para tipos sem suporte, <xref:System.Numerics.Vector%601> os membros exibiram comportamento inconsistente em diferentes plataformas e configurações de hardware.

A partir do .NET 5,0, <xref:System.Numerics.Vector%601> os membros sempre lançam um <xref:System.NotSupportedException> em todas as configurações de hardware quando `T` não é um tipo com suporte.

##### <a name="unsupported-types"></a>Tipos sem suporte

Os tipos com suporte para o parâmetro de tipo de <xref:System.Numerics.Vector%601> são:

- `byte`
- `sbyte`
- `short`
- `ushort`
- `int`
- `uint`
- `long`
- `ulong`
- `float`
- `double`

Os tipos com suporte não foram alterados, no entanto, eles podem ser alterados no futuro.

#### <a name="version-introduced"></a>Versão introduzida

5,0 Preview 8

#### <a name="recommended-action"></a>Ação recomendada

Não use um tipo sem suporte para o parâmetro de tipo de <xref:System.Numerics.Vector%601> .

#### <a name="category"></a>Categoria

Bibliotecas principais do .NET

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Numerics.Vector%601?displayProperty=fullName>e todos os seus membros

<!--

#### Affected APIs

- ``T:System.Numerics.Vector`1``

-->
