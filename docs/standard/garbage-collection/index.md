---
title: Coleta de lixo do .NET
description: Saiba mais sobre a coleta de lixo no .NET. O coletor de lixo do .NET gerencia a alocação e a liberação de memória para seu aplicativo.
ms.date: 04/21/2020
ms.technology: dotnet-standard
helpviewer_keywords:
- memory, garbage collection
- garbage collection, automatic memory management
- GC [.NET Framework]
- memory, allocating
- common language runtime, garbage collection
- garbage collector
- cleanup operations
- garbage collection
- memory, releasing
- common language runtime, automatic memory management
- automatic memory management
- runtime, automatic memory management
- runtime, garbage collection
- garbage collection, about
ms.assetid: 22b6cb97-0c80-4eeb-a2cf-5ed7655e37f9
ms.openlocfilehash: dde0012ff7233eb7ee13efab1931f129b0eae276
ms.sourcegitcommit: 7137e12f54c4e83a94ae43ec320f8cf59c1772ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84662479"
---
# <a name="garbage-collection"></a>Coleta de lixo

O coletor de lixo do .NET gerencia a alocação e a liberação de memória para seu aplicativo. Toda vez que você cria um novo objeto, o Common Language Runtime aloca memória para o objeto do heap gerenciado. Desde que exista espaço de endereço disponível no heap gerenciado, o runtime continua alocando espaço para novos objetos. No entanto, a memória não é infinita. No fim das contas, o coletor de lixo deve realizar uma coleta para liberar algum espaço na memória. O mecanismo de otimização do coletor de lixo determina o melhor momento para executar uma coleta com base nas alocações que estão sendo feitas. Quando o coletor de lixo executa uma coleta, ele verifica se há objetos no heap gerenciado que não estão mais sendo usados pelo aplicativo e realiza as operações necessárias para recuperar sua memória.  
  
## <a name="in-this-section"></a>Nesta seção
  
|Title|Descrição|  
|-----------|-----------------|  
|[Conceitos básicos da coleta de lixo](fundamentals.md)|Descreve como funciona a coleta de lixo, como os objetos são alocados no heap gerenciado e outros conceitos principais.|  
|[Coleta de lixo de estação de trabalho ou de servidor](workstation-server-gc.md)|Descreve as diferenças entre a coleta de lixo da estação de trabalho para aplicativos cliente e a coleta de lixo do servidor para aplicativos de servidor.|
|[Coleta de lixo em segundo plano](background-gc.md)|Descreve a coleta de lixo em segundo plano, que é a coleção de objetos de geração 0 e 1 enquanto a coleta de geração 2 está em andamento.|
|[O heap de objetos grandes](large-object-heap.md)|Descreve o LOH (heap de objeto grande) e como os objetos grandes são coletados por lixo.|
|[Coleta de lixo e desempenho](performance.md)|Descreve as verificações de desempenho que você pode usar para diagnosticar problemas de desempenho e de coleta de lixo.|  
|[Coletas induzidas](induced.md)|Descreve como fazer uma coleta de lixo ocorrer.|  
|[Modos de latência](latency.md)|Descreve os modos de determinam o grau de intrusão da coleta de lixo.|  
|[Otimização da hospedagem Web compartilhada](optimization-for-shared-web-hosting.md)|Descreve como otimizar a coleta de lixo em servidores compartilhados por vários sites pequenos.|  
|[Notificações da coleta de lixo](notifications.md)|Descreve como determinar quando uma coleta de lixo completa está se aproximando e quando ela é concluída.|  
|[Monitoramento de recursos do domínio do aplicativo](app-domain-resource-monitoring.md)|Descreve como monitorar o uso de CPU e memória por um domínio do aplicativo.|  
|[Referências fracas](weak-references.md)|Descreve os recursos que permitem que um objeto seja, simultaneamente, coletado pelo coletor de lixo e acessado pelo aplicativo.|  
  
## <a name="reference"></a>Referência

- <xref:System.GC?displayProperty=nameWithType>  
- <xref:System.GCCollectionMode?displayProperty=nameWithType>  
- <xref:System.GCNotificationStatus?displayProperty=nameWithType>  
- <xref:System.Runtime.GCLatencyMode?displayProperty=nameWithType>  
- <xref:System.Runtime.GCSettings?displayProperty=nameWithType>  
- <xref:System.Runtime.GCSettings.LargeObjectHeapCompactionMode%2A?displayProperty=nameWithType>  
- <xref:System.Object.Finalize%2A?displayProperty=nameWithType>  
- <xref:System.IDisposable?displayProperty=nameWithType>  
  
## <a name="see-also"></a>Confira também

- [Limpar recursos não gerenciados](unmanaged.md)
