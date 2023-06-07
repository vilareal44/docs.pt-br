---
title: Método ICorProfilerInfo3::GetFunctionLeave3Info
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo3.GetFunctionLeave3Info Method
api_location:
- Mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo3::GetFunctionLeave3Info
helpviewer_keywords:
- GetFunctionLeave3Info method [.NET Framework profiling]
- ICorProfilerInfo3::GetFunctionLeave3Info method [.NET Framework profiling]
ms.assetid: df7083d2-fd43-44c7-9ce5-912c25cef0ff
topic_type:
- apiref
ms.openlocfilehash: bab52d9179d7454cab4a47e1a2bfe80a49b00c2a
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502828"
---
# <a name="icorprofilerinfo3getfunctionleave3info-method"></a>Método ICorProfilerInfo3::GetFunctionLeave3Info
Fornece o quadro de pilha e o valor de retorno da função que está sendo relatada ao criador de perfil pela função de [função FunctionLeave3WithInfo](functionleave3withinfo-function.md) . Esse método pode ser chamado somente durante o `FunctionLeave3WithInfo` retorno de chamada.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetFunctionLeave3Info(  
            [in]  FunctionID functionId,  
            [in]  COR_PRF_ELT_INFO eltInfo,  
            [out] COR_PRF_FRAME_INFO *pFrameInfo,  
            [out] COR_PRF_FUNCTION_ARGUMENT_RANGE *pRetvalRange);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `functionId`  
 no O `FunctionID` da função que está retornando.  
  
 `eltInfo`  
 no Um identificador opaco que representa informações sobre um determinado registro de ativação. O criador de perfil deve fornecer o mesmo `eltInfo` que foi dado ao criador de perfil pela função [FunctionLeave3WithInfo](functionleave3withinfo-function.md) .  
  
 `pFrameInfo`  
 fora Um identificador opaco que representa informações genéricas sobre um determinado registro de ativação. Esse identificador é válido somente durante o `FunctionLeave3WithInfo` retorno de chamada no qual o criador de perfil chamou o `GetFunctionLeave3Info` método.  
  
 `pRetvalRange`  
 fora Um ponteiro para uma estrutura de [COR_PRF_FUNCTION_ARGUMENT_RANGE](cor-prf-function-argument-range-structure.md) que contém o valor retornado da função. Para acessar informações de valor de retorno, o `COR_PRF_ENABLE_FUNCTION_RETVAL` sinalizador deve ser definido. O criador de perfil pode usar o [método ICorProfilerInfo:: SetEventMask](icorprofilerinfo-seteventmask-method.md) para definir os sinalizadores de evento.  
  
## <a name="remarks"></a>Comentários  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [FunctionEnter3WithInfo](functionenter3withinfo-function.md)
- [FunctionLeave3WithInfo](functionleave3withinfo-function.md)
- [FunctionTailcall3WithInfo](functiontailcall3withinfo-function.md)
- [Interface ICorProfilerInfo3](icorprofilerinfo3-interface.md)
- [Criação de perfil de interfaces](profiling-interfaces.md)
- [Criação de perfil](index.md)
