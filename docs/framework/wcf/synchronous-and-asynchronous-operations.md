---
title: Operações síncronas e assíncronas
description: Saiba mais sobre como implementar e chamar operações de serviço assíncronas. Os clientes e serviços WCF podem usar operações assíncronas em dois níveis do aplicativo.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- service contracts [WCF], synchronous operations
- service contracts [WCF], asynchronous operations
ms.assetid: db8a51cb-67e6-411b-9035-e5821ed350c9
ms.openlocfilehash: f52f2613c96c0149c330bb75f80c6738f8d41146
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245903"
---
# <a name="synchronous-and-asynchronous-operations"></a>Operações síncronas e assíncronas
Este tópico discute como implementar e chamar as operações de serviço assíncronas.  
  
 Muitos aplicativos chamam métodos de forma assíncrona porque isso permite que o aplicativo continue a fazer trabalho útil enquanto a chamada do método é executada. Os serviços e clientes do Windows Communication Foundation (WCF) podem participar de chamadas de operações assíncronas em dois níveis diferentes do aplicativo, o que fornece aos aplicativos do WCF ainda mais flexibilidade para maximizar a taxa de transferência balanceada contra interatividade.  
  
## <a name="types-of-asynchronous-operations"></a>Tipos de operações assíncronas  
 Todos os contratos de serviço WCF, independentemente dos tipos de parâmetros e valores retornados, usam os atributos do WCF para especificar um padrão de troca de mensagens específico entre o cliente e o serviço. O WCF roteia automaticamente mensagens de entrada e de saída para a operação de serviço apropriada ou o código cliente em execução.  
  
 O cliente possui apenas o contrato de serviço, que especifica o padrão de troca de mensagens para uma operação específica. Os clientes podem oferecer ao desenvolvedor qualquer modelo de programação que escolherem, desde que o padrão subjacente de troca de mensagens seja observado. Portanto, os serviços também podem implementar operações de qualquer maneira, desde que o padrão de mensagem especificado seja observado.  
  
 A independência do contrato de serviço na implementação do serviço ou do cliente permite os seguintes formulários de execução assíncrona em aplicativos do WCF:  
  
- Os clientes podem chamar operações de solicitação/resposta de forma assíncrona usando uma troca de mensagens síncrona.  
  
- Os serviços podem implementar uma operação de solicitação/resposta de forma assíncrona usando uma troca de mensagens síncrona.  
  
- As trocas de mensagens podem ser unidirecionais, independentemente da implementação do cliente ou do serviço.  
  
### <a name="suggested-asynchronous-scenarios"></a>Cenários assíncronos sugeridos  
 Use uma abordagem assíncrona em uma implementação de operação do serviço se a implementação do serviço da operação fizer uma chamada de bloqueio, como ao executar trabalho de E/S. Quando você estiver em uma implementação de operação assíncrona, tente chamar operações e métodos assíncronos para estender ao máximo o caminho da chamada assíncrona. Por exemplo, chame um `BeginOperationTwo()` de dentro de `BeginOperationOne()`.  
  
- Use uma abordagem assíncrona em um aplicativo cliente ou de chamada nos seguintes casos:  
  
- Se você estiver chamando operações em um aplicativo de camada intermediária. (Para saber mais sobre esses cenários, confira [Aplicativos cliente de camada intermediária](./feature-details/middle-tier-client-applications.md).)  
  
- Se você estiver chamando operações em uma página do ASP.NET, use páginas assíncronas.  
  
- Se você estiver chamando operações em qualquer aplicativo que seja single-threaded, como o Windows Forms ou o Windows Presentation Foundation (WPF). Ao usar o modelo de chamada assíncrona com base em eventos, o evento resultante é gerado no thread da interface do usuário, adicionando capacidade de resposta ao aplicativo sem exigir que você mesmo manipule vários threads.  
  
- Em geral, se você puder escolher entre uma chamada síncrona e uma chamada assíncrona, escolha a chamada assíncrona.  
  
### <a name="implementing-an-asynchronous-service-operation"></a>Implementando uma operação de serviço assíncrona  
 As operações assíncronas podem ser implementadas usando um dos três métodos a seguir:  
  
1. O padrão assíncrono baseado em tarefas  
  
2. O padrão assíncrono baseado em eventos  
  
3. O padrão assíncrono IAsyncResult  
  
#### <a name="task-based-asynchronous-pattern"></a>O padrão assíncrono baseado em tarefas  
 O padrão assíncrono baseado em tarefas é o modo preferido de implementar operações assíncronas porque é mais fácil e mais simples. Para usar esse método, basta implementar a operação de serviço e especificar um tipo de retorno de tarefa \<T> , em que T é o tipo retornado pela operação lógica. Por exemplo:  
  
