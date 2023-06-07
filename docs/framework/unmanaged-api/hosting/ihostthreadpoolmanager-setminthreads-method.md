---
title: Método IHostThreadPoolManager::SetMinThreads
ms.date: 03/30/2017
api_name:
- IHostThreadPoolManager.SetMinThreads
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostThreadPoolManager::SetMinThreads
helpviewer_keywords:
- SetMinThreads method, IHostThreadPoolManager interface [.NET Framework hosting]
- IHostThreadPoolManager::SetMinThreads method [.NET Framework hosting]
ms.assetid: 10409db9-9fd2-4e4d-b8cd-cf6fec0afaa2
topic_type:
- apiref
ms.openlocfilehash: c5b150b161acba3820ced367049f08153dd091aa
ms.sourcegitcommit: e5772b3ddcc114c80b4c9767ffdb3f6c7fad8f05
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83842420"
---
# <a name="ihostthreadpoolmanagersetminthreads-method"></a>Método IHostThreadPoolManager::SetMinThreads
Define o número mínimo de threads ociosos que o host deve manter em antecipação de solicitações.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SetMinThreads (  
    [in] DWORD MinThreads  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `MinThreads`  
 no O novo número mínimo de threads que o host deve manter.  
  
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
 Não é necessário um host para fornecer uma implementação do `SetMinThreads` . Nesse caso, ele deve retornar um valor HRESULT de E_NOTIMPL.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Threading.ThreadPool.SetMinThreads%2A>
- <xref:System.Threading.ThreadPool>
- [Método GetMinThreads](ihostthreadpoolmanager-getminthreads-method.md)
- [Método SetMaxThreads](ihostthreadpoolmanager-setmaxthreads-method.md)
- [Interface IHostThreadPoolManager](ihostthreadpoolmanager-interface.md)
