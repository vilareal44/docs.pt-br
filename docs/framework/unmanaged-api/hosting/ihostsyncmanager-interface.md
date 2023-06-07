---
title: Interface IHostSyncManager
ms.date: 03/30/2017
api_name:
- IHostSyncManager
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostSyncManager
helpviewer_keywords:
- IHostSyncManager interface [.NET Framework hosting]
ms.assetid: 2e081a37-6a28-4c93-b7ab-1c96a464637c
topic_type:
- apiref
ms.openlocfilehash: fd3c941d89fbd93f30fc1af235f6310b23758973
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501450"
---
# <a name="ihostsyncmanager-interface"></a>Interface IHostSyncManager
Fornece métodos que permitem que o Common Language Runtime (CLR) crie primitivos de sincronização chamando o host em vez de usar as funções de sincronização do Win32.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método CreateAutoEvent](ihostsyncmanager-createautoevent-method.md)|Cria um objeto de evento de redefinição automática.|  
|[Método CreateCrst](ihostsyncmanager-createcrst-method.md)|Cria um objeto de seção crítica para sincronização.|  
|[Método CreateCrstWithSpinCount](ihostsyncmanager-createcrstwithspincount-method.md)|Cria um objeto de seção crítica com contagem de rotação para sincronização.|  
|[Método CreateManualEvent](ihostsyncmanager-createmanualevent-method.md)|Cria um objeto de evento de redefinição manual.|  
|[Método CreateMonitorEvent](ihostsyncmanager-createmonitorevent-method.md)|Cria um objeto de evento monitorado de redefinição automática.|  
|[Método CreateRWLockReaderEvent](ihostsyncmanager-createrwlockreaderevent-method.md)|Cria um objeto de evento de redefinição manual para a implementação de um bloqueio de leitor.|  
|[Método CreateRWLockWriterEvent](ihostsyncmanager-createrwlockwriterevent-method.md)|Cria um objeto de evento de redefinição automática para a implementação de um bloqueio de gravador.|  
|[Método CreateSemaphore](ihostsyncmanager-createsemaphore-method.md)|Cria um objeto [IHostSemaphore](ihostsemaphore-interface.md) para o CLR usar como um semáforo para eventos de espera.|  
|[Método SetCLRSyncManager](ihostsyncmanager-setclrsyncmanager-method.md)|Define a instância de [ICLRSyncManager](iclrsyncmanager-interface.md) a ser associada à `IHostSyncManager` instância atual.|  
  
## <a name="remarks"></a>Comentários  
 O CLR descobre a implementação do host do `IHostSyncManager` chamando o método [IHostControl:: GetHostManager](ihostcontrol-gethostmanager-method.md) com um `IID` de IID_IHostSyncManager.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICLRSyncManager](iclrsyncmanager-interface.md)
- [Interfaces de hospedagem](hosting-interfaces.md)
