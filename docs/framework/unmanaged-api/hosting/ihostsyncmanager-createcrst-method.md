---
title: Método IHostSyncManager::CreateCrst
ms.date: 03/30/2017
api_name:
- IHostSyncManager.CreateCrst
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostSyncManager::CreateCrst
helpviewer_keywords:
- CreateCrst method [.NET Framework hosting]
- IHostSyncManager::CreateCrst method [.NET Framework hosting]
ms.assetid: ac278cc8-2540-4a6c-b5c6-b90c3970b4f4
topic_type:
- apiref
ms.openlocfilehash: 3566907544c72da2735e155d9088fe09fea4a728
ms.sourcegitcommit: d223616e7e6fe2139079052e6fcbe25413fb9900
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83803471"
---
# <a name="ihostsyncmanagercreatecrst-method"></a>Método IHostSyncManager::CreateCrst
Cria um objeto de seção crítica para sincronização.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT CreateCrst (  
    [out] IHostCrst** ppCrst  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `ppCrst`  
 fora Um ponteiro para o endereço de uma instância de [IHostCrst](ihostcrst-interface.md) implementada pelo host, ou NULL se a seção crítica não puder ser criada.  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|`CreateCrst`retornado com êxito.|  
|HOST_E_CLRNOTAVAILABLE|O Common Language Runtime (CLR) não foi carregado em um processo ou o CLR está em um estado no qual não pode executar código gerenciado ou processar a chamada com êxito.|  
|HOST_E_TIMEOUT|A chamada atingiu o tempo limite.|  
|HOST_E_NOT_OWNER|O chamador não possui o bloqueio.|  
|HOST_E_ABANDONED|Um evento foi cancelado enquanto um thread ou uma fibra bloqueada estava esperando.|  
|E_FAIL|Ocorreu uma falha catastrófica desconhecida. Quando um método retorna E_FAIL, o CLR não é mais utilizável no processo. As chamadas subsequentes para métodos de hospedagem retornam HOST_E_CLRNOTAVAILABLE.|  
|E_OUTOFMEMORY|Não há memória suficiente disponível para criar a seção crítica solicitada.|  
  
## <a name="remarks"></a>Comentários  
 Os objetos de seção crítica fornecem sincronização semelhante à fornecida por um objeto mutex, exceto que as seções críticas podem ser usadas somente pelos threads de um único processo. `CreateCrst`espelha a função do Win32 `InitializeCriticalSection` .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICLRSyncManager](iclrsyncmanager-interface.md)
- [Interface IHostCrst](ihostcrst-interface.md)
- [Interface IHostSyncManager](ihostsyncmanager-interface.md)
- [Interface IHostSemaphore](ihostsemaphore-interface.md)
- [Mutexes](../../../standard/threading/mutexes.md)
- [Semaphore e SemaphoreSlim](../../../standard/threading/semaphore-and-semaphoreslim.md)
