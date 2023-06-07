---
title: Método ICLRPolicyManager::SetActionOnFailure
ms.date: 03/30/2017
api_name:
- ICLRPolicyManager.SetActionOnFailure
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRPolicyManager::SetActionOnFailure
helpviewer_keywords:
- SetActionOnFailure method [.NET Framework hosting]
- ICLRPolicyManager::SetActionOnFailure method [.NET Framework hosting]
ms.assetid: 4664033f-db97-4388-b988-2ec470796e58
topic_type:
- apiref
ms.openlocfilehash: 727cd82226b9a59c4879ffea5e87f93dd5fe38c9
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504102"
---
# <a name="iclrpolicymanagersetactiononfailure-method"></a>Método ICLRPolicyManager::SetActionOnFailure
Especifica a ação de política que o Common Language Runtime (CLR) deve executar quando a falha especificada ocorrer.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SetActionOnFailure (  
    [in] EClrFailure   failure,  
    [in] EPolicyAction action  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `failure`  
 no Um dos valores de [EClrFailure](eclrfailure-enumeration.md) , indicando o tipo de falha para a qual executar a ação.  
  
 `action`  
 no Um dos valores de [EPolicyAction](epolicyaction-enumeration.md) , indicando a ação a ser executada quando ocorrer uma falha. Para obter uma lista de valores com suporte, consulte a seção comentários.  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|`SetActionOnFailure`retornado com êxito.|  
|HOST_E_CLRNOTAVAILABLE|O CLR não foi carregado em um processo ou o CLR está em um estado no qual não pode executar código gerenciado ou processar a chamada com êxito.|  
|HOST_E_TIMEOUT|A chamada atingiu o tempo limite.|  
|HOST_E_NOT_OWNER|O chamador não possui o bloqueio.|  
|HOST_E_ABANDONED|Um evento foi cancelado enquanto um thread ou uma fibra bloqueada estava esperando.|  
|E_FAIL|Ocorreu uma falha catastrófica desconhecida. Depois que um método retorna E_FAIL, o CLR não pode mais ser usado no processo. As chamadas subsequentes para métodos de hospedagem retornam HOST_E_CLRNOTAVAILABLE.|  
|E_INVALIDARG|Uma ação de política não pode ser definida para a operação especificada ou uma ação de política inválida foi especificada para a operação.|  
  
## <a name="remarks"></a>Comentários  
 Por padrão, o CLR gera uma exceção quando ele falha ao alocar um recurso, como memória. `SetActionOnFailure`permite que o host Substitua esse comportamento especificando a ação da política a ser tomada após a falha. A tabela a seguir mostra as combinações de valores [EClrFailure](eclrfailure-enumeration.md) e [EPolicyAction](epolicyaction-enumeration.md) com suporte. (O prefixo de FAIL_ é omitido dos valores de [EClrFailure](eclrfailure-enumeration.md) .)  
  
||NonCriticalResource|CriticalResource|FatalRuntime|OrphanedLock|StackOverflow|AccessViolation|CodeContract|  
|-|-------------------------|----------------------|------------------|------------------|-------------------|---------------------|------------------|  
|`eNoAction`|X|X||||N/D||  
|eThrowException|X|X||||N/D||  
|`eAbortThread`|X|X||||N/D|X|  
|`eRudeAbortThread`|X|X||||N/D|X|  
|`eUnloadAppDomain`|X|X||X||N/D|X|  
|`eRudeUnloadAppDomain`|X|X||X|X|N/D|X|  
|`eExitProcess`|X|X||X|X|N/D|X|  
|eFastExitProcess|X|X||X|X|N/D||  
|`eRudeExitProcess`|X|X|X|X|X|N/D||  
|`eDisableRuntime`|X|X|X|X|X|N/D||  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Enumeração EClrFailure](eclrfailure-enumeration.md)
- [Enumeração EPolicyAction](epolicyaction-enumeration.md)
- [Interface ICLRControl](iclrcontrol-interface.md)
- [Interface ICLRPolicyManager](iclrpolicymanager-interface.md)
