---
ms.openlocfilehash: daf09748e69e70ad982bcee14461b66579f3bb87
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614305"
---
### <a name="avoiding-endless-recursion-for-iworkflowinstancemanagementtransactedcancel-and-iworkflowinstancemanagementtransactedterminate"></a>Evitando a recursão infinita para IWorkflowInstanceManagement.TransactedCancel e IWorkflowInstanceManagement.TransactedTerminate

#### <a name="details"></a>Detalhes

Em algumas circunstâncias, ao usar as APIs <xref:System.ServiceModel.Activities.IWorkflowInstanceManagement.TransactedCancel%2A?displayProperty=nameWithType> ou <xref:System.ServiceModel.Activities.IWorkflowInstanceManagement.TransactedTerminate%2A?displayProperty=nameWithType> para cancelar ou terminar uma instância de serviço de fluxo de trabalho, a instância de fluxo de trabalho pode encontrar um excedente de pilha devido à recursão infinita quando o runtime do `Workflow` tenta persistir a instância de serviço como parte do processamento da solicitação. O problema ocorre se a instância do fluxo de trabalho estiver em um estado em que está aguardando a conclusão de alguma outra solicitação pendente do WCF para outro serviço. As `TransactedCancel` `TransactedTerminate` operações e criam itens de trabalho que são enfileirados para a instância do serviço de Workflow. Esses itens de trabalho não são executados como parte do processamento da solicitação `TransactedCancel/TransactedTerminate`. Como a instância de serviço do fluxo de trabalho está ocupada esperando a conclusão da outra solicitação do WCF pendente, o item de trabalho criado permanece na fila. A operação `TransactedCancel/TransactedTerminate` é concluída e o controle é retornado ao cliente. Quando a transação associada à operação `TransactedCancel/TransactedTerminate` tenta ser confirmada, ela precisa persistir o estado da instância de serviço do fluxo de trabalho. Mas como há uma solicitação de `WCF` pendente para a instância, o runtime de fluxo de trabalho não consegue persistir a instância de serviço do fluxo de trabalho e um loop de recursão infinita causa o excedente de pilha. Como `TransactedCancel` e `TransactedTerminate` apenas criam um item de trabalho na memória, o fato de que existe uma transação não tem nenhum efeito. Uma reversão da transação não descarta o item de trabalho. Para resolver esse problema, começando com o .NET Framework 4.7.2, foi introduzido um `AppSetting` que pode ser adicionado ao serviço de fluxo de trabalho `web.config/app.config` para solicitar que as transações de `TransactedCancel` e `TransactedTerminate` sejam ignoradas. Isso permite que a transação seja confirmada sem esperar que a instância do fluxo de trabalho persista. O AppSetting para esse recurso é nomeado `microsoft:WorkflowServices:IgnoreTransactionsForTransactedCancelAndTransactedTerminate` . O valor `true` indica que a transação deve ser ignorada, evitando o excedente de pilha. O valor padrão dessa AppSetting é `false`, portanto, as instâncias de serviço do fluxo de trabalho existentes não são afetadas.

#### <a name="suggestion"></a>Sugestão

Se você estiver usando o AppFabric ou outro cliente <xref:System.ServiceModel.Activities.IWorkflowInstanceManagement> e encontrar um excedente de pilha na instância de serviço do fluxo de trabalho durante a tentativa de cancelar ou terminar uma instância de fluxo de trabalho, você poderá adicionar o seguinte à seção `<appSettings>` do arquivo web.config/app.config para o serviço de fluxo de trabalho:

<pre><code class="lang-xml">&lt;add key=&quot;microsoft:WorkflowServices:IgnoreTransactionsForTransactedCancelAndTransactedTerminate&quot; value=&quot;true&quot;/&gt;&#13;&#10;</code></pre>

Se o problema não ocorrer, não será necessário fazer isso.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.7.2       |
| Type    | Redirecionando |
