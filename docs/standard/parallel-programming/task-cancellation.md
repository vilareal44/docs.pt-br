---
title: Cancelamento da tarefa
description: Entenda o cancelamento da tarefa, que tem suporte nas classes Task e Task <TResult> por meio do uso de tokens de cancelamento no .net.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tasks, cancellation
- asynchronous task cancellation
ms.assetid: 3ecf1ea9-e399-4a6a-a0d6-8475f48dcb28
ms.openlocfilehash: 1d9b7b35341961c27107f007e0eafa51ef49e232
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84768658"
---
# <a name="task-cancellation"></a>Cancelamento da tarefa
As classes <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> e <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType> oferecem suporte ao cancelamento por meio do uso de tokens de cancelamento no .NET Framework. Para obter mais informações, consulte [cancelamento em threads gerenciados](../threading/cancellation-in-managed-threads.md). Nas classes de tarefas, o cancelamento envolve a cooperação entre o delegado do usuário, que representa uma operação cancelável, e o código que solicitou o cancelamento.  Um cancelamento bem-sucedido envolve o código de solicitação chamar o método <xref:System.Threading.CancellationTokenSource.Cancel%2A?displayProperty=nameWithType> e o delegado do usuário terminar a operação de forma breve. Você pode terminar a operação ao usar uma destas opções:  
  
- Simplesmente ao sair do delegado. Em muitos cenários isso é suficiente. Entretanto, uma instância da tarefa que é cancelada desse modo faz a transição para o estado <xref:System.Threading.Tasks.TaskStatus.RanToCompletion?displayProperty=nameWithType>, e não para o estado <xref:System.Threading.Tasks.TaskStatus.Canceled?displayProperty=nameWithType>.  
  
- Ao gerar um <xref:System.OperationCanceledException> e passar o token em que o cancelamento foi solicitado. O modo preferido de fazer isso é usar o método <xref:System.Threading.CancellationToken.ThrowIfCancellationRequested%2A>. Uma tarefa que é cancelada desse modo faz a transição para o estado Cancelado, o qual o código de chamada pode usar para verificar se a tarefa respondeu a sua solicitação de cancelamento.  
  
 O exemplo a seguir mostra o padrão básico para o cancelamento da tarefa que gerou a exceção. Observe que o token é passado ao delegado do usuário e à instância da tarefa em si.  
  
 [!code-csharp[TPL_Cancellation#02](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_cancellation/cs/snippet02.cs#02)]
 [!code-vb[TPL_Cancellation#02](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_cancellation/vb/module1.vb#02)]  
  
 Para obter um exemplo mais completo, confira [Como cancelar uma tarefa e seus filhos](how-to-cancel-a-task-and-its-children.md).  
  
 Quando uma instância de tarefa observa uma <xref:System.OperationCanceledException> gerada pelo código de usuário, compara o token de exceção ao token associado (aquele que foi passado para a API que criou a tarefa). Se eles forem os mesmos e a propriedade <xref:System.Threading.CancellationToken.IsCancellationRequested%2A> do token retornar verdadeiro, a tarefa interpretará isso como o cancelamento da confirmação e fará a transição para o estado cancelado. Se você não usar um método <xref:System.Threading.Tasks.Task.Wait%2A> ou <xref:System.Threading.Tasks.Task.WaitAll%2A> para aguardar a conclusão da tarefa, a tarefa apenas definirá seu status como <xref:System.Threading.Tasks.TaskStatus.Canceled>.  
  
 Se você estiver aguardando uma tarefa que faça a transição para o estado Cancelado, uma exceção <xref:System.Threading.Tasks.TaskCanceledException?displayProperty=nameWithType> (envolvida em uma exceção <xref:System.AggregateException>) será lançada. Observe que essa exceção indica o cancelamento com êxito em vez de uma situação de falha. Assim, a propriedade <xref:System.Threading.Tasks.Task.Exception%2A> da tarefa retorna `null`.  
  
 Se a propriedade <xref:System.Threading.CancellationToken.IsCancellationRequested%2A> do token retornar falso ou se o token de exceção não corresponder ao token da tarefa, <xref:System.OperationCanceledException> será tratada como uma exceção normal, fazendo com que a tarefa transicione para o estado de Falha. Observe também que a presença de outras exceções também fará com que a tarefa faça a transição para o estado de Falha. Você poderá obter o status da tarefa concluída na propriedade <xref:System.Threading.Tasks.Task.Status%2A>.  
  
 É possível que uma tarefa continue a processar alguns itens após o cancelamento ser solicitado.  
  
## <a name="see-also"></a>Veja também

- [Cancelamento em threads gerenciados](../threading/cancellation-in-managed-threads.md)
- [Como: Cancelar uma tarefa e seus filhos](how-to-cancel-a-task-and-its-children.md)
