---
title: Estrutura COR_FIELD_OFFSET
ms.date: 03/30/2017
api_name:
- COR_FIELD_OFFSET
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- COR_FIELD_OFFSET
helpviewer_keywords:
- COR_FIELD_OFFSET structure [.NET Framework metadata]
ms.assetid: cced5298-277f-4a5a-8ecf-a0050c1096ea
topic_type:
- apiref
ms.openlocfilehash: 8cc803e3cf1442d324bf2eed0a37d0d236acd86d
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84493052"
---
# <a name="cor_field_offset-structure"></a>Estrutura COR_FIELD_OFFSET
Armazena o deslocamento, dentro de uma classe, do campo especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef struct COR_FIELD_OFFSET {  
    mdFieldDef  ridOfField;  
    ULONG       ulOffset;  
} COR_FIELD_OFFSET;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`ridOfField`|Um `mdFieldDef` token de metadados que representa o campo.|  
|`ulOffset`|O deslocamento do campo dentro de sua classe.|  
  
## <a name="remarks"></a>Comentários  
 Os métodos [IMetaDataImport:: GetClassLayout](imetadataimport-getclasslayout-method.md) e [IMetaDataEmit:: SetClassLayout](imetadataemit-setclasslayout-method.md) usam um parâmetro do tipo `COR_FIELD_OFFSET` .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorHdr. h, CorProf. idl  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Estruturas de metadados](metadata-structures.md)
- [Interface IMetaDataEmit](imetadataemit-interface.md)
- [Interface IMetaDataImport](imetadataimport-interface.md)
