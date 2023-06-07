---
title: Método ICorProfilerCallback::RuntimeSuspendAborted
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.RuntimeSuspendAborted
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::RuntimeSuspendAborted
helpviewer_keywords:
- ICorProfilerCallback::RuntimeSuspendAborted method [.NET Framework profiling]
- RuntimeSuspendAborted method [.NET Framework profiling]
ms.assetid: 5a8a4277-345b-448b-a028-fc8cff9998aa
topic_type:
- apiref
ms.openlocfilehash: a3fb5c398b8ccd7caba0b005bcf03e64ecef4ba5
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503244"
---
# <a name="icorprofilercallbackruntimesuspendaborted-method"></a>Método ICorProfilerCallback::RuntimeSuspendAborted
Notifica o criador de perfil de que o tempo de execução anulou a suspensão de tempo de execução que estava ocorrendo.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT RuntimeSuspendAborted();  
```  
  
## <a name="remarks"></a>Comentários  
 A suspensão de tempo de execução poderá ser anulada se dois threads tentarem suspender simultaneamente o tempo de execução.  
  
 O retorno de chamada [ICorProfilerCallback:: RuntimeSuspendFinished](icorprofilercallback-runtimesuspendfinished-method.md) ou o `RuntimeSuspendAborted` retorno de chamada ocorrerá em um único thread após um retorno de chamada [ICorProfilerCallback:: RuntimeSuspendStarted](icorprofilercallback-runtimesuspendstarted-method.md) .  
  
 `RuntimeSuspendAborted`É garantido que o retorno de chamada ocorra no mesmo thread que o `RuntimeSuspendStarted` retorno de chamada.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback](icorprofilercallback-interface.md)
