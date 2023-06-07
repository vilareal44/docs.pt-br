---
title: Método IMetaDataEmit::DefineTypeRefByName
ms.date: 03/30/2017
api_name:
- IMetaDataEmit.DefineTypeRefByName
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit::DefineTypeRefByName
helpviewer_keywords:
- DefineTypeRefByName method [.NET Framework metadata]
- IMetaDataEmit::DefineTypeRefByName method [.NET Framework metadata]
ms.assetid: c30a4ce3-2d3e-411a-98df-e62ac4a5dd50
topic_type:
- apiref
ms.openlocfilehash: ae30140e69926be32570960a32524a74b850bda4
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84009347"
---
# <a name="imetadataemitdefinetyperefbyname-method"></a>Método IMetaDataEmit::DefineTypeRefByName
Obtém um token de metadados para um tipo que é definido no escopo especificado, que está fora do escopo atual.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT DefineTypeRefByName (
    [in]  mdToken     tkResolutionScope,
    [in]  LPCWSTR     szName,
    [out] mdTypeRef   *ptr
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `tkResolutionScope`  
 no O token que especifica o escopo de resolução. Os seguintes tipos de token são válidos:  
  
- `mdModuleRef`, se o tipo for definido no mesmo assembly no qual o chamador é definido.  
  
- `mdAssemblyRef`, se o tipo for definido em um assembly diferente daquele em que o chamador é definido.  
  
- `mdTypeRef`, se o tipo for um tipo aninhado.  
  
- `mdModule`, se o tipo for definido no mesmo módulo no qual o chamador é definido.  
  
- NULL, se o tipo for definido globalmente.  
  
 `szName`  
 no O nome do tipo de destino em Unicode.  
  
 `ptr`  
 fora Um ponteiro para o `mdTypeRef` token que é atribuído ao tipo.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataEmit](imetadataemit-interface.md)
- [Interface IMetaDataEmit2](imetadataemit2-interface.md)
