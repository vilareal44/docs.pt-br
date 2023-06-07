---
title: Método IMetaDataEmit::SetEventProps
ms.date: 03/30/2017
api_name:
- IMetaDataEmit.SetEventProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit::SetEventProps
helpviewer_keywords:
- IMetaDataEmit::SetEventProps method [.NET Framework metadata]
- SetEventProps method [.NET Framework metadata]
ms.assetid: 3b039e50-63ec-4730-99ff-2327408de477
topic_type:
- apiref
ms.openlocfilehash: 720133e64c02aa09c9ff7e43a20630b0d55c1acf
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84008749"
---
# <a name="imetadataemitseteventprops-method"></a>Método IMetaDataEmit::SetEventProps
Define ou atualiza o recurso especificado de um evento definido por uma chamada anterior para [IMetaDataEmit::D efineevent](imetadataemit-defineevent-method.md).  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SetEventProps (  
    [in]  mdEvent     ev,
    [in]  DWORD       dwEventFlags,
    [in]  mdToken     tkEventType,
    [in]  mdMethodDef mdAddOn,
    [in]  mdMethodDef mdRemoveOn,
    [in]  mdMethodDef mdFire,
    [in]  mdMethodDef rmdOtherMethods[]
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `ev`  
 no O token do evento.  
  
 `dwEventFlags`  
 no Sinalizadores de eventos. Esta é uma bitmask de `CorEventAttr` valores.  
  
 `tkEventType`  
 no O token para a classe de evento. Este é um `mdTypeDef` `mdTypeRef` token ou.  
  
 `mdAddOn`  
 no O método usado para assinar o evento ou nulo.  
  
 `mdRemoveOn`  
 no O método usado para cancelar a assinatura do evento, ou NULL.  
  
 `mdFire`  
 no O método usado (por uma classe derivada) para gerar o evento.  
  
 `rmdOtherMethods[]`  
 no Uma matriz de tokens para outros métodos associados ao evento. O último elemento da matriz deve ser `mdMethodDefNil` .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataEmit](imetadataemit-interface.md)
- [Interface IMetaDataEmit2](imetadataemit2-interface.md)
