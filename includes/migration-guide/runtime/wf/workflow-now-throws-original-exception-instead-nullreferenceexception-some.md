---
ms.openlocfilehash: ae0c7322b7415157838278b5568e6e49936e9a93
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621022"
---
### <a name="workflow-now-throws-original-exception-instead-of-nullreferenceexception-in-some-cases"></a>Agora, o fluxo de trabalho gera a exceção original em vez de NullReferenceException em alguns casos

#### <a name="details"></a>Detalhes

No .NET Framework 4.6.2 e versões anteriores, quando o método Executar de uma atividade de fluxo de trabalho gera uma exceção com um valor <code>null</code> para a propriedade <xref:System.Exception.Message>, o runtime do fluxo de trabalho System.Activities gera <xref:System.NullReferenceException?displayProperty=fullName>, mascarando a exceção original. No .NET Framework 4.7, a exceção mascarada anteriormente é gerada.

#### <a name="suggestion"></a>Sugestão

Se o código depender do tratamento de <xref:System.NullReferenceException?displayProperty=fullName>, altere-o para capturar as exceções que possam ser geradas por meio das atividades personalizadas.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.7|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Activities.CodeActivity.Execute(System.Activities.CodeActivityContext)?displayProperty=nameWithType></li><li><xref:System.Activities.AsyncCodeActivity.BeginExecute(System.Activities.AsyncCodeActivityContext,System.AsyncCallback,System.Object)?displayProperty=nameWithType></li><li><xref:System.Activities.AsyncCodeActivity%601.BeginExecute(System.Activities.AsyncCodeActivityContext,System.AsyncCallback,System.Object)?displayProperty=nameWithType></li><li><xref:System.Activities.WorkflowInvoker.Invoke?displayProperty=nameWithType></li></ul>|
