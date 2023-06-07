---
title: Método IHostAutoEvent::Wait
ms.date: 03/30/2017
api_name:
- IHostAutoEvent.Wait
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostAutoEvent::Wait
helpviewer_keywords:
- Wait method, IHostAutoEvent interface [.NET Framework hosting]
- IHostAutoEvent::Wait method [.NET Framework hosting]
ms.assetid: 535d51c5-9112-401b-8c36-85f35d7ee609
topic_type:
- apiref
ms.openlocfilehash: 7baabafc61e14d127ff3f0cdb7453be6f1a2abeb
ms.sourcegitcommit: d223616e7e6fe2139079052e6fcbe25413fb9900
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83804968"
---
# <a name="ihostautoeventwait-method"></a>Método IHostAutoEvent::Wait
Faz com que a instância de [IHostAutoEvent](ihostautoevent-interface.md) atual aguarde até que ela seja propriedade ou um período de tempo especificado decorre.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT Wait (  
    [in] DWORD dwMilliseconds,  
    [in] DWORD option  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `dwMilliseconds`  
 no O número de milissegundos que a `IHostAutoEvent` instância atual deve aguardar antes de retornar, se nenhum thread ou fibra assumir a propriedade.  
  
 `option`  
 no Um dos valores de [WAIT_OPTION](wait-option-enumeration.md) , especificando a ação que o host deve executar se essa operação bloquear.  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|`Wait`retornado com êxito.|  
|HOST_E_CLRNOTAVAILABLE|O Common Language Runtime (CLR) não foi carregado em um processo ou o CLR está em um estado no qual não pode executar código gerenciado ou processar a chamada com êxito.|  
|HOST_E_TIMEOUT|A chamada atingiu o tempo limite.|  
|HOST_E_NOT_OWNER|O chamador não possui o bloqueio.|  
|HOST_E_ABANDONED|Um evento foi cancelado enquanto um thread ou uma fibra bloqueada estava esperando.|  
|E_FAIL|Ocorreu uma falha catastrófica desconhecida. Quando um método retorna E_FAIL, o CLR não é mais utilizável no processo. As chamadas subsequentes para métodos de hospedagem retornam HOST_E_CLRNOTAVAILABLE.|  
|HOST_E_DEADLOCK|O host detectou um deadlock durante o intervalo de espera e escolheu o evento representado pela `IHostAutoEvent` instância atual como a vítima do deadlock.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICLRSyncManager](iclrsyncmanager-interface.md)
- [Interface IHostAutoEvent](ihostautoevent-interface.md)
- [Interface IHostManualEvent](ihostmanualevent-interface.md)
- [Interface IHostSyncManager](ihostsyncmanager-interface.md)
