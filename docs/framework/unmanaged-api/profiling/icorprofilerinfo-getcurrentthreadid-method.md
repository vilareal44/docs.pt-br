---
title: Método ICorProfilerInfo::GetCurrentThreadID
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.GetCurrentThreadID
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::GetCurrentThreadID
helpviewer_keywords:
- GetCurrentThreadID method, ICorProfilerInfo interface [.NET Framework profiling]
- ICorProfilerInfo::GetCurrentThreadID method [.NET Framework profiling]
ms.assetid: 39bbdb30-6a7a-4202-8da3-67ae9a0ab3a8
topic_type:
- apiref
ms.openlocfilehash: fa0fe827300a86a906a254292434e2a56ebb4a47
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84498395"
---
# <a name="icorprofilerinfogetcurrentthreadid-method"></a>Método ICorProfilerInfo::GetCurrentThreadID
Obtém a ID do thread atual, se for um thread gerenciado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetCurrentThreadID(  
    [out] ThreadID *pThreadId);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pThreadId`  
 fora Um ponteiro para a ID retornada do thread gerenciado.  
  
## <a name="remarks"></a>Comentários  
 Se o thread atual for um thread de tempo de execução interno ou outro thread não gerenciado, `GetCurrentThreadID` o retornará CORPROF_E_NOT_MANAGED_THREAD como HRESULT e o valor retornado do `pThreadId` parâmetro será NULL.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)
