---
title: Atividade
ms.date: 03/30/2017
ms.assetid: 70471705-f55f-4da1-919f-4b580f172665
ms.openlocfilehash: 41de6b9458feb4e1898eeac6635b74c4617885d6
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84602135"
---
# <a name="activity"></a>Atividade
Este tópico descreve os rastreamentos de atividade no modelo de rastreamento Windows Communication Foundation (WCF). As atividades são unidades de processamento que ajudam o usuário a restringir o escopo de uma falha. Os erros que ocorrem na mesma atividade estão diretamente relacionados. Por exemplo, uma operação falha porque a descriptografia de mensagem falhou. Os rastreamentos para a operação e falha de descriptografia de mensagem aparecem na mesma atividade, mostrando a correlação direta entre o erro de descriptografia e o erro de solicitação.  
  
## <a name="configuring-activity-tracing"></a>Configurando o rastreamento de atividade  
 O WCF fornece atividades predefinidas para processamento de aplicativos (consulte a [lista de atividades](activity-list.md)). Você também pode definir atividades programaticamente para agrupar rastreamentos de usuário. Para obter mais informações, consulte [emitindo rastreamentos de código do usuário](emitting-user-code-traces.md).  
  
 Para emitir rastreamentos de atividade em tempo de execução, use a `ActivityTracing` configuração para a `System.ServiceModel` origem de rastreamento ou outras fontes de rastreamento personalizadas ou WCF, conforme demonstrado pelo código de configuração a seguir.  
  
```xml  
<source name="System.ServiceModel" switchValue="Verbose,ActivityTracing">  
```  
  
 Para entender mais sobre o elemento de configuração e os atributos que estão sendo usados, consulte o tópico [Configurando o rastreamento](configuring-tracing.md) .  
  
## <a name="viewing-activities"></a>Exibindo atividades  
 Você pode exibir as atividades e seus utilitários na [ferramenta do Visualizador de rastreamento de serviço (SvcTraceViewer. exe)](../../service-trace-viewer-tool-svctraceviewer-exe.md). Quando o ActivityTracing está habilitado, essa ferramenta pega os rastreamentos e os classifica com base na atividade. Você também pode ver transferências de rastreamento. Uma transferência de rastreamento indica como atividades diferentes estão relacionadas entre si. Você pode ver que uma atividade específica causou a inicialização de outra. Por exemplo, uma solicitação de mensagem iniciou um handshake de segurança para obter um token de conversa seguro.  
  
### <a name="correlating-activities-in-service-trace-viewer"></a>Correlacionando atividades no Visualizador de rastreamento de serviço  
 A ferramenta Visualizador de rastreamento de serviço fornece duas exibições de atividades:  
  
- Exibição de **lista** , em que a ID da atividade é usada para correlacionar diretamente os rastreamentos entre processos. Os rastreamentos de processos diferentes, por exemplo, cliente e serviço, mas com a mesma ID de atividade, são agrupados na mesma atividade. Portanto, um erro que ocorre no serviço que causa um erro no cliente será exibido na mesma exibição de atividade na ferramenta.  
  
- Exibição de **gráfico** , em que as atividades são agrupadas por processos. Nessa exibição, um cliente e um serviço com a mesma ID de atividade têm seus rastreamentos em diferentes atividades. Para correlacionar atividades com a mesma ID de atividade em processos diferentes, a ferramenta mostra fluxos de mensagens entre as atividades relacionadas.  
  
 Para obter mais informações e ver uma exibição gráfica da ferramenta do Visualizador de rastreamento de serviço, consulte [Service Trace Viewer Tool (SvcTraceViewer. exe)](../../service-trace-viewer-tool-svctraceviewer-exe.md) e [using Service Trace Viewer para exibir rastreamentos correlacionados e solução de problemas](using-service-trace-viewer-for-viewing-correlated-traces-and-troubleshooting.md).  
  
