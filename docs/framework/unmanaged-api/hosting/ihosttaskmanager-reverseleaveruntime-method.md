---
title: Método IHostTaskManager::ReverseLeaveRuntime
ms.date: 03/30/2017
api_name:
- IHostTaskManager.ReverseLeaveRuntime
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostTaskManager::ReverseLeaveRuntime
helpviewer_keywords:
- IHostTaskManager::ReverseLeaveRuntime method [.NET Framework hosting]
- ReverseLeaveRuntime method [.NET Framework hosting]
ms.assetid: 4837d398-16a1-4e32-902c-022cd1aad3ca
topic_type:
- apiref
ms.openlocfilehash: d328afcba9761f686dd38bdb2dd651994faaac2a
ms.sourcegitcommit: e5772b3ddcc114c80b4c9767ffdb3f6c7fad8f05
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83841848"
---
# <a name="ihosttaskmanagerreverseleaveruntime-method"></a>Método IHostTaskManager::ReverseLeaveRuntime
Notifica o host que o controle está deixando o Common Language Runtime (CLR) e inserindo uma função não gerenciada que, por sua vez, foi chamada a partir do código gerenciado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT ReverseLeaveRuntime ();  
```  
  
## <a name="return-value"></a>Valor retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|`ReverseLeaveRuntime`retornado com êxito.|  
|HOST_E_CLRNOTAVAILABLE|O CLR não foi carregado em um processo ou o CLR está em um estado no qual não pode executar código gerenciado ou processar a chamada com êxito.|  
|HOST_E_TIMEOUT|A chamada atingiu o tempo limite.|  
|HOST_E_NOT_OWNER|O chamador não possui o bloqueio.|  
|HOST_E_ABANDONED|Um evento foi cancelado enquanto um thread ou uma fibra bloqueada estava esperando.|  
|E_FAIL|Ocorreu uma falha catastrófica desconhecida. Quando um método retorna E_FAIL, o CLR não é mais utilizável no processo. As chamadas subsequentes para métodos de hospedagem retornam HOST_E_CLRNOTAVAILABLE.|  
|E_OUTOFMEMORY|Não há memória suficiente disponível para concluir a alocação de recursos solicitada.|  
  
## <a name="remarks"></a>Comentários  
 O CLR chama `ReverseLeaveRuntime` para informar ao host que a tarefa em execução no momento está retornando o controle a uma função não gerenciada que, por sua vez, foi chamada a partir de código gerenciado por meio de invocação de plataforma. Cada chamada para `ReverseLeaveRuntime` corresponde a uma chamada correspondente para [ReverseEnterRuntime](ihosttaskmanager-reverseenterruntime-method.md).  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Consulte também

- [Método CallNeedsHostHook](ihosttaskmanager-callneedshosthook-method.md)
- [Método EnterRuntime](ihosttaskmanager-enterruntime-method.md)
- [Interface ICLRTask](iclrtask-interface.md)
- [Interface ICLRTaskManager](iclrtaskmanager-interface.md)
- [Interface IHostTask](ihosttask-interface.md)
- [Interface IHostTaskManager](ihosttaskmanager-interface.md)
- [Método LeaveRuntime](ihosttaskmanager-leaveruntime-method.md)
- [Um olhar detalhado sobre invocação de plataforma](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/0h9e9t7d(v=vs.100))
