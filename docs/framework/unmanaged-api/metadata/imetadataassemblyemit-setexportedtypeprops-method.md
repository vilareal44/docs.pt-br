---
title: Método IMetaDataAssemblyEmit::SetExportedTypeProps
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyEmit.SetExportedTypeProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyEmit::SetExportedTypeProps
helpviewer_keywords:
- SetExportedTypeProps method [.NET Framework metadata]
- IMetaDataAssemblyEmit::SetExportedTypeProps method [.NET Framework metadata]
ms.assetid: 1c090153-fd5f-46c7-9cff-39a78d992c8f
topic_type:
- apiref
ms.openlocfilehash: fa4f1f57cb8fe1ca81bbad6438a88bb43c48e7bf
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84008073"
---
# <a name="imetadataassemblyemitsetexportedtypeprops-method"></a>Método IMetaDataAssemblyEmit::SetExportedTypeProps
Modifica a estrutura de `ExportedType` metadados especificada.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SetExportedTypeProps (  
    [in] mdExportedType   ct,
    [in] mdToken          tkImplementation,  
    [in] mdTypeDef        tkTypeDef,  
    [in] DWORD            dwExportedTypeFlags  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `ct`  
 no O token de metadados que especifica a `ExportedType` estrutura de metadados a ser modificada.  
  
 `tkImplementation`  
 no O token, do tipo `File` , `AssemblyRef` ou `ExportedType` , que especifica como esse tipo é implementado.  
  
 `tkTypeDef`  
 no O `TypeDef` token referenciado no arquivo de código.  
  
 `dwExportedTypeFlags`  
 no Uma combinação de bits de valores que especifica atributos do tipo.  
  
## <a name="remarks"></a>Comentários  
 Para criar uma `ExportedType` estrutura de metadados, use o método [IMetaDataAssemblyEmit::D efineexportedtype](imetadataassemblyemit-defineexportedtype-method.md) .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataAssemblyEmit](imetadataassemblyemit-interface.md)
