---
title: Método ICorProfilerInfo3::SetFunctionIDMapper2
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo3.SetFunctionIDMapper2 Method
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo3::SetFunctionIDMapper2
helpviewer_keywords:
- SetFunctionIDMapper2 method [.NET Framework profiling]
- ICorProfilerInfo3::SetFunctionIDMapper2 method [.NET Framework profiling]
ms.assetid: 8cdb1188-952a-4ba8-9f05-bfebc18cdd29
topic_type:
- apiref
ms.openlocfilehash: 723cb277a7df592e0494505018f7422e4e40f5f6
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84496146"
---
# <a name="icorprofilerinfo3setfunctionidmapper2-method"></a>Método ICorProfilerInfo3::SetFunctionIDMapper2
Especifica a função implementada pelo criador de perfil que será chamada para mapear `FunctionID` valores para valores alternativos, que são passados para os ganchos de entrada/saída da função do criador de perfil. Esse método estende o método [ICorProfilerInfo:: SetFunctionIDMapper](icorprofilerinfo-setfunctionidmapper-method.md) com um parâmetro de dados adicional, que os profileres podem usar para fazer a ambiguidade entre os tempos de execução.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SetFunctionIDMapper2(  
       [in] FunctionIDMapper2 *pFunc,  
       [in] void *clientData);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pFunc`  
 no Um ponteiro para uma implementação de [FunctionIDMapper2](functionidmapper2-function.md) que será chamada para mapear os `FunctionID` valores para seus valores alternativos.  
  
 `clientData`  
 no Um ponteiro que é passado para cada chamada de função [FunctionIDMapper2](functionidmapper2-function.md) feita pelo tempo de execução atual. O criador de perfil pode usar essas informações para fazer a ambiguidade entre tempos de execução.  
  
## <a name="return-value"></a>Valor Retornado  
  
## <a name="remarks"></a>Comentários  
 As alternativas para os valores de FunctionID serão passadas para os ganchos de entrada/saída da função do criador de perfil ([FunctionEnter3](functionenter3-function.md), [FunctionLeave3](functionleave3-function.md)e [FunctionTailcall3](functiontailcall3-function.md); ou [FunctionEnter3WithInfo](functionenter3withinfo-function.md), [FunctionLeave3WithInfo](functionleave3withinfo-function.md)e [FunctionTailcall3WithInfo](functiontailcall3withinfo-function.md)) que são especificados pelo método [SetEnterLeaveFunctionHooks3](icorprofilerinfo3-setenterleavefunctionhooks3-method.md) ou [SetEnterLeaveFunctionHooks3WithInfo](icorprofilerinfo3-setenterleavefunctionhooks3withinfo-method.md) .  
  
 O `FunctionIDMapper2` método pode ser definido apenas uma vez; é recomendável defini-lo no retorno de chamada [ICorProfilerCallback:: Initialize](icorprofilercallback-initialize-method.md) .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [SetFunctionIDMapper](icorprofilerinfo-setfunctionidmapper-method.md)
- [Interface ICorProfilerInfo3](icorprofilerinfo3-interface.md)
- [Criação de perfil de interfaces](profiling-interfaces.md)
- [Criação de perfil](index.md)
