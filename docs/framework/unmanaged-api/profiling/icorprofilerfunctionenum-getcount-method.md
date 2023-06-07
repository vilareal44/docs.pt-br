---
title: Método ICorProfilerFunctionEnum::GetCount
ms.date: 03/30/2017
api_name:
- ICorProfilerFunctionEnum.GetCount Method
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerFunctionEnum::GetCount
helpviewer_keywords:
- ICorProfilerFunctionEnum::GetCount method [.NET Framework profiling]
- GetCount method, ICorProfilerFunctionEnum interface [.NET Framework profiling]
ms.assetid: 62ec65e3-3e9d-400b-ae61-d24b8963995b
topic_type:
- apiref
ms.openlocfilehash: 137b1da853535985b2fd383d52f0bcfc48f728ed
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503088"
---
# <a name="icorprofilerfunctionenumgetcount-method"></a>Método ICorProfilerFunctionEnum::GetCount
Obtém o número de funções que foram carregadas pelo aplicativo ou carregadas de modo forçado pelo criador de perfil.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetCount([out] ULONG * pcelt);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `celt`  
 fora O número de funções que foram carregadas.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerFunctionEnum](icorprofilerfunctionenum-interface.md)
- [Criação de perfil de interfaces](profiling-interfaces.md)
