---
title: Método IMetaDataImport::GetMethodSemantics
ms.date: 03/30/2017
api_name:
- IMetaDataImport.GetMethodSemantics
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetMethodSemantics
helpviewer_keywords:
- GetMethodSemantics method [.NET Framework metadata]
- IMetaDataImport::GetMethodSemantics method [.NET Framework metadata]
ms.assetid: 5e018eaa-d60e-4a0b-a2c5-8c36bd09d905
topic_type:
- apiref
ms.openlocfilehash: 2cfb66203d8f2d69ea188f6913a5ef34dd74791e
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503595"
---
# <a name="imetadataimportgetmethodsemantics-method"></a>Método IMetaDataImport::GetMethodSemantics
Obtém sinalizadores que indicam a relação entre o método referenciado pelo token MethodDef especificado e a propriedade emparelhada e o evento referenciado pelo token EventProp especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetMethodSemantics (  
   [in]  mdMethodDef   mb,  
   [in]  mdToken       tkEventProp,  
   [out] DWORD         *pdwSemanticsFlags  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `mb`  
 no Um token MethodDef que representa o método para obter as informações de função semântica para.  
  
 `tkEventProp`  
 no Um token que representa a propriedade emparelhada e o evento para o qual obter a função do método.  
  
 `pdwSemanticsFlags`  
 fora Um ponteiro para os sinalizadores de semântica associados. Esse valor é um bitmask da enumeração [CorMethodSemanticsAttr](cormethodsemanticsattr-enumeration.md) .  
  
## <a name="remarks"></a>Comentários  
 O método [IMetaDataEmit::D efineproperty](imetadataemit-defineproperty-method.md) define os sinalizadores de semântica de um método.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
