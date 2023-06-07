---
title: Método IHostSecurityManager::SetSecurityContext
ms.date: 03/30/2017
api_name:
- IHostSecurityManager.SetSecurityContext
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostSecurityManager::SetSecurityContext
helpviewer_keywords:
- SetSecurityContext method [.NET Framework hosting]
- IHostSecurityManager::SetSecurityContext method [.NET Framework hosting]
ms.assetid: e4372384-ee69-48d7-97e0-8fab7866597a
topic_type:
- apiref
ms.openlocfilehash: 6a6b4d0351e22026dc873aad8281d0259d871a14
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501476"
---
# <a name="ihostsecuritymanagersetsecuritycontext-method"></a>Método IHostSecurityManager::SetSecurityContext
Define o contexto de segurança do thread atualmente em execução.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SetSecurityContext (  
    [in]  EContextType eContextType,  
    [out] IHostSecurityContext** ppSecurityContext  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `eContextType`  
 no Um dos valores de [EContextType](econtexttype-enumeration.md) , indicando que tipo de contexto o Common Language Runtime (CLR) está colocando no host.  
  
 `ppSecurityContext`  
 fora Um ponteiro para o endereço de um novo objeto [IHostSecurityContext](ihostsecuritycontext-interface.md) .  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|`SetSecurityContext`retornado com êxito.|  
|HOST_E_CLRNOTAVAILABLE|O CLR não foi carregado em um processo ou o CLR está em um estado no qual não pode executar código gerenciado ou processar a chamada com êxito.|  
|HOST_E_TIMEOUT|A chamada atingiu o tempo limite.|  
|HOST_E_NOT_OWNER|O chamador não possui o bloqueio.|  
|HOST_E_ABANDONED|Um evento foi cancelado enquanto um thread ou uma fibra bloqueada estava esperando.|  
|E_FAIL|Ocorreu uma falha catastrófica desconhecida. Quando um método retorna E_FAIL, o CLR não é mais utilizável no processo. As chamadas subsequentes para métodos de hospedagem retornam HOST_E_CLRNOTAVAILABLE.|  
  
## <a name="remarks"></a>Comentários  
 O CLR chama `SetSecurityContext` em vários cenários. Antes de executar construtores de classe e de módulo e finalizadores, as chamadas de CLR `SetSecurityContext` para proteger o host contra falhas de execução. Em seguida, ele redefine o contexto de segurança para seu estado original após a execução do construtor ou finalizador, usando outra chamada para `SetSecurityContext` . Um padrão semelhante ocorre com a conclusão de e/s. Se o host implementar [IHostIoCompletionManager](ihostiocompletionmanager-interface.md), o CLR chamará `SetSecurityContext` depois que o host chamar [ICLRIoCompletionManager:: OnComplete](iclriocompletionmanager-oncomplete-method.md).  
  
 Em pontos assíncronos em threads de trabalho, as chamadas CLR `SetSecurityContext` dentro <xref:System.Threading.ThreadPool.QueueUserWorkItem%2A?displayProperty=nameWithType> ou dentro de [IHostThreadPoolManager:: QueueUserWorkItem](ihostthreadpoolmanager-queueuserworkitem-method.md), dependendo se o host ou o CLR está implementando o pool de threads.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.Threading.ThreadPool?displayProperty=nameWithType>
- [Enumeração EContextType](econtexttype-enumeration.md)
- [Interface ICLRIoCompletionManager](iclriocompletionmanager-interface.md)
- [Interface IHostIoCompletionManager](ihostiocompletionmanager-interface.md)
- [Interface IHostSecurityContext](ihostsecuritycontext-interface.md)
- [Interface IHostSecurityManager](ihostsecuritymanager-interface.md)
- [Interface IHostThreadPoolManager](ihostthreadpoolmanager-interface.md)
