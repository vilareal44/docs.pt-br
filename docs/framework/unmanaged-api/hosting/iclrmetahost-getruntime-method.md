---
title: Método ICLRMetaHost::GetRuntime
ms.date: 03/30/2017
api_name:
- ICLRMetaHost.GetRuntime
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRMetaHost::GetRuntime
helpviewer_keywords:
- GetRuntime method [.NET Framework hosting]
- ICLRMetaHost::GetRuntime method [.NET Framework hosting]
ms.assetid: a10749f1-ab91-47cf-982f-d8ccd2e81bd2
topic_type:
- apiref
ms.openlocfilehash: d482e25c7bf0f028e2478c8e7b7863bc54d7aeb9
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504185"
---
# <a name="iclrmetahostgetruntime-method"></a>Método ICLRMetaHost::GetRuntime
Obtém a interface [ICLRRuntimeInfo](iclrruntimeinfo-interface.md) que corresponde a uma versão específica do Common Language Runtime (CLR). Esse método substitui a função [CorBindToRuntimeEx](corbindtoruntimeex-function.md) usada com o sinalizador [STARTUP_LOADER_SAFEMODE](startup-flags-enumeration.md) .  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetRuntime (  
    [in] LPCWSTR pwzVersion,  
    [in] REFIID riid,  
    [out,iid_is(riid), retval] LPVOID *ppRuntime  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pwzVersion`  
 no A versão de compilação .NET Framework armazenada nos metadados, no formato "v*A*. *B*[.* X*] ". *A*, *B*e *X* são números decimais que correspondem à versão principal, à versão secundária e ao número da compilação.  
  
> [!NOTE]
> Esse parâmetro deve corresponder ao nome do diretório para a versão .NET Framework, pois ele aparece em C:\Windows\Microsoft.NET\Framework ou C:\Windows\Microsoft.NET\Framework64.  
  
 Os valores de exemplo são "v 1.0.3705", "v 1.1.4322", "v 2.0.50727" e "v 4.0. *X*", em que *x* depende do número de Build instalado. O prefixo "v" é necessário.  
  
 `riid`  
 no O identificador para a interface desejada. Atualmente, o único valor válido para esse parâmetro é IID_ICLRRuntimeInfo.  
  
 `ppRuntime`  
 fora Um ponteiro para a interface [ICLRRuntimeInfo](iclrruntimeinfo-interface.md) que corresponde ao tempo de execução solicitado.  
  
## <a name="return-value"></a>Valor Retornado  
 Esse método retorna os HRESULTs específicos a seguir, bem como os erros de HRESULT que indicam falha de método.  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|O método foi concluído com êxito.|  
|E_POINTER|`pwzVersion` ou `ppRuntime` é nulo.|  
  
## <a name="remarks"></a>Comentários  
 Esse método interage consistentemente com interfaces herdadas, como a interface [ICorRuntimeHost](icorruntimehost-interface.md) e funções herdadas, como as funções preteridas `CorBindTo*` (consulte [funções de Hospedagem de CLR preteridas](deprecated-clr-hosting-functions.md) na API de hospedagem do .NET Framework 2,0). Ou seja, os tempos de execução que são carregados com a API herdada são visíveis para a nova API, e os tempos de execução que são carregados com a nova API são visíveis para a API herdada.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MetaHost. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICLRMetaHost](iclrmetahost-interface.md)
- [Interfaces e coclasse de hospedagem CLR reprovadas](deprecated-clr-hosting-interfaces-and-coclasses.md)
- [Interfaces de hospedagem CLR](clr-hosting-interfaces.md)
- [Funções de hospedagem CLR reprovadas](deprecated-clr-hosting-functions.md)
- [Hosting](index.md)
