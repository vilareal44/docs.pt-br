---
title: LOH (heap de objeto grande) no Windows
description: Este artigo discute objetos grandes, como eles são gerenciados pelo coletor de lixo do .NET e as implicações de desempenho do uso de objetos grandes.
ms.date: 05/02/2018
helpviewer_keywords:
- large object heap (LOH)"
- LOH
- garbage collection, large object heap
- GC [.NET ], large object heap
ms.openlocfilehash: 87105acbd43eb8eda0daa00c65ca0635f5e1cc74
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286022"
---
# <a name="the-large-object-heap-on-windows-systems"></a>Heap de objeto grande em sistemas Windows

O GC (coletor de lixo do .NET) divide objetos em objetos pequenos e grandes. Quando um objeto é grande, alguns de seus atributos se tornam mais significativos do que se o objeto for pequeno. Por exemplo, compactá-lo &mdash; , copiá-lo na memória em outro lugar no heap &mdash; pode ser caro. Por isso, o coletor de lixo coloca objetos grandes na LOH (Large Object heap). Este artigo discute o que qualifica um objeto como um objeto grande, como os objetos grandes são coletados e que tipo de implicações de desempenho os objetos grandes impõem.

> [!IMPORTANT]
> Este artigo aborda o heap de objeto grande no .NET Framework e no .NET Core em execução somente em sistemas Windows. Não aborda o LOH em execução em implementações do .NET em outras plataformas.

## <a name="how-an-object-ends-up-on-the-loh"></a>Como um objeto termina no LOH

Se um objeto for maior ou igual a 85.000 bytes de tamanho, ele será considerado um objeto grande. Esse número foi determinado por ajuste de desempenho. Quando uma solicitação de alocação de objeto for de 85.000 ou mais bytes, o runtime a alocará no heap de objeto grande.

Para entender o que isso significa, é útil examinar alguns conceitos básicos sobre o coletor de lixo.

O coletor de lixo é um coletor de geração. Ele tem três gerações: geração 0, geração 1 e geração 2. O motivo para ter 3 gerações é que, em um aplicativo bem ajustado, a maioria dos objetos morre na gen0. Por exemplo, em um aplicativo para servidor, as alocações associadas a cada solicitação devem morrer após a conclusão da solicitação. As solicitações de alocação em andamento chegarão à gen1 e morrerão lá. Essencialmente, a gen1 atua como um buffer entre áreas de objetos jovens e áreas de objetos de vida longa.

Objetos pequenos sempre são alocados na geração 0 e, dependendo de seu tempo de vida, podem ser promovidos à geração 1 ou geração 2. Objetos grandes sempre são alocados na geração 2.

Os objetos grandes pertencem à geração 2 porque são coletados apenas durante uma coleta de geração 2. Quando uma geração é coletada, todas as suas gerações mais jovens também são coletadas. Por exemplo, quando ocorre uma GC de geração 1, as gerações 1 e 0 são coletadas. E quando ocorre uma GC de geração 2, todo o heap é coletado. Por esse motivo, um GC de geração 2 também é chamado de *GC completo*. Este artigo se refere ao GC de geração 2, em vez de ao GC completo, mas os termos são intercambiáveis.

Gerações fornecem uma exibição lógica do heap de GC. Fisicamente, os objetos residem em segmentos de heaps gerenciados. Um *segmento de heap gerenciado* é um bloco de memória que o GC reserva do sistema operacional chamando a [função VirtualAlloc](/windows/desktop/api/memoryapi/nf-memoryapi-virtualalloc) em nome do código gerenciado. Quando o CLR é carregado, o GC aloca dois segmentos de heap iniciais: um para objetos pequenos (a heap de objeto pequeno ou SOH) e outro para objetos grandes (a heap de objeto grande).

As solicitações de alocação são então atendidas colocando os objetos gerenciados em um desses segmentos de heap gerenciado. Se o objeto for menor que 85.000 bytes, ele será colocado no segmento de SOH; caso contrário, ele será colocado em um segmento de LOH. Os segmentos são confirmados (em blocos menores) à medida que mais objetos são alocados a eles.
Para o SOH, os objetos que sobrevivem a um GC são promovidos para a próxima geração. Os objetos que sobrevivem a uma coleta de geração 0 são considerados objetos de geração 1 e assim por diante. No entanto, os objetos que sobrevivem à geração mais antiga ainda serão considerados como estando na geração mais antiga. Em outras palavras, os sobreviventes da geração 2 são objetos de geração 2; e os sobreviventes de LOH são objetos LOH (que são coletados com a gen2).

