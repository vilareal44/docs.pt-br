---
title: Método IHostIoCompletionManager::SetMinThreads
ms.date: 03/30/2017
api_name:
- IHostIoCompletionManager.SetMinThreads
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostIoCompletionManager::SetMinThreads
helpviewer_keywords:
- SetMinThreads method, IHostIoCompletionManager interface [.NET Framework hosting]
- IHostIoCompletionManager::SetMinThreads method [.NET Framework hosting]
ms.assetid: dea34b81-8d2b-4cc3-8696-0ad4291d8a92
topic_type:
- apiref
ms.openlocfilehash: 29a2fc5652badcc00378192debccba0356f5339e
ms.sourcegitcommit: d223616e7e6fe2139079052e6fcbe25413fb9900
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83804664"
---
# <a name="ihostiocompletionmanagersetminthreads-method"></a>Método IHostIoCompletionManager::SetMinThreads
Define o número mínimo de threads que o host deve alocar para a conclusão de e/s.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SetMinThreads (  
    [in] DWORD dwMinIoCompletionThreads  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `dwMinIoCompletionThreads`  
 no O número mínimo de threads de conclusão de e/s que o host deve criar.  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|`SetMinThreads`retornado com êxito.|  
|HOST_E_CLRNOTAVAILABLE|O Common Language Runtime (CLR) não foi carregado em um processo ou o CLR está em um estado no qual não pode executar código gerenciado ou processar a chamada com êxito.|  
|HOST_E_TIMEOUT|A chamada atingiu o tempo limite.|  
|HOST_E_NOT_OWNER|O chamador não possui o bloqueio.|  
|HOST_E_ABANDONED|Um evento foi cancelado enquanto um thread ou uma fibra bloqueada estava esperando.|  
|E_FAIL|Ocorreu uma falha catastrófica desconhecida. Quando um método retorna E_FAIL, o CLR não é mais utilizável no processo. As chamadas subsequentes para métodos de hospedagem retornam HOST_E_CLRNOTAVAILABLE.|  
|E_NOTIMPL|O host não fornece uma implementação de `SetMinThreads` .|  
  
## <a name="remarks"></a>Comentários  
 Um host pode querer um controle exclusivo sobre o número de threads que podem ser alocados para processar solicitações de e/s, por motivos como implementação, desempenho ou escalabilidade. Por esse motivo, o host não precisa implementar `SetMinThreads` . Nesse caso, o host deve retornar E_NOTIMPL desse método.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICLRIoCompletionManager](iclriocompletionmanager-interface.md)
- [Método SetMaxThreads](ihostiocompletionmanager-setmaxthreads-method.md)
- [Interface IHostIoCompletionManager](ihostiocompletionmanager-interface.md)
