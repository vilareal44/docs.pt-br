---
title: Método ICorProfilerModuleEnum::Next
ms.date: 03/30/2017
api_name:
- ICorProfilerModuleEnum.Next Method
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerModuleEnum::Next
helpviewer_keywords:
- ICorProfilerModuleEnum::Next method [.NET Framework profiling]
- Next method, ICorProfilerModuleEnum interface [.NET Framework profiling]
ms.assetid: a3cea59d-7622-4323-897a-0a464c40f77f
topic_type:
- apiref
ms.openlocfilehash: 7a3ad94a4149d6ebb70e077926771e28d7f82779
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84494807"
---
# <a name="icorprofilermoduleenumnext-method"></a>Método ICorProfilerModuleEnum::Next
Obtém o número especificado de módulos contíguos de uma coleção sequencial de módulos, começando na posição atual do enumerador na sequência.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT Next([in]  ULONG      celt,  
             [out, size_is(celt), length_is(*pceltFetched)]  
                    ModuleID ids[],  
             [out] ULONG *   pceltFetched);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `celt`  
 no O número de módulos a serem recuperados.  
  
 `ids`  
 fora Uma matriz de `ModuleID` valores, cada um representando um módulo recuperado.  
  
 `pceltFetched`  
 fora Um ponteiro para o número de elementos realmente retornados na `ids` matriz.  
  
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
  
 **.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerModuleEnum](icorprofilermoduleenum-interface.md)
- [Criação de perfil de interfaces](profiling-interfaces.md)
