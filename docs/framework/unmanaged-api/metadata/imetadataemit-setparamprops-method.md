---
title: Método IMetaDataEmit::SetParamProps
ms.date: 03/30/2017
api_name:
- IMetaDataEmit.SetParamProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit::SetParamProps
helpviewer_keywords:
- IMetaDataEmit::SetParamProps method [.NET Framework metadata]
- SetParamProps method [.NET Framework metadata]
ms.assetid: a95a3908-9f87-4084-937e-8e01ef03ad63
topic_type:
- apiref
ms.openlocfilehash: b710f966f519e2702607b7e186fff5986110d391
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84007813"
---
# <a name="imetadataemitsetparamprops-method"></a>Método IMetaDataEmit::SetParamProps
Define ou altera recursos de um parâmetro de método que foi definido por uma chamada anterior para [IMetaDataEmit::D efineparam](imetadataemit-defineparam-method.md).  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SetParamProps (
    [in]  mdParamDef  pd,
    [in]  LPCWSTR     szName,
    [in]  DWORD       dwParamFlags,
    [in]  DWORD       dwCPlusTypeFlag,
    [in]  void const  *pValue,
    [in]  ULONG       cchValue
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pd`  
 no O token para o parâmetro de destino.  
  
 `szName`  
 no O nome do parâmetro em Unicode.  
  
 `dwParamFlags`  
 no Os sinalizadores para o parâmetro.  
  
 `dwCPlusTypeFlag`  
 no O ELEMENT_TYPE_ * para o valor constante.  
  
 `pValue`  
 no O valor da constante para o parâmetro.  
  
 `cchValue`  
 no O tamanho em caracteres (Unicode) de `pValue` .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataEmit](imetadataemit-interface.md)
- [Interface IMetaDataEmit2](imetadataemit2-interface.md)
