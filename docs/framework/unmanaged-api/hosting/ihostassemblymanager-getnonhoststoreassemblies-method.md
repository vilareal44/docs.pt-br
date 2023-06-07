---
title: Método IHostAssemblyManager::GetNonHostStoreAssemblies
ms.date: 03/30/2017
api_name:
- IHostAssemblyManager.GetNonHostStoreAssemblies
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostAssemblyManager::GetNonHostStoreAssemblies
helpviewer_keywords:
- IHostAssemblyManager::GetNonHostStoreAssemblies method [.NET Framework hosting]
- GetNonHostStoreAssemblies method [.NET Framework hosting]
ms.assetid: d2250b38-c76a-40ce-80c8-ba45149886e8
topic_type:
- apiref
ms.openlocfilehash: 9a1440be7011130b16d7112ae15026eb74856190
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501581"
---
# <a name="ihostassemblymanagergetnonhoststoreassemblies-method"></a>Método IHostAssemblyManager::GetNonHostStoreAssemblies
Obtém um ponteiro de interface para um [ICLRAssemblyReferenceList](iclrassemblyreferencelist-interface.md) que representa a lista de assemblies que o host espera que o Common Language Runtime (CLR) carregue.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetNonHostStoreAssemblies (  
    [out] ICLRAssemblyReferenceList **ppReferenceList  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `ppReferenceList`  
 fora Um ponteiro para o endereço de um `ICLRAssemblyReferenceList` que contém uma lista de referências a assemblies que o host espera que o CLR carregue.  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|`GetNonHostStoreAssemblies`retornado com êxito.|  
|HOST_E_CLRNOTAVAILABLE|O CLR não foi carregado em um processo ou o CLR está em um estado no qual não pode executar código gerenciado ou processar a chamada com êxito.|  
|HOST_E_TIMEOUT|A chamada atingiu o tempo limite.|  
|HOST_E_NOT_OWNER|O chamador não possui o bloqueio.|  
|HOST_E_ABANDONED|Um evento foi cancelado enquanto um thread ou uma fibra bloqueada estava esperando.|  
|E_FAIL|Ocorreu uma falha catastrófica desconhecida. Quando um método retorna E_FAIL, o CLR não é mais utilizável no processo. As chamadas subsequentes para métodos de hospedagem retornam HOST_E_CLRNOTAVAILABLE.|  
|E_OUTOFMEMORY|Não havia memória suficiente disponível para criar a lista de referências para o solicitado `ICLRAssemblyReferenceList` .|  
  
## <a name="remarks"></a>Comentários  
 O CLR resolve referências usando o seguinte conjunto de diretrizes:  
  
- Primeiro, ele consulta a lista de referências de assembly retornada por `GetNonHostStoreAssemblies` .  
  
- Se o assembly aparecer na lista, o CLR será associado a ele normalmente.  
  
- Se o assembly não aparecer na lista e o host tiver fornecido uma implementação de [IHostAssemblyStore](ihostassemblystore-interface.md), o CLR chamará [IHostAssemblyStore::P rovideassembly](ihostassemblystore-provideassembly-method.md) para permitir que o host forneça o assembly ao qual associar.  
  
- Caso contrário, o CLR não se associará ao assembly.  
  
 Se o host definir `ppReferenceList` como nulo, o CLR primeiro investigará o cache de assembly global, chamará `ProvideAssembly` e, em seguida, investigará a base do aplicativo para resolver uma referência de assembly.  
  
> [!NOTE]
> Na inicialização, o CLR chama `GetNonHostStoreAssemblies` apenas uma vez. O método não é chamado novamente.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICLRAssemblyReferenceList](iclrassemblyreferencelist-interface.md)
- [Interface IHostAssemblyManager](ihostassemblymanager-interface.md)
- [Interface IHostAssemblyStore](ihostassemblystore-interface.md)
