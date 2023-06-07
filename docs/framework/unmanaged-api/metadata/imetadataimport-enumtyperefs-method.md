---
title: Método IMetaDataImport::EnumTypeRefs
ms.date: 03/30/2017
api_name:
- IMetaDataImport.EnumTypeRefs
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::EnumTypeRefs
helpviewer_keywords:
- EnumTypeRefs method [.NET Framework metadata]
- IMetaDataImport::EnumTypeRefs method [.NET Framework metadata]
ms.assetid: b4896b8f-8e97-469c-8089-e72a025661b5
topic_type:
- apiref
ms.openlocfilehash: 0c7e96c50e59902cde4686f908047a86dd2b6a47
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503738"
---
# <a name="imetadataimportenumtyperefs-method"></a>Método IMetaDataImport::EnumTypeRefs
Enumera tokens TypeRef definidos no escopo de metadados atual.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT EnumTypeRefs (  
   [in, out] HCORENUM    *phEnum,
   [out] mdTypeRef       rTypeRefs[],  
   [in]  ULONG           cMax,
   [out] ULONG           *pcTypeRefs  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `phEnum`  
 [entrada, saída] Um ponteiro para o enumerador. Isso deve ser nulo para a primeira chamada deste método.  
  
 `rTypeRefs`  
 fora A matriz usada para armazenar os tokens TypeRef.  
  
 `cMax`  
 no O tamanho máximo da `rTypeRefs` matriz.  
  
 `pcTypeRefs`  
 fora Um ponteiro para o número de tokens TypeRef retornado em `rTypeRefs` .  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|`S_OK`|`EnumTypeRefs`retornado com êxito.|  
|`S_FALSE`|Não há tokens para enumerar. Nesse caso, `pcTypeRefs` é zero.|  
  
## <a name="remarks"></a>Comentários  
 Um token TypeRef representa uma referência a um tipo.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
