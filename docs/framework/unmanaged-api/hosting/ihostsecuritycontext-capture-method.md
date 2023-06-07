---
title: Método IHostSecurityContext::Capture
ms.date: 03/30/2017
api_name:
- IHostSecurityContext.Capture
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostSecurityContext::Capture
helpviewer_keywords:
- Capture method [.NET Framework hosting]
- IHostSecurityContext::Capture method [.NET Framework hosting]
ms.assetid: ae0836d0-1170-4494-bac5-d0e809df51a2
topic_type:
- apiref
ms.openlocfilehash: e1df31ed8b652837a33b360b1378f99e6800cbea
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501515"
---
# <a name="ihostsecuritycontextcapture-method"></a>Método IHostSecurityContext::Capture
Obtém um clone da instância [IHostSecurityContext](ihostsecuritycontext-interface.md) retornada de uma chamada para [IHostSecurityManager:: GetSecurityContext](ihostsecuritymanager-getsecuritycontext-method.md).  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp
HRESULT Capture (  
    [out] IHostSecurityContext** ppClonedContext  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `ppClonedContext`  
 fora Um ponteiro para o endereço de um clone do `IHostSecurityContext` objeto a ser capturado.  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|`Capture`retornado com êxito.|  
|HOST_E_CLRNOTAVAILABLE|O Common Language Runtime (CLR) não foi carregado em um processo ou o CLR está em um estado no qual não pode executar código gerenciado ou processar a chamada com êxito.|  
|HOST_E_TIMEOUT|A chamada atingiu o tempo limite.|  
|HOST_E_NOT_OWNER|O chamador não possui o bloqueio.|  
|HOST_E_ABANDONED|Um evento foi cancelado enquanto um thread ou uma fibra bloqueada estava esperando.|  
|E_FAIL|Ocorreu uma falha catastrófica desconhecida. Quando um método retorna E_FAIL, o CLR não é mais utilizável no processo. As chamadas subsequentes para métodos de hospedagem retornam HOST_E_CLRNOTAVAILABLE.|  
  
## <a name="remarks"></a>Comentários  
 O ponteiro de interface retornado de `Capture` é um clone do contexto capturado. Quando essas informações são movidas em um ponto de código assíncrono, seu tempo de vida é separado do ponteiro no qual a chamada foi feita. Portanto, o ponteiro original pode ser liberado.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IHostSecurityContext](ihostsecuritycontext-interface.md)
- [Interface IHostSecurityManager](ihostsecuritymanager-interface.md)
