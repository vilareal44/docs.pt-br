---
title: Método IHostIoCompletionManager::SetMaxThreads
ms.date: 03/30/2017
api_name:
- IHostIoCompletionManager.SetMaxThreads
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostIoCompletionManager::SetMaxThreads
helpviewer_keywords:
- IHostIoCompletionManager::SetMaxThreads method [.NET Framework hosting]
- SetMaxThreads method, IHostIoCompletionManager interface [.NET Framework hosting]
ms.assetid: ebad4f40-d9f1-4dc6-9b27-a89c9eb3926f
topic_type:
- apiref
ms.openlocfilehash: 55727903a7f3c798e7472de6de5249de98af7ae7
ms.sourcegitcommit: d223616e7e6fe2139079052e6fcbe25413fb9900
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83804671"
---
# <a name="ihostiocompletionmanagersetmaxthreads-method"></a>Método IHostIoCompletionManager::SetMaxThreads
Define o número máximo de threads que o host aloca para atender às solicitações de e/s.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SetMaxThreads (  
    [in] DWORD dwMaxIoCompletionThreads  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `dwMaxIoCompletionThreads`  
 no O número máximo de threads a serem alocados para solicitações de e/s.  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|`SetMaxThreads`retornado com êxito.|  
|HOST_E_CLRNOTAVAILABLE|O Common Language Runtime (CLR) não foi carregado em um processo ou o CLR está em um estado no qual não pode executar código gerenciado ou processar a chamada com êxito.|  
|HOST_E_TIMEOUT|A chamada atingiu o tempo limite.|  
|HOST_E_NOT_OWNER|O chamador não possui o bloqueio.|  
|HOST_E_ABANDONED|Um evento foi cancelado enquanto um thread ou uma fibra bloqueada estava esperando.|  
|E_FAIL|Ocorreu uma falha catastrófica desconhecida. Quando um método retorna E_FAIL, o CLR não é mais utilizável no processo. As chamadas subsequentes para métodos de hospedagem retornam HOST_E_CLRNOTAVAILABLE.|  
|E_NOTIMPL|O host não fornece uma implementação de `SetMaxThreads` .|  
  
## <a name="remarks"></a>Comentários  
 `SetMaxThreads`fornece ao CLR uma oportunidade para definir o número máximo de threads que estão disponíveis para solicitações de serviço em portas de e/s. Um host pode precisar de controle exclusivo sobre o tamanho do pool de threads, por motivos como implementação, desempenho ou escalabilidade. Por esse motivo, o host não precisa implementar `SetMaxThreads` . Nesse caso, um host deve retornar E_NOTIMPL desse método.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICLRIoCompletionManager](iclriocompletionmanager-interface.md)
- [Interface IHostIoCompletionManager](ihostiocompletionmanager-interface.md)
