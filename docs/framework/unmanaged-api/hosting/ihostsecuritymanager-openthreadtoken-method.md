---
title: Método IHostSecurityManager::OpenThreadToken
ms.date: 03/30/2017
api_name:
- IHostSecurityManager.OpenThreadToken
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostSecurityManager::OpenThreadToken
helpviewer_keywords:
- IHostSecurityManager::OpenThreadToken method [.NET Framework hosting]
- OpenThreadToken method [.NET Framework hosting]
ms.assetid: d5999052-8bf0-4a9e-8621-da6284406b18
topic_type:
- apiref
ms.openlocfilehash: 85c7308f794929d753b50f58f69168f67a31cb85
ms.sourcegitcommit: d223616e7e6fe2139079052e6fcbe25413fb9900
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83803882"
---
# <a name="ihostsecuritymanageropenthreadtoken-method"></a>Método IHostSecurityManager::OpenThreadToken
Abre o token de acesso discricionário associado ao thread em execução no momento.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT OpenThreadToken (  
    [in]  DWORD    dwDesiredAccess,
    [in]  BOOL     bOpenAsSelf,
    [out] HANDLE   *phThreadToken  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `dwDesiredAccess`  
 no Uma máscara de valores de acesso que especificam os tipos de acesso solicitados ao token de thread. Esses valores são definidos na função do Win32 `OpenThreadToken` . Os tipos de acesso solicitados são reconciliados com a DACL (lista de controle de acesso discricionário) do token para determinar quais tipos de acesso conceder ou negar.  
  
 `bOpenAsSelf`  
 [in] `true` para especificar que a verificação de acesso deve ser feita usando o contexto de segurança do processo para o thread de chamada; `false`para especificar que a verificação de acesso deve ser executada usando o contexto de segurança para o próprio thread de chamada. Se o thread estiver representando um cliente, o contexto de segurança poderá ser o de um processo de cliente.  
  
 `phThreadToken`  
 fora Um ponteiro para o token de acesso aberto recentemente.  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|`OpenThreadToken`retornado com êxito.|  
|HOST_E_CLRNOTAVAILABLE|O Common Language Runtime (CLR) não foi carregado em um processo ou o CLR está em um estado no qual não pode executar código gerenciado ou processar a chamada com êxito.|  
|HOST_E_TIMEOUT|A chamada atingiu o tempo limite.|  
|HOST_E_NOT_OWNER|O chamador não possui o bloqueio.|  
|HOST_E_ABANDONED|Um evento foi cancelado enquanto um thread ou uma fibra bloqueada estava esperando.|  
|E_FAIL|Ocorreu uma falha catastrófica desconhecida. Quando um método retorna E_FAIL, o CLR não é mais utilizável no processo. As chamadas subsequentes para métodos de hospedagem retornam HOST_E_CLRNOTAVAILABLE.|  
  
## <a name="remarks"></a>Comentários  
 `IHostSecurityManager::OpenThreadToken`comporta-se da mesma forma que a função Win32 correspondente do mesmo nome, exceto que a função do Win32 permite que o chamador transmita um identificador para um thread arbitrário, enquanto `IHostSecurityManager::OpenThreadToken` abre somente o token associado ao thread de chamada.  
  
 O `HANDLE` tipo não é compatível COM com, ou seja, seu tamanho é específico para o sistema operacional e requer marshaling personalizado. Portanto, esse token é para uso apenas dentro do processo, entre o CLR e o host.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IHostSecurityContext](ihostsecuritycontext-interface.md)
- [Interface IHostSecurityManager](ihostsecuritymanager-interface.md)
