---
title: Método ICLRSyncManager::CreateRWLockOwnerIterator
ms.date: 03/30/2017
api_name:
- ICLRSyncManager.CreateRWLockOwnerIterator
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRSyncManager::CreateRWLockOwnerIterator
helpviewer_keywords:
- ICLRSyncManager::CreateRWLockOwnerIterator method [.NET Framework hosting]
- CreateRWLockOwnerIterator method [.NET Framework hosting]
ms.assetid: b5535b87-9439-424e-b9b3-7d6fafb9819e
topic_type:
- apiref
ms.openlocfilehash: f8fde4905c41dffde90c6361b5a8cdffa15deb4a
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503959"
---
# <a name="iclrsyncmanagercreaterwlockowneriterator-method"></a>Método ICLRSyncManager::CreateRWLockOwnerIterator
Solicita que o Common Language Runtime (CLR) crie um iterador para o host usar para determinar o conjunto de tarefas que estão aguardando um bloqueio do gravador de leitor.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT CreateRWLockOwnerIterator (  
    [in]  SIZE_T    cookie,  
    [out] SIZE_T   *pIterator  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `cookie`  
 no O cookie associado ao bloqueio de leitor-gravador desejado.  
  
 `pIterator`  
 fora Um ponteiro para um iterador que pode ser passado para os métodos [GetRWLockOwnerNext](iclrsyncmanager-getrwlockownernext-method.md) e [DeleteRWLockOwnerIterator](iclrsyncmanager-deleterwlockowneriterator-method.md) .  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|`CreateRWLockOwnerIterator`retornado com êxito.|  
|HOST_E_CLRNOTAVAILABLE|O CLR não foi carregado em um processo ou o CLR está em um estado no qual não pode executar código gerenciado ou processar a chamada com êxito.|  
|HOST_E_TIMEOUT|A chamada atingiu o tempo limite.|  
|HOST_E_NOT_OWNER|O chamador não possui o bloqueio.|  
|HOST_E_ABANDONED|Um evento foi cancelado enquanto um thread ou uma fibra bloqueada estava esperando.|  
|E_FAIL|Ocorreu uma falha catastrófica desconhecida. Quando um método retorna E_FAIL, o CLR não é mais utilizável no processo. As chamadas subsequentes para métodos de hospedagem retornam HOST_E_CLRNOTAVAILABLE.|  
|HOST_E_INVALIDOPERATION|`CreateRWLockOwnerIterator`foi chamado em um thread que está executando código gerenciado no momento.|  
  
## <a name="remarks"></a>Comentários  
 Normalmente, os hosts chamam os `CreateRWLockOwnerIterator` `DeleteRWLockOwnerIterator` métodos, e `GetRWLockOwnerNext` durante a detecção de deadlock. O host é responsável por garantir que o bloqueio do gravador de leitor ainda seja válido, pois o CLR não faz nenhuma tentativa de manter o bloqueio do gravador de leitor ativo. Várias estratégias estão disponíveis para o host garantir a validade do bloqueio:  
  
- O host pode bloquear chamadas de versão no bloqueio do gravador Reader (por exemplo, [IHostSemaphore:: ReleaseSemaphore](ihostsemaphore-releasesemaphore-method.md)) ao garantir que esse bloco não cause deadlock.  
  
- O host pode impedir que a saída Espere no objeto de evento associado ao bloqueio do gravador de leitor, garantindo novamente que esse bloco não cause o deadlock.  
  
> [!NOTE]
> `CreateRWLockOwnerIterator`deve ser chamado somente em threads que estão executando código não gerenciado no momento.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICLRSyncManager](iclrsyncmanager-interface.md)
- [Interface IHostSyncManager](ihostsyncmanager-interface.md)
