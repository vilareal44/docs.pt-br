---
title: Coleta de lixo e desempenho
description: Leia sobre os problemas relacionados à coleta de lixo e ao uso de memória. Aprenda a minimizar o efeito da coleta de lixo em seus aplicativos.
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- garbage collection, troubleshooting
- garbage collection, performance
ms.assetid: c203467b-e95c-4ccf-b30b-953eb3463134
ms.openlocfilehash: dee5a4b54806bdadc18d759c5df7016da060fd75
ms.sourcegitcommit: 7137e12f54c4e83a94ae43ec320f8cf59c1772ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84662843"
---
# <a name="garbage-collection-and-performance"></a>Coleta de lixo e desempenho

 Este tópico descreve problemas relacionados ao uso de memória e coleta de lixo. Ele aborda problemas relacionados a heap gerenciado e explica como minimizar o efeito da coleta de lixo em seus aplicativos. Cada problema tem links para procedimentos que podem ser usados para investigar problemas.

## <a name="performance-analysis-tools"></a>Ferramentas de análise de desempenho

As seções a seguir descrevem as ferramentas que estão disponíveis para investigar problemas de coleta de lixo e de uso de memória. Os [procedimentos](#performance-check-procedures) fornecidos mais adiante neste tópico se referem a essas ferramentas.

### <a name="memory-performance-counters"></a>Contadores de Desempenho de Memória

Você pode usar os contadores de desempenho para coletar dados de desempenho. Para obter instruções, consulte [Criação de perfil do runtime](../../framework/debug-trace-profile/runtime-profiling.md). A categoria de contadores de desempenho de memória CLR do .NET, conforme descrito em [Contadores de desempenho no .NET Framework](../../framework/debug-trace-profile/performance-counters.md), fornece informações sobre o coletor de lixo.

### <a name="debugging-with-sos"></a>Depuração com SOS

Você pode usar o [Depurador do Windows (WinDbg)](/windows-hardware/drivers/debugger/index) para inspecionar objetos no heap gerenciado.

Para instalar o WinDbg, instale as Ferramentas de Depuração para Windows da página [Download Debugging Tools for Windows](/windows-hardware/drivers/debugger/debugger-download-tools) (Baixar Ferramentas de Depuração para Windows).

### <a name="garbage-collection-etw-events"></a>Eventos ETW de coleta de lixo

O ETW (Rastreamento de Eventos para Windows) é um sistema de rastreamento que complementa o suporte à criação de perfil e à depuração fornecido pelo .NET Framework. A partir do .NET Framework 4, os [eventos ETW de coleta de lixo](../../framework/performance/garbage-collection-etw-events.md) capturam informações úteis para analisar o heap gerenciado do ponto de vista estatístico. Por exemplo, o `GCStart_V1` evento, que é acionado quando uma coleta de lixo está prestes a ocorrer, fornece as seguintes informações:

- Qual geração de objetos está sendo coletada.

- O que disparou a coleta de lixo.

- Tipo de coleta de lixo (simultânea ou não simultânea).

O log de eventos ETW é eficiente e não mascarará nenhum problema de desempenho associado à coleta de lixo. Um processo pode fornecer seus próprios eventos junto com eventos ETW. Quando registrados em log, tanto os eventos de coleta de lixo quanto os eventos do aplicativo podem ser correlacionados para determinar como e quando os problemas de heap ocorrem. Por exemplo, um aplicativo para servidores pode fornecer eventos no início e no final de uma solicitação de cliente.

### <a name="the-profiling-api"></a>A API de criação de perfil

As interfaces de criação de perfil do CLR (Common Language Runtime) fornecem informações detalhadas sobre os objetos que foram afetados durante a coleta de lixo. Um criador de perfil pode ser notificado sobre quando uma coleta de lixo começa e termina. Ele pode fornecer relatórios sobre os objetos no heap gerenciado, incluindo uma identificação de objetos em cada geração. Para obter mais informações, consulte [Visão geral de ferramentas de criação de perfil](../../framework/unmanaged-api/profiling/profiling-overview.md).

Criadores de perfil podem fornecer informações abrangentes. No entanto, criadores de perfil complexos têm o potencial de modificar o comportamento de um aplicativo.

### <a name="application-domain-resource-monitoring"></a>Monitoramento de recursos de domínio de aplicativo

Do .NET Framework 4 em diante, o ARM (monitoramento de recursos de domínio de aplicativo) permite que os hosts monitorem o uso de CPU e memória por domínio de aplicativo. Para obter mais informações, consulte [Monitoramento de recursos de domínio do aplicativo](app-domain-resource-monitoring.md).

## <a name="troubleshooting-performance-issues"></a>Solucionando problemas de desempenho

A primeira etapa é [determinar se o problema é realmente a coleta de lixo](#IsGC). Se você determinar que é, selecione um item da lista a seguir para solucionar o problema.

- [Uma exceção de falta de memória é lançada](#Issue_OOM)

- [O processo usa memória demais](#Issue_TooMuchMemory)

- [O coletor de lixo não recupera objetos rápido o suficiente](#Issue_NotFastEnough)

- [O heap gerenciado está muito fragmentado](#Issue_Fragmentation)

- [As pausas na coleta de lixo são longas demais](#Issue_LongPauses)

- [A geração 0 é grande demais](#Issue_Gen0)

- [O uso da CPU durante uma coleta de lixo é muito alto](#Issue_HighCPU)

<a name="Issue_OOM"></a>

### <a name="issue-an-out-of-memory-exception-is-thrown"></a>Problema: uma exceção de falta de memória é lançada

Há dois casos legítimos para um <xref:System.OutOfMemoryException> gerenciado ser lançado:

- Falta de memória virtual.

  O coletor de lixo aloca memória do sistema em segmentos com tamanho predeterminado. Se uma alocação requer um segmento adicional mas não há nenhum bloco contíguo livre restante no espaço de memória virtual do processo, a alocação de heap gerenciado falha.

- Não há memória física suficiente para alocar.

|Verificações de desempenho|
|------------------------|
|[Determine se a exceção de falta de memória é gerenciada.](#OOMIsManaged)<br /><br /> [Determine a quantidade de memória virtual que pode ser reservada.](#GetVM)<br /><br /> [Determine se há memória física suficiente.](#Physical)|

Se você determinar que a exceção não é legítima, entre em contato com o Serviço de Suporte e Atendimento ao Cliente Microsoft com as seguintes informações:

- A pilha com a exceção de falta de memória gerenciada.

- O despejo de memória completo.

- Dados que provam que não é uma exceção de memória insuficiente legítima, incluindo dados mostrando que a memória física ou virtual não são um problema.

<a name="Issue_TooMuchMemory"></a>

### <a name="issue-the-process-uses-too-much-memory"></a>Problema: O processo usa memória demais

Um pressuposto comum é que o uso de memória a exibir na guia **Desempenho** do Gerenciador de Tarefas do Windows pode indicar quando memória demais está sendo usada. No entanto, essa exibição pertence ao conjunto de trabalho; ela não fornece informações sobre o uso de memória virtual.

Se você determinar que o problema é causado pelo heap gerenciado, meça o heap gerenciado ao longo do tempo para determinar eventuais padrões.

Se você determinar que o problema não é causado pelo heap gerenciado, use a depuração nativa.

|Verificações de desempenho|
|------------------------|
|[Determine a quantidade de memória virtual que pode ser reservada.](#GetVM)<br /><br /> [Determine a quantidade de memória que o heap gerenciado está confirmando.](#ManagedHeapCommit)<br /><br /> [Determine a quantidade de memória que o heap gerenciado reserva.](#ManagedHeapReserve)<br /><br /> [Determine objetos grandes na geração 2.](#ExamineGen2)<br /><br /> [Determine referências para os objetos.](#ObjRef)|

<a name="Issue_NotFastEnough"></a>

### <a name="issue-the-garbage-collector-does-not-reclaim-objects-fast-enough"></a>Problema: o coletor de lixo não recupera objetos com rapidez suficiente

Quando ele é exibido como se objetos não estivessem sendo recuperados conforme o esperado para coleta de lixo, determine se há quaisquer referências fortes a esses objetos.

Você também poderá encontrar esse problema caso não tenha havido nenhuma coleta de lixo para a geração que contém um objeto inativo, o que indicará que o finalizador do objeto inativo não foi executado. Por exemplo, isso é possível quando você está executando um aplicativo STA (single-threaded apartment) e o thread que cuida da fila de finalizadores não é capaz de chamá-la.

|Verificações de desempenho|
|------------------------|
|[Verifique as referências para os objetos.](#ObjRef)<br /><br /> [Determine se um finalizador foi executado.](#Induce)<br /><br /> [Determine se há objetos aguardando finalização.](#Finalize)|

<a name="Issue_Fragmentation"></a>

### <a name="issue-the-managed-heap-is-too-fragmented"></a>Problema: o heap gerenciado está fragmentado demais

O nível de fragmentação é calculado como a proporção entre o espaço livre e o total de memória alocada para a geração. Para a geração 2, um nível aceitável de fragmentação é de não mais que 20%. Já que a geração 2 pode ficar muito grande, a taxa de fragmentação é mais importante do que o valor absoluto.

Ter muito espaço livre na geração 0 não é um problema, pois esta é a geração em que novos objetos são alocados.

No heap de objetos grandes sempre ocorre fragmentação, porque ele não é compactado. Objetos livres adjacentes são naturalmente recolhidos em um único espaço para atender a solicitações de alocação de objetos grandes.

A fragmentação pode se tornar um problema na geração 1 e geração 2. Se esses gerações tiverem uma grande quantidade de espaço livre após uma coleta de lixo, o uso de objetos de um aplicativo poderá precisar de modificações e você deverá considerar reavaliar o tempo de vida dos objetos de longo prazo.

O excesso de fixação de objetos pode aumentar a fragmentação. Se a fragmentação for alta, muitos objetos poderão ter sido fixados.

Se a fragmentação da memória virtual estiver impedindo que o coletor de lixo adicione segmentos, as causas poderão ser uma dos seguintes:

- Carregamento e descarregamento frequente de muitos assemblies pequenos.

- Retenção de um número excessivo de referências a objetos COM ao interoperar com código não gerenciado.

- Criação de objetos transitórios grandes, que faz com que o heap de objeto grande aloque e libere segmentos de heap frequentemente.

  Ao hospedar o CLR, um aplicativo pode solicitar que o coletor de lixo retenha seus segmentos. Isso reduz a frequência de alocações de segmento. Isso é feito usando o sinalizador STARTUP_HOARD_GC_VM na [Enumeração STARTUP_FLAGS](../../framework/unmanaged-api/hosting/startup-flags-enumeration.md).

|Verificações de desempenho|
|------------------------|
|[Determine a quantidade de espaço livre no heap gerenciado.](#Fragmented)<br /><br /> [Determine o número de objetos fixados.](#Pinned)|

Se você acha que não há nenhuma causa legítima para a fragmentação, entre em contato com o Serviço de Suporte e Atendimento ao Cliente Microsoft.

<a name="Issue_LongPauses"></a>

### <a name="issue-garbage-collection-pauses-are-too-long"></a>Problema: as pausas na coleta de lixo são longas demais

A coleta de lixo opera em tempo real flexível, de modo que um aplicativo deve ser capaz de tolerar algumas pausas. Um critério para o tempo real flexível é que 95% das operações deve terminar no prazo.

Na coleta de lixo simultânea, a execução de threads gerenciados é permitida durante uma coleta, o que significa que as pausas são realmente mínimas.

Coletas de lixo efêmero (gerações 0 e 1) duram somente alguns milissegundos, então diminuir as pausas geralmente não é viável. No entanto, você pode diminuir as pausas em coletas da geração 2 alterando o padrão de solicitações de alocação por um aplicativo.

Outro método mais preciso é usar [eventos ETW de coleta de lixo](../../framework/performance/garbage-collection-etw-events.md). Você pode encontrar os intervalos para coletas adicionando as diferenças de carimbo de data/hora a uma sequência de eventos. A sequência de coleta inteira inclui a suspensão do mecanismo de execução, a própria coleta de lixo e a retomada do mecanismo de execução.

Você pode usar [notificações de coleta de lixo](notifications.md) para determinar se um servidor está prestes a ter uma coleta de geração 2 e se o redirecionamento solicitações para outro servidor pode reduzir os problemas com pausas.

|Verificações de desempenho|
|------------------------|
|[Determine a duração de uma coleta de lixo.](#TimeInGC)<br /><br /> [Determine o que causou uma coleta de lixo.](#Triggered)|

<a name="Issue_Gen0"></a>

### <a name="issue-generation-0-is-too-big"></a>Problema: a geração 0 é grande demais

É provável que a geração 0 tenha um número maior de objetos em um sistema de 64 bits, especialmente quando você usa a coleta de lixo do servidor em vez de uma coleta de lixo de estação de trabalho. Isso ocorre porque o limite para disparar uma coleta de lixo de geração 0 é maior nesses ambientes e as coletas da geração 0 podem ficar muito maiores. O desempenho é aprimorado quando um aplicativo aloca mais memória antes de uma coleta de lixo ser disparada.

<a name="Issue_HighCPU"></a>

### <a name="issue-cpu-usage-during-a-garbage-collection-is-too-high"></a>Problema: o uso da CPU durante uma coleta de lixo é alto demais

O uso da CPU será alto durante uma coleta de lixo. Se uma quantidade significativa de tempo de processamento é gasto em uma coleta de lixo, isso indica que o número de coletas é frequente demais ou que a coleta é longa demais. Uma maior taxa de alocação de objetos no heap gerenciado faz com que a coleta de lixo ocorra com mais frequência. Diminuir a taxa de alocação reduz a frequência de coletas de lixo.

Você pode monitorar as taxas de alocação usando o contador de desempenho de `Allocated Bytes/second`. Para obter mais informações, consulte [Contadores de desempenho no .NET Framework](../../framework/debug-trace-profile/performance-counters.md).

A duração de uma coleta é essencialmente um fator do número de objetos que sobrevivem após a alocação. O coletor de lixo deve passar por uma grande quantidade de memória se restam muitos objetos a serem coletados. O trabalho para compactar os sobreviventes é demorado. Para determinar quantos objetos foram manipulados durante uma coleta, defina um ponto de interrupção no depurador no final de uma coleta de lixo para uma geração especificada.

|Verificações de desempenho|
|------------------------|
|[Determine se o alto uso da CPU é causado pela coleta de lixo.](#HighCPU)<br /><br /> [Defina um ponto de interrupção no final da coleta de lixo.](#GenBreak)|

## <a name="troubleshooting-guidelines"></a>Diretrizes de solução de problemas

Esta seção descreve as diretrizes que você deve considerar ao começar suas investigações.

### <a name="workstation-or-server-garbage-collection"></a>Estação de trabalho ou coleta de lixo do servidor

Determine se você está usando o tipo correto de coleta de lixo. Se seu aplicativo usa vários threads e instâncias de objeto, use a coleta de lixo do servidor em vez da coleta de lixo da estação de trabalho. A coleta de lixo do servidor funciona em vários threads, enquanto a coleta de lixo da estação de trabalho requer várias instâncias de um aplicativo para executar seus próprios threads de coleta de lixo e competir pelo tempo de CPU.

Um aplicativo que tem uma carga baixa e que realiza tarefas com pouca frequência em segundo plano, por exemplo, um serviço, poderia usar a coleta de lixo de estação de trabalho com a coleta de lixo simultânea desabilitada.

### <a name="when-to-measure-the-managed-heap-size"></a>Quando medir o tamanho do heap gerenciado

A menos que você esteja usando um criador de perfil, você terá que estabelecer um padrão de medição consistente para diagnosticar problemas de desempenho de modo eficaz. Considere os pontos a seguir ao estabelecer uma agenda:

- Se você medir após uma coleta de lixo da geração 2, todo o heap gerenciado estará livre de lixo (objetos inativos).

- Se você medir imediatamente após uma coleta de lixo da geração 0, os objetos nas gerações 1 e 2 ainda não serão coletados.

- Se você medir imediatamente antes de uma coleta de lixo, você medirá o máximo de alocação possível antes do início da coleta de lixo.

- Medir durante uma coleta de lixo é problemático, pois as estruturas de dados do coletor de lixo não estão em um estado válido para passagem e podem não ser capazes de fornecer a você os resultados completos. Isso ocorre por design.

- Quando você usa a coleta de lixo de estação de trabalho com coleta de lixo simultânea, os objetos recuperados não são compactados, portanto, o tamanho do heap pode ser igual ou maior (a fragmentação pode fazer com que pareça maior).

- A coleta de lixo simultânea na geração 2 é atrasada quando a carga de memória física é alta demais.

O procedimento a seguir descreve como definir um ponto de interrupção para que você possa medir o heap gerenciado.

<a name="GenBreak"></a>

#### <a name="to-set-a-breakpoint-at-the-end-of-garbage-collection"></a>Para definir um ponto de interrupção no final da coleta de lixo

- No WinDbg, com a extensão de depurador SOS carregada, digite o seguinte comando:

  **bp mscorwks!WKS::GCHeap::RestartEE "j (dwo(mscorwks!WKS::GCHeap::GcCondemnedGeneration)==2) 'kb';'g'"**

  em que **GcCondemnedGeneration** é definido para a geração desejada. Esse comando requer símbolos privados.

  Esse comando força uma interrupção se **RestartEE** é executado após objetos da geração 2 serem recuperados para coleta de lixo.

  Na coleta de lixo do servidor, apenas um thread chama **RestartEE**, portanto, o ponto de interrupção ocorrerá apenas uma vez durante uma coleta de lixo da geração 2.

## <a name="performance-check-procedures"></a>Procedimentos de verificação de desempenho

Esta seção descreve os procedimentos a seguir para isolar a causa do problema de desempenho:

- [Determine se o problema é causado pela coleta de lixo.](#IsGC)

- [Determine se a exceção de falta de memória é gerenciada.](#OOMIsManaged)

- [Determine a quantidade de memória virtual que pode ser reservada.](#GetVM)

- [Determine se há memória física suficiente.](#Physical)

- [Determine a quantidade de memória que o heap gerenciado está confirmando.](#ManagedHeapCommit)

- [Determine a quantidade de memória que o heap gerenciado reserva.](#ManagedHeapReserve)

- [Determine objetos grandes na geração 2.](#ExamineGen2)

- [Determine referências para os objetos.](#ObjRef)

- [Determine se um finalizador foi executado.](#Induce)

- [Determine se há objetos aguardando finalização.](#Finalize)

- [Determine a quantidade de espaço livre no heap gerenciado.](#Fragmented)

- [Determine o número de objetos fixados.](#Pinned)

- [Determine a duração de uma coleta de lixo.](#TimeInGC)

- [Determine o que disparou uma coleta de lixo.](#Triggered)

- [Determine se o alto uso da CPU é causado pela coleta de lixo.](#HighCPU)

<a name="IsGC"></a>

### <a name="to-determine-whether-the-problem-is-caused-by-garbage-collection"></a>Para determinar se o problema é causado pela coleta de lixo

- Examine os dois contadores de desempenho de memória a seguir:

  - **% De tempo em GC**. Exibe o percentual de tempo decorrido que foi gasto na execução de uma coleta de lixo após o último ciclo de coleta de lixo. Use este contador para determinar se o coletor de lixo está gastando tempo demais para disponibilizar espaço de heap gerenciado. Se o tempo gasto na coleta de lixo for relativamente baixo, isso poderá indicar um problema de recurso fora do heap gerenciado. Esse contador pode não ser preciso quando coleta de lixo simultânea ou em segundo plano está envolvida.

  - **N º total de bytes confirmados**. Exibe a quantidade de memória virtual confirmada atualmente pelo coletor de lixo. Use este contador para determinar se a memória consumida pelo coletor de lixo é uma parte excessiva da memória usada pelo aplicativo.

  A maioria dos contadores de desempenho de memória é atualizada no final de cada coleta de lixo. Portanto, eles podem não refletir as condições atuais sobre as quais você deseja obter informações.

<a name="OOMIsManaged"></a>

### <a name="to-determine-whether-the-out-of-memory-exception-is-managed"></a>Para determinar se a exceção de falta de memória é gerenciada

1. No depurador do Visual Studio ou WinDbg com a extensão de depurador SOS carregada, digite o comando de exceção de impressão (**pe**):

    **! PE**

    Se a exceção for gerenciada, <xref:System.OutOfMemoryException> será exibido como o tipo de exceção, conforme mostrado no exemplo a seguir.

    ```console
    Exception object: 39594518
    Exception type: System.OutOfMemoryException
    Message: <none>
    InnerException: <none>
    StackTrace (generated):
    ```

2. Se a saída não especificar uma exceção, você precisará determinar de qual thread é a exceção de falta de memória. Digite o comando a seguir no depurador para mostrar todos os threads com suas pilhas de chamadas:

    **~\*quilobyte**

    O thread com a pilha que tem chamadas de exceção é indicado pelo argumento `RaiseTheException`. Esse é o objeto de exceção gerenciada.

    ```console
    28adfb44 7923918f 5b61f2b4 00000000 5b61f2b4 mscorwks!RaiseTheException+0xa0
    ```

3. Você pode usar o comando a seguir para despejar exceções aninhadas.

    **!pe -nested**

    Se você não encontrar nenhuma exceção, isso significará que a exceção de falta de memória se originou de código não gerenciado.

<a name="GetVM"></a>

### <a name="to-determine-how-much-virtual-memory-can-be-reserved"></a>Para determinar a quantidade de memória virtual que pode ser reservada

- No WinDbg, com a extensão de depurador SOS carregada, digite o seguinte comando para obter a maior região livre:

  **!address -summary**

  A maior região livre é exibida conforme mostrado na saída a seguir.

  ```console
  Largest free region: Base 54000000 - Size 0003A980
  ```

  Neste exemplo, o tamanho da maior região livre é aproximadamente 24.000 KB (3A980 em hexadecimal). Essa região é menor do que o tamanho requerido pelo coletor de lixo para um segmento.

  -ou-

- Use o comando **vmstat**:

  **!vmstat**

  A maior região livre é o maior valor encontrado na coluna MAXIMUM, conforme mostrado na saída a seguir.

  ```console
  TYPE        MINIMUM   MAXIMUM     AVERAGE   BLK COUNT   TOTAL
  ~~~~        ~~~~~~~   ~~~~~~~     ~~~~~~~   ~~~~~~~~~~  ~~~~
  Free:
  Small       8K        64K         46K       36          1,671K
  Medium      80K       864K        349K      3           1,047K
  Large       1,384K    1,278,848K  151,834K  12          1,822,015K
  Summary     8K        1,278,848K  35,779K   51          1,824,735K
  ```

<a name="Physical"></a>

### <a name="to-determine-whether-there-is-enough-physical-memory"></a>Para determinar se há memória física suficiente

1. Inicie o Gerenciador de tarefas do Windows.

2. Na guia **Desempenho**, examine o valor confirmado. (No Windows 7, examine **Confirmar (KB)** no **grupo Sistema**.)

    Se o **Total** está próximo do **Limite**, você está com pouca memória física.

<a name="ManagedHeapCommit"></a>

### <a name="to-determine-how-much-memory-the-managed-heap-is-committing"></a>Para determinar a quantidade de memória que o heap gerenciado está confirmando

- Use o contador de desempenho de memória de `# Total committed bytes` para obter o número de bytes que o heap gerenciado está confirmando. O coletor de lixo confirma partes em um segmento conforme necessário, nem todas ao mesmo tempo.

  > [!NOTE]
  > Não use o contador de desempenho de `# Bytes in all Heaps`, porque ele não representa o uso de memória real pelo heap gerenciado. O tamanho de uma geração está incluído no valor e é na verdade seu tamanho limite, ou seja, o tamanho que induz uma coleta de lixo caso a geração esteja preenchida com objetos. Portanto, esse valor geralmente é zero.

<a name="ManagedHeapReserve"></a>

### <a name="to-determine-how-much-memory-the-managed-heap-reserves"></a>Para determinar a quantidade de memória que o heap gerenciado reserva

- Use o contador de desempenho de memória de `# Total reserved bytes`.

  O coletor de lixo reserva de memória em segmentos, sendo que você pode determinar a localização do início de um segmento usando o comando **eeheap**.

  > [!IMPORTANT]
  > Embora você possa determinar a quantidade de memória que o coletor de lixo aloca para cada segmento, o tamanho de segmento é específico da implementação e está sujeito a alterações a qualquer momento, incluindo atualizações periódicas. Seu aplicativo nunca deve fazer suposições sobre o tamanho de um segmento em particular nem depender dele, tampouco deve tentar configurar a quantidade de memória disponível para alocações de segmento.

- No depurador do Visual Studio ou WinDbg com a extensão de depurador SOS carregada, digite o comando a seguir:

  **!eeheap -gc**

  O resultado é demonstrado a seguir.

  ```console
  Number of GC Heaps: 2
  ------------------------------
  Heap 0 (002db550)
  generation 0 starts at 0x02abe29c
  generation 1 starts at 0x02abdd08
  generation 2 starts at 0x02ab0038
  ephemeral segment allocation context: none
    segment    begin allocated     size
  02ab0000 02ab0038  02aceff4 0x0001efbc(126908)
  Large object heap starts at 0x0aab0038
    segment    begin allocated     size
  0aab0000 0aab0038  0aab2278 0x00002240(8768)
  Heap Size   0x211fc(135676)
  ------------------------------
  Heap 1 (002dc958)
  generation 0 starts at 0x06ab1bd8
  generation 1 starts at 0x06ab1bcc
  generation 2 starts at 0x06ab0038
  ephemeral segment allocation context: none
    segment    begin allocated     size
  06ab0000 06ab0038  06ab3be4 0x00003bac(15276)
  Large object heap starts at 0x0cab0038
    segment    begin allocated     size
  0cab0000 0cab0038  0cab0048 0x00000010(16)
  Heap Size    0x3bbc(15292)
  ------------------------------
  GC Heap Size   0x24db8(150968)
  ```

  Os endereços indicados por "segmento" são os endereços iniciais dos segmentos.

<a name="ExamineGen2"></a>

### <a name="to-determine-large-objects-in-generation-2"></a>Para determinar objetos grandes na geração 2

- No depurador do Visual Studio ou WinDbg com a extensão de depurador SOS carregada, digite o comando a seguir:

  **!dumpheap –stat**

  Se o heap gerenciado é grande, **dumpheap** pode levar algum tempo para concluir.

  Você pode começar a analisar pelas últimas poucas linhas da saída, pois elas listam os objetos que usam mais espaço. Por exemplo:

  ```console
  2c6108d4   173712     14591808 DevExpress.XtraGrid.Views.Grid.ViewInfo.GridCellInfo
  00155f80      533     15216804      Free
  7a747c78   791070     15821400 System.Collections.Specialized.ListDictionary+DictionaryNode
  7a747bac   700930     19626040 System.Collections.Specialized.ListDictionary
  2c64e36c    78644     20762016 DevExpress.XtraEditors.ViewInfo.TextEditViewInfo
  79124228   121143     29064120 System.Object[]
  035f0ee4    81626     35588936 Toolkit.TlkOrder
  00fcae40     6193     44911636 WaveBasedStrategy.Tick_Snap[]
  791242ec    40182     90664128 System.Collections.Hashtable+bucket[]
  790fa3e0  3154024    137881448 System.String
  Total 8454945 objects
  ```

  O último objeto listado é uma cadeia de caracteres e ocupa mais espaço. Você pode examinar o aplicativo para ver como os objetos de cadeia de caracteres podem ser otimizados. Para ver as cadeias de caracteres que têm entre 150 e 200 bytes, digite o seguinte:

  **!dumpheap -type System.String -min 150 -max 200**

  Um exemplo dos resultados é conforme demonstrado a seguir.

  ```console
  Address  MT           Size  Gen
  1875d2c0 790fa3e0      152    2 System.String HighlightNullStyle_Blotter_PendingOrder-11_Blotter_PendingOrder-11
  …
  ```

  Para uma ID, usar um número inteiro em vez de uma cadeia de caracteres pode ser mais eficiente. Se a mesma cadeia de caracteres está sendo repetida milhares de vezes, considere a centralização da cadeia de caracteres. Confira mais informações sobre a centralização da cadeia de caracteres no tópico de referência para o método <xref:System.String.Intern%2A?displayProperty=nameWithType>.

<a name="ObjRef"></a>

### <a name="to-determine-references-to-objects"></a>Para determinar referências a objetos

- No WinDbg, com a extensão de depurador SOS carregada, digite o seguinte comando para listar as referências a objetos:

  **!gcroot**

  `-or-`

- Para determinar as referências a um objeto específico, inclua o endereço:

  **!gcroot 1c37b2ac**

  Raízes encontradas em pilhas podem ser falsos positivos. Para obter mais informações, use o comando `!help gcroot`.

  ```console
  ebx:Root:19011c5c(System.Windows.Forms.Application+ThreadContext)->
  19010b78(DemoApp.FormDemoApp)->
  19011158(System.Windows.Forms.PropertyStore)->
  … [omitted]
  1c3745ec(System.Data.DataTable)->
  1c3747a8(System.Data.DataColumnCollection)->
  1c3747f8(System.Collections.Hashtable)->
  1c376590(System.Collections.Hashtable+bucket[])->
  1c376c98(System.Data.DataColumn)->
  1c37b270(System.Data.Common.DoubleStorage)->
  1c37b2ac(System.Double[])
  Scan Thread 0 OSTHread 99c
  Scan Thread 6 OSTHread 484
  ```

  O comando **gcroot** pode demorar muito tempo para concluir. Qualquer objeto que não é recuperado pela coleta de lixo é um objeto ativo. Isso significa que alguma raiz está ligando-se direta ou indiretamente ao objeto, então **gcroot** deve retornar informações de caminho do objeto. Você deve examinar os grafos retornados e ver por que esses objetos ainda são referenciados.

<a name="Induce"></a>

### <a name="to-determine-whether-a-finalizer-has-been-run"></a>Para determinar se um finalizador foi executado

- Execute um programa de teste que contenha o código a seguir:

  ```csharp
  GC.Collect();
  GC.WaitForPendingFinalizers();
  GC.Collect();
  ```

  Se o teste resolve o problema, isso significa que o coletor de lixo não estava recuperando objetos, pois os finalizadores desses objetos tinham sido suspensos. O método <xref:System.GC.WaitForPendingFinalizers%2A?displayProperty=nameWithType> permite que os finalizadores concluam suas tarefas e corrige o problema.

<a name="Finalize"></a>

### <a name="to-determine-whether-there-are-objects-waiting-to-be-finalized"></a>Para determinar se há objetos aguardando a finalização

1. No depurador do Visual Studio ou WinDbg com a extensão de depurador SOS carregada, digite o comando a seguir:

    **!finalizequeue**

    Observe o número de objetos que estão prontos para finalização. Se o número for alto, você deverá examinar o motivo pelo qual esses finalizadores não são capazes de avançar em absoluto ou não são capazes de avançar suficientemente rápido.

2. Para obter uma saída de threads, digite o seguinte comando:

    **threads -special**

    Esse comando fornece uma saída como a mostrada a seguir.

    ```console
       OSID     Special thread type
    2    cd0    DbgHelper
    3    c18    Finalizer
    4    df0    GC SuspendEE
    ```

    O thread do finalizador indica qual finalizador, caso haja um, está sendo executado. Quando um thread do finalizador não está executando nenhum finalizador, ele está aguardando que um evento o diga para fazer seu trabalho. Na maioria das vezes você verá o thread do finalizador nesse estado porque ele é executado em THREAD_HIGHEST_PRIORITY e deve concluir a execução de finalizadores, caso haja um, muito rapidamente.

<a name="Fragmented"></a>

### <a name="to-determine-the-amount-of-free-space-in-the-managed-heap"></a>Para determinar a quantidade de espaço livre no heap gerenciado

- No depurador do Visual Studio ou WinDbg com a extensão de depurador SOS carregada, digite o comando a seguir:

  **!dumpheap -type Free -stat**

  Esse comando exibe o tamanho total de todos os objetos livres no heap gerenciado, conforme mostrado no exemplo a seguir.

  ```console
  total 230 objects
  Statistics:
        MT    Count    TotalSize Class Name
  00152b18      230     40958584      Free
  Total 230 objects
  ```

- Para determinar o espaço livre na geração 0, digite o comando a seguir para obter informações de consumo de memória por geração:

  **!eeheap -gc**

  Esse comando exibe uma saída semelhante à seguinte. A última linha mostra o segmento efêmero.

  ```console
  Heap 0 (0015ad08)
  generation 0 starts at 0x49521f8c
  generation 1 starts at 0x494d7f64
  generation 2 starts at 0x007f0038
  ephemeral segment allocation context: none
  segment  begin     allocated  size
  00178250 7a80d84c  7a82f1cc   0x00021980(137600)
  00161918 78c50e40  78c7056c   0x0001f72c(128812)
  007f0000 007f0038  047eed28   0x03ffecf0(67103984)
  3a120000 3a120038  3a3e84f8   0x002c84c0(2917568)
  46120000 46120038  49e05d04   0x03ce5ccc(63855820)
  ```

- Calcule o espaço usado pela geração 0:

  **? 49e05d04-0x49521f8c**

  O resultado é demonstrado a seguir. A geração 0 usa aproximadamente 9 MB.

  ```console
  Evaluate expression: 9321848 = 008e3d78
  ```

- O comando a seguir despeja o espaço livre dentro do intervalo de geração 0:

  **!dumpheap -type Free -stat 0x49521f8c 49e05d04**

  O resultado é demonstrado a seguir.

  ```console
  ------------------------------
  Heap 0
  total 409 objects
  ------------------------------
  Heap 1
  total 0 objects
  ------------------------------
  Heap 2
  total 0 objects
  ------------------------------
  Heap 3
  total 0 objects
  ------------------------------
  total 409 objects
  Statistics:
        MT    Count TotalSize Class Name
  0015a498      409   7296540      Free
  Total 409 objects
  ```

  A saída mostra que a parte de geração 0 do heap está usando 9 MB de espaço para objetos e tem 7 MB livres. Essa análise mostra a extensão para a qual a geração 0 contribui para a fragmentação. Essa quantidade de uso do heap deve ser desconsiderada do valor total como a causa de fragmentação por objetos de longo prazo.

<a name="Pinned"></a>

### <a name="to-determine-the-number-of-pinned-objects"></a>Para determinar o número de objetos fixados

- No depurador do Visual Studio ou WinDbg com a extensão de depurador SOS carregada, digite o comando a seguir:

  **!gchandles**

  As estatísticas exibidas incluem o número de identificadores fixados, tal como mostra o exemplo a seguir.

  ```console
  GC Handle Statistics:
  Strong Handles:      29
  Pinned Handles:      10
  ```

<a name="TimeInGC"></a>

### <a name="to-determine-the-length-of-time-in-a-garbage-collection"></a>Para determinar a duração de uma coleta de lixo

- Examine o contador de desempenho de memória de `% Time in GC`.

  O valor é calculado usando um intervalo de tempo de exemplo. Já que os contadores são atualizados ao final de cada coleta de lixo, a amostra atual terá o mesmo valor que a amostra anterior se nenhuma coleta tiver ocorrido durante o intervalo.

  O tempo de coleta é obtido multiplicando o tempo de intervalo de exemplo pelo valor do percentual.

  Os dados a seguir mostram quatro intervalos de amostragem de dois segundos, para um estudo de oito segundos. As colunas `Gen0`, `Gen1` e `Gen2` mostram o número de coletas de lixo ocorridas durante esse intervalo para essa geração.

  ```console
  Interval    Gen0    Gen1    Gen2    % Time in GC
          1       9       3       1              10
          2      10       3       1               1
          3      11       3       1               3
          4      11       3       1               3
  ```

  Essas informações não mostram quando a coleta de lixo ocorreu, mas você pode determinar o número de coletas de lixo que ocorreram em um intervalo de tempo. Supondo o pior cenário possível, a décima coleta de lixo de geração 0 terminou no início do segundo intervalo e a décima primeira coleta de lixo de geração 0 foi concluída no final do quinto intervalo. O tempo entre o final da décima e o final da décima primeira coleta de lixo é cerca de dois segundos e o contador de desempenho mostra 3%, portanto, a duração da décima primeira coleta de lixo de geração 0 foi (dois segundos * 3% = 60 ms).

  Neste exemplo, há cinco períodos.

  ```console
  Interval    Gen0    Gen1    Gen2     % Time in GC
          1       9       3       1                3
          2      10       3       1                1
          3      11       4       2                1
          4      11       4       2                1
          5      11       4       2               20
  ```

  A segunda coleta de lixo de geração 2 começou durante o terceiro intervalo e terminou no quinto intervalo. Supondo o pior cenário possível, a última coleta de lixo foi para uma coleta de geração 0 que terminou no início do segundo intervalo e a coleta de lixo de geração 2 terminou no final do quinto intervalo. Portanto, o tempo entre o fim da coleta de lixo de geração 0 e o fim da coleta de lixo de geração 2 é de quatro segundos. Já que o contador de `% Time in GC` é 20%, a quantidade máxima de tempo que a coleta de lixo de geração 2 poderia ter levado é (quatro segundos * 20% = 800 ms).

- Como alternativa, você pode determinar a duração de uma coleta de lixo usando [eventos ETW de coleta de lixo](../../framework/performance/garbage-collection-etw-events.md) e analisando as informações para determinar a duração da coleta de lixo.

  Por exemplo, os dados a seguir mostram uma sequência de eventos que ocorreram durante uma coleta de lixo não simultânea.

  ```console
  Timestamp    Event name
  513052        GCSuspendEEBegin_V1
  513078        GCSuspendEEEnd
  513090        GCStart_V1
  517890        GCEnd_V1
  517894        GCHeapStats
  517897        GCRestartEEBegin
  517918        GCRestartEEEnd
  ```

  Suspender o thread gerenciado levou 26 us (`GCSuspendEEEnd` – `GCSuspendEEBegin_V1`).

  A coleta de lixo propriamente dita levou 4,8 ms (`GCEnd_V1` – `GCStart_V1`).

  Retomar os threads gerenciados levou 21 us (`GCRestartEEEnd` – `GCRestartEEBegin`).

  A saída a seguir fornece um exemplo para coleta de lixo em segundo plano e inclui o processo, thread e campos de evento. (Nem todos os dados são mostrados.)

  ```console
  timestamp(us)    event name            process    thread    event field
  42504385        GCSuspendEEBegin_V1    Test.exe    4372             1
  42504648        GCSuspendEEEnd         Test.exe    4372
  42504816        GCStart_V1             Test.exe    4372        102019
  42504907        GCStart_V1             Test.exe    4372        102020
  42514170        GCEnd_V1               Test.exe    4372
  42514204        GCHeapStats            Test.exe    4372        102020
  42832052        GCRestartEEBegin       Test.exe    4372
  42832136        GCRestartEEEnd         Test.exe    4372
  63685394        GCSuspendEEBegin_V1    Test.exe    4744             6
  63686347        GCSuspendEEEnd         Test.exe    4744
  63784294        GCRestartEEBegin       Test.exe    4744
  63784407        GCRestartEEEnd         Test.exe    4744
  89931423        GCEnd_V1               Test.exe    4372        102019
  89931464        GCHeapStats            Test.exe    4372
  ```

  O evento `GCStart_V1` em 42504816 indica que esta é uma coleta de lixo em segundo plano, porque o último campo é `1`. Essa torna-se a coleta de lixo Nº 102019.

  O evento `GCStart` ocorre porque há necessidade de uma coleta de lixo efêmero antes de você iniciar uma coleta de lixo em segundo plano. Essa torna-se a coleta de lixo Nº 102020.

  Em 42514170, a coleta de lixo Nº 102020 é concluída. Os threads gerenciados são reiniciados nesse momento. Isso é concluído no thread 4372, que disparou essa coleta de lixo em segundo plano.

  No thread 4744, ocorre uma suspensão. Essa é a única vez em que a coleta de lixo em segundo plano precisa suspender os threads gerenciados. Essa duração é de aproximadamente 99 ms ((63784407-63685394)/1000).

  O evento `GCEnd` para a coleta de lixo em segundo plano está em 89931423. Isso significa que a coleta de lixo em segundo plano durou cerca de 47 segundos ((89931423-42504816)/1000).

  Enquanto os threads gerenciados estão em execução, você pode ver qualquer número de coletas de lixo efêmero ocorrendo.

<a name="Triggered"></a>

### <a name="to-determine-what-triggered-a-garbage-collection"></a>Para determinar o que disparou uma coleta de lixo

- No depurador do Visual Studio ou WinDbg com a extensão de depurador SOS carregada, digite o comando a seguir para mostrar todos os threads com suas pilhas de chamadas:

  **~\*quilobyte**

  Esse comando exibe uma saída semelhante à seguinte.

  ```console
  0012f3b0 79ff0bf8 mscorwks!WKS::GCHeap::GarbageCollect
  0012f454 30002894 mscorwks!GCInterface::CollectGeneration+0xa4
  0012f490 79fa22bd fragment_ni!request.Main(System.String[])+0x48
  ```

  Se a coleta de lixo tiver sido causada por uma notificação de falta de memória do sistema operacional, a pilha de chamadas será semelhante, exceto pelo fato de que o thread é o thread do finalizador. O thread do finalizador obtém uma notificação assíncrona de memória insuficiente e induz à coleta de lixo.

  Se a coleta de lixo tiver sido causada pela alocação de memória, a pilha será exibida da seguinte maneira:

  ```console
  0012f230 7a07c551 mscorwks!WKS::GCHeap::GarbageCollectGeneration
  0012f2b8 7a07cba8 mscorwks!WKS::gc_heap::try_allocate_more_space+0x1a1
  0012f2d4 7a07cefb mscorwks!WKS::gc_heap::allocate_more_space+0x18
  0012f2f4 7a02a51b mscorwks!WKS::GCHeap::Alloc+0x4b
  0012f310 7a02ae4c mscorwks!Alloc+0x60
  0012f364 7a030e46 mscorwks!FastAllocatePrimitiveArray+0xbd
  0012f424 300027f4 mscorwks!JIT_NewArr1+0x148
  000af70f 3000299f fragment_ni!request..ctor(Int32, Single)+0x20c
  0000002a 79fa22bd fragment_ni!request.Main(System.String[])+0x153
  ```

  Um auxiliar just-in-time (`JIT_New*`) eventualmente chama `GCHeap::GarbageCollectGeneration`. Se você determinar que as coletas de lixo de geração 2 são causadas por alocações, você deverá determinar quais objetos são coletados por uma coleta de lixo de geração 2 e como evitá-los. Ou seja, você deseja determinar a diferença entre o início e o término de uma coleta de lixo de geração 2 e os objetos que causaram a coleta de geração 2.

  Por exemplo, digite o seguinte comando no depurador para mostrar o início de uma coleta de geração 2:

  **!dumpheap –stat**

  Saída de exemplo (abreviada para mostrar os objetos que usam mais espaço):

  ```console
  79124228    31857      9862328 System.Object[]
  035f0384    25668     11601936 Toolkit.TlkPosition
  00155f80    21248     12256296      Free
  79103b6c   297003     13068132 System.Threading.ReaderWriterLock
  7a747ad4   708732     14174640 System.Collections.Specialized.HybridDictionary
  7a747c78   786498     15729960 System.Collections.Specialized.ListDictionary+DictionaryNode
  7a747bac   700298     19608344 System.Collections.Specialized.ListDictionary
  035f0ee4    89192     38887712 Toolkit.TlkOrder
  00fcae40     6193     44911636 WaveBasedStrategy.Tick_Snap[]
  7912c444    91616     71887080 System.Double[]
  791242ec    32451     82462728 System.Collections.Hashtable+bucket[]
  790fa3e0  2459154    112128436 System.String
  Total 6471774 objects
  ```

  Repita o comando ao final da geração 2:

  **!dumpheap –stat**

  Saída de exemplo (abreviada para mostrar os objetos que usam mais espaço):

  ```console
  79124228    26648      9314256 System.Object[]
  035f0384    25668     11601936 Toolkit.TlkPosition
  79103b6c   296770     13057880 System.Threading.ReaderWriterLock
  7a747ad4   708730     14174600 System.Collections.Specialized.HybridDictionary
  7a747c78   786497     15729940 System.Collections.Specialized.ListDictionary+DictionaryNode
  7a747bac   700298     19608344 System.Collections.Specialized.ListDictionary
  00155f80    13806     34007212      Free
  035f0ee4    89187     38885532 Toolkit.TlkOrder
  00fcae40     6193     44911636 WaveBasedStrategy.Tick_Snap[]
  791242ec    32370     82359768 System.Collections.Hashtable+bucket[]
  790fa3e0  2440020    111341808 System.String
  Total 6417525 objects
  ```

  Os objetos `double[]` desapareceram do final da saída, o que significa que eles foram coletados. Esses objetos correspondem a aproximadamente 70 MB. Os objetos restantes não mudaram muito. Sendo assim, esses objetos `double[]` foram o motivo pelo qual essa coleta de lixo de geração 2 ocorreu. A próxima etapa é determinar por que os objetos `double[]` estão lá e por que eles morreram. Você pode perguntar ao desenvolvedor de código a origem desses objetos ou então você pode usar o comando **gcroot**.

<a name="HighCPU"></a>

### <a name="to-determine-whether-high-cpu-usage-is-caused-by-garbage-collection"></a>Para determinar se o alto uso da CPU é causado pela coleta de lixo

- Correlacionar o valor do contador de desempenho de memória de `% Time in GC` com o tempo de processamento.

  Se o valor de `% Time in GC` subir ao mesmo tempo que o tempo de processamento, isso significará que a coleta de lixo está causando um alto uso da CPU. Caso contrário, crie o perfil do aplicativo para encontrar o local de ocorrência do alto uso.

## <a name="see-also"></a>Confira também

- [Coleta de lixo](index.md)
