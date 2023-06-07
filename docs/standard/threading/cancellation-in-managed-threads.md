---
title: Cancelamento em threads gerenciados
description: Entenda o cancelamento em threads gerenciados. Saiba mais sobre tokens de cancelamento no cancelamento cooperativo de operações síncronas ou de execução longa.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- cancellation in .NET, overview
ms.assetid: eea11fe5-d8b0-4314-bb5d-8a58166fb1c3
ms.openlocfilehash: 9af4a64e50eff65023d5ed5bda868af2f8323a96
ms.sourcegitcommit: 7137e12f54c4e83a94ae43ec320f8cf59c1772ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84662830"
---
# <a name="cancellation-in-managed-threads"></a>Cancelamento em threads gerenciados
A partir do .NET Framework 4, o .NET Framework usa um modelo unificado para cancelamento cooperativo de operações assíncronas ou síncronas de longa execução. Este modelo é baseado em um objeto leve chamado token de cancelamento. O objeto que invoca uma ou mais operações canceláveis, por exemplo criando novos tópicos ou tarefas, passa o token para cada operação. As operações individuais podem, por sua vez, passar cópias do token para outras operações. Posteriormente, o objeto que criou o token pode usá-lo para solicitar que as operações parem o que estão fazendo. Somente o objeto solicitante pode emitir a solicitação de cancelamento, e cada ouvinte é responsável por perceber a solicitação e respondê-la de forma apropriada e oportuna.  
  
 O padrão geral para implementar o modelo de cancelamento cooperativo é:  
  
- Instancie um objeto <xref:System.Threading.CancellationTokenSource>, que gerencia e envia uma notificação de cancelamento para os tokens de cancelamento individuais.  
  
- Passe o token retornado pela propriedade <xref:System.Threading.CancellationTokenSource.Token%2A?displayProperty=nameWithType> para cada tarefa ou thread que responde ao cancelamento.  
  
- Forneça um mecanismo para cada tarefa ou thread responder ao cancelamento.  
  
- Chame o método <xref:System.Threading.CancellationTokenSource.Cancel%2A?displayProperty=nameWithType> para fornecer uma notificação de cancelamento.  
  
> [!IMPORTANT]
> A classe <xref:System.Threading.CancellationTokenSource> implementa a interface <xref:System.IDisposable>. Certifique-se de chamar o método <xref:System.Threading.CancellationTokenSource.Dispose%2A?displayProperty=nameWithType> quando terminar de usar a fonte de token de cancelamento para liberar todos os recursos não gerenciados detidos.  
  
 A ilustração a seguir mostra o relacionamento entre uma fonte de token e todas as cópias de seu token.  
  
 ![Tokens de CancellationTokenSource e cancelamento](media/vs-cancellationtoken.png "VS_CancellationToken")  
  
 O novo modelo de cancelamento facilita a criação de aplicativos e bibliotecas com reconhecimento de cancelamento, e ele dá suporte aos seguintes recursos:  
  
- O cancelamento é cooperativo e não será forçado ao ouvinte. O ouvinte determina como encerrar normalmente em resposta a um pedido de cancelamento.  
  
- Solicitar é diferente de detectar. Um objeto que invoca uma operação cancelável pode controlar quando (e se) o cancelamento será solicitado.  
  
- O objeto solicitante emite o pedido de cancelamento para todas as cópias do token usando apenas uma chamada de método.  
  
- Um ouvinte pode detectar vários tokens simultaneamente juntando-os a um *token vinculado*.  
  
- O código do usuário pode notar e responder às solicitações de cancelamento do código da biblioteca e o código da biblioteca pode observar e responder às solicitações de cancelamento do código do usuário.  
  
- Os ouvintes podem ser notificados sobre solicitações de cancelamento por sondagem, registro de retorno de chamada ou aguardando identificadores de espera.  
  
## <a name="cancellation-types"></a>Tipos de cancelamento  
 A estrutura de cancelamento é implementada como um conjunto de tipos relacionados, que estão listados na tabela a seguir.  
  
