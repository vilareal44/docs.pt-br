---
title: Enumeração WAIT_OPTION
ms.date: 03/30/2017
api_name:
- WAIT_OPTION
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- WAIT_OPTION
helpviewer_keywords:
- WAIT_OPTION enumeration [.NET Framework hosting]
ms.assetid: 962fc293-8ded-4b3b-90ce-2c21a4f1b244
topic_type:
- apiref
ms.openlocfilehash: 2f83bc5b114b746958f936c311efa823d88441d1
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503879"
---
# <a name="wait_option-enumeration"></a>Enumeração WAIT_OPTION
Contém valores que indicam a ação que um host deve executar se uma operação solicitada pelos blocos de Common Language Runtime (CLR).  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum {  
    WAIT_MSGPUMP       = 0x1,  
    WAIT_ALERTABLE     = 0x2,  
    WAIT_NOTINDEADLOCK = 0x4,  
} WAIT_OPTION;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`WAIT_ALERTABLE`|Notifica o host de que a tarefa deve ser despertada se o CLR chama o método [IHostTask:: Alert](ihosttask-alert-method.md) .|  
|`WAIT_MSGPUMP`|Notifica o host de que ele deve bombear mensagens no thread do sistema operacional atual se o thread for bloqueado. O tempo de execução especifica esse valor somente em um <xref:System.Threading.ApartmentState.STA> thread.|  
|`WAIT_NOTINDEADLOCK`|Notifica o host que a solicitação de sincronização especificada não pode ser quebrada por um host. Ou seja, o host não pode retornar `HOST_E_DEADLOCK` .|  
  
## <a name="remarks"></a>Comentários  
 Os métodos [IHostTaskManager:: Sleep](ihosttaskmanager-sleep-method.md) e [IHostTaskManager:: SwitchToTask](ihosttaskmanager-switchtotask-method.md) usam um parâmetro desse tipo.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Hospedando enumerações](hosting-enumerations.md)
