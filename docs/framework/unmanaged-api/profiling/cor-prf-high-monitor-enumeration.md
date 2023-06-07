---
title: Enumeração COR_PRF_HIGH_MONITOR
ms.date: 04/10/2018
ms.assetid: 3ba543d8-15e5-4322-b6e7-1ebfc92ed7dd
ms.openlocfilehash: b813b32ef4522f1707812d337d20790ce75f3d55
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500839"
---
# <a name="cor_prf_high_monitor-enumeration"></a>Enumeração COR_PRF_HIGH_MONITOR

[Com suporte no .NET Framework 4.5.2 e versões posteriores]  
  
Fornece sinalizadores além daqueles encontrados na enumeração de [COR_PRF_MONITOR](cor-prf-monitor-enumeration.md) que o criador de perfil pode especificar para o método [ICorProfilerInfo5:: SetEventMask2](icorprofilerinfo5-seteventmask2-method.md) quando ele está sendo carregado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp
typedef enum {  
    COR_PRF_HIGH_MONITOR_NONE                     = 0x00000000,  
    COR_PRF_HIGH_ADD_ASSEMBLY_REFERENCES          = 0x00000001,  
    COR_PRF_HIGH_IN_MEMORY_SYMBOLS_UPDATED        = 0x00000002,
    COR_PRF_HIGH_MONITOR_DYNAMIC_FUNCTION_UNLOADS = 0x00000004,
    COR_PRF_HIGH_DISABLE_TIERED_COMPILATION       = 0x00000008,
    COR_PRF_HIGH_BASIC_GC                         = 0x00000010,
    COR_PRF_HIGH_MONITOR_GC_MOVED_OBJECTS         = 0x00000020,
    COR_PRF_HIGH_MONITOR_LARGEOBJECT_ALLOCATED    = 0x00000040,
    COR_PRF_HIGH_REQUIRE_PROFILE_IMAGE            = 0,  
    COR_PRF_HIGH_ALLOWABLE_AFTER_ATTACH           = COR_PRF_HIGH_IN_MEMORY_SYMBOLS_UPDATED |
                                                    COR_PRF_HIGH_MONITOR_DYNAMIC_FUNCTION_UNLOADS |
                                                    COR_PRF_HIGH_BASIC_GC |
                                                    COR_PRF_HIGH_MONITOR_GC_MOVED_OBJECTS |
                                                    COR_PRF_HIGH_MONITOR_LARGEOBJECT_ALLOCATED,  
    COR_PRF_HIGH_MONITOR_IMMUTABLE                = COR_PRF_HIGH_DISABLE_TIERED_COMPILATION  
} COR_PRF_HIGH_MONITOR;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`COR_PRF_HIGH_MONITOR_NONE`|Nenhum sinalizador está definido.|  
|`COR_PRF_HIGH_ADD_ASSEMBLY_REFERENCES`|Controla o retorno de chamada [ICorProfilerCallback6:: GetAssemblyReference](icorprofilercallback6-getassemblyreferences-method.md) para adicionar referências de assembly durante a movimentação de fechamento de referência do assembly CLR.|  
|`COR_PRF_HIGH_IN_MEMORY_SYMBOLS_UPDATED`|Controla o retorno de chamada [ICorProfilerCallback7:: ModuleInMemorySymbolsUpdated](icorprofilercallback7-moduleinmemorysymbolsupdated-method.md) para atualizações para o fluxo de símbolos associado a um módulo na memória.|  
|`COR_PRF_HIGH_MONITOR_DYNAMIC_FUNCTION_UNLOADS`|Controla o retorno de chamada [ICorProfilerCallback9::D ynamicmethodunloaded](icorprofilercallback9-dynamicmethodunloaded-method.md) para indicar quando um método dinâmico foi coletado pelo lixo e descarregado. <br/> [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]|
|`COR_PRF_HIGH_DISABLE_TIERED_COMPILATION`|Somente .NET Core 3,0 e versões posteriores: desabilita a [compilação em camadas](../../../core/whats-new/dotnet-core-3-0.md) para os profileres.|
|`COR_PRF_HIGH_BASIC_GC`|Somente o .NET Core 3,0 e versões posteriores: fornece uma opção de criação de perfil de GC leve em comparação com [`COR_PRF_MONITOR_GC`](cor-prf-monitor-enumeration.md) . Controla somente os retornos de chamada [GarbageCollectionStarted](icorprofilercallback2-garbagecollectionstarted-method.md), [GarbageCollectionFinished](icorprofilercallback2-garbagecollectionfinished-method.md)e [GetGenerationBounds](icorprofilerinfo2-getgenerationbounds-method.md) . Ao contrário do `COR_PRF_MONITOR_GC` sinalizador, o não desabilita a `COR_PRF_HIGH_BASIC_GC` coleta de lixo simultânea.|
|`COR_PRF_HIGH_MONITOR_GC_MOVED_OBJECTS`|Somente o .NET Core 3,0 e versões posteriores: habilita os retornos de chamada [MovedReferences](icorprofilercallback-movedreferences-method.md) e [MovedReferences2](icorprofilercallback4-movedreferences2-method.md) apenas para os GCS de compactação.|
|`COR_PRF_HIGH_MONITOR_LARGEOBJECT_ALLOCATED`|Somente o .NET Core 3,0 e versões posteriores: semelhante a [`COR_PRF_MONITOR_OBJECT_ALLOCATED`](cor-prf-monitor-enumeration.md) , mas fornece informações sobre alocações de objeto somente para o LOH (heap de objeto grande).|
|`COR_PRF_HIGH_REQUIRE_PROFILE_IMAGE`|Representa todos os sinalizadores `COR_PRF_HIGH_MONITOR` que necessitam de imagens aprimoradas por perfil. Ele corresponde ao `COR_PRF_REQUIRE_PROFILE_IMAGE` sinalizador na enumeração de [COR_PRF_MONITOR](cor-prf-monitor-enumeration.md) .|  
|`COR_PRF_HIGH_ALLOWABLE_AFTER_ATTACH`|Representa todos os sinalizadores `COR_PRF_HIGH_MONITOR` que podem ser definidos após o criador de perfis ser anexado a um aplicativo em execução.|  
|`COR_PRF_HIGH_MONITOR_IMMUTABLE`|Representa todos os sinalizadores `COR_PRF_HIGH_MONITOR` que podem ser definidos apenas durante a inicialização. Tentar alterar qualquer um desses sinalizadores em outro lugar resulta em um valor de `HRESULT` que indica falha.|  
  
