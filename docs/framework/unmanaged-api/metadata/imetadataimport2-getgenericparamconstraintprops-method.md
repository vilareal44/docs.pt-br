---
title: Método IMetaDataImport2::GetGenericParamConstraintProps
ms.date: 03/30/2017
api_name:
- IMetaDataImport2.GetGenericParamConstraintProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport2::GetGenericParamConstraintProps
helpviewer_keywords:
- IMetaDataImport2::GetGenericParamConstraintProps method [.NET Framework metadata]
- GetGenericParamConstraintProps method [.NET Framework metadata]
ms.assetid: c5fee4a0-b132-4e5e-8730-e586ce314b9a
topic_type:
- apiref
ms.openlocfilehash: 8a1cdff313dae73e3f5e8918ff2ef395c80b115d
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84490621"
---
# <a name="imetadataimport2getgenericparamconstraintprops-method"></a>Método IMetaDataImport2::GetGenericParamConstraintProps
Obtém os metadados associados à restrição de parâmetro genérico representada pelo token de restrição especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetGenericParamConstraintProps (  
   [in]  mdGenericParamConstraint  gpc,  
   [out] mdGenericParam            *ptGenericParam,  
   [out] mdToken                   *ptkConstraintType  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `gpc`  
 no O token para a restrição de parâmetro genérico para a qual retornar os metadados.  
  
 `ptGenericParam`  
 fora Um ponteiro para o token que representa o parâmetro genérico que é restrito.  
  
 `ptkConstraintType`  
 fora Um ponteiro para um token de TypeDef, TypeRef ou TypeSpec que representa uma restrição em `ptGenericParam` .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport2](imetadataimport2-interface.md)
- [Interface IMetaDataImport](imetadataimport-interface.md)
