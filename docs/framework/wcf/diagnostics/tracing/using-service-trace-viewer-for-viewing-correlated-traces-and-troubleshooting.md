---
title: Utilizando o visualizador de rastreamento de serviço para visualização de rastreamento correlacionados e soluções de problemas
ms.date: 03/30/2017
ms.assetid: 05d2321c-8acb-49d7-a6cd-8ef2220c6775
ms.openlocfilehash: e1cd1443e96e7195127cb95e7ef1b2c4d6d9c176
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84587749"
---
# <a name="using-service-trace-viewer-for-viewing-correlated-traces-and-troubleshooting"></a>Utilizando o visualizador de rastreamento de serviço para visualização de rastreamento correlacionados e soluções de problemas

Este tópico descreve o formato dos dados de rastreamento, como exibi-los e abordagens que usam o Visualizador de rastreamento de serviço para solucionar problemas de seu aplicativo.

## <a name="using-the-service-trace-viewer-tool"></a>Usando a ferramenta Visualizador de Rastreamento de Serviço

A ferramenta de visualizador de rastreamento do serviço Windows Communication Foundation (WCF) ajuda a correlacionar rastreamentos de diagnóstico produzidos por ouvintes do WCF para localizar a causa raiz de um erro. A ferramenta oferece uma maneira de exibir, agrupar e filtrar rastreamentos de forma fácil para que você possa diagnosticar, reparar e verificar problemas com os serviços WCF. Para obter mais informações sobre como usar essa ferramenta, consulte [Service Trace Viewer Tool (SvcTraceViewer. exe)](../../service-trace-viewer-tool-svctraceviewer-exe.md).

Este tópico contém capturas de tela de rastreamentos gerados pela execução da amostra de [rastreamento e registro de mensagens](../../samples/tracing-and-message-logging.md) , quando exibido usando a [ferramenta do Visualizador de rastreamento de serviço (SvcTraceViewer. exe)](../../service-trace-viewer-tool-svctraceviewer-exe.md). Este tópico demonstra como entender o conteúdo do rastreamento, as atividades e sua correlação e como analisar grandes números de rastreamentos ao solucionar problemas.

## <a name="viewing-trace-content"></a>Exibindo conteúdo de rastreamento

Um evento de rastreamento contém as seguintes informações mais importantes:

- Nome da atividade quando definido.

- Tempo de emissão.

- Nível de rastreamento.

- Nome da origem do rastreamento.

- Nome do processo.

- ID do thread.

- Um identificador de rastreamento exclusivo, que é uma URL que aponta para um destino no Microsoft Docs, do qual você pode obter mais informações relacionadas ao rastreamento.

 Tudo isso pode ser visto no painel superior direito no Visualizador de rastreamento de serviço ou na seção **informações básicas** na exibição formatada do painel inferior direito ao selecionar um rastreamento.

> [!NOTE]
> Se o cliente e o serviço estiverem no mesmo computador, os rastreamentos para ambos os aplicativos estarão presentes. Eles podem ser filtrados usando a coluna **process name** .

Além disso, a exibição formatada também fornece uma descrição para o rastreamento e informações detalhadas adicionais quando disponíveis. O último pode incluir o tipo de exceção e a mensagem, as pilhas de chamadas, a ação da mensagem, os campos de/para e outras informações de exceção.

No modo de exibição XML, as marcas XML úteis incluem o seguinte:

- `<SubType>`(nível de rastreamento).

- `<TimeCreated>`.

- `<Source>`(nome da origem do rastreamento).

- `<Correlation>`(ID da atividade definida ao emitir o rastreamento).

- `<Execution>`(ID do processo e thread).

- `<Computer>`.

- `<ExtendedData>`, incluindo `<Action>` `<MessageID>` e o `<ActivityId>` definido no cabeçalho da mensagem ao enviar uma mensagem.

Se você examinar o rastreamento "Enviar uma mensagem por um canal", você poderá ver o conteúdo a seguir.

