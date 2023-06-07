---
title: Método ICorProfilerCallback::AppDomainShutdownFinished
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.AppDomainShutdownFinished
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::AppDomainShutdownFinished
helpviewer_keywords:
- AppDomainShutdownFinished method [.NET Framework profiling]
- ICorProfilerCallback::AppDomainShutdownFinished method [.NET Framework profiling]
ms.assetid: 52794819-0a59-4bb1-a265-0f158cd5cd65
topic_type:
- apiref
ms.openlocfilehash: 722a1e0adea41a13ca25829c53372c29187b80bd
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500462"
---
# <a name="icorprofilercallbackappdomainshutdownfinished-method"></a>Método ICorProfilerCallback::AppDomainShutdownFinished
Notifica o criador de perfil de que um domínio de aplicativo foi descarregado de um processo.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT AppDomainShutdownFinished(  
    [in] AppDomainID appDomainId,  
    [in] HRESULT     hrStatus);  
```  
  
## <a name="parameters"></a>Parâmetros

- `appDomainId`

  \[in] identifica o domínio no qual os assemblies do aplicativo são armazenados.

- `hrStatus`

  \[in] um HRESULT que indica se o domínio do aplicativo foi descarregado com êxito.

## <a name="remarks"></a>Comentários  
 O valor de `appDomainId` não é válido para uma solicitação de informações depois que o método [ICorProfilerCallback:: AppDomainShutdownStarted](icorprofilercallback-appdomainshutdownstarted-method.md) retorna.  
  
 Algumas partes do descarregamento do domínio do aplicativo podem continuar após o `AppDomainCreationFinished` retorno de chamada. Um HRESULT de falha em `hrStatus` indica uma falha. No entanto, um HRESULT de êxito em `hrStatus` indica apenas que a primeira parte do descarregamento do domínio do aplicativo foi bem-sucedida.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback](icorprofilercallback-interface.md)
