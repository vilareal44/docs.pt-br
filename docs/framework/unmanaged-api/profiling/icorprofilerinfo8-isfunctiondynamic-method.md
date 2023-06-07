---
title: ICorProfilerInfo8::IsFunctionDynamic
ms.date: 08/06/2019
dev_langs:
- cpp
api_name:
- ICorProfilerInfo8.IsFunctionDynamic
api_location:
- mscorwks.dll
api_type:
- COM
author: davmason
ms.author: davmason
ms.openlocfilehash: c88279d361ea78a2e910c4621e92c500902d9124
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84495119"
---
# <a name="icorprofilerinfo8isfunctiondynamic-method"></a>Método ICorProfilerInfo8:: IsFunctionDynamic

Determina se uma função não tem metadados associados.

## <a name="syntax"></a>Sintaxe

```cpp
HRESULT IsFunctionDynamic( [in]  FunctionID  functionId,
                           [out] BOOL        *isDynamic);
```

## <a name="parameters"></a>Parâmetros

- `functionId`

  \[in], o `FunctionID` que identifica a função em questão.

- `isDynamic`

  \[out] um ponteiro para um `BOOL` que conterá um valor que indica se a função não tem metadados.

## <a name="remarks"></a>Comentários

Uma função será considerada dinâmica se não tiver metadados. Determinados métodos, como stubs IL ou métodos LCG, não têm metadados associados que podem ser recuperados usando as APIs do IMetaDataImport. Esses métodos podem ser encontrados por infilers por meio de ponteiros de instrução ou ouvindo [ICorProfilerCallback::D ynamicmethodjitcompilationstarted](icorprofilercallback8-dynamicmethodjitcompilationstarted-method.md).

## <a name="requirements"></a>Requisitos

**Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).

**Cabeçalho:** CorProf. idl, CorProf. h

**Biblioteca:** CorGuids.lib

**.NET Framework versões:**[!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo8](icorprofilerinfo8-interface.md)
