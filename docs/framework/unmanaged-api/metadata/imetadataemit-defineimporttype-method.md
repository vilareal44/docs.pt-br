---
title: Método IMetaDataEmit::DefineImportType
ms.date: 03/30/2017
api_name:
- IMetaDataEmit.DefineImportType
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit::DefineImportType
helpviewer_keywords:
- DefineImportType method [.NET Framework metadata]
- IMetaDataEmit::DefineImportType method [.NET Framework metadata]
ms.assetid: 37fd27af-8062-4904-ace4-51bb78ec600a
topic_type:
- apiref
ms.openlocfilehash: edce5cb93b770fb5730e5a06633ffffacf332f7a
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84004680"
---
# <a name="imetadataemitdefineimporttype-method"></a>Método IMetaDataEmit::DefineImportType
Cria uma referência ao tipo especificado que é definido fora do escopo atual e define um token para essa referência.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT DefineImportType (
    [in]  IMetaDataAssemblyImport  *pAssemImport,
    [in]  const void               *pbHashValue,
    [in]  ULONG                    cbHashValue,
    [in]  IMetaDataImport          *pImport,
    [in]  mdTypeDef                tdImport,
    [in]  IMetaDataAssemblyEmit    *pAssemEmit,
    [out] mdTypeRef                *ptr  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pAssemImport`  
 no Uma interface [IMetaDataAssemblyImport](imetadataassemblyimport-interface.md) que representa o assembly do qual o tipo de destino é importado.  
  
 `pbHashValue`  
 no Uma matriz que contém o hash para o assembly especificado por `pAssemImport` .  
  
 `cbHashValue`  
 no O número de bytes na `pbHashValue` matriz.  
  
 `pImport`  
 no Uma interface [IMetaDataImport](imetadataimport-interface.md) que representa o escopo de metadados do qual o tipo de destino é importado.  
  
 `tdImport`  
 no Um `mdTypeDef` token que especifica o tipo de destino.  
  
 `pAssemEmit`  
 no Uma interface [IMetaDataAssemblyEmit](imetadataassemblyemit-interface.md) que representa o assembly no qual o tipo de destino é importado.  
  
 `ptr`  
 fora O `mdTypeRef` token que é definido no escopo atual para a referência de tipo.  
  
## <a name="remarks"></a>Comentários  
 Antes de chamar o método [IMetaDataEmit::D efineimportmember](imetadataemit-defineimportmember-method.md) , você pode usar o `DefineImportType` método para criar uma referência de tipo, no escopo atual, para a classe pai ou a interface pai do membro.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataEmit](imetadataemit-interface.md)
- [Interface IMetaDataEmit2](imetadataemit2-interface.md)
