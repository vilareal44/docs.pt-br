---
title: Método IMetaDataAssemblyEmit::SetManifestResourceProps
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyEmit.SetManifestResourceProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyEmit::SetManifestResourceProps
helpviewer_keywords:
- SetManifestResourceProps method [.NET Framework metadata]
- IMetaDataAssemblyEmit::SetManifestResourceProps method [.NET Framework metadata]
ms.assetid: ef77efd1-849c-4e51-ba92-7ee3d2bf0339
topic_type:
- apiref
ms.openlocfilehash: 74111a175b0decbc1beef7c8df5ade59d31d845b
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84009139"
---
# <a name="imetadataassemblyemitsetmanifestresourceprops-method"></a>Método IMetaDataAssemblyEmit::SetManifestResourceProps
Modifica a estrutura de `ManifestResource` metadados especificada.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SetManifestResourceProps (  
    [in] mdManifestResource  mr,  
    [in] mdToken             tkImplementation,
    [in] DWORD               dwOffset,  
    [in] DWORD               dwResourceFlags  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `mr`  
 no O token que especifica a `ManifestResource` estrutura de metadados a ser modificada.  
  
 `tkImplementation`  
 no O token, do tipo `File` ou `AssemblyRef` , que é mapeado para o provedor de recursos.  
  
 `dwOffset`  
 no O deslocamento para o início do recurso dentro do arquivo.  
  
 `dwResourceFlags`  
 no Uma combinação de bits de valor de sinalizador que especifica os atributos do recurso.  
  
## <a name="remarks"></a>Comentários  
 Para criar uma `ManifestResource` estrutura de metadados, use o método [IMetaDataAssemblyEmit::D efinemanifestresource](imetadataassemblyemit-definemanifestresource-method.md) .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataAssemblyEmit](imetadataassemblyemit-interface.md)
