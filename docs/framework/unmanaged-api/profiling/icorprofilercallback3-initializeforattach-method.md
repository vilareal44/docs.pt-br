---
title: Método ICorProfilerCallback3::InitializeForAttach
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback3.InitializeForAttach Method
api_location:
- Mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback3::InitializeForAttach
helpviewer_keywords:
- InitializeForAttach method [.NET Framework profiling]
- ICorProfilerCallback3::InitializeForAttach method [.NET Framework profiling]
ms.assetid: bed097b3-6d52-46c9-bee7-ac7910b6fc3f
topic_type:
- apiref
ms.openlocfilehash: 9bff594d0307153fb468b28c1535977f06997748
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84499708"
---
# <a name="icorprofilercallback3initializeforattach-method"></a>Método ICorProfilerCallback3::InitializeForAttach
Chamado pelo Common Language Runtime (CLR) para dar ao criador de perfil uma oportunidade de inicializar seu estado após uma operação de anexação.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT InitializeForAttach(  
            [in] IUnknown * pCorProfilerInfoUnk,  
            [in] void * pvClientData,  
            [in] UINT cbClientData);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pCorProfilerInfoUnk`  
 no Um ponteiro de interface para a `ICorProfilerInfo*` interface.  
  
 `pvClientData`  
 no Um ponteiro para os dados passados para o método [ICLRProfiling:: AttachProfiler](iclrprofiling-attachprofiler-method.md) em seu `pvClientData` parâmetro. Se esse parâmetro for NULL, `cbClientData` será 0 (zero). O CLR libera essa memória quando ela retorna do `InitializeForAttach` .  
  
 `cbClientData`  
 no O tamanho, em bytes, dos dados que `pvClientData` aponta para.  
  
## <a name="remarks"></a>Comentários  
 O CLR chama `InitializeForAttach` para dar ao criador de perfil uma oportunidade de solicitar retornos de chamada.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback](icorprofilercallback-interface.md)
- [Interface ICorProfilerInfo3](icorprofilerinfo3-interface.md)
- [Criação de perfil de interfaces](profiling-interfaces.md)
- [Criação de perfil](index.md)
