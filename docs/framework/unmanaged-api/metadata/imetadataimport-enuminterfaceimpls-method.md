---
title: Método IMetaDataImport::EnumInterfaceImpls
ms.date: 03/30/2017
api_name:
- IMetaDataImport.EnumInterfaceImpls
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::EnumInterfaceImpls
helpviewer_keywords:
- IMetaDataImport::EnumInterfaceImpls method [.NET Framework metadata]
- EnumInterfaceImpls method [.NET Framework metadata]
ms.assetid: ba6e178f-128b-4e47-a13c-b4be73eb106c
topic_type:
- apiref
ms.openlocfilehash: 910c40413075131765a37e00703ac892e3f39641
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84492181"
---
# <a name="imetadataimportenuminterfaceimpls-method"></a>Método IMetaDataImport::EnumInterfaceImpls
Enumera todas as interfaces implementadas pelo especificado `TypeDef` .
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT EnumInterfaceImpls (  
   [in, out]  HCORENUM       *phEnum,
   [in]   mdTypeDef          td,  
   [out]  mdInterfaceImpl    rImpls[],
   [in]   ULONG              cMax,  
   [out]  ULONG*             pcImpls  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `phEnum`  
 [entrada, saída] Um ponteiro para o enumerador.  
  
 `td`  
 no O token do TypeDef cujos tokens MethodDef que representam implementações de interface devem ser enumerados.  
  
 `rImpls`  
 fora A matriz usada para armazenar os tokens MethodDef.  
  
 `cMax`  
 no O comprimento máximo da `rImpls` matriz.  
  
 `pcImpls`  
 fora O número real de tokens retornados em `rImpls` .  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|`S_OK`|`EnumInterfaceImpls`retornado com êxito.|  
|`S_FALSE`|Não há tokens MethodDef para enumerar. Nesse caso, `pcImpls` é definido como zero.|  

## <a name="remarks"></a>Comentários

A enumeração retorna uma coleção de `mdInterfaceImpl` tokens para cada interface implementada pelo especificado `TypeDef` . Os tokens de interface são retornados na ordem em que as interfaces foram especificadas (por meio `DefineTypeDef` de ou `SetTypeDefProps` ). As propriedades dos `mdInterfaceImpl` tokens retornados podem ser consultadas usando [GetInterfaceImplProps](imetadataimport-getinterfaceimplprops-method.md).
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