O código do usuário pode alocar apenas na geração 0 (objetos pequenos) ou no LOH (objetos grandes). Apenas o GC pode "alocar" objetos na geração 1 (promovendo os sobreviventes da geração 0) e na geração 2 (promovendo os sobreviventes das gerações 1 e 2).

Quando uma coleta de lixo é disparada, o GC rastreia pelos objetos vivos e os compacta. Mas como a compactação é cara, o GC *varre* o LOH; faz uma lista livre de objetos mortos que podem ser reutilizados mais tarde para atender a solicitações de alocação de objeto grande. Objetos mortos adjacentes são transformados em um objeto livre.

O .NET Core e o .NET Framework (do .NET Framework 4.5.1 em diante) incluem a propriedade <xref:System.Runtime.GCSettings.LargeObjectHeapCompactionMode?displayProperty=nameWithType>, que permite aos usuários especificar que o LOH deve ser compactado durante o próximo GC de bloqueio completo. No futuro, o .NET pode optar por compactar o LOH automaticamente. Isso significa que, se você aloca objetos grandes e deseja ter certeza de que eles não se moverão, você ainda deve fixá-los.

A Figura 1 ilustra um cenário em que o GC forma a geração 1 após o primeiro GC de geração 0, em que `Obj1` e `Obj3` estão mortos, e forma a geração 2 após o primeiro GC de geração 1, em que `Obj2` e `Obj5` estão mortos. Observe que isso e as figuras a seguir são apenas para fins de ilustração; elas contêm muito poucos objetos para mostrar melhor o que acontece no heap. Na verdade, muitos mais objetos normalmente estão envolvidos em um GC.

![Figura 1: um GC de geração 0 e um GC de geração 1](media/loh/loh-figure-1.jpg)\
Figura 1: um GC de geração 0 e um GC de geração 1.

A Figura 2 mostra que depois de um GC de geração 2 que observou que `Obj1` e `Obj2` estão mortos, o GC forma um espaço livre contíguo fora da memória que costumava ser ocupado por `Obj1` e `Obj2`, que foi então usado para atender a uma solicitação de alocação para `Obj4`. O espaço após o último objeto, `Obj3`, ao final do segmento também pode ser usado para atender a solicitações de alocação.

![Figura 2: após uma GC de geração 2](media/loh/loh-figure-2.jpg)\
Figura 2: após um GC de geração 2

Se não houver espaço livre suficiente para acomodar as solicitações de alocação de objeto grande, o GC primeiro tentará adquirir mais segmentos do sistema operacional. Se isso falhar, ele disparará um GC de geração 2 na esperança de liberar algum espaço.

Durante um GC de geração 1 ou 2, o coletor de lixo libera segmentos que não contêm objetos vivos novamente para o sistema operacional chamando a [função VirtualFree](/windows/desktop/api/memoryapi/nf-memoryapi-virtualfree). O espaço após o último objeto vivo até o final do segmento tem a confirmação anulada (exceto no segmento efêmero em que gen0/gen1 vivem, em que o coletor de lixo mantém alguns confirmados, porque o aplicativo alocará nele imediatamente). E os espaços livres permanecem confirmados, embora estejam redefinidos, o que significa que o sistema operacional não precisa gravar dados neles novamente em disco.

Como o LOH é coletado apenas durante os GCs de geração 2, o segmento de LOH só pode ser liberado durante um desses GCs. A Figura 3 ilustra um cenário em que o coletor de lixo libera um segmento (segmento 2) novamente para o sistema operacional e anula a confirmação de mais espaço nos segmentos restantes. Se ele precisar usar o espaço com anulação de confirmação no final do segmento para atender a solicitações de alocação de objeto grande, ele confirmará a memória novamente. (Para obter uma explicação da confirmação/anulação de confirmação, confira a documentação do [VirtualAlloc](/windows/desktop/api/memoryapi/nf-memoryapi-virtualalloc).)

![Figura 3: LOH após uma GC de geração 2](media/loh/loh-figure-3.jpg)\
Figura 3: o LOH após um GC de geração 2

