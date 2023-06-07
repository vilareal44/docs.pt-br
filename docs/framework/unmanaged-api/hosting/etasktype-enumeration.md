---
title: Enumeração ETaskType
ms.date: 03/30/2017
api_name:
- ETaskType
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ETaskType
helpviewer_keywords:
- ETaskType enumeration [.NET Framework hosting]
ms.assetid: aa527b31-89d4-41f2-ad6f-63b76950b7df
topic_type:
- apiref
ms.openlocfilehash: 0fa72568df77c4916a3c6676e1dcca7c0c616c4a
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84493312"
---
# <a name="etasktype-enumeration"></a>Enumeração ETaskType
Contém valores que indicam o tipo de tarefa que é representado por uma interface [ICLRTask](iclrtask-interface.md) ou [IHostTask](ihosttask-interface.md) .  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum ETaskType {  
    TT_DEBUGGERHELPER           = 0x1,  
    TT_GC                       = 0x2,  
    TT_FINALIZER                = 0x4,  
    TT_THREADPOOL_TIMER         = 0x8,  
    TT_THREADPOOL_GATE          = 0x10,  
    TT_THREADPOOL_WORKER        = 0x20,  
    TT_THREADPOOL_IOCOMPLETION  = 0x40,  
    TT_ADUNLOAD                 = 0x80,  
    TT_USER                     = 0x100,  
    TT_THREADPOOL_WAIT          = 0x200,  
    TT_UNKNOWN                  = 0x80000000  
} ETaskType;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`TT_ADUNLOAD`|A interface representa uma tarefa de descarregamento de domínio de aplicativo.|  
|`TT_DEBUGGERHELPER`|A interface representa uma tarefa auxiliar do depurador.|  
|`TT_FINALIZER`|A interface representa uma tarefa Finalizadora.|  
|`TT_GC`|A interface representa uma tarefa de coleta de lixo.|  
|`TT_THREADPOOL_GATE`|A interface representa uma tarefa portão thread.|  
|`TT_THREADPOOL_IOCOMPLETION`|A interface representa uma tarefa de thread de e/s ou uma tarefa de thread de porta de conclusão.|  
|`TT_THREADPOOL_TIMER`|A interface representa uma tarefa de thread de temporizador.|  
|`TT_THREADPOOL_WAIT`|A interface representa uma tarefa de thread de espera.|  
|`TT_THREADPOOL_WORKER`|A interface representa uma tarefa de thread de trabalho.|  
|`TT_UNKNOWN`|A tarefa é desconhecida.|  
|`TT_USER`|A interface representa uma tarefa de usuário.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Hospedando enumerações](hosting-enumerations.md)
