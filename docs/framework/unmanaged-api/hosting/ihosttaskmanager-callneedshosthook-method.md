---
title: Método IHostTaskManager::CallNeedsHostHook
ms.date: 03/30/2017
api_name:
- IHostTaskManager.CallNeedsHostHook
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostTaskManager::CallNeedsHostHook
helpviewer_keywords:
- CallNeedsHostHook method [.NET Framework hosting]
- IHostTaskManager::CallNeedsHostHook method [.NET Framework hosting]
ms.assetid: b60f1f59-9825-4b57-961f-d2979518e6a7
topic_type:
- apiref
ms.openlocfilehash: 8cbac3b4ad25ba7dc01413f0c1b44541c43b3999
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503868"
---
# <a name="ihosttaskmanagercallneedshosthook-method"></a>Método IHostTaskManager::CallNeedsHostHook
Permite que o host especifique se o Common Language Runtime (CLR) pode embutir a chamada especificada para uma função não gerenciada.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT CallNeedsHostHook (  
    [in]  SIZE_T target,
    [out] BOOL   *pbCallNeedsHostHook  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `target`  
 no O endereço dentro do arquivo PE (executável portátil) mapeado da função não gerenciada que deve ser chamada.  
  
 `pbCallNeedsHostHook`  
 fora Um ponteiro para um valor booliano que indica se o host requer que a chamada seja conectada.  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|`CallNeedsHostHook`retornado com êxito.|  
|HOST_E_CLRNOTAVAILABLE|O CLR não foi carregado em um processo ou o CLR está em um estado no qual não pode executar código gerenciado ou processar a chamada com êxito.|  
|HOST_E_TIMEOUT|A chamada atingiu o tempo limite.|  
|HOST_E_NOT_OWNER|O chamador não possui o bloqueio.|  
|HOST_E_ABANDONED|Um evento foi cancelado enquanto um thread ou uma fibra bloqueada estava esperando.|  
|E_FAIL|Ocorreu uma falha catastrófica desconhecida. Quando um método retorna E_FAIL, o CLR não é mais utilizável no processo. As chamadas subsequentes para métodos de hospedagem retornam HOST_E_CLRNOTAVAILABLE.|  
  
## <a name="remarks"></a>Comentários  
 Para ajudar a otimizar a execução de código, o CLR executa uma análise de cada chamada de invocação de plataforma durante a compilação para determinar se a chamada pode ser embutida. `CallNeedsHostHook`permite que o host Substitua essa decisão exigindo que uma chamada para uma função não gerenciada seja conectada. Se o host exigir um gancho, o tempo de execução não embuti a chamada.  
  
 O host normalmente exigiria um gancho no qual ele deve ajustar um estado de ponto flutuante ou ao receber a notificação de que uma chamada está entrando em um estado em que o host não pode rastrear as solicitações do tempo de execução para a memória ou quaisquer bloqueios tirados. Quando o host exige que a chamada seja conectada, o tempo de execução notifica o host de transições de e para código gerenciado usando chamadas para [EnterRuntime](ihosttaskmanager-enterruntime-method.md), [LeaveRuntime](ihosttaskmanager-leaveruntime-method.md), [ReverseEnterRuntime](ihosttaskmanager-reverseenterruntime-method.md)e [ReverseLeaveRuntime](ihosttaskmanager-reverseleaveruntime-method.md).  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICLRTask](iclrtask-interface.md)
- [Interface ICLRTaskManager](iclrtaskmanager-interface.md)
- [Interface IHostTask](ihosttask-interface.md)
- [Interface IHostTaskManager](ihosttaskmanager-interface.md)
