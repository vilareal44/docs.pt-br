---
title: Interface ICorProfilerCallback3
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback3
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback3
helpviewer_keywords:
- ICorProfilerCallback3 interface [.NET Framework profiling]
ms.assetid: be83af41-3dec-4c77-8529-9dd6b8042af6
topic_type:
- apiref
ms.openlocfilehash: db07e2afa64ea2bf80416e6ab8cba5a4dcdc8dcf
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84499669"
---
# <a name="icorprofilercallback3-interface"></a>Interface ICorProfilerCallback3
Fornece métodos de retorno de chamada que o Common Language Runtime (CLR) usa para comunicar a anexação e desanexar informações de estado ao criador de perfil.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método InitializeForAttach](icorprofilercallback3-initializeforattach-method.md)|Chamado pelo CLR para dar ao criador de perfil uma oportunidade de inicializar seu estado após uma operação de anexação.|  
|[Método ProfilerAttachComplete](icorprofilercallback3-profilerattachcomplete-method.md)|Chamado pelo CLR para indicar que o criador de perfil agora pode chamar os métodos de atualização.|  
|[Método ProfilerDetachSucceeded](icorprofilercallback3-profilerdetachsucceeded-method.md)|Notifica o criador de perfil de que o Common Language Runtime (CLR) está prestes a descarregar a DLL do criador de perfil.|  
  
## <a name="remarks"></a>Comentários  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Criação de perfil de interfaces](profiling-interfaces.md)
- [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)
- [Interface ICorProfilerCallback2](icorprofilercallback2-interface.md)
- [Interface ICorProfilerCallback4](icorprofilercallback4-interface.md)