```xml
<E2ETraceEvent xmlns="http://schemas.microsoft.com/2004/06/E2ETraceEvent">
   <System xmlns="http://schemas.microsoft.com/2004/06/windows/eventlog/system">
      <EventID>262163</EventID>
      <Type>3</Type>
      <SubType Name="Information">0</SubType>
      <Level>8</Level>
      <TimeCreated SystemTime="2006-08-04T18:45:30.8491051Z" />
      <Source Name="System.ServiceModel" />
       <Correlation ActivityID="{27c6331d-8998-43aa-a382-03239013a6bd}"/>
       <Execution ProcessName="client" ProcessID="1808" ThreadID="1" />
       <Channel />
       <Computer>TEST1</Computer>
   </System>
   <ApplicationData>
       <TraceData>
          <DataItem>
             <TraceRecord xmlns="http://schemas.microsoft.com/2004/10/E2ETraceEvent/TraceRecord" Severity="Information">
                 <TraceIdentifier>http://msdn.microsoft.com/library/System.ServiceModel.Channels.MessageSent.aspx</TraceIdentifier>
                 <Description>Sent a message over a channel.</Description>
                 <AppDomain>client.exe</AppDomain>
                 <Source>System.ServiceModel.Channels.ClientFramingDuplexSessionChannel/35191196</Source>
                <ExtendedData xmlns="http://schemas.microsoft.com/2006/08/ServiceModel/MessageTransmitTraceRecord">

                  <MessageProperties>
                     <AllowOutputBatching>False</AllowOutputBatching>
                  </MessageProperties>
                  <MessageHeaders>
                     <Action d4p1:mustUnderstand="1" xmlns:d4p1="http://www.w3.org/2003/05/soap-envelope" xmlns="http://www.w3.org/2005/08/addressing">http://Microsoft.ServiceModel.Samples/ICalculator/Multiply</Action>
                     <MessageID xmlns="http://www.w3.org/2005/08/addressing">urn:uuid:7c6670d8-4c9c-496e-b6a0-2ceb6db35338</MessageID>
                     <ActivityId CorrelationId="b02e2189-0816-4387-980c-dd8e306440f5" xmlns="http://schemas.microsoft.com/2004/09/ServiceModel/Diagnostics">27c6331d-8998-43aa-a382-03239013a6bd</ActivityId>
                     <ReplyTo xmlns="http://www.w3.org/2005/08/addressing">
                        <Address>http://www.w3.org/2005/08/addressing/anonymous</Address>
                    </ReplyTo>
                    <To d4p1:mustUnderstand="1" xmlns:d4p1="http://www.w3.org/2003/05/soap-envelope" xmlns="http://www.w3.org/2005/08/addressing">net.tcp://localhost/servicemodelsamples/service</To>
                  </MessageHeaders>
                  <RemoteAddress>net.tcp://localhost/servicemodelsamples/service</RemoteAddress>
                </ExtendedData>
            </TraceRecord>
          </DataItem>
       </TraceData>
   </ApplicationData>
</E2ETraceEvent>
```

## <a name="servicemodel-e2e-tracing"></a>Rastreamento de E2E de ServiceModel

Quando a `System.ServiceModel` origem do rastreamento é definida com um `switchValue` diferente de off, e o `ActivityTracing` WCF cria atividades e transferências para o processamento do WCF.

Uma atividade é uma unidade lógica de processamento que agrupa todos os rastreamentos relacionados a essa unidade de processamento. Por exemplo, você pode definir uma atividade para cada solicitação. As transferências criam uma relação de causal entre as atividades nos pontos de extremidade. A propagação da ID de atividade permite que você relacione as atividades nos pontos de extremidade. Isso pode ser feito definindo `propagateActivity` = `true` na configuração em todos os pontos de extremidade. Atividades, transferências e propagação permitem que você execute a correlação de erros. Dessa forma, você pode encontrar a causa raiz de um erro mais rapidamente.

No cliente, uma atividade WCF é criada para cada chamada de modelo de objeto (por exemplo, abrir ChannelFactory, Add, divide e assim por diante.) Cada uma das chamadas de operação é processada em uma atividade de "processar ação".