|Nome do tipo|Descrição|  
|---------------|-----------------|  
|<xref:System.Threading.CancellationTokenSource>|O objeto que cria um token de cancelamento, e também emite o pedido de cancelamento para todas as cópias desse token.|  
|<xref:System.Threading.CancellationToken>|O tipo de valor leve passado a um ou mais ouvintes, normalmente como um parâmetro de método. Os ouvintes monitoram o valor da propriedade `IsCancellationRequested` do token por sondagem, retorno de chamada ou identificador de espera.|  
|<xref:System.OperationCanceledException>|As sobrecargas do construtor desta exceção aceitam <xref:System.Threading.CancellationToken> como um parâmetro. Os ouvintes podem, opcionalmente, lançar essa exceção para verificar a origem do cancelamento e notificar aos outros que ela respondeu a uma solicitação de cancelamento.|  
  
 O novo modelo de cancelamento está integrado ao .NET Framework em vários tipos. Os mais importantes são <xref:System.Threading.Tasks.Parallel?displayProperty=nameWithType>, <xref:System.Threading.Tasks.Task?displayProperty=nameWithType>, <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType> e <xref:System.Linq.ParallelEnumerable?displayProperty=nameWithType>. Recomendamos o uso desse novo modelo de cancelamento para todas as novas bibliotecas e códigos de aplicativos.  
  
## <a name="code-example"></a>Exemplo de código  
 No exemplo a seguir, o objeto solicitante cria um objeto <xref:System.Threading.CancellationTokenSource> e, em seguida, passa sua propriedade <xref:System.Threading.CancellationTokenSource.Token%2A> para a operação cancelável. A operação que recebe a solicitação monitora o valor da propriedade <xref:System.Threading.CancellationToken.IsCancellationRequested%2A> do token por sondagem. Quando o valor se torna `true`, o ouvinte pode concluir de qualquer maneira apropriada. Neste exemplo, o método apenas sai, que é tudo necessário em muitos casos.  
  
