---
title: Emitindo traços de código de usuário
ms.date: 03/30/2017
ms.assetid: fa54186a-8ffa-4332-b0e7-63867126fd49
ms.openlocfilehash: e8b2031165a83e24ba15a2fcf847a170f47e696a
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84589286"
---
# <a name="emitting-user-code-traces"></a>Emitindo traços de código de usuário

Além de habilitar o rastreamento na configuração para coletar dados de instrumentação gerados pelo Windows Communication Foundation (WCF), você também pode emitir rastreamentos programaticamente no código do usuário. Dessa forma, você pode criar proativamente dados de instrumentação que você pode examinar posteriormente para fins de diagnóstico. Este tópico discute como você pode fazer isso.

Além disso, o exemplo de [rastreamento estendido](../../samples/extending-tracing.md) inclui todo o código demonstrado nas seções a seguir.

## <a name="creating-a-trace-source"></a>Criando uma origem de rastreamento

Você pode usar o código a seguir para criar uma origem de rastreamento de usuário.

```csharp
TraceSource ts = new TraceSource("myUserTraceSource");
```

## <a name="creating-activities"></a>Criando atividades

As atividades são unidade lógica de processamento. Você pode criar uma atividade para cada unidade de processamento principal na qual você deseja que os rastreamentos sejam agrupados juntos. Por exemplo, você pode criar uma atividade para cada solicitação para o serviço. Para fazer isso, execute as etapas a seguir.

1. Salve a ID da atividade no escopo.

2. Crie uma nova ID de atividade.

3. Transfira da atividade no escopo para a nova, defina a nova atividade no escopo e emita um rastreamento de início para essa atividade.

O código a seguir demonstra como fazer isso.

```csharp
Guid oldID = Trace.CorrelationManager.ActivityId;
Guid traceID = Guid.NewGuid();
ts.TraceTransfer(0, "transfer", traceID);
Trace.CorrelationManager.ActivityId = traceID; // Trace is static
ts.TraceEvent(TraceEventType.Start, 0, "Add request");
```

## <a name="emitting-traces-within-a-user-activity"></a>Emitindo rastreamentos dentro de uma atividade de usuário

O código a seguir emite rastreamentos dentro de uma atividade de usuário.

```csharp
double value1 = 100.00D;
double value2 = 15.99D;
ts.TraceInformation("Client sends message to Add " + value1 + ", " + value2);
double result = client.Add(value1, value2);
ts.TraceInformation("Client receives Add response '" + result + "'");
```

## <a name="stopping-the-activities"></a>Interrompendo as atividades

Para interromper as atividades, transfira de volta para a atividade antiga, interrompa a ID da atividade atual e redefina a ID da atividade antiga no escopo.

O código a seguir demonstra como fazer isso.

```csharp
ts.TraceTransfer(0, "transfer", oldID);
ts.TraceEvent(TraceEventType.Stop, 0, "Add request");
Trace.CorrelationManager.ActivityId = oldID;
```

## <a name="propagating-the-activity-id-to-a-service"></a>Propagando a ID da atividade para um serviço

Se você definir o `propagateActivity` atributo como `true` para a `System.ServiceModel` origem do rastreamento nos arquivos de configuração do cliente e do serviço, o processamento do serviço para a solicitação de adição ocorrerá na mesma atividade que a definida no cliente. Se o serviço definir suas próprias atividades e transferências, os rastreamentos de serviço não aparecerão na atividade propagada pelo cliente. Em vez disso, eles aparecem em uma atividade correlacionada pelos rastreamentos de transferência para a atividade cuja ID é propagada pelo cliente.

> [!NOTE]
> Se o `propagateActivity` atributo for definido como `true` no cliente e no serviço, a atividade ambiente no escopo da operação do serviço será definida pelo WCF.

Você pode usar o código a seguir para verificar se uma atividade foi definida no escopo pelo WCF.

```csharp
// Check if an activity was set in scope by WCF, if it was
// propagated from the client. If not, ( ambient activity is
// equal to Guid.Empty), create a new one.
if(Trace.CorrelationManager.ActivityId == Guid.Empty)
{
    Guid newGuid = Guid.NewGuid();
    Trace.CorrelationManager.ActivityId = newGuid;
}
// Emit your Start trace.
ts.TraceEvent(TraceEventType.Start, 0, "Add Activity");

// Emit the processing traces for that request.
serviceTs.TraceInformation("Service receives Add "
                            + n1 + ", " + n2);
// double result = n1 + n2;
serviceTs.TraceInformation("Service sends Add result" + result);

// Emit the Stop trace and exit the method scope.
ts.TraceEvent(TraceEventType.Stop, 0, "Add Activity");
// return result;
```

## <a name="tracing-exceptions-thrown-in-code"></a>Exceções de rastreamento lançadas no código

Quando você lança uma exceção no código, também pode rastrear a exceção no nível de aviso ou acima usando o código a seguir.

```csharp
ts.TraceEvent(TraceEventType.Warning, 0, "Throwing exception " + "exceptionMessage");
```

