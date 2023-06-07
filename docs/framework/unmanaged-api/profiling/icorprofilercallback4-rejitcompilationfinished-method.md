---
title: Método ICorProfilerCallback4::ReJITCompilationFinished
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback4.ReJITCompilationFinished
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback4::ReJITCompilationFinished
helpviewer_keywords:
- ICorProfilerCallback4::ReJITCompilationFinished method [.NET Framework profiling]
- ReJITCompilationFinished method, ICorProfilerCallback4 interface [.NET Framework profiling]
ms.assetid: 3b5cff02-2005-44eb-a2bc-50214c4b0e1d
topic_type:
- apiref
ms.openlocfilehash: ff06c285bf5306977b520ed9ff845e70fb25989a
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84499370"
---
# <a name="icorprofilercallback4rejitcompilationfinished-method"></a>Método ICorProfilerCallback4::ReJITCompilationFinished
Notifica o criador de perfil de que o compilador JIT (just-in-time) concluiu a recompilação de uma função.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT ReJITCompilationFinished(  
    [in] FunctionID functionId,    [in] ReJITID rejitId,  
    [in] HRESULT    hrStatus,  
    [in] BOOL       fIsSafeToBlock);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `functionId`  
 no A ID da função que foi recompilada.  
  
 `rejitId`  
 no A identidade da função de compilação JIT recompilada.  
  
 `hrStatus`  
 no Um valor que indica se a recompilação JIT foi bem-sucedida.  
  
 `fIsSafeToBlock`  
 [in] `true` para indicar que o bloqueio pode fazer com que o tempo de execução aguarde o thread de chamada retornar desse retorno de chamada; `false`para indicar que o bloqueio não afetará a operação do tempo de execução.  
  
 Um valor de não `true` danifica o tempo de execução, mas pode afetar os resultados da criação de perfil.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback](icorprofilercallback-interface.md)
- [Interface ICorProfilerCallback4](icorprofilercallback4-interface.md)
- [Método JITCompilationStarted](icorprofilercallback-jitcompilationstarted-method.md)
- [Método ReJITCompilationStarted](icorprofilercallback4-rejitcompilationstarted-method.md)
