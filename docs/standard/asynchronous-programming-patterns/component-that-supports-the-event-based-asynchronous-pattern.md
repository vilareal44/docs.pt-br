---
title: Como implementar um componente compatível com o padrão assíncrono baseado em evento
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
- components [.NET Framework], asynchronous
- AsyncOperation class
- threading [Windows Forms], asynchronous features
- AsyncCompletedEventArgs class
ms.assetid: 61f676b5-936f-40f6-83ce-f22805ec9c2f
ms.openlocfilehash: 83a60e0e793f33b8b0a1cec8342942fd05c82f55
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84289883"
---
# <a name="how-to-implement-a-component-that-supports-the-event-based-asynchronous-pattern"></a>Como implementar um componente compatível com o padrão assíncrono baseado em evento
Se você estiver escrevendo uma classe com algumas operações que possam causar atrasos notáveis, considere a opção de fornecer funcionalidade assíncrona Implementando a [visão geral de padrão assíncrono baseado em evento](event-based-asynchronous-pattern-overview.md).  
  
 Este passo a passo ilustra como criar um componente que implemente o Padrão Assíncrono Baseado em Evento. Ele é implementado usando classes auxiliares do namespace <xref:System.ComponentModel?displayProperty=nameWithType>, o que garante que o componente funcione corretamente em qualquer modelo de aplicativo, incluindo ASP.NET, aplicativos do Windows Forms e aplicativos de Console. Esse componente também é projetável com um controle <xref:System.Windows.Forms.PropertyGrid> e seus próprios designers personalizados.  
  
 Quando terminar, você terá um aplicativo que calcula números primos de forma assíncrona. O aplicativo terá um thread de IU (interface do usuário) do usuário principal e um thread para cada cálculo de números primos. Embora o teste para saber se um número grande é primo possa levar um tempo considerável, o thread de interface do usuário principal não será interrompido por esse atraso e o formulário responderá durante o cálculo. Você poderá executar tantos cálculos quantos desejar simultaneamente e cancelar seletivamente cálculos pendentes.  
  
 As tarefas ilustradas neste passo a passo incluem:  
  
- Criando o componente  
  
- Definir representantes e eventos assíncronos públicos  
  
- Definir representantes privados  
  
- Implementar eventos públicos  
  
- Implementar o método de conclusão  
  
- Implementar os métodos de trabalhador  
  
- Implementar métodos Iniciar e Cancelar  
  
 Para copiar o código deste tópico como uma única lista, confira [Como implementar um cliente do Padrão assíncrono baseado em evento](how-to-implement-a-client-of-the-event-based-asynchronous-pattern.md).  
  
## <a name="creating-the-component"></a>Criando o componente  
 A primeira etapa é criar o componente que implementará o Padrão Assíncrono Baseado em Evento.  
  
### <a name="to-create-the-component"></a>Para criar o componente  
  
- Criar uma classe chamada `PrimeNumberCalculator` que herda de <xref:System.ComponentModel.Component>.  
  
## <a name="defining-public-asynchronous-events-and-delegates"></a>Definir representantes e eventos assíncronos públicos  
 O componente se comunica com clientes usando eventos. O evento _MethodName_**Completed** alerta os clientes da conclusão de uma tarefa assíncrona, e o evento _MethodName_**ProgressChanged** informa os clientes do progresso de uma tarefa assíncrona.  
  
### <a name="to-define-asynchronous-events-for-clients-of-your-component"></a>Para definir eventos assíncronos para clientes do componente:  
  
1. Importe os namespaces <xref:System.Threading?displayProperty=nameWithType> e <xref:System.Collections.Specialized?displayProperty=nameWithType> na parte superior do arquivo.  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#11](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#11)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#11](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#11)]  
  
2. Antes da definição de classe `PrimeNumberCalculator`, declare representantes para eventos de andamento e conclusão.  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#7](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#7)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#7](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#7)]  
  
