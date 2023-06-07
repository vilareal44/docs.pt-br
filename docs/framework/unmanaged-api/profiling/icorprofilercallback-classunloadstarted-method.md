---
title: Método ICorProfilerCallback::ClassUnloadStarted
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.ClassUnloadStarted
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::ClassUnloadStarted
helpviewer_keywords:
- ClassUnloadStarted method [.NET Framework profiling]
- ICorProfilerCallback::ClassUnloadStarted method [.NET Framework profiling]
ms.assetid: bc93bead-f3a9-415c-b919-ddd3ca80facc
topic_type:
- apiref
ms.openlocfilehash: 86402abca5386f34256f1f44f674f1e1898ad5fd
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500345"
---
# <a name="icorprofilercallbackclassunloadstarted-method"></a>Método ICorProfilerCallback::ClassUnloadStarted
Notifica o criador de perfil de que uma classe está sendo descarregada.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT ClassUnloadStarted(  
    [in] ClassID classId);  
```  
  
## <a name="parameters"></a>Parâmetros

- `classId`

  \[in] identifica a classe que está sendo descarregada.

## <a name="remarks"></a>Comentários  
 O valor de `classId` não é válido para uma solicitação de informações após o `ClassUnloadStarted` retorno do método — essa é a última chance do criador de perfil obter informações sobre essa classe.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback](icorprofilercallback-interface.md)
- [Método ClassUnloadFinished](icorprofilercallback-classunloadfinished-method.md)
