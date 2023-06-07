---
title: Interface ICorProfilerCallback4
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback4
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback4
helpviewer_keywords:
- ICorProfilerCallback4 interface [.NET Framework profiling]
ms.assetid: 665f3cfc-cd6f-4880-906c-ea65ad384783
topic_type:
- apiref
ms.openlocfilehash: 295d3d440529623f4569fd6c5f4debe7e4558990
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84499435"
---
# <a name="icorprofilercallback4-interface"></a>Interface ICorProfilerCallback4
Fornece métodos de retorno de chamada que o Common Language Runtime (CLR) usa para comunicar informações ao criador de perfil.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método GetReJITParameters](icorprofilercallback4-getrejitparameters-method.md)|Permite que o criador de perfil de código defina sinalizadores de geração de código alternativos para um novo corpo de método recompilado.|  
|[Método MovedReferences2](icorprofilercallback4-movedreferences2-method.md)|Relata o novo layout de objetos no heap como resultado de uma coleta de lixo de compactação.|  
|[Método ReJITCompilationFinished](icorprofilercallback4-rejitcompilationfinished-method.md)|Notifica o criador de perfil de que o compilador JIT (just-in-time) concluiu a recompilação de uma função.|  
|[Método ReJITCompilationStarted](icorprofilercallback4-rejitcompilationstarted-method.md)|Notifica o criador de perfil de que o compilador JIT (just-in-time) começou a recompilar uma função.|  
|[Método ReJITError](icorprofilercallback4-rejiterror-method.md)|Relata um erro encontrado durante o processamento de uma solicitação de recompilação.|  
|[Método SurvivingReferences2](icorprofilercallback4-survivingreferences2-method.md)|Relata o layout dos objetos no heap como resultado de uma coleta de lixo sem compactação.|  
  
## <a name="remarks"></a>Comentários  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback2](icorprofilercallback2-interface.md)
- [Criação de perfil de interfaces](profiling-interfaces.md)
- [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)