## <a name="viewing-user-traces-in-the-service-trace-viewer-tool"></a>Exibindo rastreamentos de usuário na ferramenta do Visualizador de rastreamento de serviço

Esta seção contém capturas de tela de rastreamentos gerados pela execução da amostra de [rastreamento de extensão](../../samples/extending-tracing.md) , quando exibidas usando a ferramenta do Visualizador de [rastreamento de serviço (SvcTraceViewer. exe)](../../service-trace-viewer-tool-svctraceviewer-exe.md).

No diagrama a seguir, a atividade "Adicionar solicitação" criada anteriormente está selecionada no painel esquerdo. Ela é listada com três outras atividades de operação matemática (dividir, subtrair, multiplicar) que constituem o programa cliente do aplicativo. O código do usuário definiu uma nova atividade para cada operação a fim de isolar possíveis ocorrências de erro em solicitações diferentes.

Para demonstrar o uso de transferências no exemplo de [rastreamento estendido](../../samples/extending-tracing.md) , uma atividade de calculadora que encapsula as quatro solicitações de operação também é criada. Para cada solicitação, há uma transferência para frente e para trás da atividade de calculadora para a atividade de solicitação (o rastreamento é realçado no painel superior direito na figura).

Quando você seleciona uma atividade no painel esquerdo, os rastreamentos incluídos por essa atividade são mostrados no painel superior direito. Se `propagateActivity` estiver `true` em cada ponto de extremidade no caminho da solicitação, os rastreamentos na atividade de solicitação serão de todos os processos que participam da solicitação. Neste exemplo, você pode ver rastreamentos do cliente e do serviço na 4ª coluna do painel.

Essa atividade mostra a seguinte ordem de processamento:

1. O cliente envia a mensagem para adicionar.

2. O serviço recebe a mensagem de solicitação de adição.

3. O serviço envia adicionar resposta.

4. O cliente recebe adicionar resposta.

Todos esses rastreamentos foram emitidos no nível de informações. Clicar em um rastreamento no painel superior direito mostra os detalhes desse rastreamento no painel inferior direito.

No diagrama a seguir, também vemos os rastreamentos de transferência de e para a atividade de calculadora, bem como dois pares de rastreamentos de iniciar e parar por atividade de solicitação, um para o cliente e outro para o serviço (um para cada origem de rastreamento).

![Visualizador de rastreamento: emitindo rastreamentos de código de&#45;de usuário](media/242c9358-475a-4baf-83f3-4227aa942fcd.gif "242c9358-475a-4baf-83f3-4227aa942fcd") Lista de atividades por hora de criação (painel esquerdo) e suas atividades aninhadas (painel superior direito)

Se o código do serviço lançar uma exceção que faz com que o cliente seja acionado também (por exemplo, quando o cliente não obteve a resposta à sua solicitação), o serviço e as mensagens de aviso ou de erro do cliente ocorrerão na mesma atividade para correlação direta. Na imagem a seguir, o serviço gera uma exceção que declara "o serviço se recusa a processar essa solicitação no código do usuário". O cliente também gera uma exceção afirmando "o servidor não pôde processar a solicitação devido a um erro interno".

As imagens a seguir mostram que os erros nos pontos de extremidade de uma determinada solicitação aparecem na mesma atividade se a ID da atividade de solicitação foi propagada:

![Captura de tela que mostra erros nos pontos de extremidade de uma determinada solicitação.](./media/emitting-user-code-traces/trace-viewer-endpoint-errors.gif)

Clicar duas vezes na atividade multiplicar no painel esquerdo mostra o grafo a seguir, com os rastreamentos para a atividade de multiplicação de cada processo envolvido. Podemos ver que um aviso ocorreu primeiro no serviço (exceção gerada), que é seguido por avisos e erros no cliente porque a solicitação não pôde ser processada. Portanto, podemos implicar a relação de erro causal entre os pontos de extremidade e derivar a causa raiz do erro.

A imagem a seguir mostra uma exibição de gráfico de correlação de erros:

![Captura de tela que mostra a exibição de gráfico da correlação de erros.](./media/emitting-user-code-traces/trace-viewer-error-correlation.gif)

Para obter os rastreamentos anteriores, definimos `ActivityTracing` para as fontes de rastreamento do usuário e `propagateActivity=true` para a `System.ServiceModel` origem do rastreamento. Não definimos `ActivityTracing` para a `System.ServiceModel` origem de rastreamento para habilitar o código do usuário para a propagação da atividade de código do usuário. (Quando o rastreamento de atividade ServiceModel está ativado, a ID da atividade definida no cliente não é propagada até o código de usuário do serviço; No entanto, as transferências correlacionam as atividades de código do usuário do cliente e do serviço às atividades intermediárias do WCF.)

A definição de atividades e a propagação da ID da atividade nos permite executar a correlação de erros diretas entre pontos de extremidade. Dessa forma, podemos localizar a causa raiz de um erro mais rapidamente.

## <a name="see-also"></a>Consulte também

- [Estendendo rastreamento](../../samples/extending-tracing.md)
