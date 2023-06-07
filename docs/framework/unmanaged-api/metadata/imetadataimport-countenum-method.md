---
title: Método IMetaDataImport::CountEnum
ms.date: 03/30/2017
api_name:
- IMetaDataImport.CountEnum
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::CountEnum
helpviewer_keywords:
- CountEnum method [.NET Framework metadata]
- IMetaDataImport::CountEnum method [.NET Framework metadata]
ms.assetid: d1de53ad-9435-4b5f-9df7-07f21210e5b5
topic_type:
- apiref
ms.openlocfilehash: 4521a3f15ec358a4d786a4533efb6b99d0e1c1cc
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84492376"
---
# <a name="imetadataimportcountenum-method"></a>Método IMetaDataImport::CountEnum
Obtém o número de elementos na enumeração que foi recuperado pelo enumerador especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT CountEnum (  
   [in]  HCORENUM    hEnum,
   [out] ULONG       *pulCount  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `hEnum`  
 no O identificador do enumerador.  
  
 `pulCount`  
 fora O número de elementos enumerados.  
  
## <a name="remarks"></a>Comentários  
 O identificador especificado por `hEnum` é obtido de uma chamada de `Enum` *nome* anterior (por exemplo, [IMetaDataImport:: EnumTypeDefs](imetadataimport-enumtypedefs-method.md)).  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
