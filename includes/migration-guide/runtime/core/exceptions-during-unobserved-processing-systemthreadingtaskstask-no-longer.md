---
ms.openlocfilehash: 5ba2ddb76ab946339449246840667ba4a48e9c66
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619783"
---
### <a name="exceptions-during-unobserved-processing-in-systemthreadingtaskstask-no-longer-propagate-on-finalizer-thread"></a>Exceções durante o processamento não observado em System.Threading.Tasks.Task não são mais propagadas no thread do finalizador

#### <a name="details"></a>Detalhes

Como a classe <xref:System.Threading.Tasks.Task?displayProperty=fullName> representa uma operação assíncrona, ela captura todas as exceções não graves que ocorrem durante o processamento assíncrono. No .NET Framework 4.5, se uma exceção não for observada e seu código nunca aguarda a tarefa, a exceção não será mais propagada no thread do finalizador e causará a falha do processo durante a coleta de lixo. Essa alteração melhora a confiabilidade de aplicativos que usam a classe Task para executar processamento assíncrono não observado.

#### <a name="suggestion"></a>Sugestão

Se um aplicativo depender de exceções assíncronas não observadas que se propagam para o thread do finalizador, o comportamento anterior poderá ser restaurado com o fornecimento de um manipulador apropriado para o evento <xref:System.Threading.Tasks.TaskScheduler.UnobservedTaskException> ou com a definição de um [elemento de configuração de runtime](~/docs/framework/configure-apps/file-schema/runtime/throwunobservedtaskexceptions-element.md).

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Threading.Tasks.Task.Run(System.Action)?displayProperty=nameWithType></li><li><xref:System.Threading.Tasks.Task.Run(System.Action,System.Threading.CancellationToken)?displayProperty=nameWithType></li><li><xref:System.Threading.Tasks.Task.Run(System.Func{System.Threading.Tasks.Task})?displayProperty=nameWithType></li><li><xref:System.Threading.Tasks.Task.Run(System.Func{System.Threading.Tasks.Task},System.Threading.CancellationToken)?displayProperty=nameWithType></li><li><xref:System.Threading.Tasks.Task.Run%60%601(System.Func{%60%600})?displayProperty=nameWithType></li><li><xref:System.Threading.Tasks.Task.Run%60%601(System.Func{%60%600},System.Threading.CancellationToken)?displayProperty=nameWithType></li><li><xref:System.Threading.Tasks.Task.Run%60%601(System.Func{System.Threading.Tasks.Task{%60%600}})?displayProperty=nameWithType></li><li><xref:System.Threading.Tasks.Task.Run%60%601(System.Func{System.Threading.Tasks.Task{%60%600}},System.Threading.CancellationToken)?displayProperty=nameWithType></li><li><xref:System.Threading.Tasks.Task.Start?displayProperty=nameWithType></li><li><xref:System.Threading.Tasks.Task.Start(System.Threading.Tasks.TaskScheduler)?displayProperty=nameWithType></li></ul>|
