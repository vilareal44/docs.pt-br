---
title: Método ICorProfilerFunctionEnum::Next
ms.date: 03/30/2017
api_name:
- ICorProfilerFunctionEnum.Next Method
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerFunctionEnum::Next
helpviewer_keywords:
- ICorProfilerFunctionEnum::Next method [.NET Framework profiling]
- Next method, ICorProfilerFunctionEnum interface [.NET Framework profiling]
ms.assetid: 5ed4aa83-ce56-4b9f-9237-5da7587787fe
topic_type:
- apiref
ms.openlocfilehash: df62ad1af0ea91783cb62bb0590b6e36d812de3a
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503062"
---
# <a name="icorprofilerfunctionenumnext-method"></a>Método ICorProfilerFunctionEnum::Next
Obtém o número especificado de funções contíguas de uma coleção sequencial de funções, começando na posição atual do enumerador na sequência.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT Next([in]  ULONG      celt,  
             [out, size_is(celt), length_is(*pceltFetched)]  
                    COR_PRF_FUNCTION ids[],  
             [out] ULONG *   pceltFetched);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `celt`  
 no O número de funções a serem recuperadas.  
  
 `ids`  
 fora Uma matriz de `COR_PRF_FUNCTION` valores, cada um representando uma função recuperada.  
  
 `pceltFetched`  
 fora Um ponteiro para o número de funções realmente retornadas na `ids` matriz.  
  
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

- [Interface ICorProfilerFunctionEnum](icorprofilerfunctionenum-interface.md)
- [Criação de perfil de interfaces](profiling-interfaces.md)
