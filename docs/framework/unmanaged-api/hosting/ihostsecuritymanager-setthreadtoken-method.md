---
title: Método IHostSecurityManager::SetThreadToken
ms.date: 03/30/2017
api_name:
- IHostSecurityManager.SetThreadToken
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostSecurityManager::SetThreadToken
helpviewer_keywords:
- SetThreadToken method [.NET Framework hosting]
- IHostSecurityManager::SetThreadToken method [.NET Framework hosting]
ms.assetid: e951c345-8a86-4587-911b-a1a57bc6428a
topic_type:
- apiref
ms.openlocfilehash: 7891ddc5085eedd2a9010023f119d08f101e2fa3
ms.sourcegitcommit: d223616e7e6fe2139079052e6fcbe25413fb9900
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83803772"
---
# <a name="ihostsecuritymanagersetthreadtoken-method"></a>Método IHostSecurityManager::SetThreadToken
Define um identificador para o thread em execução no momento.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SetThreadToken (  
    [in] HANDLE hToken  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `hToken`  
 no Um identificador para o token a ser definido para o thread em execução no momento.  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|`SetThreadToken`retornado com êxito.|  
|HOST_E_CLRNOTAVAILABLE|O Common Language Runtime (CLR) não foi carregado em um processo ou o CLR está em um estado no qual não pode executar código gerenciado ou processar a chamada com êxito.|  
|HOST_E_TIMEOUT|A chamada atingiu o tempo limite.|  
|HOST_E_NOT_OWNER|O chamador não possui o bloqueio.|  
|HOST_E_ABANDONED|Um evento foi cancelado enquanto um thread ou uma fibra bloqueada estava esperando.|  
|E_FAIL|Ocorreu uma falha catastrófica desconhecida. Quando um método retorna E_FAIL, o CLR não é mais utilizável no processo. As chamadas subsequentes para métodos de hospedagem retornam HOST_E_CLRNOTAVAILABLE.|  
  
## <a name="remarks"></a>Comentários  
 `IHostSecurityManager::SetThreadToken`comporta-se da mesma forma que a função Win32 correspondente do mesmo nome, exceto que a função do Win32 permite que o chamador passe um identificador para um thread arbitrário, enquanto `IHostSecurityManager::SetThreadToken` pode associar um token somente com o thread em execução no momento.  
  
 O `HANDLE` tipo não é compatível COM com; ou seja, seu tamanho é específico para um sistema operacional e requer marshaling personalizado. Portanto, esse token é para uso apenas dentro do processo, entre o CLR e o host.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IHostSecurityManager](ihostsecuritymanager-interface.md)
- [Interface IHostThreadPoolManager](ihostthreadpoolmanager-interface.md)
