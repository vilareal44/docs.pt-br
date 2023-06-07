---
title: Método ICLRDebugManager::BeginConnection
ms.date: 03/30/2017
api_name:
- ICLRDebugManager.BeginConnection
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRDebugManager::BeginConnection
helpviewer_keywords:
- ICLRDebugManager::BeginConnection method [.NET Framework hosting]
- BeginConnection method [.NET Framework hosting]
ms.assetid: bdd98146-ff4d-4150-a264-a4c1a32d31f3
topic_type:
- apiref
ms.openlocfilehash: 98e4efe149cab1b822c9993e4df28806f773c61d
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504245"
---
# <a name="iclrdebugmanagerbeginconnection-method"></a>Método ICLRDebugManager::BeginConnection
Estabelece uma nova conexão entre o host e o depurador para associar uma lista de tarefas com um identificador e um nome amigável.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT BeginConnection (  
    [in] CONNID dwConnectionId,  
    [in, string] wchar_t* szConnectionName  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `dwConnectionId`  
 no Um identificador a ser associado à lista de tarefas de Common Language Runtime (CLR).  
  
 `szConnectionName`  
 no Um nome amigável para associar à lista de tarefas do CLR.  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|`BeginConnection`retornado com êxito.|  
|HOST_E_CLRNOTAVAILABLE|O CLR não foi carregado em um processo ou o CLR está em um estado no qual não pode executar código gerenciado ou processar a chamada com êxito.|  
|HOST_E_TIMEOUT|A chamada atingiu o tempo limite.|  
|HOST_E_NOT_OWNER|O chamador não possui o bloqueio.|  
|HOST_E_ABANDONED|Um evento foi cancelado enquanto um thread ou uma fibra bloqueada estava esperando.|  
|E_FAIL|Ocorreu uma falha catastrófica desconhecida. Depois que um método retorna E_FAIL, o CLR não pode mais ser usado no processo. As chamadas subsequentes para métodos de hospedagem retornam HOST_E_CLRNOTAVAILABLE.|  
|E_INVALIDARG|`dwConnectionId`era zero, ou `BeginConnection` já foi chamado usando este `dwConnectionId` valor, ou `szConnectionName` era nulo.|  
|E_OUTOFMEMORY|Não foi possível alocar memória suficiente para manter a lista de tarefas associadas a esta conexão.|  
  
## <a name="remarks"></a>Comentários  
 O [ICLRDebugManager](iclrdebugmanager-interface.md) fornece três métodos, `BeginConnection` , [SetConnectionTasks](iclrdebugmanager-setconnectiontasks-method.md)e [EndConnect](iclrdebugmanager-endconnection-method.md), para associar listas de tarefas com identificadores e nomes amigáveis.  
  
> [!IMPORTANT]
> Esses três métodos devem ser chamados em uma ordem específica para cada conjunto de tarefas. `BeginConnection`é chamado primeiro para estabelecer uma nova conexão. `SetConnectionTasks`é chamado ao lado de fornecer o conjunto de tarefas a ser associado a essa conexão. `EndConnection`é chamado por último para remover a associação entre a lista de tarefas e o identificador e o nome amigável. No entanto, as chamadas para diferentes conexões podem ser aninhadas.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICLRControl](iclrcontrol-interface.md)
- [Interface ICLRDebugManager](iclrdebugmanager-interface.md)
- [Método EndConnection](iclrdebugmanager-endconnection-method.md)
- [Método SetConnectionTasks](iclrdebugmanager-setconnectiontasks-method.md)
- [Interface IHostControl](ihostcontrol-interface.md)