## <a name="when-is-a-large-object-collected"></a>Quando um objeto grande é coletado?

Em geral, um GC ocorre sob uma das três condições a seguir:

- A alocação excede o limite de objeto grande ou de geração 0.

  O limite é uma propriedade de uma geração. Um limite para uma geração é definido quando o coletor de lixo aloca objetos nele. Quando o limite é excedido, um GC é disparado nessa geração. Quando você aloca objetos pequenos ou grandes, consome os limites da geração 0 e do LOH, respectivamente. Quando o coletor de lixo aloca na geração 1 e 2, ele consome seus limites. Esses limites são dinamicamente ajustados à medida que o programa é executado.

  Esse é o caso típico; a maioria dos GCs ocorre por causa de alocações no heap gerenciado.

- O método <xref:System.GC.Collect%2A?displayProperty=nameWithType> é chamado.

  Se o método <xref:System.GC.Collect?displayProperty=nameWithType> sem parâmetros é chamado ou outra sobrecarga recebe <xref:System.GC.MaxGeneration?displayProperty=nameWithType> como argumento, o LOH é coletado juntamente com o restante do heap gerenciado.

- O sistema está em situação de pouca memória.

  Isso ocorre quando o coletor de lixo recebe uma notificação de memória alta do sistema operacional. Se o coletor de lixo considera que fazer um GC de geração 2 será produtivo, ele disparará um.

## <a name="loh-performance-implications"></a>Implicações de desempenho de LOH

As alocações no heap de objeto grande afetam o desempenho das maneiras mostradas a seguir.

- Custo de alocação.

  O CLR garante que a memória de cada novo objeto fornecido é apagada. Isso significa que o custo de alocação de um objeto grande é completamente dominado pela limpeza da memória (a menos que ela dispare um GC). Se levar dois ciclos para apagar um byte, serão necessários 170.000 ciclos para apagar o menor objeto grande. A limpeza da memória de um objeto de 16 MB em um computador de 2 GHz leva aproximadamente 16 ms. É um custo muito alto.

- Custo da coleção.

  Como o LOH e a geração 2 são coletados juntos, se o limites de um dos dois for excedido, uma coleta de geração 2 será disparada. Se uma coleta de geração 2 for disparada devido ao LOH, a geração 2 não necessariamente será muito menor após o GC. Se não houver muitos dados na geração 2, isso terá um impacto mínimo. Mas se a geração 2 for grande, ela poderá causar problemas de desempenho se muitos GCs de geração 2 forem disparados. Se vários objetos grandes forem alocados de forma muito temporária e você tiver um SOH grande, você poderá gastar muito tempo realizando GCs. Além disso, o custo de alocação poderá realmente aumentar se você continuar alocando e liberando objetos muito grandes.

- Elementos de matriz com tipos de referência.

  Objetos muito grandes no LOH são normalmente matrizes (é muito raro ter um objeto de instância que seja realmente muito grande). Se os elementos de uma matriz forem ricos em referência, ela resultará em um custo que não está presente se os elementos não forem ricos em referência. Se o elemento não contiver nenhuma referência, o coletor de lixo não precisará percorrer a matriz. Por exemplo, se você usa uma matriz para armazenar nós em uma árvore binária, uma maneira de implementar é se referir ao nó direito e esquerdo de um nó pelos nós reais:

  ```csharp
  class Node
  {
     Data d;
     Node left;
     Node right;
  };

  Node[] binary_tr = new Node [num_nodes];
  ```

  Se `num_nodes` for grande, o coletor de lixo precisará percorrer, pelo menos, duas referências por elemento. Uma abordagem alternativa é armazenar o índice dos nós direito e esquerdo:

  ```csharp
  class Node
  {
     Data d;
     uint left_index;
     uint right_index;
  } ;
  ```

  Em vez de referenciar os dados do nó esquerdo como `left.d`, você se referirá a eles como `binary_tr[left_index].d`. Além disso, o coletor de lixo não precisa examinar nenhuma referência dos nós esquerdo e direito.

Entre os três fatores, os dois primeiros são geralmente mais significativos do que o terceiro. Por isso, recomendamos que você aloque um pool de objetos grandes que você reutilizará em vez de alocar temporários.

