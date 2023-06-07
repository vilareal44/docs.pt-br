---
title: Padrão assíncrono baseado em tarefa (TAP)
description: Saiba mais sobre o padrão assíncrono baseado em tarefa (toque). TOQUE em é o padrão de design assíncrono recomendado para desenvolvimento no .NET.
ms.date: 02/26/2019
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- .NET Framework, and TAP
- asynchronous design patterns, .NET Framework
- TAP, .NET Framework support for
- Task-based Asynchronous Pattern, .NET Framework support for
- .NET Framework, asynchronous design patterns
ms.assetid: 8cef1fcf-6f9f-417c-b21f-3fd8bac75007
ms.openlocfilehash: 21675d26fa2f11d93801e2ba4ffec96b238b97b8
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85325079"
---
# <a name="task-based-asynchronous-pattern"></a>Padrão assíncrono baseado em tarefa

O padrão assíncrono baseado em tarefa (TAP) é baseado nos <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> tipos e <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType> no <xref:System.Threading.Tasks?displayProperty=nameWithType> namespace, que são usados para representar operações assíncronas arbitrárias. O TAP é o padrão de design assíncrono recomendado para novos desenvolvimentos.  
  
## <a name="naming-parameters-and-return-types"></a>Nomenclatura, parâmetros e tipos de retorno