## <a name="remarks"></a>Comentários

Os `COR_PRF_HIGH_MONITOR` sinalizadores são usados com o `pdwEventsHigh` parâmetro dos métodos [ICorProfilerInfo5:: GetEventMask2](icorprofilerinfo5-geteventmask2-method.md) e [ICorProfilerInfo5:: SetEventMask2](icorprofilerinfo5-seteventmask2-method.md) .  
  
Começando com o .NET Framework 4.6.1, o valor da `COR_PRF_HIGH_ALLOWABLE_AFTER_ATTACH` alteração de 0 para `COR_PRF_HIGH_IN_MEMORY_SYMBOLS_UPDATED` (0x00000002). Começando com o .NET Framework 4.7.2, seu valor mudou de `COR_PRF_HIGH_IN_MEMORY_SYMBOLS_UPDATED` para `COR_PRF_HIGH_IN_MEMORY_SYMBOLS_UPDATED | COR_PRF_HIGH_MONITOR_DYNAMIC_FUNCTION_UNLOADS` .

`COR_PRF_HIGH_MONITOR_IMMUTABLE`destina-se a ser um bitmask que representa todos os sinalizadores que só podem ser definidos durante a inicialização. A tentativa de alterar qualquer um desses sinalizadores em outro lugar resulta em uma falha `HRESULT` .

## <a name="requirements"></a>Requisitos

**Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
**Cabeçalho:** CorProf. idl, CorProf. h  
  
**Biblioteca:** CorGuids.lib  
  
**.NET Framework versões:**[!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Criando perfil de enumerações](profiling-enumerations.md)
- [Enumeração COR_PRF_MONITOR](cor-prf-monitor-enumeration.md)
- [Interface ICorProfilerInfo5](icorprofilerinfo5-interface.md)
