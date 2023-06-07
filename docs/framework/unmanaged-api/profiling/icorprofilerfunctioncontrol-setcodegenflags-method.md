---
title: Método ICorProfilerFunctionControl::SetCodegenFlags
ms.date: 03/30/2017
api_name:
- ICorProfilerFunctionControl.SetCodegenFlags
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerFunctionControl::SetCodegenFlags
helpviewer_keywords:
- ICorProfilerFunctionControl::SetCodegenFlags method [.NET Framework profiling]
- SetCodegenFlags method, ICorProfilerFunctionControl interface [.NET Framework profiling]
ms.assetid: a2d5daa5-b990-4ae5-bf2a-c0862fe58bd7
topic_type:
- apiref
ms.openlocfilehash: a3d5527fc34ad7292974c005179e9d9cad210c94
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503114"
---
# <a name="icorprofilerfunctioncontrolsetcodegenflags-method"></a>Método ICorProfilerFunctionControl::SetCodegenFlags
Define um ou mais sinalizadores da enumeração de [COR_PRF_CODEGEN_FLAGS](cor-prf-codegen-flags-enumeration.md) para controlar a geração de código para uma função recompilada JIT (just-in-time).  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SetCodegenFlags(  
    [in] DWORD flags);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `flags`  
 no Um ou mais sinalizadores da enumeração de [COR_PRF_CODEGEN_FLAGS](cor-prf-codegen-flags-enumeration.md) .  
  
## <a name="remarks"></a>Comentários  
 O criador de perfil Obtém uma instância dessa interface por meio do retorno de chamada [ICorProfilerCallback4:: GetReJITParameters](icorprofilercallback4-getrejitparameters-method.md) . `SetCodegenFlags`permite que o criador de perfil controle a geração de código para a função recompilada. Assim como acontece com todos os outros parâmetros de recompilação JIT, os sinalizadores de geração de código se aplicam a todas as instâncias da função.  
  
 O compilador JIT considera esses sinalizadores de compilação, juntamente com outros sinalizadores especificados por outras fontes, ao compilar uma função.  As outras fontes incluem o depurador, os sinalizadores globais definidos pelo criador de perfil na inicialização usando o método [ICorProfilerInfo:: SetEventMask](icorprofilerinfo-seteventmask-method.md) (com os valores `COR_PRF_DISABLE_INLINING` e `COR_PRF_DISABLE_OPTIMIZATIONS` ) e o retorno de chamada [ICorProfilerCallback:: JITInlining](icorprofilercallback-jitinlining-method.md) do criador de perfil.  O compilador JIT dá precedência a uma fonte que solicita a menor quantidade de otimização.  Por exemplo, se o criador de perfil especificar `COR_PRF_DISABLE_INLINING` na inicialização, mas não especificar `COR_PRF_CODEGEN_DISABLE_INLINING` no retorno de chamada [ICorProfilerFunctionControl:: SetCodegenFlags](icorprofilerfunctioncontrol-setcodegenflags-method.md) , o inlining ainda estará desabilitado.  Da mesma forma, se o criador de perfil não especificar `COR_PRF_CODEGEN_DISABLE_INLINING` em `SetCodegenFlags` , mas, em seguida, desabilitar o inalinhamento usando o retorno de chamada [ICorProfilerCallback:: JITInlining](icorprofilercallback-jitinlining-method.md) , o inlining será desabilitado.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerFunctionControl](icorprofilerfunctioncontrol-interface.md)
