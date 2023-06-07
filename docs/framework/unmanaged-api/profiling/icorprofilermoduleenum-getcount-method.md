---
title: Método ICorProfilerModuleEnum::GetCount
ms.date: 03/30/2017
api_name:
- ICorProfilerModuleEnum.GetCount Method
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerModuleEnum::GetCount
helpviewer_keywords:
- ICorProfilerModuleEnum::GetCount method [.NET Framework profiling]
- GetCount method, ICorProfilerModuleEnum interface [.NET Framework profiling]
ms.assetid: f0a4a5e0-4689-474b-b0f4-37ca0639c918
topic_type:
- apiref
ms.openlocfilehash: 604ecb2122cce6e24f0e5168fa286a523d8bb4f7
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84495067"
---
# <a name="icorprofilermoduleenumgetcount-method"></a>Método ICorProfilerModuleEnum::GetCount
Obtém o número de módulos gerenciados que foram carregados no aplicativo.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetCount([out] ULONG * pcelt);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `celt`  
 fora O número de módulos de tempo de execução na coleção.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerModuleEnum](icorprofilermoduleenum-interface.md)
- [Criação de perfil de interfaces](profiling-interfaces.md)