```csharp  
public class SampleService:ISampleService
{
   // ...  
   public async Task<string> SampleMethodTaskAsync(string msg)
   {
      return Task<string>.Factory.StartNew(() =>
      {
         return msg;
      });
   }  
   // ...  
}  
```  
  
 A operação SampleMethodTaskAsync retorna a tarefa \<string> porque a operação lógica retorna uma cadeia de caracteres. Para saber mais sobre o padrão assíncrono baseado em tarefas, confira [O padrão assíncrono baseado em tarefas](https://go.microsoft.com/fwlink/?LinkId=232504).  
  
> [!WARNING]
> Ao usar o padrão assíncrono baseado em tarefas, uma T: System.AggregateException pode ser gerada se uma exceção ocorrer ao aguardar a conclusão da operação. Esta exceção pode ocorrer no cliente ou nos serviços  
  
#### <a name="event-based-asynchronous-pattern"></a>O padrão assíncrono baseado em eventos  
 Um serviço que dá suporte ao padrão assíncrono baseada em eventos terá uma ou mais operações chamadas MethodNameAsync. Esses métodos podem espelhar versões síncronas, que realizam a mesma operação no thread atual. A classe também pode ter um evento MethodNameCompleted e pode ter um método MethodNameAsyncCancel (ou simplesmente CancelAsync). Um cliente que deseja chamar a operação definirá um manipulador de eventos para ser chamado quando a operação for concluída,  
  
 O snippet de código a seguir ilustra como declarar operações assíncronas usando o padrão assíncrono baseado em eventos.  
  
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
  
 Para saber mais sobre o padrão assíncrono baseado em eventos, confira [O padrão assíncrono baseado em eventos](../../standard/asynchronous-programming-patterns/event-based-asynchronous-pattern-overview.md).  
  
#### <a name="iasyncresult-asynchronous-pattern"></a>O padrão assíncrono IAsyncResult  
 Uma operação de serviço pode ser implementada de maneira assíncrona usando o padrão de programação assíncrona .NET Framework e marcando o `<Begin>` método com a <xref:System.ServiceModel.OperationContractAttribute.AsyncPattern%2A> propriedade definida como `true` . Nesse caso, a operação assíncrona é exposta nos metadados no mesmo formulário que uma operação síncrona: é exposta como única operação com uma mensagem de solicitação e uma mensagem de resposta correlacionada. Os modelos de programação do cliente têm uma opção. Podem representar esse padrão como uma operação síncrona ou assíncrona, desde que, quando o serviço for chamado, ocorra uma troca de mensagens de solicitação de resposta.  
  
 Em geral, com a natureza assíncrona dos sistemas, você não deve usar uma dependência nos threads.  A maneira mais confiável de transmitir dados para vários estágios do processamento da expedição da operação é usar extensões.  
  
 Para ter um exemplo, confira [Como implementar uma operação de serviço assíncrona](how-to-implement-an-asynchronous-service-operation.md).  
  
 Para definir uma operação `X` do contrato que é executada de forma assíncrona independentemente de como é chamada no aplicativo cliente:  
  
- Defina dois métodos usando o padrão `BeginOperation` e `EndOperation`.  
  
- O método `BeginOperation` inclui os parâmetros `in` e `ref` para a operação e retorna um tipo <xref:System.IAsyncResult>.  
  
- O método `EndOperation` inclui um parâmetro <xref:System.IAsyncResult> e também os parâmetros `out` e `ref` e retorna o tipo de retorno das operações.  
  
 Por exemplo, consulte o seguinte método.  
  
```csharp  
int DoWork(string data, ref string inout, out string outonly)  
```  
  
```vb  
Function DoWork(ByVal data As String, ByRef inout As String, _out outonly As out) As Integer  
```  
  
 Para criar uma operação assíncrona, os dois métodos seriam:  
  
```csharp  
[OperationContract(AsyncPattern=true)]
IAsyncResult BeginDoWork(string data,
                         ref string inout,
                         AsyncCallback callback,
                         object state);
int EndDoWork(ref string inout, out string outonly, IAsyncResult result);  
```  
  
```vb  
<OperationContract(AsyncPattern := True)>
Function BeginDoWork(ByVal data As String, _
                     ByRef inout As String, _
                     ByVal callback As AsyncCallback, _
                     ByVal state As Object) As IAsyncResult
Function EndDoWork(ByRef inout As String, ByRef outonly As String, ByVal result As IAsyncResult) As Integer  
```  
  
> [!NOTE]
> O atributo <xref:System.ServiceModel.OperationContractAttribute> é aplicado apenas ao método `BeginDoWork`. O contrato resultante tem uma operação WSDL denominada `DoWork`.  
  
### <a name="client-side-asynchronous-invocations"></a>Chamadas assíncronas do lado do cliente  
 Um aplicativo cliente do WCF pode usar qualquer um dos três modelos de chamada assíncrona descritos anteriormente  
  
 Ao usar o modelo baseado em tarefas, basta chamar a operação usando a palavra-chave de espera conforme mostrado no snippet de código a seguir.  
  
```csharp  
await simpleServiceClient.SampleMethodTaskAsync("hello, world");  
```  
  
 Usar o padrão assíncrono baseado em eventos requer apenas adicionar um manipulador de eventos para receber uma notificação da resposta -- e o evento resultante é gerado automaticamente no thread da interface do usuário. Para usar essa abordagem, especifique as opções de comando **/async** e **/tcv:Version35** com a [Ferramenta Utilitário de Metadados ServiceModel (Svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md), como no exemplo a seguir.  
  
```console  
svcutil http://localhost:8000/servicemodelsamples/service/mex /async /tcv:Version35  
```  
  
 Quando isso é feito, Svcutil.exe gera uma classe de cliente do WCF com a infraestrutura do evento que permite que o aplicativo de chamada implemente e atribua um manipulador de eventos para receber a resposta e executar a ação apropriada. Para ver um exemplo completo, confira [Como chamar operações de serviço de forma assíncrona](./feature-details/how-to-call-wcf-service-operations-asynchronously.md).  
  
 No entanto, o modelo assíncrono baseado em evento só está disponível no .NET Framework 3,5. Além disso, não há suporte para ele mesmo no .NET Framework 3,5 quando um canal de cliente WCF é criado usando um <xref:System.ServiceModel.ChannelFactory%601?displayProperty=nameWithType> . Com objetos de canal de cliente do WCF, você deve usar objetos <xref:System.IAsyncResult?displayProperty=nameWithType> para chamar suas operações de forma assíncrona. Para usar essa abordagem, especifique a opção de comando **/async** com a [Ferramenta Utilitário de Metadados ServiceModel (Svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md), como no exemplo a seguir.  
  
```console  
svcutil http://localhost:8000/servicemodelsamples/service/mex /async
```  
  
 Isso gera um contrato de serviço no qual cada operação é modelada como um método `<Begin>` com a propriedade <xref:System.ServiceModel.OperationContractAttribute.AsyncPattern%2A> definida como `true` e um método `<End>` correspondente. Para ver um exemplo completo do uso de um <xref:System.ServiceModel.ChannelFactory%601>, confira [Como chamar operações assíncronas usando uma fábrica de canais](./feature-details/how-to-call-operations-asynchronously-using-a-channel-factory.md).  
  
 Em ambos os casos, os aplicativos podem chamar uma operação de forma assíncrona mesmo que o serviço seja implementado de forma síncrona, da mesma forma que um aplicativo pode usar o mesmo padrão para chamar de forma assíncrona um método síncrono local. Como a operação é implementada não é significativo para o cliente. Quando a mensagem de resposta chega, seu conteúdo é expedido para o método <`End`> assíncrono do cliente e o cliente recupera as informações.  
  
### <a name="one-way-message-exchange-patterns"></a>Padrões de troca de mensagens unidirecional  
 Você também pode criar um padrão de troca de mensagens assíncronas no qual as operações unidirecionais (operações na quais <xref:System.ServiceModel.OperationContractAttribute.IsOneWay%2A?displayProperty=nameWithType> é `true` não têm resposta correlacionada) podem ser enviadas em qualquer direção pelo cliente ou pelo serviço independentemente do outro lado. (Isso usa o padrão de troca de mensagens duplex com mensagens unidirecionais.) Nesse caso, o contrato de serviço especifica uma troca de mensagens unidirecional que o lado pode implementar como chamadas ou implementações assíncronas, ou não, conforme apropriado. Em geral, quando o contrato é uma troca de mensagens unidirecional, as implementações podem ser amplamente assíncronas porque, depois que uma mensagem é enviada, o aplicativo não espera uma resposta e pode continuar a fazer outro trabalho.  
  
### <a name="event-based-asynchronous-clients-and-message-contracts"></a>Clientes e contratos de mensagens assíncronas com base em eventos  
 As diretrizes de design do modelo assíncrono baseado em eventos declaram que, se mais de um valor for retornado, um valor será retornado como a propriedade `Result` e os outros serão retornados como propriedades no objeto <xref:System.EventArgs>. Um resultado é que, se um cliente importar metadados usando as opções de comando assíncrono com base em eventos, e a operação retornar mais de um valor, o objeto padrão <xref:System.EventArgs> retornará um valor como a propriedade `Result` e os restantes serão propriedades do objeto <xref:System.EventArgs>.  
  
 Se você desejar receber o objeto de mensagem como a propriedade `Result` e ter valores retornados como propriedades nesse objeto, use a opção de comando **/messageContract**. Isso gera uma assinatura que retorna a mensagem de resposta como a propriedade `Result` no objeto <xref:System.EventArgs>. Todos os valores de retorno internos são propriedades do objeto da mensagem de resposta.  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.OperationContractAttribute.IsOneWay%2A>
- <xref:System.ServiceModel.OperationContractAttribute.AsyncPattern%2A>
