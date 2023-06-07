---
title: Método IHostTaskManager::EnterRuntime
ms.date: 03/30/2017
api_name:
- IHostTaskManager.EnterRuntime
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostTaskManager::EnterRuntime
helpviewer_keywords:
- IHostTaskManager::EnterRuntime method [.NET Framework hosting]
- EnterRuntime method [.NET Framework hosting]
ms.assetid: 1aa7a4b1-636a-4f5e-b834-b406d72f7120
topic_type:
- apiref
ms.openlocfilehash: b255a1d35aa32975c879aac00f7d4edb8ea88d3b
ms.sourcegitcommit: e5772b3ddcc114c80b4c9767ffdb3f6c7fad8f05
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83842030"
---
# <a name="ihosttaskmanagerenterruntime-method"></a>Método IHostTaskManager::EnterRuntime
Notifica o host de que uma chamada para um método não gerenciado, como um método de invocação de plataforma, está retornando o controle de execução para o Common Language Runtime (CLR).  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT EnterRuntime ();  
```  
  
## <a name="return-value"></a>Valor retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|`EnterRuntime`retornado com êxito.|  
|HOST_E_CLRNOTAVAILABLE|O CLR não foi carregado em um processo ou o CLR está em um estado no qual não pode executar código gerenciado ou processar a chamada com êxito.|  
|HOST_E_TIMEOUT|A chamada atingiu o tempo limite.|  
|HOST_E_NOT_OWNER|O chamador não possui o bloqueio.|  
|HOST_E_ABANDONED|Um evento foi cancelado enquanto um thread ou uma fibra bloqueada estava esperando.|  
|E_FAIL|Ocorreu uma falha catastrófica desconhecida. Quando um método retorna E_FAIL, o CLR não é mais utilizável no processo. As chamadas subsequentes para métodos de hospedagem retornam HOST_E_CLRNOTAVAILABLE.|  
|E_OUTOFMEMORY|Não havia memória suficiente disponível para concluir a alocação solicitada.|  
  
## <a name="remarks"></a>Comentários  
 `EnterRuntime`é chamado para notificar o host de que uma função não gerenciada, para a qual uma chamada anterior para o método [LeaveRuntime](ihosttaskmanager-leaveruntime-method.md) foi feita, concluiu a execução e está retornando o controle de execução para o tempo de execução.  
  
> [!NOTE]
> [ReverseEnterRuntime](ihosttaskmanager-reverseenterruntime-method.md) é chamado para notificar o host de que uma função não gerenciada, para a qual uma chamada anterior `LeaveRuntime` foi feita, está fazendo uma chamada para código gerenciado.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Consulte também

- [Interoperabilidade COM avançada](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bd9cdfyx(v=vs.100))
- [Como: chamar DLLs nativas do código gerenciado usando PInvoke](/cpp/dotnet/how-to-call-native-dlls-from-managed-code-using-pinvoke)
- [Interface ICLRTask](iclrtask-interface.md)
- [Interface ICLRTaskManager](iclrtaskmanager-interface.md)
- [Interface IHostTask](ihosttask-interface.md)
- [Interface IHostTaskManager](ihosttaskmanager-interface.md)
- [Método LeaveRuntime](ihosttaskmanager-leaveruntime-method.md)
