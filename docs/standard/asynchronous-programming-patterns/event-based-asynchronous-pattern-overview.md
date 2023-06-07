---
title: Visão geral do padrão assíncrono baseado em evento
description: Examine os padrões assíncronos baseados em eventos (EAPs) no .NET, que disponibilizam as vantagens dos aplicativos multissegmentados, mas ocultam algumas complexidades de design.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Event-based Asynchronous Pattern
- ProgressChangedEventArgs class
- BackgroundWorker component
- events [.NET Framework], asynchronous
- Asynchronous Pattern
- AsyncOperationManager class
- threading [.NET Framework], asynchronous features
- AsyncOperation class
- AsyncCompletedEventArgs class
ms.assetid: 792aa8da-918b-458e-b154-9836b97735f3
ms.openlocfilehash: 18fbdb29e5a1fb02601dea00964538144c07122c
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84768853"
---
# <a name="event-based-asynchronous-pattern-overview"></a>Visão geral do padrão assíncrono baseado em evento
Aplicativos que realizam várias tarefas simultaneamente, mas que ainda permanecem responsivos para interação com o usuário, geralmente exigem um projeto que utilize vários threads. O namespace <xref:System.Threading> oferece todas as ferramentas necessárias para criar aplicativos commulti-thread de alto desempenho. No entanto, usar essas ferramentas de maneira eficaz requer um nível de experiência significativo com engenharia de software com multi-thread. Para aplicativos com multi-thread relativamente simples, o componente <xref:System.ComponentModel.BackgroundWorker> oferece uma solução direta. Para aplicativos assíncronos mais sofisticados, considere implementar uma classe que adere ao Padrão Assíncrono baseado em Evento.  
  
 O Padrão Assíncrono baseado em Evento oferece as vantagens de aplicativos com multi-threaded e, ao mesmo tempo, não apresenta muitos dos problemas complexos inerentes em projetos com multi-thread. Usar uma classe com suporte a esse padrão pode permitir que você:  
  
- Realize tarefas que consomem tempo, como downloads e operações do banco de dados, "em segundo plano", sem interromper o aplicativo.  
  
- Execute várias operações simultaneamente, recebendo notificações quando cada uma delas for concluída.  
  
- Espere até que recursos sejam disponibilizados sem interromper ("bloquear") seu aplicativo.  
  
- Comunicar-se com operações assíncronas pendentes usando o modelo familiar de eventos e representantes. Para saber mais sobre como usar manipuladores e representantes de eventos, confira [Eventos](../events/index.md).  
  
 Uma classe compatível com o Padrão Assíncrono baseado em Evento terá um ou mais métodos denominados _MethodName_**Async**. Esses métodos podem espelhar versões síncronas, que realizam a mesma operação no thread atual. A classe também pode ter um evento _MethodName_**Completed** e pode ter um método _MethodName_**AsyncCancel** (ou simplesmente **CancelAsync**).  
  
 <xref:System.Windows.Forms.PictureBox> é um componente típico que oferece suporte ao Padrão Assíncrono baseado em Evento. Você pode baixar uma imagem de maneira síncrona chamando seu método <xref:System.Windows.Forms.PictureBox.Load%2A>, mas se a imagem for grande, ou se a conexão de rede estiver lenta, seu aplicativo deixará de responder até que a operação de download seja concluída e a chamada para <xref:System.Windows.Forms.PictureBox.Load%2A> retorne.  
  
 Se quiser que seu aplicativo continue em execução enquanto a imagem estiver carregando, você pode chamar o método <xref:System.Windows.Forms.PictureBox.LoadAsync%2A> e lidar com o evento <xref:System.Windows.Forms.PictureBox.LoadCompleted>, assim como lidaria com qualquer outro evento. Ao chamar o método <xref:System.Windows.Forms.PictureBox.LoadAsync%2A>, se aplicativo continuará a ser executado enquanto o download continua em um thread separado ("em segundo plano"). Seu manipulador de evento será chamado quando a operação de carregamento da imagem estiver concluída e o manipulador de evento puder examinar o parâmetro <xref:System.ComponentModel.AsyncCompletedEventArgs> para determinar se o download foi concluído com sucesso.  
  
 O Padrão Assíncrono baseado em Evento requer que uma operação assíncrona possa ser cancelada, e o controle <xref:System.Windows.Forms.PictureBox> oferece suporte a esse requisito com seu método <xref:System.Windows.Forms.PictureBox.CancelAsync%2A>. Chamar <xref:System.Windows.Forms.PictureBox.CancelAsync%2A> envia uma solicitação para interromper o download pendente e, quando a tarefa é cancelada, o evento <xref:System.Windows.Forms.PictureBox.LoadCompleted> é elevado.  
  