O TAP usa um único método para representar o início e a conclusão de uma operação assíncrona. Isso contrasta com o Modelo de programação assíncrona (APM ou `IAsyncResult`) e o Padrão assíncrono baseado em eventos (EAP). O APM requer os métodos `Begin` e `End`. O EAP requer um método que tenha o sufixo `Async` e também requer um ou mais eventos, tipos delegados de manipulador de eventos e tipos derivados de `EventArg`. Os métodos assíncronos no TAP incluem o sufixo `Async` após o nome da operação para os métodos que retornam tipos aguardáveis, como <xref:System.Threading.Tasks.Task>, <xref:System.Threading.Tasks.Task%601>, <xref:System.Threading.Tasks.ValueTask> e <xref:System.Threading.Tasks.ValueTask%601>. Por exemplo, uma operação `Get` assíncrona que retorna um `Task<String>` pode ser nomeada `GetAsync`. Se você estiver adicionando um método TAP a uma classe que já contenha o nome do método EAP com o sufixo `Async`, use, em vez dele, o sufixo `TaskAsync`. Por exemplo, se a classe já possuir um método `GetAsync`, use o nome `GetTaskAsync`. Se um método inicia com uma operação assíncrona, mas não retorna um tipo aguardável, o nome dele deve começar com `Begin`, `Start` ou algum outro verbo que sugira que esse método não retorna nem gera o resultado da operação.  
  
 Um método TAP retorna <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> ou <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType>, dependendo se o método síncrono correspondente retorna void ou um tipo `TResult`.  
  
 Os parâmetros de um método TAP devem corresponder aos parâmetros de suas contrapartes síncronas e devem ser fornecidos na mesma ordem.  No entanto, os parâmetros `out` e `ref` são isentos dessa regra e devem ser evitados inteiramente. Quaisquer dados retornados por um parâmetro `out` ou `ref` deve, em vez disso, ser retornado como parte do `TResult` retornado por <xref:System.Threading.Tasks.Task%601> e deve usar uma tupla ou uma estrutura de dados personalizada para acomodar diversos valores. Além disso, considere adicionar um <xref:System.Threading.CancellationToken> parâmetro mesmo se a contraparte síncrona do método Tap não oferecer uma.

 Os métodos que são dedicados exclusivamente à criação, ao tratamento ou à combinação de tarefas (onde a intenção assíncrona do método está clara no nome do método ou no nome do tipo ao qual o método pertence) não precisam seguir esse padrão de nomenclatura; esses métodos são geralmente denominados *combinadores*. Os exemplos de combinadores incluem <xref:System.Threading.Tasks.Task.WhenAll%2A> e <xref:System.Threading.Tasks.Task.WhenAny%2A> e são discutidos na seção [Usando os combinadores baseados em tarefas internos](consuming-the-task-based-asynchronous-pattern.md#combinators) do artigo [Consumindo o padrão assíncrono baseado em tarefa](consuming-the-task-based-asynchronous-pattern.md).  
  
 Para obter exemplos de como a sintaxe do TAP difere da sintaxe usada em padrões de programação assíncronos herdados, como APM (Asynchronous Programming Model) e EAP (Event-based Asynchronous Pattern), confira [Padrões de programação assíncrona ](index.md).  
  
## <a name="initiating-an-asynchronous-operation"></a>Como iniciar uma operação assíncrona  
 Um método assíncrono baseado no TAP pode fazer uma pequena quantidade de trabalho de forma síncrona, como validar argumentos e iniciar a operação assíncrona, antes de retornar a tarefa resultante. O trabalho síncrono deve ser mantido no mínimo possível para que o método assíncrono possa retornar rapidamente. Os motivos para um retorno rápido incluem:  
  
- Os métodos assíncronos podem ser chamados de segmentos de interface do usuário (UI), e qualquer trabalho síncrono longo pode prejudicar a resposta do aplicativo.  
  
- É possível iniciar vários métodos assíncronos simultaneamente. Portanto, qualquer trabalho longo na parte síncrona de um método assíncrono pode atrasar a inicialização de outras operações assíncronas, diminuindo assim os benefícios de simultaneidade.  
  
 Em alguns casos, a quantidade de trabalho necessária para concluir a operação é menor do que a quantidade de trabalho necessária para iniciar a operação de forma assíncrona. Ler de um fluxo em que a operação de leitura pode ser satisfeita pelos dados que já estão armazenados no buffer de memória é um exemplo de cenário desse tipo. Em casos como esse, a operação pode ser concluída de forma síncrona e retornar uma tarefa que já havia sido concluída.  
  
## <a name="exceptions"></a>Exceções  
 Um método assíncrono deve gerar uma exceção para ser lançada fora da chamada do método assíncrono somente em resposta a um erro de uso. Erros de uso nunca devem ocorrer em código de produção. Por exemplo, se a passagem de uma referência nula ( `Nothing` em Visual Basic) como um dos argumentos do método causar um estado de erro (geralmente representado por uma <xref:System.ArgumentNullException> exceção), você poderá modificar o código de chamada para garantir que uma referência nula nunca seja passada. Para todos outros erros, as exceções que ocorrem quando um método assíncrono está em execução devem ser atribuídas a tarefa retornada, mesmo quando o método assíncrono é concluído de forma síncrona antes de a tarefa ser retornada. Normalmente, uma tarefa contém no máximo uma exceção. No entanto, se a tarefa representa várias operações (por exemplo, <xref:System.Threading.Tasks.Task.WhenAll%2A>), várias exceções podem ser associadas a uma única tarefa.  
  
## <a name="target-environment"></a>Ambiente de destino  
 Quando você implementa um método TAP, pode determinar o local em que a execução assíncrona ocorre. Você pode optar por executar a carga de trabalho no pool de threads, implementá-la usando e/s assíncrona (sem estar associada a um thread para a maioria da execução da operação), executá-la em um thread específico (como o thread de interface do usuário) ou usar qualquer número de contextos potenciais. Um método TAP pode até não ter nada a executar e pode retornar apenas um <xref:System.Threading.Tasks.Task>, que representa a ocorrência de uma condição em outro lugar no sistema (por exemplo, uma tarefa que representa os dados que chegam a uma estrutura de dados na fila).

 O chamador do método TAP pode bloquear a espera para o método TAP ser concluído com a espera síncrona na tarefa resultante ou pode executar código adicional (continuação) quando a operação assíncrona é concluída. O criador do código de continuação tem o controle sobre o local em que o código é executado. Você pode criar o código de continuação explicitamente com os métodos da classe <xref:System.Threading.Tasks.Task> (por exemplo, <xref:System.Threading.Tasks.Task.ContinueWith%2A>) ou implicitamente usando o suporte à linguagem compilado nas continuações (por exemplo, `await` em C#, `Await` em Visual Basic, `AwaitValue` em F#).  
  
## <a name="task-status"></a>Status da tarefa  
 A classe <xref:System.Threading.Tasks.Task> fornece um ciclo de vida para operações assíncronas, e esse ciclo é representado pela enumeração <xref:System.Threading.Tasks.TaskStatus>. Para oferecer suporte a casos extremos de tipos que derivam de <xref:System.Threading.Tasks.Task> e de <xref:System.Threading.Tasks.Task%601> e suporte à separação da construção do agendamento, a classe <xref:System.Threading.Tasks.Task> expõe um método <xref:System.Threading.Tasks.Task.Start%2A>. As tarefas que são criadas pelos construtores públicos <xref:System.Threading.Tasks.Task> são chamadas de *tarefas frias*, porque começam seu ciclo de vida no estado não agendado <xref:System.Threading.Tasks.TaskStatus.Created> e são agendadas somente quando <xref:System.Threading.Tasks.Task.Start%2A> é chamado nessas instâncias.

 Todas tarefas restantes começam seu ciclo de vida em um estado quente, o que significa que as operações assíncronas que elas representam já foram iniciadas, e seus status de tarefa são um valor de enumeração diferente de <xref:System.Threading.Tasks.TaskStatus.Created?displayProperty=nameWithType>. Todas as tarefas que são retornadas dos métodos TAP devem ser ativadas. **Se um método TAP usa internamente um construtor de tarefa para instanciar a tarefa a ser retornada, o método TAP deve chamar <xref:System.Threading.Tasks.Task.Start%2A> no <xref:System.Threading.Tasks.Task> objeto antes de retorná-lo.** Os consumidores de um método TAP podem presumir com segurança que a tarefa retornada está ativa e não devem tentar chamar <xref:System.Threading.Tasks.Task.Start%2A> em qualquer <xref:System.Threading.Tasks.Task> que é retornado de um método TAP. A chamada <xref:System.Threading.Tasks.Task.Start%2A> em uma tarefa ativa resulta em uma exceção de <xref:System.InvalidOperationException>.  
  
## <a name="cancellation-optional"></a>Cancelamento (opcional)  
 Em TAP, o cancelamento é opcional para implementadores e consumidores de métodos assíncronos. Se uma operação permite o cancelamento, ela expõe uma sobrecarga do método assíncrono que aceita um símbolo de cancelamento (<xref:System.Threading.CancellationToken>). Por convenção, o parâmetro é chamado `cancellationToken`.  
  
 [!code-csharp[Conceptual.TAP#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.tap/cs/examples1.cs#1)]
 [!code-vb[Conceptual.TAP#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.tap/vb/examples1.vb#1)]  
  
 A operação assíncrona monitora esse token para solicitações de cancelamento. Se ela recebe uma solicitação de cancelamento, pode escolher honrar a solicitação e cancelar a operação. Se a solicitação de cancelamento resulta em algum trabalho ser encerrado prematuramente, o método TAP retorna uma tarefa que termina em estado de <xref:System.Threading.Tasks.TaskStatus.Canceled>; não há nenhum resultado disponível, e nenhuma exceção é gerada.  O estado <xref:System.Threading.Tasks.TaskStatus.Canceled> é considerado um estado final (concluído) para uma tarefa, juntamente com os estados <xref:System.Threading.Tasks.TaskStatus.Faulted> e <xref:System.Threading.Tasks.TaskStatus.RanToCompletion>. Portanto, se uma tarefa está no estado <xref:System.Threading.Tasks.TaskStatus.Canceled>, sua propriedade <xref:System.Threading.Tasks.Task.IsCompleted%2A> retorna `true`. Quando uma tarefa termina no estado <xref:System.Threading.Tasks.TaskStatus.Canceled>, todas as continuações registradas com a tarefa ou agendadas são executadas, a menos que uma opção de continuação como <xref:System.Threading.Tasks.TaskContinuationOptions.NotOnCanceled> tenha sido especificada para optar pela não continuação. Qualquer código que esteja aguardando de forma assíncrona uma tarefa cancelada com o uso de recursos de linguagem continuará a ser executada, mas receberá <xref:System.OperationCanceledException> ou uma exceção derivada dela. O código bloqueado que espera de forma síncrona a tarefa através de métodos como <xref:System.Threading.Tasks.Task.Wait%2A> e <xref:System.Threading.Tasks.Task.WaitAll%2A> também continuará a ser executado com uma exceção.  
  
 Se um token de cancelamento solicitou o cancelamento antes do método TAP que aceita esse token ter sido chamado, o método TAP deve retornar uma tarefa <xref:System.Threading.Tasks.TaskStatus.Canceled>.  No entanto, se o cancelamento é solicitado enquanto a operação assíncrona é executada, a operação assíncrona não precisa aceitar a solicitação de cancelamento.  A tarefa retornada deverá terminar no estado <xref:System.Threading.Tasks.TaskStatus.Canceled> somente se a operação terminar como um resultado da solicitação de cancelamento. Se o cancelamento for solicitado, mas um resultado ou uma exceção ainda forem gerados, a tarefa deve terminar no estado <xref:System.Threading.Tasks.TaskStatus.RanToCompletion> ou <xref:System.Threading.Tasks.TaskStatus.Faulted>.

 Para métodos assíncronos que desejam expor a capacidade de ser cancelada primeiro e mais importante, você não precisa fornecer uma sobrecarga que não aceite um token de cancelamento. Para os métodos que não podem ser cancelados, não forneça sobrecargas que aceitem um token de cancelamento; isso ajuda a indicar ao chamador se o método de destino é realmente cancelável.  O código consumidor que não deseja cancelamento pode chamar um método que aceita um <xref:System.Threading.CancellationToken> e fornece <xref:System.Threading.CancellationToken.None%2A> como o valor do argumento. <xref:System.Threading.CancellationToken.None%2A> é funcionalmente equivalente ao <xref:System.Threading.CancellationToken> padrão.  
  
## <a name="progress-reporting-optional"></a>Relatório de progresso (opcional)  
 Algumas operações assíncronas beneficiam-se do fornecimento de notificações de progresso; esses são normalmente usados para atualizar uma interface do usuário com informações sobre o andamento da operação assíncrona.

 No TAP, o progresso é tratado por uma interface <xref:System.IProgress%601> que é passada para o método assíncrono como um parâmetro que é chamado geralmente de `progress`.  Fornecer a interface de progresso quando o método assíncrono é chamado ajuda a eliminar as condições de corrida resultantes do uso incorreto (isto é, quando os manipuladores de eventos registrados incorretamente depois que a operação é iniciada podem carecer de atualizações).  Mais importante, a interface de progresso suporta implementações de variação de progresso, conforme determinado pelo código consumidor.  Por exemplo, o código consumidor só pode importar-se com a última atualização de progresso, pode desejar armazenar em buffer todas as atualizações, pode querer chamar uma ação para cada atualização ou pode desejar controlar se a chamada é vinculada a um thread específico. Todas essas opções podem ser obtidas usando uma implementação diferente da interface, personalizada para as necessidades específicas do consumidor.  Assim como no cancelamento, as implementações de TAP devem fornecer um parâmetro de <xref:System.IProgress%601> somente se a API oferecer suporte a notificações de progresso.

 Por exemplo, se o método `ReadAsync` discutido anteriormente nesse artigo for capaz de relatar o progresso intermediário na forma do número de bytes lidos até aqui, o retorno de chamada de progresso poderá ser uma interface <xref:System.IProgress%601>:  
  
 [!code-csharp[Conceptual.TAP#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.tap/cs/examples1.cs#2)]
 [!code-vb[Conceptual.TAP#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.tap/vb/examples1.vb#2)]  
  
 Se um `FindFilesAsync` método retornar uma lista de todos os arquivos que atendem a um padrão de pesquisa específico, o retorno de chamada de progresso poderá fornecer uma estimativa do percentual de trabalho concluído e o conjunto atual de resultados parciais. Ele pode fornecer essas informações com uma tupla:  
  
 [!code-csharp[Conceptual.TAP#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.tap/cs/examples1.cs#3)]
 [!code-vb[Conceptual.TAP#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.tap/vb/examples1.vb#3)]  
  
 ou com um tipo de dados que é específico para a API:  
  
 [!code-csharp[Conceptual.TAP#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.tap/cs/examples1.cs#4)]
 [!code-vb[Conceptual.TAP#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.tap/vb/examples1.vb#4)]  
  
 No último caso, o tipo de dados especial geralmente é sufixado com `ProgressInfo`.  
  
 Se as implementações de toque fornecem sobrecargas que aceitam um `progress` parâmetro, elas devem permitir que o argumento seja `null` , caso em que nenhum progresso é relatado. As implementações do TAP devem relatar o progresso ao <xref:System.Progress%601> objeto de forma síncrona, o que permite que o método assíncrono forneça rapidamente o progresso. Ele também permite que o consumidor do progresso determine como e onde melhor lidar com as informações. Por exemplo, a instância de progresso poderia optar por controlar retornos de chamada e gerar eventos em um contexto de sincronização capturado.  
  
## <a name="iprogresst-implementations"></a>\<T>Implementações de IProgress  
 O .NET Framework 4.5 fornece uma implementação única de <xref:System.IProgress%601>: <xref:System.Progress%601>. A classe <xref:System.Progress%601> é declarada da seguinte forma:  
  
```csharp  
public class Progress<T> : IProgress<T>  
{  
    public Progress();  
    public Progress(Action<T> handler);  
    protected virtual void OnReport(T value);  
    public event EventHandler<T> ProgressChanged;  
}  
```  
  
```vb  
Public Class Progress(Of T) : Inherits IProgress(Of T)  
    Public Sub New()  
    Public Sub New(handler As Action(Of T))  
    Protected Overridable Sub OnReport(value As T)  
    Public Event ProgressChanged As EventHandler(Of T>  
End Class  
```  
  
 Uma instância de <xref:System.Progress%601> expõe um evento <xref:System.Progress%601.ProgressChanged>, o qual é gerado sempre que a operação assíncrona relata uma atualização de progresso. O evento <xref:System.Progress%601.ProgressChanged> é gerado no objeto de <xref:System.Threading.SynchronizationContext> que foi capturado quando a instância de <xref:System.Progress%601> foi criada. Se não havia contexto de sincronização disponível, um contexto padrão que tem como alvo o pool de segmentos é usado. É possível registrar manipuladores com esse evento. Um único manipulador também pode ser fornecido para o construtor <xref:System.Progress%601> para maior conveniência. Seu comportamento é exatamente como um manipulador de eventos para o evento <xref:System.Progress%601.ProgressChanged>. As atualizações de progresso são geradas de forma assíncrona para evitar atrasar a operação assíncrona enquanto os manipuladores de eventos são executados. Outra implementação de <xref:System.IProgress%601> pode optar por aplicar uma semântica diferente.  
  
## <a name="choosing-the-overloads-to-provide"></a>Escolhendo as sobrecargas a serem fornecidas  
 Se uma implementação de TAP ambos os parâmetros <xref:System.Threading.Tasks.TaskFactory.CancellationToken%2A> e <xref:System.IProgress%601> opcionais, ela poderá potencialmente exigir até quatro cargas de trabalho:  
  
```csharp  
public Task MethodNameAsync(…);  
public Task MethodNameAsync(…, CancellationToken cancellationToken);  
public Task MethodNameAsync(…, IProgress<T> progress);
public Task MethodNameAsync(…,
    CancellationToken cancellationToken, IProgress<T> progress);  
```  
  
```vb  
Public MethodNameAsync(…) As Task  
Public MethodNameAsync(…, cancellationToken As CancellationToken cancellationToken) As Task  
Public MethodNameAsync(…, progress As IProgress(Of T)) As Task
Public MethodNameAsync(…, cancellationToken As CancellationToken,
                       progress As IProgress(Of T)) As Task  
```  
  
 No entanto, muitas implementações de toque não fornecem recursos de cancelamento ou de progresso, portanto, eles exigem um único método:  
  
```csharp  
public Task MethodNameAsync(…);  
```  
  
```vb  
Public MethodNameAsync(…) As Task  
```  
  
 Se uma implementação de TAP oferecer suporte a cancelamento ou progresso, mas não a ambos, ela poderá fornecer duas sobrecargas:  
  
```csharp  
public Task MethodNameAsync(…);  
public Task MethodNameAsync(…, CancellationToken cancellationToken);  
  
// … or …  
  
public Task MethodNameAsync(…);  
public Task MethodNameAsync(…, IProgress<T> progress);  
```  
  
```vb  
Public MethodNameAsync(…) As Task  
Public MethodNameAsync(…, cancellationToken As CancellationToken) As Task  
  
' … or …  
  
Public MethodNameAsync(…) As Task  
Public MethodNameAsync(…, progress As IProgress(Of T)) As Task  
```  
  
 Se uma implementação de TAP oferecer suporte a cancelamento e progresso, ela poderá expor todas as quatro sobrecargas. No entanto, ela pode fornecer somente estas duas:  
  
```csharp  
public Task MethodNameAsync(…);  
public Task MethodNameAsync(…,
    CancellationToken cancellationToken, IProgress<T> progress);  
```  
  
```vb  
Public MethodNameAsync(…) As Task  
Public MethodNameAsync(…, cancellationToken As CancellationToken,
                       progress As IProgress(Of T)) As Task  
```  
  
 Para compensar as duas combinações intermediárias ausentes, os desenvolvedores podem passar <xref:System.Threading.CancellationToken.None%2A> ou um <xref:System.Threading.CancellationToken> padrão para o parâmetro `cancellationToken` e `null` para o parâmetro `progress`.  
  
 Se você esperar que cada uso do método TAP ofereça suporte ao cancelamento ou ao progresso, poderá omitir as sobrecargas que não aceitam o parâmetro relevante.  
  
 Se você decidir expor várias sobrecargas para tornar o cancelamento ou o progresso opcional, as sobrecargas que não suportam o cancelamento ou o progresso devem se comportar como se estivessem <xref:System.Threading.CancellationToken.None%2A> para cancelamento ou `null` para o progresso para a sobrecarga que oferece suporte a esses.  
  
## <a name="related-articles"></a>Artigos relacionados
  
|Title|Descrição|  
|-----------|-----------------|  
|[Padrões de programação assíncrona](index.md)|Apresenta os três padrões para execução de operações assíncronas: TAP (Padrão Assíncrono Baseado em Tarefas), APM (Modelo de Programação Assíncrona) e EAP (Padrão Assíncrono Baseado em Eventos).|  
|[Implementando o padrão assíncrono baseado em tarefa](implementing-the-task-based-asynchronous-pattern.md)|Descreve como implementar o TAP de três formas: usando os compiladores C# e Visual Basic no Visual Studio, manualmente ou por meio de uma combinação de compilador com o método manual.|  
|[Consumindo o padrão assíncrono baseado em tarefa](consuming-the-task-based-asynchronous-pattern.md)|Descreve como você pode usar tarefas e retornos de chamada para implementar a espera sem causar bloqueios.|  
|[Interoperabilidade com outros tipos e padrões assíncronos](interop-with-other-asynchronous-patterns-and-types.md)|Descreve como usar o TAP (Padrão Assíncrono Baseado em Tarefas) para implementar o APM (Modelo de Programação Assíncrona) e o EAP (Padrão Assíncrono Baseado em Eventos).|