## <a name="defining-the-scope-of-an-activity"></a>Definindo o escopo de uma atividade  
 Uma atividade é definida em tempo de design e denota uma unidade lógica de trabalho. Os rastreamentos emitidos com o mesmo identificador de atividade estão diretamente relacionados, eles fazem parte da mesma atividade. Como uma atividade pode cruzar limites de ponto de extremidade (uma solicitação), dois escopos para uma atividade são definidos.  
  
- `Global`escopo, por aplicativo. Nesse escopo, a atividade é identificada por seu identificador de atividade globalmente exclusivo de 128 bits, o gAId. O gAid é o que é propagado entre pontos de extremidade.  
  
- `Local`escopo, por ponto de extremidade. Nesse escopo, a atividade é identificada por seu gAId, juntamente com o nome de origem do rastreamento que emite os rastreamentos de atividade e a ID do processo. Esse terceto constitui a ID da atividade local, disposta. O layout é usado para definir os limites (locais) de uma atividade.  
  
## <a name="trace-schema"></a>Esquema de rastreamento  
 Os rastreamentos podem ser emitidos usando qualquer esquema e em plataformas Microsoft. "E2E" (para "ponta a ponta") é um esquema comumente usado. Esse esquema inclui um identificador de 128 bits (gAId), o nome da origem do rastreamento e a ID do processo. No código gerenciado, o <xref:System.Diagnostics.XmlWriterTraceListener> emite rastreamentos no esquema E2E.  
  
 Os desenvolvedores podem definir a ajuda emitida com um rastreamento definindo a <xref:System.Diagnostics.CorrelationManager.ActivityId%2A> propriedade com um GUID no armazenamento local de thread (TLS). O exemplo a seguir demonstra isso.  
  
```csharp
// set the current Activity ID to a new GUID.  
CorrelationManager.ActivityId = Guid.NewGuid();  
```
  
 Definir o gAId em TLS será evidente quando os rastreamentos forem emitidos usando uma origem de rastreamento, conforme mostrado pelo exemplo a seguir.  
  
```csharp
TraceSource traceSource = new TraceSource("myTraceSource");  
traceSource.TraceEvent(TraceEventType.Warning, eventId, "Information");  
```  
  
 O rastreamento emitido conterá o gAId atualmente em TLS, o nome de origem do rastreamento passado como um parâmetro para o construtor da origem do rastreamento e a ID do processo atual.  
  
## <a name="activity-lifetime"></a>Vida útil da atividade  
 Em termos mais estritos, a evidência de uma atividade começa na primeira vez em que a ID da atividade é usada em um rastreamento emitido e termina na última vez em que é usada em um rastreamento emitido. Um conjunto predefinido de tipos de rastreamento é fornecido pelo <xref:System.Diagnostics> , incluindo iniciar e parar, para marcar explicitamente os limites de tempo de vida da atividade.  
  
- Start: indica o início de uma atividade. Um rastreamento de "início" fornece um registro do início de uma nova etapa de processamento. Ele contém uma nova ID de atividade para uma determinada origem de rastreamento em um determinado processo, exceto quando a ID da atividade é propagada entre pontos de extremidade; nesse caso, vemos um "Start" por ponto de extremidades. Exemplos de como iniciar uma nova atividade incluem a criação de um novo thread para processamento ou a inserção de um novo método público.  
  
- Parar: indica o fim de uma atividade. Um rastreamento "Stop" fornece um registro do encerramento de uma etapa de processamento existente. Ele contém uma ID de atividade existente para uma determinada origem de rastreamento em um determinado processo, exceto quando a ID da atividade é propagada entre pontos de extremidade; nesse caso, vemos um "Stop" por ponto de interrupção.  Exemplos de como parar uma atividade incluem o encerramento de um thread de processamento ou a saída de um método cujo início foi indicado com um rastreamento de "início".  
  