> [!CAUTION]
> É possível que o download termine assim que a solicitação <xref:System.Windows.Forms.PictureBox.CancelAsync%2A> seja enviada, por isso, <xref:System.ComponentModel.AsyncCompletedEventArgs.Cancelled%2A> pode não refletir a solicitação de cancelamento. Isso é chamado de *condição de corrida* e é um problema comum na programação com multi-thread. Para saber mais sobre problemas de programação multi-thread, confira [Práticas recomendadas de threading gerenciado](../threading/managed-threading-best-practices.md).  
  
## <a name="characteristics-of-the-event-based-asynchronous-pattern"></a>Características do Padrão Assíncrono baseado em Evento  
 O Padrão Assíncrono baseado em Evento pode assumir diversos formatos, dependendo da complexidade das operações compatíveis com uma classe específica. As classes mais simples podem ter um único método _MethodName_**Async** e um evento _MethodName_**Completed** correspondente. Classes mais complexas podem ter vários métodos _MethodName_**Async**, cada um com um evento _MethodName_**Completed** correspondente, bem como versões síncronas desses métodos. As classes podem opcionalmente oferecer suporte a cancelamento, relatórios de progresso e resultados incrementais para cada método assíncrono.  
  
 Um método assíncrono também pode oferecer suporte a várias chamadas pendentes (várias invocações simultâneas), permitindo que seu código o chame quantas vezes desejar antes de concluir outras operações pendentes. Lidar da maneira correta com essa situação pode exigir que seu aplicativo acompanhe a conclusão de cada operação.  
  
### <a name="examples-of-the-event-based-asynchronous-pattern"></a>Exemplos do Padrão Assíncrono baseado em Evento  
 Os componentes <xref:System.Media.SoundPlayer> e <xref:System.Windows.Forms.PictureBox> representam implementações simples do Padrão Assíncrono baseado em Evento. Os componentes <xref:System.Net.WebClient> e <xref:System.ComponentModel.BackgroundWorker> representam implementações mais complexas do Padrão Assíncrono baseado em Evento.  
  
 Abaixo está uma declaração de classe de exemplo que está em conformidade com o padrão:  
  
```vb  
Public Class AsyncExample  
    ' Synchronous methods.  
    Public Function Method1(ByVal param As String) As Integer
    Public Sub Method2(ByVal param As Double)
  
    ' Asynchronous methods.  
    Overloads Public Sub Method1Async(ByVal param As String)
    Overloads Public Sub Method1Async(ByVal param As String, ByVal userState As Object)
    Public Event Method1Completed As Method1CompletedEventHandler  
  
    Overloads Public Sub Method2Async(ByVal param As Double)
    Overloads Public Sub Method2Async(ByVal param As Double, ByVal userState As Object)
    Public Event Method2Completed As Method2CompletedEventHandler  
  
    Public Sub CancelAsync(ByVal userState As Object)
  
    Public ReadOnly Property IsBusy () As Boolean  
  
    ' Class implementation not shown.  
End Class  
```  
  
```csharp  
public class AsyncExample  
{  
    // Synchronous methods.  
    public int Method1(string param);  
    public void Method2(double param);  
  
    // Asynchronous methods.  
    public void Method1Async(string param);  
    public void Method1Async(string param, object userState);  
    public event Method1CompletedEventHandler Method1Completed;  
  
    public void Method2Async(double param);  
    public void Method2Async(double param, object userState);  
    public event Method2CompletedEventHandler Method2Completed;  
  
    public void CancelAsync(object userState);  
  
    public bool IsBusy { get; }  
  
    // Class implementation not shown.  
}  
```  
  
 A classe fictícia `AsyncExample` possui dois métodos, ambos com suporte a invocações síncronas e assíncronas. As sobrecargas síncronas se comportam como qualquer chamada de método e executam a operação no thread de chamada; se a operação consumir tempo, pode haver um atrasa notável antes de a chamada retornar. As sobrecargas assíncronas iniciarão a operação em outro thread e depois retornarão imediatamente, permitindo que o thread de chamada prossiga enquanto a operação é executada "em segundo plano".  
  
### <a name="asynchronous-method-overloads"></a>Sobrecargas de Método Assíncronas  
 Há possivelmente duas sobrecargas para as operações assíncronas: invocação simples e invocação múltipla. Você pode distinguir essas duas formas por suas assinaturas de método: a forma de invocação múltipla tem um parâmetro extra chamado `userState`. Essa forma faz com que seja possível para seu código chama `Method1Async(string param, object userState)` diversas vezes sem esperar até que qualquer operação assíncrona pendente seja concluída. Se, por outro lado, você tentar chamar `Method1Async(string param)` antes de uma invocação anterior tiver sido concluída, o método eleva um <xref:System.InvalidOperationException>.  
  
 O parâmetro `userState` para as sobrecargas de invocação múltipla permite que você faça a distinção entre as operações assíncronas. Você fornece um valor exclusivo (por exemplo, um GUID ou código hash) para cada chamada para `Method1Async(string param, object userState)`, e quando cada operação for concluída, seu manipulador de evento poderá determinar qual instância da operação elevou o evento de conclusão.  
  
