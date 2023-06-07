---
title: Método IHostTask::SetPriority
ms.date: 03/30/2017
api_name:
- IHostTask.SetPriority
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostTask::SetPriority
helpviewer_keywords:
- IHostTask::SetPriority method [.NET Framework hosting]
- SetPriority method [.NET Framework hosting]
ms.assetid: cd8c379b-c7a0-434f-8e23-899bd26be75d
topic_type:
- apiref
ms.openlocfilehash: ac3a8479cdf05e55885bd55d4e4fb8e6e47686f9
ms.sourcegitcommit: e5772b3ddcc114c80b4c9767ffdb3f6c7fad8f05
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83842392"
---
# <a name="ihosttasksetpriority-method"></a>Método IHostTask::SetPriority
Solicita que o host ajuste o nível de prioridade de thread para a tarefa representada pela instância de [IHostTask](ihosttask-interface.md) atual.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SetPriority (  
    [in] int newPriority  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `newPriority`  
 no Um inteiro que representa o valor de prioridade de thread solicitado para a tarefa representada pela `IHostTask` instância atual.  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|`SetPriority`retornado com êxito.|  
|HOST_E_CLRNOTAVAILABLE|O Common Language Runtime (CLR) não foi carregado em um processo ou o CLR está em um estado no qual não pode executar código gerenciado ou processar a chamada com êxito.|  
|HOST_E_TIMEOUT|A chamada atingiu o tempo limite.|  
|HOST_E_NOT_OWNER|O chamador não possui o bloqueio.|  
|HOST_E_ABANDONED|Um evento foi cancelado enquanto um thread ou uma fibra bloqueada estava esperando.|  
|E_FAIL|Ocorreu uma falha catastrófica desconhecida. Quando um método retorna E_FAIL, o CLR não é mais utilizável no processo. As chamadas subsequentes para métodos de hospedagem retornam HOST_E_CLRNOTAVAILABLE.|  
  
## <a name="remarks"></a>Comentários  
 Os threads recebem o tempo de processamento usando um sistema Round Robin que é parcialmente baseado no nível de prioridade de um thread. `SetPriority`permite que o CLR defina o nível de prioridade do thread para a tarefa atual. Há `newPriority` suporte para os valores a seguir.  
  
- THREAD_PRIORITY_ABOVE_NORMAL  
  
- THREAD_PRIORITY_BELOW_NORMAL  
  
- THREAD_PRIORITY_HIGHEST  
  
- THREAD_PRIORITY_IDLE  
  
- THREAD_PRIORITY_LOWEST  
  
- THREAD_PRIORITY_NORMAL  
  
- THREAD_PRIORITY_TIME_CRITICAL  
  
 O CLR chama `SetPriority` quando o valor de <xref:System.Threading.Thread.Priority%2A?displayProperty=nameWithType> é modificado pelo código do usuário. Um host pode definir seus próprios algoritmos para atribuição de prioridade de thread e é livre para ignorar essa solicitação.  
  
> [!NOTE]
> `SetPriority`não relata se o nível de prioridade do thread foi alterado. Chame [IHostTask:: Getanteriory](ihosttask-getpriority-method.md) para determinar o valor do nível de prioridade de thread da tarefa.  
  
 Os valores de nível de prioridade de thread são definidos pela função do Win32 `SetThreadPriority` . Para obter mais informações sobre prioridade de thread, consulte a documentação da plataforma Windows.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Threading.Thread>
- [Interface ICLRTask](iclrtask-interface.md)
- [Interface ICLRTaskManager](iclrtaskmanager-interface.md)
- [Interface IHostTask](ihosttask-interface.md)
- [Método GetPriority](ihosttask-getpriority-method.md)
- [Interface IHostTaskManager](ihosttaskmanager-interface.md)
