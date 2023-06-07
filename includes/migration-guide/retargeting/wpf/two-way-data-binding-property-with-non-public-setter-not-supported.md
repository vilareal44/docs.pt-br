---
ms.openlocfilehash: 5f1a8af37a305ab0904801002dd99e17e8eca62e
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85616021"
---
### <a name="two-way-data-binding-to-a-property-with-a-non-public-setter-is-not-supported"></a>Não há suporte para a associação de dados bidirecionais para uma propriedade com um setter não público

#### <a name="details"></a>Detalhes

A tentativa de associar dados a uma propriedade sem um setter público nunca foi um cenário com suporte. A partir do .NET Framework 4.5.1, esse cenário vai gerar uma <xref:System.InvalidOperationException?displayProperty=fullName>. Observe que essa nova exceção será gerada somente para aplicativos destinados especificamente ao .NET Framework 4.5.1. Se um aplicativo for destinado ao .NET Framework 4.5, a chamada será permitida. Se o aplicativo não for destinado a uma versão específica do .NET Framework, a associação será tratada como unidirecional.

#### <a name="suggestion"></a>Sugestão

O aplicativo deve ser atualizado para usar a associação unidirecional ou expor publicamente o setter da propriedade. Como alternativa, o direcionamento ao .NET Framework 4.5 fará com que o aplicativo demonstre o comportamento antigo.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.5.1       |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Windows.Data.BindingMode.TwoWay?displayProperty=nameWithType>
