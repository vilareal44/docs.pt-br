---
title: Método ICorProfilerInfo2::SetEnterLeaveFunctionHooks2
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo2.SetEnterLeaveFunctionHooks2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo2::SetEnterLeaveFunctionHooks2
helpviewer_keywords:
- ICorProfilerInfo2::SetEnterLeaveFunctionHooks2 method [.NET Framework profiling]
- SetEnterLeaveFunctionHooks2 method [.NET Framework profiling]
ms.assetid: 3c26b3e7-f72b-48a5-bf8c-edc122523a4b
topic_type:
- apiref
ms.openlocfilehash: 78489aae840ff17e68b10bd7593fb7be4dae1af7
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84496710"
---
# <a name="icorprofilerinfo2setenterleavefunctionhooks2-method"></a>Método ICorProfilerInfo2::SetEnterLeaveFunctionHooks2
Especifica as funções implementadas pelo Profiler a serem chamadas nas versões atualizadas dos ganchos "Enter", "sair" e "tailcall" das funções gerenciadas.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SetEnterLeaveFunctionHooks2(  
    [in] FunctionEnter2    *pFuncEnter,  
    [in] FunctionLeave2    *pFuncLeave,  
    [in] FunctionTailcall2 *pFuncTailcall);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pFuncEnter`  
 no Um ponteiro para a implementação a ser usado como o retorno de chamada [FunctionEnter2](functionenter2-function.md) .  
  
 `pFuncLeave`  
 no Um ponteiro para a implementação a ser usado como o retorno de chamada [FunctionLeave2](functionleave2-function.md) .  
  
 `pFuncTailcall`  
 no Um ponteiro para a implementação a ser usado como o retorno de chamada [FunctionTailcall2](functiontailcall2-function.md) .  
  
## <a name="remarks"></a>Comentários  
 O `SetEnterLeaveFunctionHooks2` método é semelhante ao método [ICorProfilerInfo:: SetEnterLeaveFunctionHooks](icorprofilerinfo-setenterleavefunctionhooks-method.md) . Use o primeiro para especificar funções a serem usadas como versões mais recentes dos retornos de chamada enter/leave/tailcall e a última para especificar as funções a serem usadas como versões mais antigas dos retornos de chamada enter/leave/tailcall.  
  
 Somente um conjunto de retornos de chamada pode estar ativo por vez. Portanto, se um criador de perfil chama `ICorProfilerInfo::SetEnterLeaveFunctionHooks` e `SetEnterLeaveFunctionHooks2` , `SetEnterLeaveFunctionHooks2` é usado.  
  
 O `SetEnterLeaveFunctionHooks2` método pode ser chamado somente do retorno de chamada [ICorProfilerCallback:: Initialize](icorprofilercallback-initialize-method.md) do criador de perfil.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)
- [Interface ICorProfilerInfo2](icorprofilerinfo2-interface.md)
