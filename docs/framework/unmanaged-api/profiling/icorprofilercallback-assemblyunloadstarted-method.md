---
title: Método ICorProfilerCallback::AssemblyUnloadStarted
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.AssemblyUnloadStarted
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::AssemblyUnloadStarted
helpviewer_keywords:
- ICorProfilerCallback::AssemblyUnloadStarted method [.NET Framework profiling]
- AssemblyUnloadStarted method [.NET Framework profiling]
ms.assetid: 6e47b7e5-0335-4dd3-8c42-d3c07d62b102
topic_type:
- apiref
ms.openlocfilehash: 80054a8292c69b957664cb3573b0a8694c7f9fd2
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500397"
---
# <a name="icorprofilercallbackassemblyunloadstarted-method"></a>Método ICorProfilerCallback::AssemblyUnloadStarted
Notifica o criador de perfil de que um assembly está sendo descarregado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT AssemblyUnloadStarted(  
    [in] AssemblyID assemblyId);  
```  
  
## <a name="parameters"></a>Parâmetros

- `assemblyId`

  \[in] identifica o assembly que está sendo descarregado.

## <a name="remarks"></a>Comentários  
 O valor de `assemblyId` não é válido para uma solicitação de informações após o `AssemblyUnloadStarted` retorno do método — essa é a última chance do criador de perfil obter informações sobre esse assembly.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback](icorprofilercallback-interface.md)
- [Método AssemblyUnloadFinished](icorprofilercallback-assemblyunloadfinished-method.md)