## <a name="collect-performance-data-for-the-loh"></a>Coletar dados de desempenho para o LOH

Antes de coletar dados de desempenho para uma área específica, você deverá já ter feito o seguinte:

1. Encontrar evidência de que você deve observar essa área.

2. Ter esgotado outras áreas conhecidas sem encontrar algo que poderia explicar o problema de desempenho observado.

Visite o blog [Entender o problema antes de tentar encontrar uma solução](https://devblogs.microsoft.com/dotnet/understand-the-problem-before-you-try-to-find-a-solution/) para obter mais informações sobre os conceitos básicos sobre memória e CPU.

Use as seguintes ferramentas para coletar dados sobre o desempenho de LOH:

- [Contadores de desempenho da memória do CLR do .NET](#net-clr-memory-performance-counters)

- [eventos ETW](#etw-events)

- [Um depurador](#a-debugger)

### <a name="net-clr-memory-performance-counters"></a>Contadores de Desempenho da Memória do CLR do .NET

Esses contadores de desempenho geralmente são uma boa primeira etapa na investigação de problemas de desempenho (embora recomendamos que você use [eventos ETW](#etw-events)). Configure o Monitor de Desempenho adicionando os contadores desejados, como mostra a Figura 4. Aqueles que são relevantes para o LOH são:

- **Coletas de Geração 2**

   Exibe o número de vezes que os GCs de geração 2 ocorreram desde o início do processo. O contador é incrementado no final de uma coleta de geração 2 (também conhecida como uma coleta de lixo completa). Esse contador exibe o último valor observado.

- **Tamanho de Heap de objeto grande**

   Exibe o tamanho atual, em bytes, incluindo o espaço livre, de LOH. Esse contador é atualizado no final de uma coleta de lixo e não em cada alocação.

Uma maneira comum de examinar contadores de desempenho é com o Monitor de Desempenho (perfmon.exe). Use "Adicionar Contadores" para adicionar o contador interessante a processos de seu interesse. Você pode salvar os dados do contador de desempenho em um arquivo de log como mostra a Figura 4:

![Captura de tela que mostra a adição de contadores de desempenho.](media/large-object-heap/add-performance-counter.png)
Figura 4: o LOH após um GC de geração 2

Os contadores de desempenho também podem ser consultados de forma programática. Várias pessoas os coletam dessa maneira como parte de seu processo de teste de rotina. Quando elas identificam contadores com valores fora do comum, elas usam outro meio de obter dados mais detalhados para ajudar na investigação.

> [!NOTE]
> Recomendamos o uso de eventos ETW em vez de contadores de desempenho, pois o ETW fornece informações muito mais sofisticadas.

### <a name="etw-events"></a>eventos ETW

O coletor de lixo fornece um conjunto rico de eventos ETW para ajudá-lo a entender o que o heap está fazendo e por quê. As seguintes postagens no blog mostram como coletar e entender eventos GC com o ETW:

- [Eventos de ETW do GC-1](https://devblogs.microsoft.com/dotnet/gc-etw-events-1/)

- [Eventos ETW de GC – 2](https://devblogs.microsoft.com/dotnet/gc-etw-events-2/)

- [Eventos ETW de GC – 3](https://devblogs.microsoft.com/dotnet/gc-etw-events-3/)

- [Eventos ETW de GC – 4](https://devblogs.microsoft.com/dotnet/gc-etw-events-4/)

Para identificar GCs excessivos de geração 2 causados por alocações de LOH temporárias, veja a coluna Motivo de Gatilho dos GCs. Para um teste simples que aloca apenas objetos grandes temporários, você pode coletar informações sobre eventos ETW com a seguinte linha de comando do [PerfView](https://www.microsoft.com/download/details.aspx?id=28567):

```console
perfview /GCCollectOnly /AcceptEULA /nogui collect
```

O resultado é semelhante a este:

![Captura de tela que mostra os eventos ETW no PerfView.](media/large-object-heap/event-tracing-windows-perfview.png)
Figura 5: Eventos ETW mostrados usando PerfView

Como você pode ver, todos os GCs são GCs de geração 2 e são disparados por AllocLarge, o que significa que a alocação de um objeto grande disparou esse GC. Sabemos que essas alocações são temporárias porque a coluna **% da Taxa de Sobrevivência de LOH** indica 1%.

Você pode coletar eventos ETW adicionais que indicam quem alocou esses objetos grandes. A seguinte linha de comando:

```console
perfview /GCOnly /AcceptEULA /nogui collect
```

coleta um evento AllocationTick disparado aproximadamente a cada 100 mil alocações. Em outras palavras, um evento é disparado sempre que um objeto grande é alocado. Em seguida, você pode observar uma das exibições de Alocação de Heap de GC, que mostra as pilhas de chamadas que alocaram objetos grandes:

![Captura de tela que mostra a exibição de heap de um coletor de lixo.](media/large-object-heap/garbage-collector-heap.png)
Figura 6: uma exibição de Alocação de Heap de GC

Como você pode ver, esse é um teste muito simples que aloca apenas objetos grandes de seu método `Main`.

### <a name="a-debugger"></a>Um depurador

Se tudo o que você tem é um despejo de memória e você precisa examinar quais objetos estão realmente no LOH, use a [extensão de depurador SoS](../../framework/tools/sos-dll-sos-debugging-extension.md) fornecida pelo .NET.

> [!NOTE]
> Os comandos de depuração mencionados nesta seção são aplicáveis aos [Depuradores do Windows](https://www.microsoft.com/whdc/devtools/debugging/default.mspx).

O seguinte exemplo mostra uma saída de exemplo da análise de LOH:

```console
0:003> .loadby sos mscorwks
0:003> !eeheap -gc
Number of GC Heaps: 1
generation 0 starts at 0x013e35ec
sdgeneration 1 starts at 0x013e1b6c
generation 2 starts at 0x013e1000
ephemeral segment allocation context: none
segment   begin allocated     size
0018f2d0 790d5588 790f4b38 0x0001f5b0(128432)
013e0000 013e1000 013e35f8 0x000025f8(9720)
Large object heap starts at 0x023e1000
segment   begin allocated     size
023e0000 023e1000 033db630 0x00ffa630(16754224)
033e0000 033e1000 043cdf98 0x00fecf98(16699288)
043e0000 043e1000 05368b58 0x00f87b58(16284504)
Total Size 0x2f90cc8(49876168)
------------------------------
GC Heap Size 0x2f90cc8(49876168)
0:003> !dumpheap -stat 023e1000 033db630
total 133 objects
Statistics:
MT   Count   TotalSize Class Name
001521d0       66     2081792     Free
7912273c       63     6663696 System.Byte[]
7912254c       4     8008736 System.Object[]
Total 133 objects
```

O tamanho do heap de LOH é (16.754.224 + 16.699.288 + 16.284.504) = 49.738.016 bytes. Entre os endereços 023e1000 e 033db630, 8.008.736 bytes são ocupados por uma matriz de <xref:System.Object?displayProperty=nameWithType> objetos, 6.663.696 bytes são ocupados por uma matriz de <xref:System.Byte?displayProperty=nameWithType> objetos e 2.081.792 bytes são ocupados por espaço livre.

Às vezes, o depurador mostra que o tamanho total do LOH é menor que 85.000 bytes. Isso ocorre porque o próprio runtime usa o LOH para alocar alguns objetos menores que um objeto grande.

Como o LOH não é compactado, às vezes ele é considerado uma fonte de fragmentação. Fragmentação significa:

- Fragmentação do heap gerenciado, que é indicada pela quantidade de espaço livre entre objetos gerenciados. No SoS, o comando `!dumpheap –type Free` exibe a quantidade de espaço livre entre objetos gerenciados.

- Fragmentação do espaço de endereço da VM (memória virtual), que é a memória marcada como `MEM_FREE`. Você pode obtê-lo usando vários comandos de depurador no windbg.

   O seguinte exemplo mostra a fragmentação no espaço de VM:

   ```console
   0:000> !address
   00000000 : 00000000 - 00010000
   Type     00000000
   Protect 00000001 PAGE_NOACCESS
   State   00010000 MEM_FREE
   Usage   RegionUsageFree
   00010000 : 00010000 - 00002000
   Type     00020000 MEM_PRIVATE
   Protect 00000004 PAGE_READWRITE
   State   00001000 MEM_COMMIT
   Usage   RegionUsageEnvironmentBlock
   00012000 : 00012000 - 0000e000
   Type     00000000
   Protect 00000001 PAGE_NOACCESS
   State   00010000 MEM_FREE
   Usage   RegionUsageFree
   … [omitted]
   -------------------- Usage SUMMARY --------------------------
   TotSize (     KB)   Pct(Tots) Pct(Busy)   Usage
   701000 (   7172) : 00.34%   20.69%   : RegionUsageIsVAD
   7de15000 ( 2062420) : 98.35%   00.00%   : RegionUsageFree
   1452000 (   20808) : 00.99%   60.02%   : RegionUsageImage
   300000 (   3072) : 00.15%   08.86%   : RegionUsageStack
   3000 (     12) : 00.00%   00.03%   : RegionUsageTeb
   381000 (   3588) : 00.17%   10.35%   : RegionUsageHeap
   0 (       0) : 00.00%   00.00%   : RegionUsagePageHeap
   1000 (       4) : 00.00%   00.01%   : RegionUsagePeb
   1000 (       4) : 00.00%   00.01%   : RegionUsageProcessParametrs
   2000 (       8) : 00.00%   00.02%   : RegionUsageEnvironmentBlock
   Tot: 7fff0000 (2097088 KB) Busy: 021db000 (34668 KB)

   -------------------- Type SUMMARY --------------------------
   TotSize (     KB)   Pct(Tots) Usage
   7de15000 ( 2062420) : 98.35%   : <free>
   1452000 (   20808) : 00.99%   : MEM_IMAGE
   69f000 (   6780) : 00.32%   : MEM_MAPPED
   6ea000 (   7080) : 00.34%   : MEM_PRIVATE

   -------------------- State SUMMARY --------------------------
   TotSize (     KB)   Pct(Tots) Usage
   1a58000 (   26976) : 01.29%   : MEM_COMMIT
   7de15000 ( 2062420) : 98.35%   : MEM_FREE
   783000 (   7692) : 00.37%   : MEM_RESERVE

   Largest free region: Base 01432000 - Size 707ee000 (1843128 KB)
   ```

É mais comum ver a fragmentação da VM causada por objetos grandes temporários que exigem que o coletor de lixo adquira frequentemente novos segmentos de heap gerenciado do sistema operacional e libere os segmentos vazios novamente para o sistema operacional.

Para verificar se o LOH está causando a fragmentação da VM, defina um ponto de interrupção em [VirtualAlloc](/windows/desktop/api/memoryapi/nf-memoryapi-virtualalloc) e [VirtualFree](/windows/desktop/api/memoryapi/nf-memoryapi-virtualfree) para ver quem os chama. Por exemplo, para ver quem tentou alocar blocos de memória virtual maiores que 8 MB do sistema operacional, defina um ponto de interrupção como este:

```console
bp kernel32!virtualalloc "j (dwo(@esp+8)>800000) 'kb';'g'"
```

Esse comando interromperá o depurador e mostrará a pilha de chamadas somente se o [VirtualAlloc](/windows/desktop/api/memoryapi/nf-memoryapi-virtualalloc) for chamado com um tamanho de alocação maior que 8MB (0x800000).

O CLR 2.0 adicionou um recurso chamado *VM Hoarding*, que pode ser útil para cenários em que os segmentos (incluindo heaps de objeto grande e pequeno) são frequentemente adquiridos e liberados. Para especificar o VM Hoarding, você especifica um sinalizador de inicialização chamado `STARTUP_HOARD_GC_VM` pela API de hospedagem. Em vez de liberar segmentos vazios novamente para o sistema operacional, o CLR anula a confirmação da memória nesses segmentos e os coloca em uma lista de espera. (Observe que o CLR não faz isso para segmentos muito grandes.) O CLR usa posteriormente esses segmentos para atender a novas solicitações de segmento. Na próxima vez que seu aplicativo precisar de um novo segmento, o CLR usará um dessa lista de espera, caso consiga encontrar um que seja grande o suficiente.

A VM Hoarding também é útil para aplicativos que desejam manter os segmentos que já adquiriram, como alguns aplicativos de servidor que são os aplicativos mais dominantes em execução no sistema, para evitar exceções de memória insuficiente.

É altamente recomendável que você teste cuidadosamente seu aplicativo quando usar esse recurso, para garantir que o aplicativo tem um uso de memória razoavelmente estável.
