---
title: Interface ICorProfilerInfo8
ms.date: 08/06/2019
author: davmason
ms.author: davmason
ms.openlocfilehash: 2cca915eda44d73aea7469e5f752c57309c2300d
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84495158"
---
# <a name="icorprofilerinfo8-interface"></a>Interface ICorProfilerInfo8

Uma subclasse de [ICorProfilerInfo7](icorprofilerinfo7-interface.md) que fornece métodos para consultar informações sobre métodos dinâmicos.

## <a name="methods"></a>Métodos  

| Método|Descrição|  
| ------------|-----------------|  
|[Método IsFunctionDynamic](icorprofilerinfo8-isfunctiondynamic-method.md)| Determina se uma função não tem metadados associados.|
|[Método GetFunctionFromIP3](icorprofilerinfo8-getfunctionfromip3-method.md)| Mapeia um ponteiro de instrução de código gerenciado para uma FunctionID. Esse método funciona para métodos dinâmicos e não dinâmicos. |
|[Método GetDynamicFunctionInfo](icorprofilerinfo8-getdynamicfunctioninfo-method.md)| Recupera informações sobre métodos dinâmicos. |

## <a name="requirements"></a>Requisitos  
**Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
**Cabeçalho:** CorProf. idl, CorProf. h  
**.NET Framework versões:**[!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]  

## <a name="see-also"></a>Confira também

- [Criação de perfil de interfaces](profiling-interfaces.md)
