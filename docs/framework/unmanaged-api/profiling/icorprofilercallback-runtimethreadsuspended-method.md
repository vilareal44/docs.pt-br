---
title: Método ICorProfilerCallback::RuntimeThreadSuspended
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.RuntimeThreadSuspended
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::RuntimeThreadSuspended
helpviewer_keywords:
- RuntimeThreadSuspended method [.NET Framework profiling]
- ICorProfilerCallback::RuntimeThreadSuspended method [.NET Framework profiling]
ms.assetid: de830a8b-6ee1-4900-ace3-4237108f6b12
topic_type:
- apiref
ms.openlocfilehash: 54f7dae511c8bf25dc96451e6477acf26655430a
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503192"
---
# <a name="icorprofilercallbackruntimethreadsuspended-method"></a>Método ICorProfilerCallback::RuntimeThreadSuspended
Notifica o criador de perfil de que o thread especificado foi suspenso ou está prestes a ser suspenso.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT RuntimeThreadSuspended(  
    [in] ThreadID threadId);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `threadId`  
 no A ID do thread que foi suspenso.  
  
## <a name="remarks"></a>Comentários  
 A `RuntimeThreadSuspended` notificação pode ocorrer a qualquer momento entre o [ICorProfilerCallback:: RuntimeSuspendStarted](icorprofilercallback-runtimesuspendstarted-method.md) e os retornos de chamada [ICorProfilerCallback:: RuntimeResumeStarted](icorprofilercallback-runtimeresumestarted-method.md) associados. As notificações que ocorrem entre [ICorProfilerCallback:: RuntimeSuspendFinished](icorprofilercallback-runtimesuspendfinished-method.md) e `RuntimeResumeStarted` são para threads que estavam em execução em código não gerenciado e foram suspensas na entrada para o tempo de execução.  
  
 Geralmente, esse retorno de chamada ocorre apenas depois que um thread é suspenso. No entanto, se o thread atualmente em execução (o thread que chamou esse retorno de chamada) for aquele que está sendo suspenso, esse retorno de chamada ocorrerá logo antes de o thread ser suspenso.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback](icorprofilercallback-interface.md)
- [Método RuntimeThreadResumed](icorprofilercallback-runtimethreadresumed-method.md)
