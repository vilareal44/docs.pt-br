---
ms.openlocfilehash: 358103d5816eb61c88738e9626fb02c563b507f8
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617115"
---
### <a name="workflow-30-types-are-obsolete"></a>Os tipos do WorkFlow 3.0 estão obsoletos

#### <a name="details"></a>Detalhes

As APIs (aquelas do namespace System.Workflow) do WWF (Windows Workflow Foundation) 3.0 agora estão obsoletas.

#### <a name="suggestion"></a>Sugestão

Agora devem ser usadas as novas APIs (em System.Activities) do WWF 4.0. Um exemplo de como usar as novas APIs pode ser encontrado [aqui](~/docs/framework/windows-workflow-foundation/how-to-update-the-definition-of-a-running-workflow-instance.md), e outras diretrizes estão disponíveis [aqui](https://docs.microsoft.com/archive/blogs/workflowteam/wf3-types-marked-obsolete-in-net-4-5). Como alternativa, uma vez que as APIs do WWF 3.0 ainda têm suporte, elas poderão ser usadas, e o aviso de hora da compilação deve ser evitado suprimindo-o ou usando um compilador mais antigo.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Principal       |
| Versão | 4.5         |
| Type    | Redirecionando |
