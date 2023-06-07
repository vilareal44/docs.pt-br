---
title: Método ICorProfilerInfo::GetHandleFromThread
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.GetHandleFromThread
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::GetHandleFromThread
helpviewer_keywords:
- GetHandleFromThread method [.NET Framework profiling]
- ICorProfilerInfo::GetHandleFromThread method [.NET Framework profiling]
ms.assetid: 36cdc9f5-7579-4cd2-aa36-fc05c741584c
topic_type:
- apiref
ms.openlocfilehash: 419195d9450bf07e5ad8c7cedcac76e175137c96
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84498174"
---
# <a name="icorprofilerinfogethandlefromthread-method"></a>Método ICorProfilerInfo::GetHandleFromThread
Mapeia a ID de um thread para um identificador de thread do Win32.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetHandleFromThread(  
    [in]  ThreadID threadId,  
    [out] HANDLE  *phThread);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `threadId`  
 no A ID do thread a ser mapeada.  
  
 `phThread`  
 fora Um ponteiro para um identificador de Thread Win32.  
  
## <a name="remarks"></a>Comentários  
 O criador de perfil deve chamar a função do Win32 `DuplicateHandle` na alça antes de usá-la.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)
