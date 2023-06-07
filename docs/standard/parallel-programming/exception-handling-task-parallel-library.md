---
title: Tratamento de exceções (biblioteca de paralelismo de tarefas)
description: Explore a manipulação de exceção usando a TPL (biblioteca paralela de tarefas) no .NET. Consulte exceções de agregação aninhadas, exceções internas, exceções de tarefas não observadas, & mais.
ms.date: 04/20/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tasks, exceptions
ms.assetid: beb51e50-9061-4d3d-908c-56a4f7c2e8c1
ms.openlocfilehash: f1c1a994f4b3a8df0556a0190bc4eacb63f2921e
ms.sourcegitcommit: 7137e12f54c4e83a94ae43ec320f8cf59c1772ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84662531"
---
# <a name="exception-handling-task-parallel-library"></a>Tratamento de exceções (biblioteca de paralelismo de tarefas)

As exceções sem tratamento que são lançadas pelo código de usuário que está sendo executado dentro de uma tarefa são propagadas de volta para o thread de chamada, exceto em certos cenários que são descritos mais adiante neste tópico. As exceções são propagadas quando você usa um dos métodos estáticos ou de instância de <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType> e manipula-os incluindo a chamada em uma instrução `try`/`catch`. Se uma tarefa é o pai das tarefas filho anexadas, ou se você está esperando várias tarefas, várias exceções podem ser lançadas.

Para propagar todas as exceções de volta ao thread de chamada, a infraestrutura da Tarefa envolve-as em uma instância de <xref:System.AggregateException>. A exceção <xref:System.AggregateException> tem uma propriedade <xref:System.AggregateException.InnerExceptions%2A> que pode ser enumerada para examinar todas as exceções originais que foram lançadas e manipular (ou não manipular) cada uma individualmente. Você também pode manipular as exceções originais usando o método <xref:System.AggregateException.Handle%2A?displayProperty=nameWithType>.

Mesmo que apenas uma exceção seja lançada, ela ainda está envolvida em uma exceção <xref:System.AggregateException>, como mostra o exemplo a seguir.

