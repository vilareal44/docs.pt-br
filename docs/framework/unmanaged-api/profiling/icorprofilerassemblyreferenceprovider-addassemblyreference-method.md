---
title: ICorProfilerAssemblyReferenceProvider::Método AddAssemblyReference
ms.date: 03/30/2017
dev_langs:
- cpp
api_name:
- ICorProfilerAssemblyReferenceProvider.AddAssemblyReference
api_location:
- mscorwks.dll
api_type:
- COM
ms.assetid: 3d5af8e7-c337-48f4-9fa6-97c83878b9b1
topic_type:
- apiref
ms.openlocfilehash: 6d732c6c2871cc5e042b8504db26aabb19963f8c
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500501"
---
# <a name="icorprofilerassemblyreferenceprovideraddassemblyreference-method"></a>ICorProfilerAssemblyReferenceProvider::Método AddAssemblyReference
[Com suporte no .NET Framework 4.5.2 e versões posteriores]  
  
 Informa o Common Language Runtime (CLR) de uma referência de assembly que o criador de perfil planeja adicionar ao retorno de chamada [ICorProfilerCallback:: ModuleLoadFinished](icorprofilercallback-moduleloadfinished-method.md) .  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp
HRESULT AddAssemblyReference(  
        const COR_PRF_ASSEMBLY_REFERENCE_INFO* pAssemblyRefInfo  
);  
```  
  
## <a name="parameters"></a>Parâmetros

- `pAssemblyRefInfo`

  Um ponteiro para uma estrutura de [COR_PRF_ASSEMBLY_REFERENCE_INFO](cor-prf-assembly-reference-info-structure.md) que fornece ao CLR informações sobre uma referência de assembly que deve ser considerada ao executar uma movimentação de fechamento de referência de assembly.
  
## <a name="remarks"></a>Comentários  
 O criador de perfil chama esse método para cada assembly de destino que planeja fazer referência a partir do assembly especificado no `wszAssemblyPath` argumento do retorno de chamada [ICorProfilerCallback6:: GetAssemblyReferences](icorprofilercallback6-getassemblyreferences-method.md) . O objeto da interface [ICorProfilerAssemblyReferenceProvider](icorprofilerassemblyreferenceprovider-interface.md) é passado para o retorno de chamada [ICorProfilerCallback6:: GetAssemblyReferences](icorprofilercallback6-getassemblyreferences-method.md) do criador de perfil, juntamente com o caminho e o nome do assembly no `wszAssemblyPath` argumento.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerAssemblyReferenceProvider](icorprofilerassemblyreferenceprovider-interface.md)
- [Método GetAssemblyReferences](icorprofilercallback6-getassemblyreferences-method.md)
- [Método ModuleLoadFinished](icorprofilercallback-moduleloadfinished-method.md)
