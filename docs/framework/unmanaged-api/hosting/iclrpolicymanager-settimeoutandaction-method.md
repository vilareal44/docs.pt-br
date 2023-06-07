---
title: Método ICLRPolicyManager::SetTimeoutAndAction
ms.date: 03/30/2017
api_name:
- ICLRPolicyManager.SetTimeoutAndAction
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRPolicyManager::SetTimeoutAndAction
helpviewer_keywords:
- ICLRPolicyManager::SetTimeoutAndAction method [.NET Framework hosting]
- SetTimeoutAndAction method [.NET Framework hosting]
ms.assetid: 60454f91-d855-4ddf-bb6d-60a02f5eabab
topic_type:
- apiref
ms.openlocfilehash: 02e836601be72d54f561e077cd3c466470bafb25
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504089"
---
# <a name="iclrpolicymanagersettimeoutandaction-method"></a>Método ICLRPolicyManager::SetTimeoutAndAction
Define um valor de tempo limite para a operação especificada e especifica a ação de política que o Common Language Runtime (CLR) deve executar quando a operação ocorre.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SetTimeoutAndAction (  
    [in] EClrOperation operation,  
    [in] DWORD dwMilliseconds,  
    [in] EPolicyAction action  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `operation`  
 no Um dos valores de [EClrOperation](eclroperation-enumeration.md) , que indica a operação para a qual definir o tempo limite e a política `action` . Os seguintes valores têm suporte:  
  
- OPR_AppDomainUnload  
  
- OPR_ProcessExit  
  
- OPR_ThreadRudeAbortInCriticalRegion  
  
- OPR_ThreadRudeAbortInNonCriticalRegion  
  
 `dwMilliseconds`  
 no O novo valor de tempo limite, em milissegundos. Um valor de infinito causa `operation` a expiração do tempo limite.  
  
 `action`  
 no Um dos valores de [EPolicyAction](epolicyaction-enumeration.md) , indicando a ação de política que o CLR deve executar quando `operation` ocorre.  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|`SetTimeoutAndAction`retornado com êxito.|  
|HOST_E_CLRNOTAVAILABLE|O CLR não foi carregado em um processo ou o CLR está em um estado no qual não pode executar código gerenciado ou processar a chamada com êxito.|  
|HOST_E_TIMEOUT|A chamada atingiu o tempo limite.|  
|HOST_E_NOT_OWNER|O chamador não possui o bloqueio.|  
|HOST_E_ABANDONED|Um evento foi cancelado enquanto um thread ou uma fibra bloqueada estava esperando.|  
|E_FAIL|Ocorreu uma falha catastrófica desconhecida. Depois que um método retorna E_FAIL, o CLR não pode mais ser usado no processo. As chamadas subsequentes para métodos de hospedagem retornam HOST_E_CLRNOTAVAILABLE.|  
|E_INVALIDARG|Não é possível definir um tempo limite para o especificado `operation` ou um valor inválido foi fornecido para `action` .|  
  
## <a name="remarks"></a>Comentários  
 `SetTimeoutAndAction`encapsula os recursos dos métodos [ICLRPolicyManager:: SetTimeout](iclrpolicymanager-settimeout-method.md) e [ICLRPolicyManager:: SetActionOnTimeout](iclrpolicymanager-setactionontimeout-method.md) e pode ser chamado no lugar de chamadas sequenciais para esses dois métodos.  
  
> [!IMPORTANT]
> Nem todos os valores de ação de política podem ser especificados como o comportamento de tempo limite para operações de CLR. Consulte as seções comentários dos tópicos para esses dois métodos para obter os valores válidos.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Enumeração EClrOperation](eclroperation-enumeration.md)
- [Enumeração EPolicyAction](epolicyaction-enumeration.md)
- [Interface ICLRPolicyManager](iclrpolicymanager-interface.md)
- [Método SetActionOnTimeout](iclrpolicymanager-setactionontimeout-method.md)
- [ICLRPolicyManager:: SetTimeoutAndAction](iclrpolicymanager-settimeoutandaction-method.md)
