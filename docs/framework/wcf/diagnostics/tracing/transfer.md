---
title: Transferência
ms.date: 03/30/2017
ms.assetid: dfcfa36c-d3bb-44b4-aa15-1c922c6f73e6
ms.openlocfilehash: 52b0cf35a2f8bab17252d3711f3143738c2bc39c
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84587762"
---
# <a name="transfer"></a>Transferência
Este tópico descreve a transferência no modelo de rastreamento de atividades do Windows Communication Foundation (WCF).  
  
## <a name="transfer-definition"></a>Definição de transferência  
 As transferências entre as atividades representam as relações causal entre os eventos nas atividades relacionadas nos pontos de extremidade. Duas atividades estão relacionadas com transferências quando os fluxos de controle entre essas atividades, por exemplo, uma chamada de método que atravessa limites de atividade. No WCF, quando bytes são recebidos no serviço, a atividade escutar na é transferida para a atividade receber bytes em que o objeto de mensagem é criado. Para obter uma lista de cenários de rastreamento de ponta a ponta e suas respectivas atividades e design de rastreamento, consulte [cenários de rastreamento de ponta a ponta](end-to-end-tracing-scenarios.md).  
  
 Para emitir rastreamentos de transferência, use a `ActivityTracing` configuração na origem do rastreamento, conforme demonstrado pelo código de configuração a seguir.  
  
```xml  
<source name="System.ServiceModel" switchValue="Verbose,ActivityTracing">  
```  
  
## <a name="using-transfer-to-correlate-activities-within-endpoints"></a>Usando a transferência para correlacionar atividades nos pontos de extremidade  
 Atividades e transferências permitem que o usuário Probabilistic localize a causa raiz de um erro. Por exemplo, se transferimos para frente e para trás entre as atividades M e N, respectivamente nos componentes M e N, e uma falha ocorre em N logo após a transferência de volta para M, podemos desenhar a conclusão de que é provável que a passagem de N dados volte para M.  
  
 Um rastreamento de transferência é emitido da atividade M para a atividade N quando há um fluxo de controle entre M e N. Por exemplo, N executa algum trabalho para M devido a uma chamada de método que atravessa os limites das atividades. N pode já existir ou ter sido criado. N é gerado por M quando N é uma nova atividade que executa algum trabalho para M.  
  
 Uma transferência de M para N pode não ser seguida de uma transferência de N para M. Isso ocorre porque M pode gerar algum trabalho em N e não rastrear quando N concluir esse trabalho. Na verdade, M pode terminar antes de N concluir sua tarefa. Isso acontece na atividade "Open ServiceHost" (M) que gera atividades de ouvinte (N) e, em seguida, termina. Uma transferência de volta de N para M significa que N concluiu o trabalho relacionado a M.  
  
 N pode continuar executando outro processamento não relacionado a M, por exemplo, uma atividade de autenticador existente (N) que mantém o recebimento de solicitações de logon (M) de diferentes atividades de logon.  
  
 Uma relação de aninhamento não existe necessariamente entre as atividades M e N. Isso pode ocorrer devido a dois motivos. Primeiro, quando a atividade M não monitora o processamento real executado em N, embora M iniciado N. Em segundo lugar, quando N já existir.  
  
## <a name="example-of-transfers"></a>Exemplo de transferências  
 O exemplo a seguir lista dois exemplos de transferência.  
  
- Quando você cria um host de serviço, o Construtor Obtém o controle do código de chamada ou o código de chamada transfere para o construtor. Quando o Construtor conclui a execução, ele retorna o controle para o código de chamada ou o Construtor transfere de volta para o código de chamada. Esse é o caso de uma relação aninhada.  
  
- Quando um ouvinte inicia o processamento de dados de transporte, ele cria um novo thread e passa para a atividade receber bytes o contexto apropriado para processamento, passagem de controle e dados. Quando esse thread terminar de processar a solicitação, a atividade receber bytes não passará nada de volta para o ouvinte. Nesse caso, temos uma transferência no, mas nenhuma transferência para fora da nova atividade thread. As duas atividades estão relacionadas, mas não aninhadas.  
  
## <a name="activity-transfer-sequence"></a>Sequência de transferência de atividade  
 Uma sequência de transferência de atividade bem formada inclui as etapas a seguir.  
  
1. Inicie uma nova atividade, que consiste em selecionar um novo gAId.  
  
2. Emitir um rastreamento de transferência para esse novo gAId da ID da atividade atual  
  
3. Definir a nova ID em TLS  
  
4. Emita um rastreamento de início para indicar o início da nova atividade pelo.  
  
5. Retornar à atividade original consiste no seguinte:  
  
6. Emitir um rastreamento de transferência para o gAId original  
  
7. Emitir um rastreamento de interrupção para indicar o final da nova atividade  
  
8. Defina TLS para o antigo gAId.  
  
 O exemplo de código a seguir demonstra como fazer isso. Este exemplo pressupõe que uma chamada de bloqueio é feita ao transferir para a nova atividade e inclui rastreamentos de suspensão/retomada.  
  
```csharp
// 0. Create a trace source  
TraceSource ts = new TraceSource("myTS");  

// 1. remember existing ("ambient") activity for clean up  
Guid oldGuid = Trace.CorrelationManager.ActivityId;  
// this will be our new activity  
Guid newGuid = Guid.NewGuid();

// 2. call transfer, indicating that we are switching to the new AID  
ts.TraceTransfer(667, "Transferring.", newGuid);  

// 3. Suspend the current activity.  
ts.TraceEvent(TraceEventType.Suspend, 667, "Suspend: Activity " + i-1);  

// 4. set the new AID in TLS  
Trace.CorrelationManager.ActivityId = newGuid;  

// 5. Emit the start trace  
ts.TraceEvent(TraceEventType.Start, 667, "Boundary: Activity " + i);  

// trace something  
ts.TraceEvent(TraceEventType.Information, 667, "Hello from activity " + i);  

// Perform Work  
// some work.  
// Return  
ts.TraceEvent(TraceEventType.Information, 667, "Work complete on activity " + i);

// 6. Emit the transfer returning to the original activity  
ts.TraceTransfer(667, "Transferring Back.", oldGuid);  

// 7. Emit the End trace  
ts.TraceEvent(TraceEventType.Stop, 667, "Boundary: Activity " + i);  

// 8. Change the tls variable to the original AID  
Trace.CorrelationManager.ActivityId = oldGuid;

// 9. Resume the old activity  
ts.TraceEvent(TraceEventType.Resume, 667, "Resume: Activity " + i-1);  
```  
  
## <a name="see-also"></a>Confira também

- [Configurando o rastreamento](configuring-tracing.md)
- [Utilizando o visualizador de rastreamento de serviço para visualização de rastreamento correlacionados e soluções de problemas](using-service-trace-viewer-for-viewing-correlated-traces-and-troubleshooting.md)
- [Cenários de rastreamento ponta a ponta](end-to-end-tracing-scenarios.md)
- [Ferramenta Visualizador de rastreamento de serviço (SvcTraceViewer.exe)](../../service-trace-viewer-tool-svctraceviewer-exe.md)
