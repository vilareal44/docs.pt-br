---
title: Método IMetaDataEmit::SetPermissionSetProps
ms.date: 03/30/2017
api_name:
- IMetaDataEmit.SetPermissionSetProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit::SetPermissionSetProps
helpviewer_keywords:
- SetPermissionSetProps method [.NET Framework metadata]
- IMetaDataEmit::SetPermissionSetProps method [.NET Framework metadata]
ms.assetid: 8eaee971-40bf-45e2-a3d8-6e57674213b6
topic_type:
- apiref
ms.openlocfilehash: 1e6ee1f2f497ef30a5390e7afac55c54705248ed
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84007800"
---
# <a name="imetadataemitsetpermissionsetprops-method"></a>Método IMetaDataEmit::SetPermissionSetProps
Define ou atualiza recursos da assinatura de metadados de um conjunto de permissões definido por uma chamada anterior para [IMetaDataEmit::D efinepermissionset](imetadataemit-definepermissionset-method.md).  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SetPermissionSetProps (
    [in]  mdToken         tk,
    [in]  DWORD           dwAction,
    [in]  void const      *pvPermission,
    [in]  ULONG           cbPermission,
    [out] mdPermission    *ppm
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `tk`  
 no Um token de metadados que representa o objeto a ser decorado.  
  
 `dwAction`  
 no Um valor de [CorDeclSecurity](cordeclsecurity-enumeration.md) que especifica o tipo de segurança declarativa a ser usado.  
  
 `pvPermission`  
 no O BLOB de permissão.  
  
 `cbPermission`  
 no O tamanho, em bytes, de `pvPermission` .  
  
 `ppm`  
 fora Um `mdPermission` token de metadados que representa as permissões atualizadas.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataEmit](imetadataemit-interface.md)
- [Interface IMetaDataEmit2](imetadataemit2-interface.md)
