---
title: Método IHostTaskManager::SetLocale
ms.date: 03/30/2017
api_name:
- IHostTaskManager.SetLocale
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostTaskManager::SetLocale
helpviewer_keywords:
- SetLocale method, IHostTaskManager interface [.NET Framework hosting]
- IHostTaskManager::SetLocale method [.NET Framework hosting]
ms.assetid: 747ee407-ee8c-484d-9583-25089236d2d1
topic_type:
- apiref
ms.openlocfilehash: 841827017262b731fd5e6f6bd0b5862fecaf2744
ms.sourcegitcommit: e5772b3ddcc114c80b4c9767ffdb3f6c7fad8f05
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83841718"
---
# <a name="ihosttaskmanagersetlocale-method"></a>Método IHostTaskManager::SetLocale
Notifica o host de que o Common Language Runtime (CLR) alterou a localidade, ou cultura, na tarefa em execução no momento.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SetLocale (  
    [in] LCID lcid  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `lcid`  
 no O valor do identificador de localidade que mapeia para a cultura geográfica e a linguagem recém atribuídas.  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|`SetLocale`retornado com êxito.|  
|HOST_E_CLRNOTAVAILABLE|O CLR não foi carregado em um processo ou o CLR está em um estado no qual não pode executar código gerenciado ou processar a chamada com êxito.|  
|HOST_E_TIMEOUT|A chamada atingiu o tempo limite.|  
|HOST_E_NOT_OWNER|O chamador não possui o bloqueio.|  
|HOST_E_ABANDONED|Um evento foi cancelado enquanto um thread ou uma fibra bloqueada estava esperando.|  
|E_FAIL|Ocorreu uma falha catastrófica desconhecida. Quando um método retorna E_FAIL, o CLR não é mais utilizável no processo. As chamadas subsequentes para métodos de hospedagem retornam HOST_E_CLRNOTAVAILABLE.|  
|E_NOTIMPL|O host não permite que o código de usuário gerenciado modifique a localidade.|  
  
## <a name="remarks"></a>Comentários  
 O tempo de execução chama `SetLocale` quando o valor da <xref:System.Threading.Thread.CurrentCulture%2A?displayProperty=nameWithType> propriedade é alterado por código gerenciado. Esse método fornece uma oportunidade para o host executar qualquer mecanismo que ele possa ter para a sincronização de localidades. Se um host não permitir que a localidade seja alterada do código gerenciado ou não implementar um mecanismo para sincronizar as localidades, ele deverá retornar E_NOTIMPL desse método.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Consulte também

- [Interface ICLRTask](iclrtask-interface.md)
- [Interface ICLRTaskManager](iclrtaskmanager-interface.md)
- [Interface IHostTask](ihosttask-interface.md)
- [Interface IHostTaskManager](ihosttaskmanager-interface.md)
- [Método SetUILocale](ihosttaskmanager-setuilocale-method.md)