Na captura de tela a seguir, extraída da amostra de [rastreamento e log de mensagem](../../samples/tracing-and-message-logging.md) o painel esquerdo exibe a lista de atividades criadas no processo do cliente, classificadas por hora de criação. A seguir está uma lista cronológica de atividades:

- Construída a fábrica de canais (ClientBase).

- Aberta a fábrica de canais.

- A ação de adição foi processada.

- Configure a sessão segura (isso ocorreu na primeira solicitação) e processou três mensagens de resposta de infraestrutura de segurança: RST, RSTR, SCT (processar mensagem 1, 2, 3).

- Processadas as solicitações subtrair, multiplicar e dividir.

- Fechou a fábrica de canais e isso fechou a sessão segura e processou a resposta da mensagem de segurança cancelar.

 Vemos as mensagens de infraestrutura de segurança por causa da wsHttpBinding.

> [!NOTE]
> No WCF, mostramos que as mensagens de resposta estão sendo processadas inicialmente em uma atividade separada (processar mensagem) antes de correlacioná-las à atividade de ação de processo correspondente que inclui a mensagem de solicitação, por meio de uma transferência. Isso acontece para mensagens de infraestrutura e solicitações assíncronas e é devido ao fato de que devemos inspecionar a mensagem, ler o cabeçalho ActivityId e identificar a atividade de ação do processo existente com essa ID para correlacioná-la. Para solicitações síncronas, estamos bloqueando a resposta e, portanto, sabemos a qual ação de processo a resposta está relacionada.

A imagem a seguir mostra as atividades do cliente do WCF listadas por hora de criação (painel esquerdo) e suas atividades e rastreamentos aninhados (painel superior direito):

![Captura de tela mostrando as atividades do cliente WCF listadas por hora de criação.](./media/using-service-trace-viewer-for-viewing-correlated-traces-and-troubleshooting/wcf-client-activities-creation-time.gif)

Quando selecionamos uma atividade no painel esquerdo, podemos ver as atividades e os rastreamentos aninhados no painel superior direito. Portanto, essa é uma exibição hierárquica reduzida da lista de atividades à esquerda, com base na atividade pai selecionada. Como a ação de processo selecionada adicionar é a primeira solicitação feita, essa atividade contém a atividade configurar sessão segura (transferir para, transferir para trás) e rastreamentos para o processamento real da ação adicionar.

Se clicarmos duas vezes na atividade processar ação adicionar no painel esquerdo, poderemos ver uma representação gráfica das atividades do cliente WCF relacionadas a adicionar. A primeira atividade à esquerda é a atividade raiz (0000), que é a atividade padrão. O WCF transfere da atividade ambiente. Se isso não for definido, o WCF transfere de 0000. Aqui, a segunda atividade, processar ação adicionar, transfere de 0. Em seguida, vemos configuração de sessão segura.

A imagem a seguir mostra uma exibição de gráfico das atividades do cliente WCF, especificamente a atividade ambiente (aqui 0), processar ação e configurar sessão segura:

![Grafo no Visualizador de rastreamento mostrando atividade de ambiente e ação de processo.](./media/using-service-trace-viewer-for-viewing-correlated-traces-and-troubleshooting/wcf-activities-graph-ambient-process.gif)

No painel superior direito, podemos ver todos os rastreamentos relacionados à ação de processo adicionar atividade. Especificamente, enviamos a mensagem de solicitação ("enviou uma mensagem por um canal") e recebemos a resposta ("recebeu uma mensagem por um canal") na mesma atividade. Isso é mostrado no grafo a seguir. Para maior clareza, a atividade configurar sessão segura é recolhida no grafo.

A imagem a seguir mostra uma lista de rastreamentos para a atividade processar ação. Enviamos a solicitação e recebemos a resposta na mesma atividade.

![Captura de tela do Visualizador de rastreamento mostrando uma lista de rastreamentos para a atividade processar ação](./media/using-service-trace-viewer-for-viewing-correlated-traces-and-troubleshooting/process-action-traces.gif)

