---
title: Método IMetaDataEmit::SetPinvokeMap
ms.date: 03/30/2017
api_name:
- IMetaDataEmit.SetPinvokeMap
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit::SetPinvokeMap
helpviewer_keywords:
- IMetaDataEmit::SetPinvokeMap method [.NET Framework metadata]
- SetPinvokeMap method [.NET Framework metadata]
ms.assetid: c6bfd574-1da3-4ba7-82f2-46ca5efcbaba
topic_type:
- apiref
ms.openlocfilehash: 0d34c7a2992a2779b96ec87f1a0175d8fcbce34a
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84007787"
---
# <a name="imetadataemitsetpinvokemap-method"></a>Método IMetaDataEmit::SetPinvokeMap
Define ou altera recursos da assinatura do PInvoke de um método, conforme definido por uma chamada anterior para [IMetaDataEmit::D efinepinvokemap](imetadataemit-definepinvokemap-method.md).  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SetPinvokeMap (
    [in]  mdToken      tk,
    [in]  DWORD        dwMappingFlags,  
    [in]  LPCWSTR      szImportName,
    [in]  mdModuleRef  mrImportDLL
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `tk`  
 no O `mdToken` ao qual as informações de mapeamento se aplicam.  
  
 `dwMappingFlags`  
 no Sinalizadores usados pelo PInvoke para fazer o mapeamento. Esta é uma bitmask de `CorPinvokeMap` valores.  
  
 `szImportName`  
 no O nome da exportação de destino na DLL nativa.  
  
 `mrImportDLL`  
 no O `mdModuleRef` token para a dll não gerenciada de destino.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataEmit](imetadataemit-interface.md)
- [Interface IMetaDataEmit2](imetadataemit2-interface.md)
