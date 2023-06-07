---
title: Método ICorProfilerInfo4::GetReJITIDs
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo4.GetReJITIDs
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo4::GetReJITIDs
helpviewer_keywords:
- GetReJITIDs method, ICorProfilerInfo4 interface [.NET Framework profiling]
- ICorProfilerInfo4::GetReJITIDs method [.NET Framework profiling]
ms.assetid: 634ac28c-a5b7-4fc3-af84-256c24ca8177
topic_type:
- apiref
ms.openlocfilehash: ba15440df79dded95a8afa9438657d064e167f36
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84495964"
---
# <a name="icorprofilerinfo4getrejitids-method"></a>Método ICorProfilerInfo4::GetReJITIDs
Retorna uma matriz de IDs que identifica todas as versões recompiladas por JIT da função especificada que ainda estão alocadas. Isso inclui versões recompiladas do JIT de funções que foram revertidas posteriormente, mas ainda não foram liberadas (por exemplo, quando o domínio do aplicativo que contém a função revertida ainda estiver em uso).  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp
HRESULT GetReJITIDs (  
     [in]  FunctionID          functionId,  
     [in]  ULONG               cReJitIds,  
     [out] ULONG *             pcReJitIds,  
     [out, size_is(cReJitIds), length_is(*pcReJitIds)]   ReJITID        reJitIds[]);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `functionId`  
 no O `FunctionID` da instância de função para a qual enumerar versões.  
  
 `cReJitIds`  
 no O número de IDs recompiladas por JIT alocadas na `reJitIds` matriz.  
  
 `pcReJitIds`  
 fora O número real de IDs recompiladas por JIT.  
  
 `reJitIds`  
 fora Uma matriz alocada pelo chamador que conterá as IDs de compilação JIT recompiladas para a função especificada.  
  
## <a name="remarks"></a>Comentários  
 `GetReJITIDs`enumera as IDs de recompilação JIT ativas para uma determinada instância de função. Ele segue o mesmo padrão de uso que outras `ICorProfilerInfo` funções que aceitam buffers alocados pelo chamador.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo4](icorprofilerinfo4-interface.md)
- [Criação de perfil de interfaces](profiling-interfaces.md)
- [Criação de perfil](index.md)
