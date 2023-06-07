---
title: Método ICLRRuntimeInfo::IsLoaded
ms.date: 03/30/2017
api_name:
- ICLRRuntimeInfo.IsLoaded
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRRuntimeInfo::IsLoaded
helpviewer_keywords:
- IsLoaded method [.NET Framework hosting]
- ICLRRuntimeInfo::IsLoaded method [.NET Framework hosting]
ms.assetid: fdc5a3a7-71ff-4025-99a1-59e4ee0bfe1b
topic_type:
- apiref
ms.openlocfilehash: 45e27ac3c2d4912d2ed3e5d43ea3020b9db5dbdc
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504024"
---
# <a name="iclrruntimeinfoisloaded-method"></a>Método ICLRRuntimeInfo::IsLoaded
Indica se o Common Language Runtime (CLR) associado à interface [ICLRRuntimeInfo](iclrruntimeinfo-interface.md) é carregado em um processo. Um tempo de execução pode ser carregado sem também ser iniciado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT IsLoaded(  
[in]  HANDLE hndProcess,  
[out, retval] BOOL *pbLoaded);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `hndProcess`  
 no Um identificador para o processo.  
  
 `pbLoaded`  
 [fora] `true` Se o CLR for carregado no processo; caso contrário, `false` .  
  
## <a name="return-value"></a>Valor Retornado  
 Esse método retorna os HRESULTs específicos a seguir, bem como os erros de HRESULT que indicam falha de método.  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|O método foi concluído com êxito.|  
|E_POINTER|`pbLoaded` é nulo.|  
  
## <a name="remarks"></a>Comentários  
 Esse método é compatível com versões anteriores com as seguintes funções e interfaces:  
  
- Interface [ICorRuntimeHost](icorruntimehost-interface.md) (na API de hospedagem do .NET Framework versão 1).  
  
- Interface [ICLRRuntimeHost](iclrruntimehost-interface.md) (na API de hospedagem .NET Framework 2,0).  
  
- Funções preteridas `CorBindTo*` (consulte [funções de Hospedagem de CLR preteridas](deprecated-clr-hosting-functions.md) na API de hospedagem .NET Framework 2,0).  
  
 Um host pode chamar uma das funções preteridas `CorBindTo*` , como a função [CorBindToRuntime](corbindtoruntime-function.md) , para instanciar uma versão específica do CLR. O host poderia então chamar o método [ICLRMetaHost:: GetRuntime](iclrmetahost-getruntime-method.md) e especificar o mesmo número de versão para obter uma interface [ICLRRuntimeInfo](iclrruntimeinfo-interface.md) .  
  
 Se o host chamar o `IsLoaded` método na interface [ICLRRuntimeInfo](iclrruntimeinfo-interface.md) retornada, `pbLoaded` retornará `true` ; caso contrário, retornará `false` .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MetaHost. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICLRRuntimeInfo](iclrruntimeinfo-interface.md)
- [Interfaces de hospedagem](hosting-interfaces.md)
- [Hosting](index.md)