Aqui, carregamos rastreamentos de cliente apenas para fins de clareza, mas os rastreamentos de serviço (mensagem de solicitação recebida e mensagem de resposta enviada) aparecem na mesma atividade se eles também estiverem carregados na ferramenta e estiverem `propagateActivity` definidos como `true.` isso é mostrado em uma ilustração posterior.

No serviço, o modelo de atividade é mapeado para os conceitos do WCF da seguinte maneira:

1. Construímos e abrimos um ServiceHost (isso pode criar várias atividades relacionadas ao host, por exemplo, no caso de segurança).

2. Criamos uma atividade escutar na para cada ouvinte no ServiceHost (com transferências para dentro e fora do ServiceHost aberto).

3. Quando o ouvinte detecta uma solicitação de comunicação iniciada pelo cliente, ele transfere para uma atividade de "bytes de recebimento", na qual todos os bytes enviados do cliente são processados. Nessa atividade, podemos ver quaisquer erros de conexão que ocorreram durante a interação do serviço cliente.

4. Para cada conjunto de bytes recebido que corresponde a uma mensagem, processamos esses bytes em uma atividade de "mensagem de processo", na qual criamos o objeto de mensagem do WCF. Nessa atividade, vemos erros relacionados a um envelope inválido ou a uma mensagem malformada.

5. Depois que a mensagem é formada, transferimos para uma atividade processar ação. Se `propagateActivity` é definido como `true` no cliente e no serviço, essa atividade tem a mesma ID que aquela definida no cliente e descrita anteriormente. Neste estágio, começamos a se beneficiar da correlação direta entre pontos de extremidade, pois todos os rastreamentos emitidos no WCF relacionados à solicitação estão na mesma atividade, incluindo o processamento de mensagens de resposta.

6. Para a ação fora do processo, criamos uma atividade "executar código do usuário" para isolar os rastreamentos emitidos no código do usuário a partir daqueles emitidos no WCF. No exemplo anterior, o rastreamento "serviço envia a resposta" é emitido na atividade "executar código do usuário" não está na atividade propagada pelo cliente, se aplicável.

Na ilustração a seguir, a primeira atividade à esquerda é a atividade raiz (0000), que é a atividade padrão. As próximas três atividades são abrir o ServiceHost. A atividade na coluna 5 é o ouvinte e as atividades restantes (6 a 8) descrevem o processamento do WCF de uma mensagem, do processamento de bytes à ativação do código do usuário.

A imagem a seguir mostra uma exibição de gráfico das atividades do serviço WCF:

![Captura de tela do Visualizador de rastreamento mostrando uma lista de atividades do serviço WCF](./media/using-service-trace-viewer-for-viewing-correlated-traces-and-troubleshooting/wcf-service-activities.gif)

A captura de tela a seguir mostra as atividades do cliente e do serviço e realça a ação do processo adicionar atividade entre processos (laranja). As setas relacionam as mensagens de solicitação e resposta enviadas e recebidas pelo cliente e serviço. Os rastreamentos de ação de processo são separados entre processos no grafo, mas mostrados como parte da mesma atividade no painel superior direito. Nesse painel, podemos ver rastreamentos de cliente para mensagens enviadas seguidas por rastreamentos de serviço para mensagens recebidas e processadas.

As imagens a seguir mostram uma exibição de gráfico de atividades de cliente e serviço do WCF

![Grafo do Visualizador de rastreamento que mostra as atividades de cliente e de serviço do WCF.](./media/using-service-trace-viewer-for-viewing-correlated-traces-and-troubleshooting/wcf-client-service-activities.gif)

No cenário de erro a seguir, os rastreamentos de erro e de aviso no serviço e no cliente estão relacionados. Uma exceção é lançada primeiro no código do usuário no serviço (a atividade verde mais à direita que inclui um rastreamento de aviso para a exceção "o serviço não pode processar essa solicitação no código do usuário."). Quando a resposta é enviada ao cliente, um rastreamento de aviso é emitido novamente para denotar a mensagem de falha (atividade da esquerda rosa). Em seguida, o cliente fecha seu cliente WCF (atividade amarela no lado inferior esquerdo), que anula a conexão com o serviço. O serviço gera um erro (atividade rosa mais longa à direita).

