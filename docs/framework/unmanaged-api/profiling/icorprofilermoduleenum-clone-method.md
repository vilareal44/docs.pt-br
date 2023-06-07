---
title: Método ICorProfilerModuleEnum::Clone
ms.date: 03/30/2017
api_name:
- ICorProfilerModuleEnum.Clone Method
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerModuleEnum::Clone
helpviewer_keywords:
- Clone method, ICorProfilerModuleEnum interface [.NET Framework profiling]
- ICorProfilerModuleEnum::Clone method [.NET Framework profiling]
ms.assetid: 7dec7e36-8d88-416d-b437-abf98b76c1df
topic_type:
- apiref
ms.openlocfilehash: ccbb051ac30fb25199ce12c16fff74bb0b9944fa
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84495080"
---
# <a name="icorprofilermoduleenumclone-method"></a>Método ICorProfilerModuleEnum::Clone
Obtém um ponteiro de interface para uma cópia desta interface [ICorProfilerModuleEnum](icorprofilermoduleenum-interface.md) .  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT Clone([out] ICorProfilerObjectEnum **ppEnum);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `ppEnum`  
 fora Um ponteiro para o ponteiro de interface que, por sua vez, aponta para a cópia dessa interface [ICorProfilerModuleEnum](icorprofilermoduleenum-interface.md) . A cópia do enumerador mantém seu próprio estado de enumeração separadamente deste enumerador. No entanto, a posição inicial do cursor da cópia é igual à posição atual do cursor do enumerador.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerModuleEnum](icorprofilermoduleenum-interface.md)
- [Criação de perfil de interfaces](profiling-interfaces.md)
