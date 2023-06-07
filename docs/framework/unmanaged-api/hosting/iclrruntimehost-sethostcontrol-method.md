---
title: Método ICLRRuntimeHost::SetHostControl
ms.date: 03/30/2017
api_name:
- ICLRRuntimeHost.SetHostControl
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRRuntimeHost::SetHostControl
helpviewer_keywords:
- SetHostControl method [.NET Framework hosting]
- ICLRRuntimeHost::SetHostControl method [.NET Framework hosting]
ms.assetid: 6136be87-e631-4756-81ed-74b66581bad4
topic_type:
- apiref
ms.openlocfilehash: 644b31ae8e8f0c51c08bcad57220a028406cfd3a
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504064"
---
# <a name="iclrruntimehostsethostcontrol-method"></a>Método ICLRRuntimeHost::SetHostControl
Define o ponteiro de interface que o Common Language Runtime (CLR) pode usar para obter a implementação do host da [interface IHostControl](ihostcontrol-interface.md).  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SetHostControl(  
    [in] IHostControl* pHostControl  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pHostControl`  
 no Um ponteiro de interface para a implementação do host da [interface IHostControl](ihostcontrol-interface.md).  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|`SetHostControl`retornado com êxito.|  
|HOST_E_CLRNOTAVAILABLE|O CLR não foi carregado em um processo ou o CLR está em um estado no qual não pode executar código gerenciado ou processar a chamada com êxito.|  
|HOST_E_TIMEOUT|A chamada atingiu o tempo limite.|  
|HOST_E_NOT_OWNER|O chamador não possui o bloqueio.|  
|HOST_E_ABANDONED|Um evento foi cancelado enquanto um thread ou uma fibra bloqueada estava esperando.|  
|E_FAIL|Ocorreu uma falha catastrófica desconhecida. Se um método retornar E_FAIL, o CLR não poderá mais ser usado no processo. As chamadas subsequentes para métodos de hospedagem retornam HOST_E_CLRNOTAVAILABLE.|  
|E_CLR_ALREADY_STARTED|O CLR já foi inicializado.|  
  
## <a name="remarks"></a>Comentários  
 Você deve chamar `SetHostControl` antes que o CLR seja inicializado, ou seja, antes de chamar o [método Start](iclrruntimehost-start-method.md) ou usar qualquer uma das [interfaces de metadados](../metadata/metadata-interfaces.md). É recomendável que você chame `SetHostControl` imediatamente depois de chamar a [função CorBindToCurrentRuntime](corbindtocurrentruntime-function.md) ou a [função CorBindToRuntimeEx](corbindtoruntimeex-function.md).  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICLRRuntimeHost](iclrruntimehost-interface.md)
- [Interface IHostControl](ihostcontrol-interface.md)
