---
ms.openlocfilehash: ddc98448101c65003001ad05e67f29d75d99ad44
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85616016"
---
### <a name="workflowdesignerload-doesnt-remove-symbol-property"></a>WorkflowDesigner.Load não remove a propriedade de símbolo

#### <a name="details"></a>Detalhes

Ao destinar para o .NET Framework 4.5 no designer de fluxo de trabalho e carregar um fluxo de trabalho 3.5 hospedado novamente com o método <xref:System.Activities.Presentation.WorkflowDesigner.Load>, uma <xref:System.Xaml.XamlDuplicateMemberException?displayProperty=fullName> é gerada durante o salvamento do fluxo de trabalho.

#### <a name="suggestion"></a>Sugestão

Esse bug se manifesta somente no direcionamento do .NET Framework 4.5 no designer de fluxo de trabalho, de modo que a solução alternativa é definir o `WorkflowDesigner.Context.Services.GetService&lt;DesignerConfigurationService&gt;().TargetFrameworkName` para o .NET Framework 4.0. O problema também pode ser evitado usando o método <xref:System.Activities.Presentation.WorkflowDesigner.Load(System.String)> para carregar o fluxo de trabalho, em vez de <xref:System.Activities.Presentation.WorkflowDesigner.Load>.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Principal       |
| Versão | 4.5         |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Activities.Presentation.WorkflowDesigner.Load?displayProperty=nameWithType>
