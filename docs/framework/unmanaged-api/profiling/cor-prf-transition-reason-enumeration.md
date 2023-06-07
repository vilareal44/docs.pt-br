---
title: Enumeração COR_PRF_TRANSITION_REASON
ms.date: 03/30/2017
api_name:
- COR_PRF_TRANSITION_REASON
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- COR_PRF_TRANSITION_REASON
helpviewer_keywords:
- COR_PRF_TRANSITION_REASON enumeration [.NET Framework profiling]
ms.assetid: da941118-01b7-4197-ae5b-9f2f8adcd623
topic_type:
- apiref
ms.openlocfilehash: e890c62a54654e86bb4a825613807efe142c8d5a
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500735"
---
# <a name="cor_prf_transition_reason-enumeration"></a>Enumeração COR_PRF_TRANSITION_REASON
Indica o motivo para uma transição de código gerenciado para não gerenciado ou vice-versa.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum {  
    COR_PRF_TRANSITION_CALL,  
    COR_PRF_TRANSITION_RETURN  
} COR_PRF_TRANSITION_REASON;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`COR_PRF_TRANSITION_CALL`|A transição é devido a uma chamada em uma função.|  
|`COR_PRF_TRANSITION_RETURN`|A transição é devido a um retorno de uma função.|  
  
## <a name="remarks"></a>Comentários  
 Quando ocorre uma transição, o criador de perfil recebe um retorno de chamada [ICorProfilerCallback:: ManagedToUnmanagedTransition](icorprofilercallback-managedtounmanagedtransition-method.md) ou [ICorProfilerCallback:: UnmanagedToManagedTransition](icorprofilercallback-unmanagedtomanagedtransition-method.md) , o que fornece um valor da `COR_PRF_TRANSITION_REASON` enumeração para indicar o motivo da transição.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