![Usando o Visualizador de Rastreamento](media/wcfc-e2etrace9s.gif "wcfc_e2etrace9s")

Correlação de erros entre o serviço e o cliente

O exemplo usado para gerar esses rastreamentos é uma série de solicitações síncronas usando o wsHttpBinding. Há desvios desse grafo para cenários sem segurança, ou com solicitações assíncronas, em que a atividade processar ação abrange as operações de início e término que constituem a chamada assíncrona e mostra transferências para uma atividade de retorno de chamada. Para obter mais informações sobre cenários adicionais, consulte [cenários de rastreamento de ponta a ponta](end-to-end-tracing-scenarios.md).

## <a name="troubleshooting-using-the-service-trace-viewer"></a>Solução de problemas usando o Visualizador de rastreamento de serviço

Ao carregar arquivos de rastreamento na ferramenta do Visualizador de rastreamento de serviço, você pode selecionar qualquer atividade vermelha ou amarela no painel esquerdo para rastrear a causa de um problema em seu aplicativo. A atividade 000 normalmente tem exceções não tratadas que emergiram para o usuário.

A imagem a seguir mostra como selecionar uma atividade vermelha ou amarela para localizar a raiz de um problema.
![Captura de tela de atividades vermelhas ou amarelas para localizar a raiz de um problema.](./media/using-service-trace-viewer-for-viewing-correlated-traces-and-troubleshooting/service-trace-viewer.gif)

No painel superior direito, você pode examinar os rastreamentos para a atividade que você selecionou à esquerda. Em seguida, você pode examinar os rastreamentos vermelhos ou amarelos nesse painel e ver como eles estão correlacionados. No grafo anterior, vemos rastreamentos de avisos para o cliente e o serviço na mesma atividade de ação de processo.

Se esses rastreamentos não fornecerem a causa raiz do erro, você poderá utilizar o gráfico clicando duas vezes na atividade selecionada no painel esquerdo (aqui processar ação). Em seguida, o grafo com atividades relacionadas é exibido. Em seguida, você pode expandir as atividades relacionadas (clicando nos sinais "+") para localizar o primeiro rastreamento emitido em vermelho ou amarelo em uma atividade relacionada. Continue expandindo as atividades que ocorreram pouco antes do rastreamento vermelho ou amarelo de interesse, seguindo as transferências para atividades relacionadas ou fluxos de mensagens entre pontos de extremidade, até que você acompanhe a causa raiz do problema.

![Usando o Visualizador de Rastreamento](media/wcfc-e2etrace9s.gif "wcfc_e2etrace9s")

Expandindo atividades para acompanhar a causa raiz de um problema

Se o ServiceModel `ActivityTracing` estiver desativado, mas o rastreamento de ServiceModel estiver ativado, você poderá ver os rastreamentos de ServiceModel emitidos na atividade 0000. No entanto, isso requer mais esforço para entender a correlação desses rastreamentos.

Se o log de mensagens estiver habilitado, você poderá usar a guia mensagem para ver qual mensagem é afetada pelo erro. Ao clicar duas vezes em uma mensagem em vermelho ou amarelo, você poderá ver o modo de exibição de gráfico das atividades relacionadas. Essas atividades são as mais bem relacionadas à solicitação em que ocorreu um erro.

![Captura de tela do Visualizador de rastreamento com o log de mensagem habilitado.](./media/using-service-trace-viewer-for-viewing-correlated-traces-and-troubleshooting/message-logging-enabled.gif)

Para iniciar a solução de problemas, você também pode escolher um rastreamento de mensagem vermelho ou amarelo e clicar duas vezes nele para controlar a causa raiz.

## <a name="see-also"></a>Consulte também

- [Cenários de rastreamento ponta a ponta](end-to-end-tracing-scenarios.md)
- [Ferramenta Visualizador de rastreamento de serviço (SvcTraceViewer.exe)](../../service-trace-viewer-tool-svctraceviewer-exe.md)
- [Rastreamento](index.md)
