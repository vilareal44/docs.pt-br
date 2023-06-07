---
title: Método ICorProfilerInfo::SetFunctionIDMapper
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.SetFunctionIDMapper
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::SetFunctionIDMapper
helpviewer_keywords:
- ICorProfilerInfo::SetFunctionIDMapper method [.NET Framework profiling]
- SetFunctionIDMapper method [.NET Framework profiling]
ms.assetid: 1a6e5dae-d366-4497-9c02-7b5b1f43f9ec
topic_type:
- apiref
ms.openlocfilehash: 5272c5bf256f6e21a83470db094ab79317932018
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84497472"
---
# <a name="icorprofilerinfosetfunctionidmapper-method"></a>Método ICorProfilerInfo::SetFunctionIDMapper
Especifica a função implementada pelo criador de perfil que será chamada para mapear `FunctionID` valores para valores alternativos, que são passados para os ganchos de entrada/saída da função do criador de perfil.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SetFunctionIDMapper (  
    [in] FunctionIDMapper *pFunc);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pFunc`  
 no Um ponteiro para a implementação de [FunctionIDMapper](functionidmapper-function.md) que será chamado para mapear os `FunctionID` valores para seus valores alternativos.  
  
## <a name="remarks"></a>Comentários  
 As alternativas para os `FunctionID` valores serão passadas para os ganchos de entrada/saída da função do criador de perfil ([FunctionEnter2](functionenter2-function.md), [FunctionLeave2](functionleave2-function.md)e [FunctionTailcall2](functiontailcall2-function.md)) que são especificados pelo método [ICorProfilerInfo2:: SetEnterLeaveFunctionHooks2](icorprofilerinfo2-setenterleavefunctionhooks2-method.md) .  
  
 O `FunctionIDMapper` pode ser definido apenas uma vez e é recomendável defini-lo no retorno de chamada [ICorProfilerCallback:: Initialize](icorprofilercallback-initialize-method.md) .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)
