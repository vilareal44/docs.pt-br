---
title: Tarefas filho anexadas e desanexadas
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tasks, child tasks
ms.assetid: c95788bf-90a6-4e96-b7bc-58e36a228cc5
ms.openlocfilehash: c8a5d2c1ccb8bb2d272c2582cd416cdfd75506d8
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84285684"
---
# <a name="attached-and-detached-child-tasks"></a>Tarefas filho anexadas e desanexadas
Uma *tarefa filho* (ou *tarefa aninhada*) é uma instância <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> que é criada no representante do usuário de outra tarefa, conhecida como a *tarefa pai*. Uma tarefa filho pode ser desanexada ou anexada. Um *tarefa filho desanexada* é uma tarefa que é executada de forma independente de sua tarefa pai. Uma *tarefa filho anexada* é uma tarefa aninhada criada com a opção <xref:System.Threading.Tasks.TaskCreationOptions.AttachedToParent?displayProperty=nameWithType> cuja tarefa pai não explicitamente ou por padrão proíbe-a de ser anexada. Uma tarefa pode criar qualquer número de tarefas filho anexadas e desanexadas que só são limitadas pelos recursos do sistema.  
  
 A tabela a seguir lista as diferenças básicas entre os dois tipos de tarefas filho.  
  
|Categoria|Tarefas filho desanexadas|Tarefas filho anexadas|  
|--------------|--------------------------|--------------------------|  
|A tarefa pai aguarda a conclusão de tarefas filho.|Não|Sim|  
|A tarefa pai propaga exceções lançadas pelas tarefas filho.|Não|Sim|  
|O status da tarefa pai depende do status da tarefa filho.|Não|Sim|  
  
 Na maioria dos cenários, é recomendável usar tarefas filho desanexadas porque suas relações com outras tarefas são menos complexas. É por isso que as tarefas criadas dentro de tarefas pai são desanexadas por padrão e você deve especificar explicitamente a opção <xref:System.Threading.Tasks.TaskCreationOptions.AttachedToParent?displayProperty=nameWithType> para criar uma tarefa filho anexada.  
  
