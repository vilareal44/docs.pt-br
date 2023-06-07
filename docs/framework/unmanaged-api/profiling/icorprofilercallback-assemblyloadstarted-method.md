---
title: Método ICorProfilerCallback::AssemblyLoadStarted
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.AssemblyLoadStarted
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::AssemblyLoadStarted
helpviewer_keywords:
- ICorProfilerCallback::AssemblyLoadStarted method [.NET Framework profiling]
- AssemblyLoadStarted method [.NET Framework profiling]
ms.assetid: 67e8209d-a0ca-4118-a6e6-c1ee0abc2221
topic_type:
- apiref
ms.openlocfilehash: df172edb97a82ae3bf2d46c8be6ea05d5445a09a
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500423"
---
# <a name="icorprofilercallbackassemblyloadstarted-method"></a>Método ICorProfilerCallback::AssemblyLoadStarted
Notifica o criador de perfil de que um assembly está sendo carregado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT AssemblyLoadStarted(  
    [in] AssemblyID assemblyId);  
```  
  
## <a name="parameters"></a>Parâmetros

- `assemblyId`

  \[in] identifica o assembly que está sendo carregado.

## <a name="remarks"></a>Comentários  
 O valor de `assemblyId` não é válido para uma solicitação de informações até que o método [ICorProfilerCallback:: AssemblyLoadFinished](icorprofilercallback-assemblyloadfinished-method.md) seja chamado.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback](icorprofilercallback-interface.md)