3. Na definição de classe `PrimeNumberCalculator`, declare eventos para relatar o andamento e a conclusão aos clientes.  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#8](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#8)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#8](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#8)]  
  
4. Após a definição de classe `PrimeNumberCalculator`, derive a classe `CalculatePrimeCompletedEventArgs` para relatar o resultado de cada cálculo ao manipulador de eventos do cliente para o evento `CalculatePrimeCompleted`. Além das propriedades `AsyncCompletedEventArgs`, essa classe permite que o cliente determine qual número foi testado, se ele é primo e qual é o primeiro divisor, se não for primo.  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#6](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#6)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#6](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#6)]  
  
## <a name="checkpoint"></a>Ponto de verificação  
 Neste ponto, você pode compilar o componente.  
  
### <a name="to-test-your-component"></a>Para testar o componente  
  
- Compile o componente.  
  
     Você receberá dois avisos do compilador:  
  
    ```console  
    warning CS0067: The event 'AsynchronousPatternExample.PrimeNumberCalculator.ProgressChanged' is never used  
    warning CS0067: The event 'AsynchronousPatternExample.PrimeNumberCalculator.CalculatePrimeCompleted' is never used  
    ```  
  
     Esses avisos serão removidos na próxima seção.  
  
## <a name="defining-private-delegates"></a>Definir representantes privados  
 Os aspectos assíncronos do componente `PrimeNumberCalculator` são implementados internamente com um representante especial conhecido como <xref:System.Threading.SendOrPostCallback>. Um <xref:System.Threading.SendOrPostCallback> representa um método de retorno de chamada que é executado em um thread <xref:System.Threading.ThreadPool>. O método de retorno de chamada deve ter uma assinatura que use um único parâmetro do tipo <xref:System.Object>, o que significa que você precisará passar o estado entre representantes em uma classe wrapper. Para obter mais informações, consulte <xref:System.Threading.SendOrPostCallback>.  
  
### <a name="to-implement-your-components-internal-asynchronous-behavior"></a>Para implementar o comportamento assíncrono interno do componente:  
  
