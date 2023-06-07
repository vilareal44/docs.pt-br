---
title: Método ICorProfilerInfo3::EnumModules
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo3.EnumModules Method
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo3::EnumModules
helpviewer_keywords:
- ICorProfilerInfo3::EnumModules method [.NET Framework profiling]
- EnumModules method [.NET Framework profiling]
ms.assetid: 1bf00b42-69da-4019-91b3-bf88026e83fb
topic_type:
- apiref
ms.openlocfilehash: 85adf2dbdbb8c02192a9017bc4f664274a08ee24
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84496575"
---
# <a name="icorprofilerinfo3enummodules-method"></a>Método ICorProfilerInfo3::EnumModules
Retorna um enumerador que fornece métodos para iterar em sequência por meio de uma coleção de módulos gerenciados que são carregados no aplicativo.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT EnumModules([out] ICorProfilerModuleEnum** ppEnum);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `ppEnum`  
 fora Um ponteiro para uma interface [ICorProfilerModuleEnum](icorprofilermoduleenum-interface.md) .  
  
## <a name="remarks"></a>Comentários  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerFunctionEnum](icorprofilerfunctionenum-interface.md)
- [Interface ICorProfilerInfo3](icorprofilerinfo3-interface.md)
- [Criação de perfil de interfaces](profiling-interfaces.md)
- [Criação de perfil](index.md)
