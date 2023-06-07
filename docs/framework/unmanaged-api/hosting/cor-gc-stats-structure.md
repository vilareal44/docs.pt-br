---
title: Estrutura COR_GC_STATS
ms.date: 03/30/2017
api_name:
- COR_GC_STATS
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- COR_GC_STATS
helpviewer_keywords:
- COR_GC_STATS structure [.NET Framework hosting]
ms.assetid: 8d4ff73e-739b-40f6-9349-359fbc99c2f9
topic_type:
- apiref
ms.openlocfilehash: 7a6553de31d4f9627809af7691218c39dc734c6f
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501658"
---
# <a name="cor_gc_stats-structure"></a>Estrutura COR_GC_STATS
Fornece estatísticas sobre o mecanismo de coleta de lixo do Common Language Runtime (CLR).  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef struct _COR_GC_STATS {  
    ULONG   Flags;
    SIZE_T  ExplicitGCCount;  
    SIZE_T  GenCollectionsTaken[3];  
    SIZE_T  CommittedKBytes;
    SIZE_T  ReservedKBytes;  
    SIZE_T  Gen0HeapSizeKBytes;  
    SIZE_T  Gen1HeapSizeKBytes;  
    SIZE_T  Gen2HeapSizeKBytes;  
    SIZE_T  LargeObjectHeapSizeKBytes;  
    SIZE_T  KBytesPromotedFromGen0;  
    SIZE_T  KBytesPromotedFromGen1;  
} COR_GC_STATS;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`Flags`|Indica quais valores de campo devem ser calculados e retornados.|  
|`ExplicitGCCount`|Indica o número de coletas de lixo que foram forçadas por solicitação externa.|  
|`GenCollectionsTaken`|Indica o número de coletas de lixo executadas para cada geração.|  
|`CommittedKBytes`|O número total de quilobytes confirmados em todos os heaps.|  
|`ReservedKBytes`|O número total de kilobytes reservados em todos os heaps.|  
|`Gen0HeapSizeKBytes`|O tamanho, em quilobytes, do heap de geração-zero.|  
|`Gen1HeapSizeKBytes`|O tamanho, em quilobytes, do heap de geração-um.|  
|`Gen2HeapSizeKBytes`|O tamanho, em quilobytes, do heap de geração-dois.|  
|`LargeObjectHeapSizeKBytes`|O tamanho, em quilobytes, do heap de objeto grande.|  
|`KBytesPromotedFromGen0`|O tamanho, em quilobytes, dos objetos promovidos da geração zero para a geração um.|  
|`KBytesPromotedFromGen1`|O tamanho, em quilobytes, dos objetos promovidos da geração um para a geração dois.|  
  
## <a name="remarks"></a>Comentários  
 O método [ICLRGCManager:: GetStats](iclrgcmanager-getstats-method.md) exige que o `Flags` campo da `COR_GC_STATS` estrutura seja definido como um ou mais valores da enumeração [COR_GC_STAT_TYPES](cor-gc-stat-types-enumeration.md) para especificar quais estatísticas devem ser definidas.  
  
 A tabela a seguir mapeia as estatísticas fornecidas por essa estrutura para os dois [COR_GC_STAT_TYPES](cor-gc-stat-types-enumeration.md) valores de enumeração `COR_GC_COUNTS` e `COR_GC_MEMORYUSAGE` .  
  
|Especificado por COR_GC_COUNTS|Especificado por COR_GC_MEMORYUSAGE|  
|----------------------------------|---------------------------------------|  
|`ExplicitGCCount`<br /><br /> `GenCollectionsTaken`|`CommittedKBytes`<br /><br /> `ReservedKBytes`<br /><br /> `Gen0HeapSizeKBytes`<br /><br /> `Gen1HeapSizeKBytes`<br /><br /> `Gen2HeapSizeKBytes`<br /><br /> `LargeObjectHeapSizeKBytes`<br /><br /> `KBytesPromotedFromGen0`<br /><br /> `KBytesPromotedFromGen1`|  
  
 Um exemplo de uso é o seguinte:  
  
```cpp  
COR_GC_STATS GCStats;  
GCStats.Flags = COR_GC_COUNTS | COR_GC_MEMORYUSAGE;  
pCLRGCManager->GetStats(&GCStats);  
```  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** GCHost. idl  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Estruturas de hospedagem](hosting-structures.md)
- [Gerenciamento automático de memória](../../../standard/automatic-memory-management.md)
- [Coleta de lixo](../../../standard/garbage-collection/index.md)