### <a name="tracking-pending-operations"></a>Acompanhando Operações Pendentes  
 Se você usar as sobrecargas de invocação múltipla, seu código precisará monitorar os objetos `userState` (IDs de tarefa) para as tarefas pendentes. Para cada chamada para `Method1Async(string param, object userState)`, você geralmente gera um novo objeto `userState` exclusivo e o adiciona a uma coleção. Quando a tarefa correspondente a esse objeto `userState` eleva o evento de conclusão, sua implementação do método de conclusão irá examinar <xref:System.ComponentModel.AsyncCompletedEventArgs.UserState%2A?displayProperty=nameWithType> e removê-lo da coleção. Usado dessa forma, o parâmetro `userState` assume a função de uma ID de tarefa.  
  
> [!NOTE]
> Você deve ter o cuidado de fornecer um valor exclusivo para `userState` em suas chamadas para sobrecargas de invocação múltipla. IDs de tarefa não exclusivos farão com que a classe assíncrona lance um <xref:System.ArgumentException>.  
  
### <a name="canceling-pending-operations"></a>Cancelando Operações Pendentes  
 É importante poder cancelar operações assíncronas a qualquer momento antes de sua conclusão. Classes que implementam o Padrão Assíncrono baseado em Evento terão um método `CancelAsync` (se houver somente um método assíncrono) ou um método _MethodName_**AsyncCancel** (se houver vários métodos assíncronos).  
  
 Métodos que permitem várias invocações assumem um parâmetro `userState`, que pode ser usado para acompanhar o tempo de vida de cada tarefa. `CancelAsync` utiliza um parâmetro `userState`, que permite o cancelamento de tarefas pendentes específicas.  
  
 Métodos com suporte a somente uma única operação pendente por vez, como `Method1Async(string param)`, não podem ser cancelados.  
  
### <a name="receiving-progress-updates-and-incremental-results"></a>Recebendo Atualizações de Andamento e Resultados Incrementais  
 Uma classe que adere ao Padrão Assíncrono baseado em Evento pode opcionalmente oferecer um evento para acompanhar o andamento e os resultados incrementais. Ele geralmente será denominado `ProgressChanged` ou _MethodName_**ProgressChanged** e seu manipulador de evento correspondente assumirá um parâmetro <xref:System.ComponentModel.ProgressChangedEventArgs>.  
  
 O manipulador de evento para o evento `ProgressChanged` pode examinar a propriedade <xref:System.ComponentModel.ProgressChangedEventArgs.ProgressPercentage%2A?displayProperty=nameWithType> para determinar qual porcentagem de uma tarefa assíncrona foi concluída. Essa propriedade estará entre 0 e 100, e pode ser usada para atualizar a propriedade <xref:System.Windows.Forms.ProgressBar.Value%2A> de um <xref:System.Windows.Forms.ProgressBar>. Se várias operações assíncronas estiverem pendentes, você poderá usar a propriedade <xref:System.ComponentModel.ProgressChangedEventArgs.UserState%2A?displayProperty=nameWithType> para distinguir qual operação está relatando o andamento.  
  
 Algumas classes podem relatar resultados incrementais conforme as operações assíncronas prosseguem. Esses resultados serão armazenados em uma classe derivada de <xref:System.ComponentModel.ProgressChangedEventArgs> e aparecerão como propriedades na classe derivada. Você pode acessar esses resultados no manipulador de eventos para o evento `ProgressChanged`, assim como acessaria a propriedade <xref:System.ComponentModel.ProgressChangedEventArgs.ProgressPercentage%2A>. Se várias operações assíncronas estiverem pendentes, você poderá usar a propriedade <xref:System.ComponentModel.ProgressChangedEventArgs.UserState%2A> para distinguir qual operação está relatando resultados incrementais.  
  
## <a name="see-also"></a>Veja também

- <xref:System.ComponentModel.ProgressChangedEventArgs>
- <xref:System.ComponentModel.BackgroundWorker>
- <xref:System.ComponentModel.AsyncCompletedEventArgs>
- [Como usar componentes compatíveis com o padrão assíncrono baseado em evento](how-to-use-components-that-support-the-event-based-asynchronous-pattern.md)
- [Como executar uma operação no plano de fundo](../../framework/winforms/controls/how-to-run-an-operation-in-the-background.md)
- [Como implementar um formulário que usa uma operação em segundo plano](../../framework/winforms/controls/how-to-implement-a-form-that-uses-a-background-operation.md)
- [EAP (Padrão Assíncrono baseado em Evento)](event-based-asynchronous-pattern-eap.md)
- [Práticas recomendadas para a implementação do padrão assíncrono baseado em evento](best-practices-for-implementing-the-event-based-asynchronous-pattern.md)
- [Decidindo quando implementar o padrão assíncrono baseado em evento](deciding-when-to-implement-the-event-based-asynchronous-pattern.md)
