---
title: Método ICorProfilerThreadEnum::Next
ms.date: 03/30/2017
api_name:
- ICorProfilerThreadEnum.Next
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerThreadEnum::Next
helpviewer_keywords:
- ICorProfilerThreadEnum::Next method [.NET Framework profiling]
- Next method, ICorProfilerThreadEnum interface [.NET Framework profiling]
ms.assetid: f3535279-3c63-41a2-ab0e-a129dc5a01e8
topic_type:
- apiref
ms.openlocfilehash: 37d1acfa70a1a2b2c18fb34fb5d6024f3b61e2ab
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84494339"
---
# <a name="icorprofilerthreadenumnext-method"></a>Método ICorProfilerThreadEnum::Next
Obtém o número especificado de threads contíguos de uma coleção sequencial de threads, começando na posição atual do enumerador na sequência.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT Next (    [in]  ULONG      celt,  
    [out, size_is(celt), length_is(*pceltFetched)]  
                    ThreadID ids[],  
    [out] ULONG *   pceltFetched  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `celt`  
 no O número de threads a serem recuperados.  
  
 `ids`  
 fora Uma matriz de `ThreadID` valores, cada um representando um thread recuperado.  
  
 `pceltFetched`  
 fora Um ponteiro para o número de threads realmente retornados na `ids` matriz.  
  
## <a name="return-value"></a>Valor Retornado  
 Esse método retorna os HRESULTs específicos a seguir, bem como os erros de HRESULT que indicam falha de método.  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|`celt`elementos foram retornados.|  
|S_FALSE|Menos de `celt` elementos foram retornados, o que indica que a enumeração foi concluída.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerThreadEnum](icorprofilerthreadenum-interface.md)
- [Criação de perfil de interfaces](profiling-interfaces.md)
