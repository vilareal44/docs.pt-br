---
title: Interface IGCThreadControl
ms.date: 03/30/2017
api_name:
- IGCThreadControl
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IGCThreadControl
helpviewer_keywords:
- IGCThreadControl interface [.NET Framework hosting]
ms.assetid: 3ff04d75-85ac-4df9-886d-dbaa037c0552
topic_type:
- apiref
ms.openlocfilehash: 78e667acf1573769a1a67b4c964d7801f11838fe
ms.sourcegitcommit: d223616e7e6fe2139079052e6fcbe25413fb9900
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83805123"
---
# <a name="igcthreadcontrol-interface"></a>Interface IGCThreadControl
Fornece métodos para participar do agendamento de threads que, de outra forma, seriam bloqueados para uma coleta de lixo.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método SuspensionEnding](igcthreadcontrol-suspensionending-method.md)|Notifica o host de que o tempo de execução está retomando threads após uma coleta de lixo ou outra suspensão.|  
|[Método SuspensionStarting](igcthreadcontrol-suspensionstarting-method.md)|Notifica o host de que o tempo de execução está começando uma suspensão de thread para uma coleta de lixo ou outra suspensão.|  
|[Método ThreadIsBlockingForSuspension](igcthreadcontrol-threadisblockingforsuspension-method.md)|Notifica o host que o thread que está fazendo a chamada está prestes a bloquear, talvez para uma coleta de lixo ou outra suspensão.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interfaces de hospedagem](hosting-interfaces.md)