- Suspender: indica a suspensão do processamento de uma atividade. Um rastreamento "suspender" contém uma ID de atividade existente cujo processamento deve ser retomado em um momento posterior. Nenhum rastreamento é emitido com essa ID entre os eventos suspender e retomar da origem de rastreamento atual. Os exemplos incluem pausar uma atividade ao chamar uma função de biblioteca externa ou ao aguardar um recurso, como uma porta de conclusão de e/s.  
  
- Retomar: indica a retomada do processamento de uma atividade. Um rastreamento "retomar" contém uma ID de atividade existente cujo rastreamento emitido pela última vez da fonte de rastreamento atual era um rastreamento de "suspensão". Os exemplos incluem retornar de uma chamada para uma função de biblioteca externa ou quando sinalizado para retomar o processamento por um recurso, como uma porta de conclusão de e/s.  
  
- Transferência: como algumas atividades são causadas por outras pessoas, ou relacionadas a outras, as atividades podem estar relacionadas a outras atividades por meio de rastreamentos de "transferência". Uma transferência registra a relação direcionada de uma atividade para outra  
  
 Os rastreamentos de iniciar e parar não são críticos para correlação. No entanto, eles podem ajudar a aumentar o desempenho, a criação de perfil e a validação do escopo da atividade.  
  
 Usando esses tipos, as ferramentas podem otimizar a navegação pelos logs de rastreamento para localizar os eventos imediatamente relacionados da mesma atividade ou eventos em atividades relacionadas se a ferramenta seguir rastreamentos de transferência. Por exemplo, as ferramentas deixarão de analisar os logs de uma determinada atividade quando verão um rastreamento de iniciar/parar.  
  
 Esses tipos de rastreamento também podem ser usados para a criação de perfil. Os recursos consumidos entre os marcadores de início e de parada representam o tempo inclusivo da atividade, incluindo atividades lógicas contidas. Subtrair os intervalos de tempo entre os rastreamentos suspender e retomar fornece o tempo de atividade real.  
  
 O rastreamento de parada também é particularmente útil para validar o escopo das atividades implementadas. Se alguns rastreamentos de processamento aparecerem após o rastreamento de parada em vez de dentro de uma determinada atividade, isso poderá sugerir defeitos de código.  
  
## <a name="guidelines-for-using-activity-tracing"></a>Diretrizes para usar o rastreamento de atividades  
 Veja a seguir uma diretriz de uso de rastreamentos do ActivityTracing (Iniciar, parar, suspender, retomar e transferir).  
  
- O rastreamento é um grafo cíclico direcionado, não uma árvore. Você pode retornar o controle a uma atividade que gerou uma atividade.  
  
- Uma atividade denota um limite de processamento que pode ser significativo para o administrador do sistema ou para fins de suporte.  
  
- Cada método WCF, tanto no cliente quanto no servidor, é limitado pelo início de uma nova atividade e, em seguida, (após o trabalho ser concluído), encerrando a nova atividade e retornando para a atividade ambiente.  
  
- Atividades de longa execução (contínuas), como a escuta de conexões ou a espera de mensagens, são representadas por marcadores de início/parada correspondentes.  
  
- As atividades disparadas pelo recebimento ou processamento de uma mensagem são representadas por limites de rastreamento.  
  
- As atividades representam atividades, não necessariamente objetos. Uma atividade deve ser interpretada como "isso estava ocorrendo quando. . . (ocorreu uma emissão de rastreamento significativa). "  
  
## <a name="see-also"></a>Consulte também

- [Configurando o rastreamento](configuring-tracing.md)
- [Utilizando o visualizador de rastreamento de serviço para visualização de rastreamento correlacionados e soluções de problemas](using-service-trace-viewer-for-viewing-correlated-traces-and-troubleshooting.md)
- [Cenários de rastreamento ponta a ponta](end-to-end-tracing-scenarios.md)
- [Ferramenta Visualizador de rastreamento de serviço (SvcTraceViewer.exe)](../../service-trace-viewer-tool-svctraceviewer-exe.md)
- [Emitindo traços de código de usuário](emitting-user-code-traces.md)