1. Declare e crie os representantes <xref:System.Threading.SendOrPostCallback> na classe `PrimeNumberCalculator`. Crie os objetos <xref:System.Threading.SendOrPostCallback> em um método de utilitário chamado `InitializeDelegates`.  
  
     Você precisará de dois representantes: uma para relatar o andamento ao cliente e outro para relatar a conclusão ao cliente.  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#9](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#9)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#9](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#9)]  
    [!code-csharp[System.ComponentModel.AsyncOperationManager#20](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#20)]
    [!code-vb[System.ComponentModel.AsyncOperationManager#20](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#20)]  
  
2. Chame o método `InitializeDelegates` no construtor do componente.  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#21](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#21)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#21](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#21)]  
  
3. Declare um representante na classe `PrimeNumberCalculator` que manipula o trabalho real para ser executado de forma assíncrona. Esse representante encapsula o método de trabalho que verifica se um número é primo. O representante utiliza um parâmetro <xref:System.ComponentModel.AsyncOperation>, que será usado para acompanhar o tempo de vida da operação assíncrona.  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#22](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#22)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#22](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#22)]  
  
4. Crie um conjunto para gerenciar os tempos de vida de operações assíncronas pendentes. O cliente precisa de uma maneira para rastrear as operações à medida que elas são executadas e concluídas. Esse acompanhamento é feito exigindo que o cliente passe um token exclusivo, ou ID de tarefa, quando o cliente faz a chamada ao método assíncrono. O componente `PrimeNumberCalculator` deve manter o controle de cada chamada associando a ID da tarefa a sua invocação correspondente. Se o cliente passar uma ID de tarefa que não seja exclusiva, o componente `PrimeNumberCalculator` deverá gerar uma exceção.  
  
     O componente `PrimeNumberCalculator` mantém o controle de ID da tarefa usando uma classe de coleção especial chamada <xref:System.Collections.Specialized.HybridDictionary>. Na definição de classe, crie um <xref:System.Collections.Specialized.HybridDictionary> chamado `userTokenToLifetime`.  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#23](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#23)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#23](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#23)]  
  
## <a name="implementing-public-events"></a>Implementar eventos públicos  
 Os componentes que implementam o Padrão Assíncrono baseado em Evento se comunicam com clientes usando os eventos. Esses eventos são invocados no thread adequado com a ajuda da classe <xref:System.ComponentModel.AsyncOperation>.  
  
### <a name="to-raise-events-to-your-components-clients"></a>Para gerar eventos para clientes do componente:  
  
1. Implemente eventos públicos para relatórios para os clientes. Você precisará de um evento para o relatório de andamento e de outro para o relatório de conclusão.  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#24](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#24)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#24](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#24)]  
  
## <a name="implementing-the-completion-method"></a>Implementar o método de conclusão  
 O representante de conclusão é o método que o comportamento assíncrono subjacente independente de thread chamará quando a operação assíncrona terminar como bem-sucedida, com erro ou cancelamento. Essa chamada ocorre em um thread arbitrário.  
  
 Esse método é onde a ID da tarefa do cliente é removida da coleção interna de tokens de cliente exclusivos. Esse método também termina o tempo de vida de determinada operação assíncrona chamando o método <xref:System.ComponentModel.AsyncOperation.PostOperationCompleted%2A> no <xref:System.ComponentModel.AsyncOperation> correspondente. Essa chamada gera o evento de conclusão no thread que é apropriado para o modelo de aplicativo. Após o método <xref:System.ComponentModel.AsyncOperation.PostOperationCompleted%2A> ser chamado, essa instância do <xref:System.ComponentModel.AsyncOperation> não pode mais ser usada, e quaisquer tentativas subsequentes de usá-lo gerarão uma exceção.  
  
 A assinatura `CompletionMethod` deve conter todos os estados necessárias para descrever o resultado da operação assíncrona. Mantém o estado para o número que foi testado por essa operação assíncrona específica, indica se o número é primo e o fornece valor do primeiro divisor, caso não seja um número primo. Também contém o estado que descreve qualquer exceção que ocorreu e o <xref:System.ComponentModel.AsyncOperation> correspondente a essa tarefa específica.  
  
### <a name="to-complete-an-asynchronous-operation"></a>Para concluir uma operação assíncrona:  
  
- Implemente o método de conclusão. Utiliza seis parâmetros, que são usados para popular um `CalculatePrimeCompletedEventArgs` que é retornado ao cliente por meio do `CalculatePrimeCompletedEventHandler` desse cliente. Remove o token de ID de tarefa do cliente da coleção interna e termina o tempo de vida da operação assíncrona com uma chamada para <xref:System.ComponentModel.AsyncOperation.PostOperationCompleted%2A>. O <xref:System.ComponentModel.AsyncOperation> controla a chamada para o thread ou o contexto apropriado para o modelo de aplicativo.  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#26](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#26)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#26](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#26)]  
  
## <a name="checkpoint"></a>Ponto de verificação  
 Neste ponto, você pode compilar o componente.  
  
### <a name="to-test-your-component"></a>Para testar o componente  
  
- Compile o componente.  
  
     Você receberá um aviso do compilador:  
  
    ```console  
    warning CS0169: The private field 'AsynchronousPatternExample.PrimeNumberCalculator.workerDelegate' is never used  
    ```  
  
     Esse aviso será resolvido na próxima seção.  
  
## <a name="implementing-the-worker-methods"></a>Implementar os métodos de trabalhador  
 Até agora, você implementou o código de suporte assíncrono para o componente `PrimeNumberCalculator`. Agora você pode implementar o código que faz o trabalho real. Você implementará os três métodos: `CalculateWorker`, `BuildPrimeNumberList` e `IsPrime`. Juntos, `BuildPrimeNumberList` e `IsPrime` compõem um algoritmo bem conhecido chamado Sieve de Eratosthenes, que determina se um número é primo localizando todos os números primos até a raiz quadrada do número de teste. Se nenhum divisor for encontrado até esse ponto, o número de teste será primo.  
  
 Se esse componente fosse desenvolvido para máxima eficiência, se lembraria de todos os números primos descobertos por várias invocações de diferentes números de teste. Também deve verificar se há divisores triviais como 2, 3 e 5. A intenção desse exemplo é demonstrar como operações demoradas podem ser executadas de forma assíncrona, no entanto. Assim, essas otimizações são deixadas como um exercício para você.  
  
 O método `CalculateWorker` é encapsulado em um representante e é invocado de forma assíncrona com uma chamada para `BeginInvoke`.  
  
> [!NOTE]
> O relatório de progresso é implementado no método `BuildPrimeNumberList`. Em computadores rápidos, eventos `ProgressChanged` podem ser gerados em sucessão rápida. O thread de cliente, em que esses eventos são gerados, deve ser capaz de lidar com essa situação. O código de interface do usuário pode ser inundado com mensagens e impossível de acompanhar, o que resulta em falta de resposta. Para uma interface de usuário de exemplo que manipula essa situação, confira [Como implementar um cliente do padrão assíncrono baseado em evento](how-to-implement-a-client-of-the-event-based-asynchronous-pattern.md).  
  
### <a name="to-execute-the-prime-number-calculation-asynchronously"></a>Para executar o cálculo de número primo de forma assíncrona:  
  
1. Implementar o método de utilitário `TaskCanceled`. Isso verifica a coleção de tempo de vida de tarefa para a ID de tarefa específica e retorna `true` se a ID da tarefa não é encontrada.  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#32](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#32)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#32](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#32)]  
  
2. Implementar o método de `CalculateWorker` . Usa dois parâmetros: um número a ser testado e um <xref:System.ComponentModel.AsyncOperation>.  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#27](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#27)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#27](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#27)]  
  
3. Implementar `BuildPrimeNumberList`. Usa dois parâmetros: o número a ser testado e um <xref:System.ComponentModel.AsyncOperation>. Usa o <xref:System.ComponentModel.AsyncOperation> para relatar o andamento e os resultados incrementais. Isso garante que os manipuladores de eventos do cliente sejam chamados no thread ou contexto adequado para o modelo de aplicativo. Quando `BuildPrimeNumberList` localiza um número primo, o relata como um resultado incremental para o manipulador de eventos do cliente para o evento `ProgressChanged`. Isso requer uma classe derivada de <xref:System.ComponentModel.ProgressChangedEventArgs>, chamada `CalculatePrimeProgressChangedEventArgs`, que tem uma propriedade adicionada chamada `LatestPrimeNumber`.  
  
     O método `BuildPrimeNumberList` também chama o método `TaskCanceled` e sai se o método retorna `true`.  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#5](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#5)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#5](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#5)]  
  
4. Implementar `IsPrime`. Usa três parâmetros: uma lista de números primos conhecidos, o número a ser testado e um parâmetro de saída para o primeiro divisor encontrado. Dada a lista de números primos, determina se o número de teste é primo.  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#28](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#28)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#28](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#28)]  
  
5. Derive `CalculatePrimeProgressChangedEventArgs` de <xref:System.ComponentModel.ProgressChangedEventArgs>. Essa classe é necessária para relatar resultados incrementais ao manipulador de eventos do cliente para o evento `ProgressChanged`. Tem uma propriedade adicional chamada `LatestPrimeNumber`.  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#29](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#29)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#29](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#29)]  
  
## <a name="checkpoint"></a>Ponto de verificação  
 Neste ponto, você pode compilar o componente.  
  
### <a name="to-test-your-component"></a>Para testar o componente  
  
- Compile o componente.  
  
     Tudo o que resta a serem gravado são os métodos para iniciar e cancelar operações assíncronas, `CalculatePrimeAsync` e `CancelAsync`.  
  
## <a name="implementing-the-start-and-cancel-methods"></a>Implementar os métodos Iniciar e Cancelar  
 Você inicia o método de trabalho em seu próprio thread chamando `BeginInvoke` no representante que o encapsula. Para gerenciar o tempo de vida de determinada operação assíncrona, você deve chamar o método <xref:System.ComponentModel.AsyncOperationManager.CreateOperation%2A> na classe auxiliar <xref:System.ComponentModel.AsyncOperationManager>. Isso retorna um <xref:System.ComponentModel.AsyncOperation>, que realiza marshaling de chamadas nos manipuladores de eventos do cliente para o contexto ou thread adequado.  
  
 Você cancela determinada operação pendente chamando <xref:System.ComponentModel.AsyncOperation.PostOperationCompleted%2A> em seu <xref:System.ComponentModel.AsyncOperation> correspondente. Isso encerra a operação, e qualquer chamada subsequente para seu <xref:System.ComponentModel.AsyncOperation> gerará uma exceção.  
  
### <a name="to-implement-start-and-cancel-functionality"></a>Para implementar a funcionalidade de Iniciar e Cancelar:  
  
1. Implementar o método de `CalculatePrimeAsync` . Verifique se o token fornecido pelo cliente (ID da tarefa) é exclusivo em relação a todos os tokens que representam tarefas pendentes atualmente. Se o cliente passar um token não exclusivo, `CalculatePrimeAsync` gerará uma exceção. Caso contrário, o token será adicionado à coleção de ID de tarefa.  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#3](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#3)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#3](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#3)]  
  
2. Implementar o método de `CancelAsync` . Se o parâmetro `taskId` existir na coleção de token, será removido. Isso impede que tarefas canceladas que não foram iniciadas sejam executadas. Se a tarefa estiver em execução, o método `BuildPrimeNumberList` será encerrado quando detectar que a ID da tarefa foi removida da coleção de tempo de vida.  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#4](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#4)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#4](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#4)]  
  
## <a name="checkpoint"></a>Ponto de verificação  
 Neste ponto, você pode compilar o componente.  
  
### <a name="to-test-your-component"></a>Para testar o componente  
  
- Compile o componente.  
  
 O componente `PrimeNumberCalculator` agora está concluído e pronto para uso.  
  
 Para um cliente de exemplo que usa o componente `PrimeNumberCalculator`, confira [Como implementar um cliente do padrão assíncrono baseado em evento](how-to-implement-a-client-of-the-event-based-asynchronous-pattern.md).  
  
## <a name="next-steps"></a>Próximas etapas  
 Você pode preencher este exemplo escrevendo `CalculatePrime`, o equivalente síncrono do método `CalculatePrimeAsync`. Isso tornará o componente `PrimeNumberCalculator` totalmente compatível com o Padrão Assíncrono baseado em Evento.  
  
 Você pode melhorar este exemplo mantendo a lista de todos os números primos descoberto por várias invocações de diferentes números de teste. Usando essa abordagem, cada tarefa se beneficiará do trabalho realizado por tarefas anteriores. Tenha cuidado para proteger essa lista com regiões `lock`, para que o acesso à lista por threads diferentes seja serializado.  
  
 Você também pode melhorar este exemplo testando divisores triviais, como 2, 3 e 5.  
  
## <a name="see-also"></a>Veja também

- [Como executar uma operação no plano de fundo](../../framework/winforms/controls/how-to-run-an-operation-in-the-background.md)
- [Visão geral do padrão assíncrono baseado em evento](event-based-asynchronous-pattern-overview.md)
- [EAP (Padrão Assíncrono baseado em Evento)](event-based-asynchronous-pattern-eap.md)