## <a name="detached-child-tasks"></a>Tarefas filho desanexadas  
 Embora uma tarefa filho seja criada por uma tarefa pai, por padrão ela é independente da tarefa pai. No exemplo a seguir, uma tarefa pai cria uma tarefa filho simples. Se você executar o exemplo de código várias vezes, perceberá que a saída do exemplo é diferente daquilo que é exibido e, também, que a saída pode mudar sempre que o código for executado. Isso ocorre porque as tarefas pai e filho são executadas de forma independente e a tarefa filho é uma tarefa desanexada. O exemplo espera apenas que a tarefa pai seja concluída e a tarefa filho pode não ser executada ou concluída antes do término do aplicativo de console.  
  
 [!code-csharp[TPL_ChildTasks#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_childtasks/cs/nested1.cs#1)]
 [!code-vb[TPL_ChildTasks#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_childtasks/vb/nested1.vb#1)]  
  
 Se a tarefa filho é representada por um objeto <xref:System.Threading.Tasks.Task%601> em vez de um objeto <xref:System.Threading.Tasks.Task>, é possível garantir que a tarefa pai aguardará a conclusão da tarefa filho acessando a propriedade <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=nameWithType> da tarefa filho, mesmo que ela seja uma tarefa filho desanexada. A propriedade <xref:System.Threading.Tasks.Task%601.Result%2A> é bloqueada até a tarefa ser concluída, conforme mostra o exemplo a seguir.  
  
 [!code-csharp[TPL_ChildTasks#4](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_childtasks/cs/childtasks.cs#4)]
 [!code-vb[TPL_ChildTasks#4](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_childtasks/vb/tpl_childtasks.vb#4)]  
  
## <a name="attached-child-tasks"></a>Tarefas filho anexadas  
 Ao contrário das tarefas filho desanexadas, as tarefas filho anexadas são estreitamente sincronizadas com as tarefas pai. Altere a tarefa filho desanexada do exemplo anterior para uma tarefa filho anexada usando a opção <xref:System.Threading.Tasks.TaskCreationOptions.AttachedToParent?displayProperty=nameWithType> na instrução de criação da tarefa, conforme mostrado no exemplo a seguir. Nesse código, a tarefa filho anexada é concluída antes de sua tarefa pai. Como resultado, a saída do exemplo é a mesma sempre que você executar o código.  
  
 [!code-csharp[TPL_ChildTasks#2](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_childtasks/cs/child1.cs#2)]
 [!code-vb[TPL_ChildTasks#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_childtasks/vb/child1.vb#2)]  
  
 Você pode usar tarefas filho anexadas para criar gráficos totalmente sincronizados de operações assíncronas.  
  
 No entanto, uma tarefa filho pode se anexada à tarefa pai somente se a tarefa pai não proibir tarefas filho anexadas. As tarefas pai podem explicitamente impedir que tarefas filho se anexem a elas especificando a opção <xref:System.Threading.Tasks.TaskCreationOptions.DenyChildAttach?displayProperty=nameWithType> no constructo de classe da tarefa pai ou no método <xref:System.Threading.Tasks.TaskFactory.StartNew%2A?displayProperty=nameWithType>. As tarefas pai podem explicitamente impedir que as tarefas filho se anexem a elas se elas tiverem sido criadas chamando o método <xref:System.Threading.Tasks.Task.Run%2A?displayProperty=nameWithType>. O exemplo a seguir ilustra isto. Ele é idêntico ao exemplo anterior, exceto que a tarefa pai é criada chamando o método <xref:System.Threading.Tasks.Task.Run%28System.Action%29?displayProperty=nameWithType> em vez do método <xref:System.Threading.Tasks.TaskFactory.StartNew%28System.Action%29?displayProperty=nameWithType>. Como a tarefa filho não consegue se anexar à sua tarefa pai, a saída do exemplo é imprevisível. Como as opções de criação da tarefa padrão para as sobrecargas <xref:System.Threading.Tasks.Task.Run%2A?displayProperty=nameWithType> incluem <xref:System.Threading.Tasks.TaskCreationOptions.DenyChildAttach?displayProperty=nameWithType>, este exemplo é funcionalmente equivalente ao primeiro exemplo na seção "Tarefas filho desanexadas".  
  
 [!code-csharp[TPL_ChildTasks#3](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_childtasks/cs/child1a.cs#3)]
 [!code-vb[TPL_ChildTasks#3](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_childtasks/vb/child1a.vb#3)]  
  
## <a name="exceptions-in-child-tasks"></a>Exceções em tarefas filho  
 Se uma tarefa filho desanexada lançar uma exceção, essa exceção deverá ser observada ou manipulada diretamente na tarefa pai assim como ocorre com todas as tarefas não aninhadas. Se uma tarefa filho anexada lançar uma exceção, a exceção será propagada automaticamente para a tarefa pai e de volta para o thread que espera ou tenta acessar a propriedade <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=nameWithType> da tarefa. Portanto, usando tarefas filho anexadas, você pode tratar de todas as exceções em apenas um ponto na chamada para <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType> no thread da chamada. Para saber mais, veja [Tratamento de exceção](exception-handling-task-parallel-library.md).  
  
## <a name="cancellation-and-child-tasks"></a>Cancelamento e tarefas filho  
 O cancelamento de tarefas é cooperativo. Ou seja, para ser anulável, todas as tarefas filho anexadas ou desanexadas devem monitorar o status do token de cancelamento. Se quiser cancelar uma tarefa pai e todas as tarefas filho usando uma solicitação de cancelamento, passe o mesmo token como um argumento para todas as tarefas e forneça em cada tarefa a lógica para responder à solicitação de cada tarefa. Para obter mais informações, consulte [Cancelamento de tarefas](task-cancellation.md) e [Como cancelar uma tarefa e seus filhos](how-to-cancel-a-task-and-its-children.md).  
  
### <a name="when-the-parent-cancels"></a>Quando a tarefa pai é cancelada  
 Se houver o cancelamento de uma tarefa pai antes do início de sua tarefa filho, a tarefa filho nunca será iniciada. Se houver o cancelamento de uma tarefa pai após sua tarefa filho já tiver sido iniciada, a tarefa filho será executada até ser concluída, a não ser que ela tenha sua própria lógica de cancelamento. Para obter mais informações, consulte [Cancelamento de tarefas](task-cancellation.md).  
  
### <a name="when-a-detached-child-task-cancels"></a>Quando uma tarefa filho desanexada é cancelada  
 Se houver o cancelamento de uma tarefa filho desanexada usando o mesmo token que foi passado para a tarefa pai e a tarefa pai não aguardar a tarefa filho, nenhuma exceção será propagada já que a exceção é tratada como um cancelamento de cooperação benigno. Esse comportamento é igual em todas as tarefas de nível superior.  
  
### <a name="when-an-attached-child-task-cancels"></a>Quando uma tarefa filho anexada é cancelada  
 Quando há o cancelamento de uma tarefa filho anexada usando o mesmo token que foi passado para sua tarefa pai, uma <xref:System.Threading.Tasks.TaskCanceledException> é propagada para o thread de junção dentro de um <xref:System.AggregateException>. Você deve aguardar a tarefa pai para tratar de todas as exceções benignas, além de todas as exceções de falha que são propagadas em um gráfico de tarefas filho anexadas.  
  
 Para saber mais, veja [Tratamento de exceção](exception-handling-task-parallel-library.md).  
  
## <a name="preventing-a-child-task-from-attaching-to-its-parent"></a>Impedir uma tarefa filho de se anexar à sua tarefa pai  
 Uma exceção sem tratamento que é gerada por uma tarefa filho é propagada para a tarefa pai. Você pode usar esse comportamento para observar todas as exceções da tarefa filho de uma tarefa raiz em vez de percorrer uma árvore de tarefas. No entanto, a propagação da exceção pode ser problemática quando uma tarefa pai não espera anexos de outro código. Por exemplo, considere um aplicativo que chama um componente de biblioteca de terceiros de um objeto <xref:System.Threading.Tasks.Task>. Se o componente da biblioteca de terceiros também criar um objeto <xref:System.Threading.Tasks.Task> e especificar <xref:System.Threading.Tasks.TaskCreationOptions.AttachedToParent?displayProperty=nameWithType> para anexá-lo à tarefa pai, todas as exceções sem tratamento que ocorrem na tarefa filho serão propagadas para o pai. Isso pode resultar em comportamento inesperado no aplicativo principal.  
  
 Para impedir que uma tarefa filho seja anexada a uma tarefa pai, especifique a opção <xref:System.Threading.Tasks.TaskCreationOptions.DenyChildAttach?displayProperty=nameWithType> ao criar a <xref:System.Threading.Tasks.Task> pai ou o objeto <xref:System.Threading.Tasks.Task%601>. Quando uma tarefa tenta se anexar à sua tarefa pai e a tarefa pai especifica a opção <xref:System.Threading.Tasks.TaskCreationOptions.DenyChildAttach?displayProperty=nameWithType>, a tarefa filho não poderá se anexar a uma tarefa pai e será executada apenas como se a opção <xref:System.Threading.Tasks.TaskCreationOptions.AttachedToParent?displayProperty=nameWithType> não fosse especificada.  
  
 É provável que você também queira impedir que uma tarefa filho se anexe à sua tarefa pai quando a tarefa filho não for concluída de maneira oportuna. Como a tarefa pai não termina até que todas as tarefas filho sejam concluídas, uma tarefa filho de longa duração pode fazer com que o aplicativo geral tenha um baixo desempenho. Para obter um exemplo que mostre como melhorar o desempenho do aplicativo impedindo que ele seja anexado à sua tarefa pai, confira [Como evitar que uma tarefa filho se anexe à sua tarefa pai](how-to-prevent-a-child-task-from-attaching-to-its-parent.md).  
  
## <a name="see-also"></a>Veja também

- [Programação paralela](index.md)
- [Paralelismo de dados](data-parallelism-task-parallel-library.md)
