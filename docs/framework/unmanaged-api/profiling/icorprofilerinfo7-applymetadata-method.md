---
title: 'Método ICorProfilerInfo7:: ApplyMetaData'
ms.date: 02/15/2019
dev_langs:
- cpp
api_name:
- ICorProfilerInfo7.ApplyMetaData
api_location:
- mscorwks.dll
api_type:
- COM
ms.assetid: a1bfb649-4584-4d35-b3e6-8fe59b53992a
ms.openlocfilehash: c30206145d08a22af49c4a6a0dc83fd7382bcc06
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84495497"
---
# <a name="icorprofilerinfo7applymetadata-method"></a>Método ICorProfilerInfo7:: ApplyMetaData
[Com suporte no .NET Framework 4.6.1 e versões posteriores]  
  
 Aplica os metadados recentemente definidos pelos `IMetadataEmit::Define*` métodos a um módulo especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp
HRESULT ApplyMetaData(  
        [in] ModuleID moduleID  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `moduleID`  
 no O identificador do módulo cujos metadados foram alterados.  
  
## <a name="remarks"></a>Comentários  
 Se forem feitas alterações de metadados após o retorno de chamada [ModuleLoadFinished](icorprofilercallback-moduleloadfinished-method.md) , você deverá chamar esse método antes de usar os novos metadados.  
  
 `ApplyMetaData`o só dá suporte à adição dos seguintes tipos de metadados:  
  
- `AssemblyRef`registros, que você cria chamando o [IMetaDataAssemblyEmit::D efineassemblyref](../metadata/imetadataassemblyemit-defineassemblyref-method.md). método.  
  
- `TypeRef`registros, que você cria chamando o método [IMetaDataEmit::D efinetyperefbyname](../metadata/imetadataemit-definetyperefbyname-method.md) .  
  
- `TypeSpec`registros, que você cria chamando o método [IMetaDataEmit:: GetTokenFromTypeSpec](../metadata/imetadataemit-gettokenfromtypespec-method.md) .  
  
- `MemberRef`registros, que você cria chamando o método [IMetaDataEmit::D efinememberref](../metadata/imetadataemit-definememberref-method.md) .  
  
- `MemberSpec`registros, que você cria chamando o método [IMetaDataEmit2::D efinemethodspec](../metadata/imetadataemit2-definemethodspec-method.md) .  
  
- `UserString`registros, que você cria chamando o método [IMetaDataEmit::D efineuserstring](../metadata/imetadataemit-defineuserstring-method.md) .  

A partir do .NET Core 3,0, `ApplyMetaData` o também oferece suporte aos seguintes tipos:

- `TypeDef`registros, que você cria chamando o método [IMetaDataEmit::D efinetypedef](../metadata/imetadataemit-definetypedef-method.md) .

- `MethodDef`registros, que você cria chamando o método [IMetaDataEmit::D efinemethod](../metadata/imetadataemit-definemethod-method.md) . No entanto, não há suporte para a adição de métodos virtuais a um tipo existente. Métodos virtuais devem ser adicionados antes do retorno de chamada [ModuleLoadFinished](icorprofilercallback-moduleloadfinished-method.md) .

## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v461plus](../../../../includes/net-current-v461plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo7](icorprofilerinfo7-interface.md)
