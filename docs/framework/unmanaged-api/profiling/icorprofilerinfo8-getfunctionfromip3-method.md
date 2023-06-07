---
title: ICorProfilerInfo8::GetFunctionFromIP3
ms.date: 08/06/2019
dev_langs:
- cpp
api_name:
- ICorProfilerInfo8.GetFunctionFromIP3
api_location:
- mscorwks.dll
api_type:
- COM
author: davmason
ms.author: davmason
ms.openlocfilehash: 6822757608429ca5f4ef9520ab7574d440b67b26
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84495250"
---
# <a name="icorprofilerinfo8getfunctionfromip3-method"></a>Método ICorProfilerInfo8:: GetFunctionFromIP3

Mapeia um ponteiro de instrução de código gerenciado para uma FunctionID. Esse método funciona para métodos dinâmicos e não dinâmicos.

## <a name="syntax"></a>Sintaxe

```cpp
HRESULT GetFunctionFromIP3([in] LPCBYTE ip,
                           [out] FunctionID *functionId,
                           [out] ReJITID * pReJitId);
```

## <a name="parameters"></a>Parâmetros

- `ip`

  \[in] o ponteiro de instrução no código gerenciado.

- `pFunctionId`

  \[out] a ID da função.

- `pReJitId`

  \[out] a identidade da versão recompilada do JIT da função.

## <a name="remarks"></a>Comentários

Esse método funciona para métodos dinâmicos e não dinâmicos. É um superconjunto de [GetFunctionFromIP2](icorprofilerinfo4-getfunctionfromip2-method.md), que funciona apenas para funções com metadados.

## <a name="requirements"></a>Requisitos

**Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).

**Cabeçalho:** CorProf. idl, CorProf. h

**Biblioteca:** CorGuids.lib

**.NET Framework versões:**[!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo8](icorprofilerinfo8-interface.md)
