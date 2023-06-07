---
title: Enumeração COR_PRF_FINALIZER_FLAGS
ms.date: 03/30/2017
api_name:
- COR_PRF_FINALIZER_FLAGS
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- COR_PRF_FINALIZER_FLAGS
helpviewer_keywords:
- COR_PRF_FINALIZER_FLAGS enumeration [.NET Framework profiling]
ms.assetid: 297d7721-3911-4f36-9e34-d9da0c33e22a
topic_type:
- apiref
ms.openlocfilehash: b273faafd7abb86ace58bb5c24473406af3ce20e
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500965"
---
# <a name="cor_prf_finalizer_flags-enumeration"></a>Enumeração COR_PRF_FINALIZER_FLAGS
Descreve o finalizador de um objeto.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum {  
    COR_PRF_FINALIZER_CRITICAL = 0x1  
} COR_PRF_FINALIZER_FLAGS;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`COR_PRF_FINALIZER_CRITICAL`|O finalizador é crítico.|  
  
## <a name="remarks"></a>Comentários  
 A `COR_PRF_FINALIZER_FLAGS` enumeração é usada pelo método [ICorProfilerCallback2:: FinalizeableObjectQueued](icorprofilercallback2-finalizeableobjectqueued-method.md) para descrever o finalizador de um objeto.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Criando perfil de enumerações](profiling-enumerations.md)
