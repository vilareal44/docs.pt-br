---
ms.openlocfilehash: b6cb7edcd6bed50efdf59f3321320ac8cd1b1ab8
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617111"
---
### <a name="some-workflow-drag-and-drop-apis-are-obsolete"></a>Algumas APIs arrastar e soltar do Fluxo de Trabalho estão obsoletas

#### <a name="details"></a>Detalhes

A API Arrastar/Soltar do Fluxo de Trabalho está obsoleta e vai gerar avisos do compilador se o aplicativo for recompilado na versão 4.5.

#### <a name="suggestion"></a>Sugestão

No lugar, devem ser usadas as novas APIs <xref:System.Activities.Presentation.DragDropHelper?displayProperty=fullName> compatíveis com operações com vários objetos. Como alternativa, os avisos de compilação podem ser suprimidos ou evitados usando um compilador mais antigo. Ainda há suporte para as APIs.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.5         |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Activities.Presentation.DragDropHelper.DoDragMove(System.Activities.Presentation.WorkflowViewElement,System.Windows.Point)?displayProperty=nameWithType>
- <xref:System.Activities.Presentation.DragDropHelper.GetCompositeView(System.Windows.DragEventArgs)?displayProperty=nameWithType>
- <xref:System.Activities.Presentation.DragDropHelper.GetDraggedModelItem(System.Windows.DragEventArgs)?displayProperty=nameWithType>
- <xref:System.Activities.Presentation.DragDropHelper.GetDroppedObject(System.Windows.DependencyObject,System.Windows.DragEventArgs,System.Activities.Presentation.EditingContext)?displayProperty=nameWithType>
