---
title: Método ICorProfilerCallback::ThreadCreated
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.ThreadCreated
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::ThreadCreated
helpviewer_keywords:
- ICorProfilerCallback::ThreadCreated method [.NET Framework profiling]
- ThreadCreated method [.NET Framework profiling]
ms.assetid: cca0f799-09b8-4689-a33c-6d6537943a9b
topic_type:
- apiref
ms.openlocfilehash: 25a4b101388bfc0151ba7c9c52da6561d48f806b
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503153"
---
# <a name="icorprofilercallbackthreadcreated-method"></a>Método ICorProfilerCallback::ThreadCreated
Notifica o criador de perfil de que um thread foi criado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT ThreadCreated(  
    [in] ThreadID threadId);
```  
  
## <a name="parameters"></a>Parâmetros  
 `threadId`  
 no A ID do thread que foi criado.  
  
## <a name="remarks"></a>Comentários  
 O `threadId` valor é válido imediatamente.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback](icorprofilercallback-interface.md)
- [Método ThreadDestroyed](icorprofilercallback-threaddestroyed-method.md)