[!code-csharp[TPL_Exceptions#21](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_exceptions/cs/handling21.cs#21)]
[!code-vb[TPL_Exceptions#21](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_exceptions/vb/handling21.vb#21)]

Você poderia evitar uma exceção sem tratamento apenas pegando o <xref:System.AggregateException> e não observando nenhuma das exceções internas. No entanto, recomendamos que você não faça isso, pois é análogo o capturar o tipo base <xref:System.Exception> em cenários não paralelos. Capturar uma exceção sem tomar medidas específicas para se recuperar pode deixar seu programa em um estado indeterminado.

Caso não deseje chamar o método <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType> para aguardar a conclusão de uma tarefa, você também poderá recuperar a exceção <xref:System.AggregateException> na propriedade <xref:System.Threading.Tasks.Task.Exception%2A> da tarefa, como mostra o exemplo a seguir. Para obter mais informações, consulte a seção [observando exceções usando a Propriedade Task. Exception](#observing-exceptions-by-using-the-taskexception-property) neste tópico.

[!code-csharp[TPL_Exceptions#29](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_exceptions/cs/handling22.cs#29)]
[!code-vb[TPL_Exceptions#29](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_exceptions/vb/handling22.vb#29)]

Se você não aguardar uma tarefa que propaga uma exceção ou acessar sua propriedade <xref:System.Threading.Tasks.Task.Exception%2A>, a exceção será escalada de acordo com a política de exceção .NET quando a tarefa for coletada como lixo.

Quando as exceções tiverem permissão de emergirem novamente para o thread de associação, será possível que uma tarefa continue a processar alguns itens após a geração da exceção.

> [!NOTE]
> Se a opção "Apenas Meu Código" estiver habilitada, o Visual Studio em alguns casos interromperá na linha que lança a exceção e exibirá uma mensagem de erro que diz "exceção não tratada pelo código do usuário". Esse erro é benigno. Você pode pressionar F5 para continuar e ver o comportamento de tratamento de exceção, demonstrado nos exemplos a seguir. Para impedir que o Visual Studio seja interrompido no primeiro erro, basta desmarcar a caixa de seleção **Habilitar Apenas Meu Código** em **Ferramentas, Opções, Depuração, Geral**.

## <a name="attached-child-tasks-and-nested-aggregateexceptions"></a>Tarefas filho anexadas e AggregateExceptions aninhadas

Se uma tarefa tiver uma tarefa filho anexada que lança uma exceção, essa exceção é enrolada em um <xref:System.AggregateException> antes de se propagar para a tarefa pai, que envolve essa exceção em sua própria <xref:System.AggregateException> antes de propagá-la de volta ao thread de chamada. Nesses casos, a propriedade <xref:System.AggregateException.InnerExceptions%2A> da exceção <xref:System.AggregateException> que é capturada nos métodos <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType>, <xref:System.Threading.Tasks.Task.WaitAny%2A> ou <xref:System.Threading.Tasks.Task.WaitAll%2A> contém uma ou mais instâncias <xref:System.AggregateException>, não as exceções originais que causaram a falha. Para evitar ter que iterar sobre exceções aninhadas <xref:System.AggregateException>, você pode usar o método <xref:System.AggregateException.Flatten%2A> para remover todas as exceções aninhadas <xref:System.AggregateException>, de modo que a propriedade <xref:System.AggregateException.InnerExceptions%2A?displayProperty=nameWithType> contenha as exceções originais. No exemplo a seguir, as instâncias de <xref:System.AggregateException> aninhadas são achatadas e manipuladas em apenas um loop.

[!code-csharp[TPL_Exceptions#22](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_exceptions/cs/flatten2.cs#22)]
[!code-vb[TPL_Exceptions#22](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_exceptions/vb/flatten2.vb#22)]

Você também pode usar o método <xref:System.AggregateException.Flatten%2A?displayProperty=nameWithType> para relançar as exceções internas de múltiplas instâncias de <xref:System.AggregateException> lançadas por várias tarefas em uma única instância <xref:System.AggregateException>, como mostra o exemplo a seguir.

[!code-csharp[TPL_Exceptions#13](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_exceptions/cs/taskexceptions2.cs#13)]
[!code-vb[TPL_Exceptions#13](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_exceptions/vb/taskexceptions2.vb#13)]

## <a name="exceptions-from-detached-child-tasks"></a>Exceções de tarefas filho desanexadas

Por padrão, as tarefas filho são criadas como desanexadas. As exceções lançadas a partir de tarefas separadas devem ser tratadas ou revogadas na tarefa pai imediata; elas não são propagadas de volta para o tópico de chamada da mesma maneira que as tarefas filho anexadas são propagadas de volta. O pai mais alto pode relançar manualmente uma exceção de uma tarefa filho separada para fazer com que ela seja encapsulada em um <xref:System.AggregateException> e propagada de volta para o thread de chamada.

[!code-csharp[TPL_Exceptions#23](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_exceptions/cs/detached21.cs#23)]
[!code-vb[TPL_Exceptions#23](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_exceptions/vb/detached21.vb#23)]

Mesmo que você use uma continuação para observar uma exceção em uma tarefa filho, a exceção ainda deve ser observada pela tarefa pai.

## <a name="exceptions-that-indicate-cooperative-cancellation"></a>Exceções que indicam o cancelamento cooperativo

Quando o código do usuário em uma tarefa responde a uma solicitação de cancelamento, o procedimento correto é lançar um <xref:System.OperationCanceledException> no token de cancelamento no qual a solicitação foi comunicada. Antes de tentar propagar a exceção, a instância da tarefa compara o token na exceção ao que foi passado para ela quando foi criada. Se eles são iguais, a tarefa propaga um <xref:System.Threading.Tasks.TaskCanceledException> encapsulado no <xref:System.AggregateException>, e pode ser visto quando as exceções internas são examinadas. No entanto, se o thread de chamada não estiver aguardando a tarefa, essa exceção específica não será propagada. Para obter mais informações, consulte [Cancelamento de tarefas](task-cancellation.md).

[!code-csharp[TPL_Exceptions#4](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_exceptions/cs/exceptions.cs#4)]
[!code-vb[TPL_Exceptions#4](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_exceptions/vb/tpl_exceptions.vb#4)]

## <a name="using-the-handle-method-to-filter-inner-exceptions"></a>Usando o método handle para filtrar exceções internas

Você pode usar o método <xref:System.AggregateException.Handle%2A?displayProperty=nameWithType> para filtrar exceções que você pode tratar como "manipuladas" sem usar qualquer lógica adicional. No delegado de usuário que é fornecido ao método <xref:System.AggregateException.Handle%28System.Func%7BSystem.Exception%2CSystem.Boolean%7D%29?displayProperty=nameWithType>, você pode examinar o tipo de exceção, sua propriedade <xref:System.Exception.Message%2A> ou qualquer outra informação sobre isso que permitirá determinar se é benigno. Quaisquer exceções para as quais o delegado retorna `false` são relançadas em uma nova instância do <xref:System.AggregateException> imediatamente após o retorno do método <xref:System.AggregateException.Handle%2A?displayProperty=nameWithType>.

O exemplo a seguir é funcionalmente equivalente ao primeiro exemplo neste tópico, que examina cada exceção na coleção <xref:System.AggregateException.InnerExceptions%2A?displayProperty=nameWithType>.  Em vez disso, esse manipulador de exceção chama o objeto de método <xref:System.AggregateException.Handle%2A?displayProperty=nameWithType> para cada exceção, e lança novamente apenas exceções que não são instâncias de `CustomException`.

[!code-csharp[TPL_Exceptions#26](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_exceptions/cs/handlemethod21.cs#26)]
[!code-vb[TPL_Exceptions#26](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_exceptions/vb/handlemethod21.vb#26)]

O seguinte é um exemplo mais completo que usa o método <xref:System.AggregateException.Handle%2A?displayProperty=nameWithType> para fornecer tratamento especial para uma exceção <xref:System.UnauthorizedAccessException> ao enumerar arquivos.

[!code-csharp[TPL_Exceptions#12](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_exceptions/cs/taskexceptions.cs#12)]
[!code-vb[TPL_Exceptions#12](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_exceptions/vb/taskexceptions.vb#12)]

## <a name="observing-exceptions-by-using-the-taskexception-property"></a>Observando exceções usando a propriedade Task.Exception

Se uma tarefa for concluída no estado <xref:System.Threading.Tasks.TaskStatus.Faulted?displayProperty=nameWithType>, sua propriedade <xref:System.Threading.Tasks.Task.Exception%2A> pode ser examinada para descobrir qual exceção específica causou a falha. Uma boa forma de observar a propriedade <xref:System.Threading.Tasks.Task.Exception%2A> é usar uma continuação que seja executada somente se a tarefa anterior falhar, como mostrado no exemplo a seguir.

[!code-csharp[TPL_Exceptions#27](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_exceptions/cs/exceptionprop21.cs#27)]
[!code-vb[TPL_Exceptions#27](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_exceptions/vb/exceptionprop21.vb#27)]

Em um aplicativo significativo, o delegado de continuação poderia registrar informações detalhadas sobre a exceção e possivelmente gerar novas tarefas para se recuperar da exceção. Se houver falhas de tarefa, as expressões a seguir lançarão a exceção:

- `await task`
- `task.Wait()`
- `task.Result`
- `task.GetAwaiter().GetResult()`

Use uma [`try-catch`](../../csharp/language-reference/keywords/try-catch.md) instrução para manipular e observar exceções geradas. Como alternativa, observe a exceção acessando a <xref:System.Threading.Tasks.Task.Exception%2A?displayProperty=nameWithType> propriedade.

## <a name="unobservedtaskexception-event"></a>Evento UnobservedTaskException

Em alguns cenários, como ao hospedar plug-ins não confiáveis, exceções benignas podem ser comuns, e pode ser muito difícil observar todas manualmente. Nesses casos, você pode lidar com o evento <xref:System.Threading.Tasks.TaskScheduler.UnobservedTaskException?displayProperty=nameWithType>. A instância do <xref:System.Threading.Tasks.UnobservedTaskExceptionEventArgs?displayProperty=nameWithType> que é passada para seu manipulador pode ser usada para evitar que a exceção não observada seja propagada de volta para o thread de junção.

## <a name="see-also"></a>Confira também

- [Biblioteca de tarefas paralelas (TPL)](task-parallel-library-tpl.md)
