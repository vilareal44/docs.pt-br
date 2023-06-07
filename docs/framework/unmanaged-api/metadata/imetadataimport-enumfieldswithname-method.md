---
title: Método IMetaDataImport::EnumFieldsWithName
ms.date: 03/30/2017
api_name:
- IMetaDataImport.EnumFieldsWithName
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::EnumFieldsWithName
helpviewer_keywords:
- IMetaDataImport::EnumFieldsWithName method [.NET Framework metadata]
- EnumFieldsWithName method [.NET Framework metadata]
ms.assetid: 42145e8d-000f-4d0b-ae43-c08201190fa2
topic_type:
- apiref
ms.openlocfilehash: 68261b165847a5c3ee29adbc4908451fb00c5443
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84492259"
---
# <a name="imetadataimportenumfieldswithname-method"></a>Método IMetaDataImport::EnumFieldsWithName
Enumera os tokens FieldDef do tipo especificado com o nome especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT EnumFieldsWithName (  
   [in, out] HCORENUM    *phEnum,
   [in]  mdTypeDef       cl,
   [in]  LPCWSTR         szName,
   [out] mdFieldDef      rFields[],
   [in]  ULONG           cMax,
   [out] ULONG           *pcTokens
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `phEnum`  
 [entrada, saída] Um ponteiro para o enumerador.  
  
 `cl`  
 no O token do tipo cujos campos devem ser enumerados.  
  
 `szName`  
 no O nome do campo que limita o escopo da enumeração.  
  
 `rFields`  
 fora Matriz usada para armazenar os tokens FieldDef.  
  
 `cMax`  
 no O tamanho máximo da `rFields` matriz.  
  
 `pcTokens`  
 fora O número real de tokens FieldDef retornados em `rFields` .  
  
## <a name="remarks"></a>Comentários  
 Ao contrário de [IMetaDataImport:: EnumFields](imetadataimport-enumfields-method.md), `EnumFieldsWithName` descarta todos os tokens de campo que não têm o nome especificado.  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|`S_OK`|`EnumFieldsWithName`retornado com êxito.|  
|`S_FALSE`|Não há campos para enumerar. Nesse caso, `pcTokens` é zero.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
