---
title: Método IHostTaskManager::CreateTask
ms.date: 03/30/2017
api_name:
- IHostTaskManager.CreateTask
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostTaskManager::CreateTask
helpviewer_keywords:
- CreateTask method, IHostTaskManager interface [.NET Framework hosting]
- IHostTaskManager::CreateTask method [.NET Framework hosting]
ms.assetid: a6f8ad36-61e1-42b0-9db2-add575646d18
topic_type:
- apiref
ms.openlocfilehash: 4037ffe63d8ebfca67cbd0b3293d36be7481b1bd
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501411"
---
# <a name="ihosttaskmanagercreatetask-method"></a>Método IHostTaskManager::CreateTask
Solicita que o host crie uma nova tarefa.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT CreateTask (  
    [in]  DWORD stacksize,
    [in]  LPTHREAD_START_ROUTINE pStartAddress,  
    [in]  PVOID pParameter,  
    [out] IHostTask **ppTask  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `stacksize`  
 no O tamanho solicitado, em bytes, da pilha solicitada ou de 0 (zero) para o tamanho padrão.  
  
 `pStartAddress`  
 no Um ponteiro para a função que a tarefa deve executar.  
  
 `pParameter`  
 no Um ponteiro para os dados do usuário a serem passados para a função, ou NULL se a função não usar parâmetros.  
  
 `ppTask`  
 fora Um ponteiro para o endereço de uma instância de [IHostTask](ihosttask-interface.md) criada pelo host ou NULL se a tarefa não puder ser criada. A tarefa permanece em um estado suspenso até que seja explicitamente iniciada por uma chamada para [IHostTask:: Start](ihosttask-start-method.md).  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|`CreateTask`retornado com êxito.|  
|HOST_E_CLRNOTAVAILABLE|O Common Language Runtime (CLR) não foi carregado em um processo ou o CLR está em um estado no qual não pode executar código gerenciado ou processar a chamada com êxito.|  
|HOST_E_TIMEOUT|A chamada atingiu o tempo limite.|  
|HOST_E_NOT_OWNER|O chamador não possui o bloqueio.|  
|HOST_E_ABANDONED|Um evento foi cancelado enquanto um thread ou uma fibra bloqueada estava esperando.|  
|E_FAIL|Ocorreu uma falha catastrófica desconhecida. Quando um método retorna E_FAIL, o CLR não é mais utilizável no processo. As chamadas subsequentes para métodos de hospedagem retornam HOST_E_CLRNOTAVAILABLE.|  
|E_OUTOFMEMORY|Não há memória suficiente disponível para criar a tarefa solicitada.|  
  
## <a name="remarks"></a>Comentários  
 O CLR chama `CreateTask` para solicitar que o host crie uma nova tarefa. O host retorna um ponteiro de interface para uma `IHostTask` instância. A tarefa retornada deve permanecer suspensa até ser explicitamente iniciada por uma chamada para `IHostTask::Start` .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICLRTask](iclrtask-interface.md)
- [Interface ICLRTaskManager](iclrtaskmanager-interface.md)
- [Interface IHostTask](ihosttask-interface.md)
- [Interface IHostTaskManager](ihosttaskmanager-interface.md)