> [!NOTE]
> O exemplo usa o método <xref:System.Threading.ThreadPool.QueueUserWorkItem%2A> para demonstrar que a nova estrutura de cancelamento é compatível com as APIs legadas. Para um exemplo que usa o tipo <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> novo e preferido, confira [Como cancelar uma tarefa e seus filhos](../parallel-programming/how-to-cancel-a-task-and-its-children.md).  
  
 [!code-csharp[Cancellation#1](../../../samples/snippets/csharp/VS_Snippets_Misc/cancellation/cs/cancellationex1.cs#1)]
 [!code-vb[Cancellation#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cancellation/vb/cancellationex1.vb#1)]  
  
## <a name="operation-cancellation-versus-object-cancellation"></a>Cancelamento de operação versus cancelamento de objeto  
 Na nova estrutura de cancelamento, o cancelamento refere-se a operações, não a objetos. A solicitação de cancelamento significa que a operação deve ser interrompida o mais rápido possível após a conclusão de qualquer limpeza necessária. Um token de cancelamento deve se referir a uma "operação cancelável", no entanto, essa operação pode ser implementada em seu programa. Depois que a propriedade <xref:System.Threading.CancellationToken.IsCancellationRequested%2A> do token foi configurada para `true`, não pode ser redefinida para `false`. Portanto, os tokens de cancelamento não podem ser reutilizados depois de terem sido cancelados.  
  
 Se você precisar de um mecanismo de cancelamento de objeto, pode baseá-lo no mecanismo de cancelamento da operação chamando o método <xref:System.Threading.CancellationToken.Register%2A?displayProperty=nameWithType>, como mostrado no exemplo a seguir.  
  
 [!code-csharp[Cancellation#2](../../../samples/snippets/csharp/VS_Snippets_Misc/cancellation/cs/objectcancellation1.cs#2)]
 [!code-vb[Cancellation#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cancellation/vb/objectcancellation1.vb#2)]  
  
 Se um objeto oferecer suporte a mais de uma operação simultânea cancelável, passe um token separado como entrada para cada operação cancelável distinta. Dessa forma, uma operação pode ser cancelada sem afetar as outras.  
  
## <a name="listening-and-responding-to-cancellation-requests"></a>Detectar e responder a solicitações de cancelamento  
 No delegado do usuário, o implementador de uma operação cancelável determina como concluir a operação em resposta a uma solicitação de cancelamento. Em muitos casos, o delegado do usuário pode apenas executar qualquer limpeza necessária e depois retornar imediatamente.  
  
 No entanto, em casos mais complexos, pode ser necessário que o delegado do usuário notifique ao código da biblioteca que ocorreu o cancelamento. Nesses casos, a maneira correta de concluir a operação é o delegado chamar o método <xref:System.Threading.CancellationToken.ThrowIfCancellationRequested%2A>, que fará com que um <xref:System.OperationCanceledException> seja lançado. O código da biblioteca pode capturar essa exceção no thread do delegado de usuário e examinar o token da exceção para determinar se a exceção indica cancelamento cooperativo ou alguma outra situação excepcional.  
  
 A classe <xref:System.Threading.Tasks.Task> lida com o <xref:System.OperationCanceledException> dessa forma. Para obter mais informações, consulte [Cancelamento de tarefas](../parallel-programming/task-cancellation.md).  
  
### <a name="listening-by-polling"></a>Detectar por sondagem  
 Para cálculos de longa execução que fazem loop ou repetição, você pode detectar uma solicitação de cancelamento periodicamente pesquisando o valor da propriedade <xref:System.Threading.CancellationToken.IsCancellationRequested%2A?displayProperty=nameWithType>. Se o valor for `true`, o método deve ser limpo e concluído o mais rápido possível. A frequência de pesquisa ideal depende do tipo de aplicativo. Cabe ao desenvolvedor determinar a melhor frequência de sondagem para qualquer programa. A sondagem em si não afeta o desempenho significativamente. O exemplo a seguir mostra uma abordagem possível para a sondagem.  
  
 [!code-csharp[Cancellation#3](../../../samples/snippets/csharp/VS_Snippets_Misc/cancellation/cs/cancellationex11.cs#3)]
 [!code-vb[Cancellation#3](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cancellation/vb/cancellationex11.vb#3)]  
  
 Para um exemplo mais completo, confira [Como detectar solicitações de cancelamento por sondagem](how-to-listen-for-cancellation-requests-by-polling.md).  
  
### <a name="listening-by-registering-a-callback"></a>Ouvir ao registrar um retorno de chamada  
 Algumas operações podem ser bloqueadas de tal forma que não podem verificar o valor do token de cancelamento em tempo hábil. Para esses casos, você pode registrar um método de retorno de chamada que desbloqueia o método quando um pedido de cancelamento é recebido.  
  
 O método <xref:System.Threading.CancellationToken.Register%2A> retorna um objeto <xref:System.Threading.CancellationTokenRegistration> que é usado especificamente para essa finalidade. O exemplo a seguir mostra como usar o método <xref:System.Threading.CancellationToken.Register%2A> para cancelar uma solicitação da Web assíncrona.  
  
 [!code-csharp[Cancellation#4](../../../samples/snippets/csharp/VS_Snippets_Misc/cancellation/cs/cancellationex4.cs#4)]
 [!code-vb[Cancellation#4](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cancellation/vb/cancellationex4.vb#4)]  
  
 O objeto <xref:System.Threading.CancellationTokenRegistration> gerencia a sincronização de thread e garante que o retorno de chamada será interrompido em um determinado momento.  
  
 Para garantir a capacidade de resposta do sistema e evitar deadlocks, as seguintes diretrizes devem ser seguidas ao registrar retornos de chamada:  
  
- O método de retorno de chamada deve ser rápido porque é chamado de forma síncrona e, portanto, a chamada para <xref:System.Threading.CancellationTokenSource.Cancel%2A> não retorna até o retorno da chamada.  
  
- Se você chamar o <xref:System.Threading.CancellationTokenRegistration.Dispose%2A> enquanto o retorno de chamada estiver sendo executado e segurar um bloqueio em que o retorno de chamada está aguardando, seu programa pode chegar a um deadlock. Depois do retorno de `Dispose`, você pode liberar todos os recursos necessários para o retorno de chamada.  
  
- Os retornos de chamada não devem executar qualquer thread manual ou uso de <xref:System.Threading.SynchronizationContext> em um retorno de chamada. Se um retorno de chamada deve ser executado em um thread específico, use o construtor <xref:System.Threading.CancellationTokenRegistration?displayProperty=nameWithType> que permite especificar que o destino syncContext é o <xref:System.Threading.SynchronizationContext.Current%2A?displayProperty=nameWithType> ativo. Realizar threading manual em um retorno de chamada pode causar um deadlock.  
  
 Para um exemplo mais completo, confira [Como registrar retornos de chamada para solicitações de cancelamento](how-to-register-callbacks-for-cancellation-requests.md).  
  
### <a name="listening-by-using-a-wait-handle"></a>Ouvir usando um identificador de espera  
 Quando uma operação cancelável pode bloquear enquanto aguarda um primitivo de sincronização, como <xref:System.Threading.ManualResetEvent?displayProperty=nameWithType> ou <xref:System.Threading.Semaphore?displayProperty=nameWithType>, você pode usar a propriedade <xref:System.Threading.CancellationToken.WaitHandle%2A?displayProperty=nameWithType> para permitir que a operação aguarde o evento e a solicitação de cancelamento. O identificador de espera do token de cancelamento será sinalizado em resposta a um pedido de cancelamento e o método pode usar o valor de retorno do método <xref:System.Threading.WaitHandle.WaitAny%2A> para determinar se foi o token de cancelamento que sinalizou. A operação poderá, em seguida, simplesmente sair ou lançar um <xref:System.OperationCanceledException>, conforme apropriado.  
  
 [!code-csharp[Cancellation#5](../../../samples/snippets/csharp/VS_Snippets_Misc/cancellation/cs/cancellationex9.cs#5)]
 [!code-vb[Cancellation#5](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cancellation/vb/cancellationex9.vb#5)]  
  
 No novo código que tem como alvo o .NET Framework 4, <xref:System.Threading.ManualResetEventSlim?displayProperty=nameWithType> e <xref:System.Threading.SemaphoreSlim?displayProperty=nameWithType> dão suporte à nova estrutura de cancelamento em seus métodos `Wait`. Você pode passar o <xref:System.Threading.CancellationToken> para o método, e quando o cancelamento é solicitado, o evento é acionado e lança um <xref:System.OperationCanceledException>.  
  
 [!code-csharp[Cancellation#6](../../../samples/snippets/csharp/VS_Snippets_Misc/cancellation/cs/cancellationex10.cs#6)]
 [!code-vb[Cancellation#6](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cancellation/vb/cancellationex10.vb#6)]  
  
 Para um exemplo mais completo, confira [Como detectar solicitações de cancelamento que possuem identificadores de espera](how-to-listen-for-cancellation-requests-that-have-wait-handles.md).  
  
### <a name="listening-to-multiple-tokens-simultaneously"></a>Detectar vários tokens simultaneamente  
 Em alguns casos, um ouvinte pode ter que detectar vários tokens de cancelamento simultaneamente. Por exemplo, uma operação cancelável pode ter que monitorar um token de cancelamento interno, além de um token passado externamente como um argumento para um parâmetro de método. Para fazer isso, crie uma fonte de token vinculada que pode juntar dois ou mais tokens em um token, como mostrado no exemplo a seguir.  
  
 [!code-csharp[Cancellation#7](../../../samples/snippets/csharp/VS_Snippets_Misc/cancellation/cs/cancellationex13.cs#7)]
 [!code-vb[Cancellation#7](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cancellation/vb/cancellationex13.vb#7)]  
  
 Observe que você deve chamar `Dispose` na fonte de token vinculada quando concluir. Para um exemplo mais completo, confira [Como detectar múltiplas solicitações de cancelamento](how-to-listen-for-multiple-cancellation-requests.md).  
  
## <a name="cooperation-between-library-code-and-user-code"></a>Cooperação entre o código de biblioteca e o código do usuário  
 A estrutura de cancelamento unificada permite que o código da biblioteca cancele o código do usuário e que o código do usuário cancele o código da biblioteca de forma cooperativa. A cooperação sem problemas depende da conformidade com as seguintes diretrizes:  
  
- Se o código da biblioteca fornece operações canceláveis, ele também deve fornecer métodos públicos que aceitam um token de cancelamento externo para que o código do usuário possa solicitar o cancelamento.  
  
- Se o código da biblioteca chamar o código do usuário, o código da biblioteca deve interpretar um OperationCanceledException(externalToken) como *cancelamento cooperativo* e não necessariamente como uma exceção de falha.  
  
- Os delegados de usuários devem tentar responder as solicitações de cancelamento do código da biblioteca em tempo hábil.  
  
 <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> e <xref:System.Linq.ParallelEnumerable?displayProperty=nameWithType> são exemplos de classes que seguem essas diretrizes. Para obter mais informações, consulte [cancelamento de tarefas](../parallel-programming/task-cancellation.md) e [como: cancelar uma consulta PLINQ](../parallel-programming/how-to-cancel-a-plinq-query.md).  
  
## <a name="see-also"></a>Confira também

- [Noções básicas sobre Threading gerenciado](managed-threading-basics.md)
