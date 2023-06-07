---
ms.openlocfilehash: 3eab96149be1e40d528cfd552bbe05ca766cf4e8
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619788"
---
### <a name="systemthreadingtaskstask-no-longer-throw-objectdisposedexception-after-object-is-disposed"></a>System.Threading.Tasks.Task não gera mais ObjectDisposedException depois que o objeto é descartado

#### <a name="details"></a>Detalhes

Exceto por <xref:System.Threading.Tasks.Task.System%23IAsyncResult%23AsyncWaitHandle>, os métodos <xref:System.Threading.Tasks.Task?displayProperty=fullName>não geram mais uma exceção <xref:System.ObjectDisposedException?displayProperty=fullName> após o objeto ser descartado. Essa alteração permite o uso de tarefas em cache. Por exemplo, um método pode retornar uma tarefa em cache para representar uma operação já concluída em vez de alocar uma nova tarefa. Isso era impossível nas versões anteriores do .NET Framework porque qualquer consumidor da tarefa poderia descartá-los, o que a tornava inutilizável.

#### <a name="suggestion"></a>Sugestão

Lembre-se de que os métodos Task podem não gerar mais <xref:System.ObjectDisposedException?displayProperty=fullName> nos casos em que o objeto é descartado. Se um aplicativo dependia dessa exceção para saber que uma tarefa foi descartada, ele deverá ser atualizado para verificar explicitamente o status da tarefa usando <xref:System.Threading.Tasks.Task.Status>.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.5|
|Type|Runtime|
