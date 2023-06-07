---
title: Coletas induzidas
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- garbage collection, forced
ms.assetid: 019008fe-4708-4e65-bebf-04fd9941e149
ms.openlocfilehash: 36dff45587c6c28ba17fd7389dc3863893ff8f61
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286035"
---
# <a name="induced-collections"></a>Coletas induzidas
Na maioria dos casos, o coletor de lixo pode determinar o melhor momento para executar uma coleta e você deve permitir que ele seja executado de modo independente. Existem situações raras nas quais uma coleta forçada pode melhorar o desempenho do seu aplicativo. Nesses casos, você poderá induzir a coleta de lixo usando o método <xref:System.GC.Collect%2A?displayProperty=nameWithType> para forçar uma coleta de lixo.  
  
 Use o método <xref:System.GC.Collect%2A?displayProperty=nameWithType> quando houver uma redução significativa na quantidade de memória que está sendo usada em um ponto específico no código do aplicativo. Por exemplo, se o aplicativo usar uma caixa de diálogo complexo que tem vários controles, chamar <xref:System.GC.Collect%2A> quando a caixa de diálogo é fechada poderá melhorar o desempenho, recuperando imediatamente a memória usada pela caixa de diálogo. Certifique-se de que seu aplicativo não está induzindo a coleta de lixo com muita frequência, pois isso poderá diminuir o desempenho se o coletor de lixo estiver tentando recuperar os objetos em horários não ideais. Você pode fornecer um valor de enumeração <xref:System.GCCollectionMode.Optimized?displayProperty=nameWithType> para que o método <xref:System.GC.Collect%2A> faça a coleta somente quando ela for produtiva, conforme discutido na próxima seção.  
  
## <a name="gc-collection-mode"></a>Modo de coleta de GC  
 Você pode usar uma das sobrecargas do método <xref:System.GC.Collect%2A?displayProperty=nameWithType>, que inclui um valor <xref:System.GCCollectionMode>, para especificar o comportamento de uma coleta forçada do modo a seguir.  
  
|`GCCollectionMode` valor|Description|  
|------------------------------|-----------------|  
|<xref:System.GCCollectionMode.Default>|Usa a configuração de coleta de lixo padrão para a versão do .NET Framework em execução.|  
|<xref:System.GCCollectionMode.Forced>|Força a coleta de lixo a ocorrer imediatamente. Isso é equivalente a chamar a sobrecarga <xref:System.GC.Collect?displayProperty=nameWithType>. Isso resulta em uma coleta de bloqueio total de todas as gerações.<br /><br /> Você também pode compactar o heap de objeto grande definindo a propriedade <xref:System.Runtime.GCSettings.LargeObjectHeapCompactionMode%2A?displayProperty=nameWithType> como <xref:System.Runtime.GCLargeObjectHeapCompactionMode.CompactOnce?displayProperty=nameWithType> antes de forçar uma coleta de lixo de bloqueio completo imediata.|  
|<xref:System.GCCollectionMode.Optimized>|Permite que o coletor de lixo determine se o horário atual é ideal para recuperar objetos.<br /><br /> O coletor de lixo pode determinar que uma coleta não seria suficientemente produtiva para se justificar, caso em que ele retornará sem recuperar objetos.|  
  
## <a name="background-or-blocking-collections"></a>Coletas de bloqueio ou em segundo plano  
 Você pode chamar a sobrecarga de método <xref:System.GC.Collect%28System.Int32%2CSystem.GCCollectionMode%2CSystem.Boolean%29?displayProperty=nameWithType> para especificar se uma coleta induzida realiza o bloqueio ou não. O tipo de coleta executada depende de uma combinação dos parâmetros `mode` e `blocking` do método. `mode` é membro da enumeração <xref:System.GCCollectionMode>, e `blocking` é um valor <xref:System.Boolean>. A tabela a seguir resume a interação entre os argumentos `mode` e `blocking`.  
  
|`mode`|`blocking` = `true`|`blocking` = `false`|  
|------------|--------------------------|---------------------------|  
|<xref:System.GCCollectionMode.Forced> ou <xref:System.GCCollectionMode.Default>|Uma coleção de bloqueio é executada assim que possível. Se uma coleta em segundo plano estiver em andamento e a geração for 0 ou 1, o método <xref:System.GC.Collect%28System.Int32%2CSystem.GCCollectionMode%2CSystem.Boolean%29> disparará imediatamente uma coleta de bloqueio e retornará quando a coleção for concluída. Se uma coleta em segundo plano estiver em andamento e o parâmetro `generation` for 2, o método aguardará até que a coleta em segundo plano seja concluída, disparará uma coleta de bloqueio de geração 2 e retornará.|Uma coleta é executada assim que possível. O método <xref:System.GC.Collect%28System.Int32%2CSystem.GCCollectionMode%2CSystem.Boolean%29> solicita uma coleta em segundo plano, mas isso não é garantido; dependendo das circunstâncias, uma coleta de bloqueio ainda pode ser executada. Se uma coleta em segundo plano já estiver em andamento, o método retornará imediatamente.|  
|<xref:System.GCCollectionMode.Optimized>|Uma coleta de bloqueio pode ser executada, dependendo do estado do coletor de lixo e do parâmetro `generation`. O coletor de lixo tenta fornecer um desempenho ideal.|Uma coleta pode ser executada, dependendo do estado do coletor de lixo. O método <xref:System.GC.Collect%28System.Int32%2CSystem.GCCollectionMode%2CSystem.Boolean%29> solicita uma coleta em segundo plano, mas isso não é garantido; dependendo das circunstâncias, uma coleta de bloqueio ainda pode ser executada. O coletor de lixo tenta fornecer um desempenho ideal. Se uma coleta em segundo plano já estiver em andamento, o método retornará imediatamente.|  
  
## <a name="see-also"></a>Veja também

- [Modos de latência](latency.md)
- [Coleta de lixo](index.md)
