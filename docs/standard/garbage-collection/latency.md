---
title: Modos de latência
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- garbage collection, intrusiveness
- garbage collection, latency modes
ms.assetid: 96278bb7-6eab-4612-8594-ceebfc887d81
ms.openlocfilehash: ee45fe5e8016c7507bc3a873e615fd8379810a8e
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286009"
---
# <a name="latency-modes"></a>Modos de latência

Para recuperar objetos, o GC (coletor de lixo) deve parar todos os threads em execução em um aplicativo. O período de tempo durante o qual o coletor de lixo está ativo é conhecido como sua *latência*.

Em algumas situações, como quando um aplicativo recupera dados ou exibe conteúdo, uma coleta de lixo completa pode ocorrer em um momento crítico e impedir o desempenho. Você pode ajustar a intrusão do coletor de lixo, definindo a propriedade <xref:System.Runtime.GCSettings.LatencyMode%2A?displayProperty=nameWithType> como um dos valores <xref:System.Runtime.GCLatencyMode?displayProperty=nameWithType>.

## <a name="low-latency-settings"></a>Configurações de baixa latência

Usar uma configuração de latência "baixa" significa que o coletor de lixo não faz menos em seu aplicativo. A coleta de lixo é mais conservadora sobre a recuperação de memória.

A enumeração <xref:System.Runtime.GCLatencyMode?displayProperty=nameWithType> fornece duas configurações de baixa latência:

- [GCLatencyMode. LowLatency](xref:System.Runtime.GCLatencyMode.LowLatency) suprime as coleções de geração 2 e executa somente coleções de geração 0 e 1. Ela pode ser usada somente por curtos períodos. Em períodos mais longos, se o sistema estiver sob pressão de memória, o coletor de lixo disparará uma coleção, que pode pausar o aplicativo rapidamente e interromper uma operação de tempo crítico. Essa configuração está disponível somente para coleta de lixo de estação de trabalho.

- [GCLatencyMode. SustainedLowLatency](xref:System.Runtime.GCLatencyMode.SustainedLowLatency) suprime as coleções da geração 2 de primeiro plano e executa somente coleções de geração 0, 1 e segundo plano 2. Ela pode ser usada por longos períodos e está disponível para coleta de lixo de estação de trabalho e servidor. Essa configuração não poderá ser usada se a coleta de lixo em segundo plano estiver desabilitada.

Durante períodos de baixa latência, coletas da geração 2 são suprimidas, a menos que ocorra o seguinte:

- O sistema recebe uma notificação de falta de memória do sistema operacional.

- O código do aplicativo induzi uma coleção chamando o <xref:System.GC.Collect%2A?displayProperty=nameWithType> método e especificando 2 para o `generation` parâmetro.

## <a name="scenarios"></a>Cenários

A tabela a seguir lista os cenários de aplicativo para usar os <xref:System.Runtime.GCLatencyMode> valores:

|Modo de latência|Cenários de aplicativos|
|------------------|---------------------------|
|<xref:System.Runtime.GCLatencyMode.Batch>|Para aplicativos que não têm nenhuma interface do usuário ou operações do lado do servidor.<br /><br />Quando a coleta de lixo em segundo plano está desabilitada, esse é o modo padrão para a coleta de lixo da estação de trabalho e do servidor. <xref:System.Runtime.GCLatencyMode.Batch>também substitui a configuração [gcConcurrent](../../framework/configure-apps/file-schema/runtime/gcconcurrent-element.md) , ou seja, impede coleções em segundo plano ou simultâneas.|
|<xref:System.Runtime.GCLatencyMode.Interactive>|Para a maioria dos aplicativos que têm uma interface do usuário.<br /><br />Este é o modo padrão para a coleta de lixo da estação de trabalho e do servidor. No entanto, se um aplicativo estiver hospedado, as configurações do coletor de lixo do processo de hospedagem têm precedência.|
|<xref:System.Runtime.GCLatencyMode.LowLatency>|Para aplicativos que têm operações de curto prazo, sensíveis ao tempo, durante o qual as interrupções do coletor de lixo podem ser prejudiciais. Por exemplo, aplicativos que renderizam animações ou funções de aquisição de dados.|
|<xref:System.Runtime.GCLatencyMode.SustainedLowLatency>|Para aplicativos que têm operações sensíveis ao tempo, por uma duração contida, mas potencialmente maior, durante a qual as interrupções do coletor de lixo poderiam ser prejudiciais. Por exemplo, aplicativos que precisam de tempos de resposta rápidos, como alterações de dados de mercado durante o horário comercial.<br /><br />Esse modo resulta em um tamanho maior do heap gerenciado do que outros modos. Como ele não compacta o heap gerenciado, é possível maior fragmentação. Verifique se há memória suficiente disponível.|

## <a name="guidelines-for-using-low-latency"></a>Diretrizes para usar baixa latência

Ao usar o modo [GCLatencyMode. LowLatency](xref:System.Runtime.GCLatencyMode.LowLatency) , considere as seguintes diretrizes:

- Mantenha o período de baixa latência o mais curto possível.

- Evite a alocação de grande quantidade de memória durante períodos de baixa latência. Notificações de memória insuficiente podem ocorrer porque a coleta de lixo recupera menos objetos.

- No modo de baixa latência, minimize o número de novas alocações, em alocações específicas na heap de objeto grande e objetos fixados.

- Esteja atento a ameaças que possam ser alocadas. Como a <xref:System.Runtime.GCSettings.LatencyMode%2A> configuração de propriedade é de todo o processo, as <xref:System.OutOfMemoryException> exceções podem ser geradas em qualquer thread que esteja alocando.

- Empacote o código de baixa latência em regiões de execução restrita. Para obter mais informações, consulte [regiões de execução restrita](../../framework/performance/constrained-execution-regions.md).

- Você pode forçar coletas da geração 2 durante um período de baixa latência ao chamar o método <xref:System.GC.Collect%28System.Int32%2CSystem.GCCollectionMode%29?displayProperty=nameWithType>.

## <a name="see-also"></a>Veja também

- <xref:System.GC?displayProperty=nameWithType>
- [Coleções induzidas](induced.md)
- [Coleta de lixo](index.md)
