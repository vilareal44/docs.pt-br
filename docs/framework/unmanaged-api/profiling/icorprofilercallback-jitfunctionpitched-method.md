---
title: Método ICorProfilerCallback::JITFunctionPitched
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.JITFunctionPitched
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::JITFunctionPitched
helpviewer_keywords:
- JITFunctionPitched method [.NET Framework profiling]
- ICorProfilerCallback::JITFunctionPitched method [.NET Framework profiling]
ms.assetid: 116085df-7a77-404a-afac-d0557a12b986
topic_type:
- apiref
ms.openlocfilehash: 2715a5b6b03a5ad33a6f18fb736fce3911bfbef0
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500020"
---
# <a name="icorprofilercallbackjitfunctionpitched-method"></a>Método ICorProfilerCallback::JITFunctionPitched
Notifica o criador de perfil de que uma função que foi compilada JIT (just-in-time) foi removida da memória.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT JITFunctionPitched(  
    [in] FunctionID functionId);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `functionId`  
 no A ID da função que foi removida.  
  
## <a name="remarks"></a>Comentários  
 Se a função removida for chamada, o criador de perfil receberá novos eventos de compilação JIT quando a função for recompilada. Atualmente, o compilador JIT do Common Language Runtime (CLR) não remove funções da memória, portanto, esse retorno de chamada não é usado no momento e não será recebido pelo criador de perfil.  
  
 O valor de `functionId` não é válido até que a função seja recompilada. Quando a função for recompilada, o mesmo `functionId` valor será usado.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback](icorprofilercallback-interface.md)
