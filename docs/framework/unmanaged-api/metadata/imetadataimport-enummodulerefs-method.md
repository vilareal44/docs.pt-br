---
title: Método IMetaDataImport::EnumModuleRefs
ms.date: 03/30/2017
api_name:
- IMetaDataImport.EnumModuleRefs
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::EnumModuleRefs
helpviewer_keywords:
- EnumModuleRefs method [.NET Framework metadata]
- IMetaDataImport::EnumModuleRefs method [.NET Framework metadata]
ms.assetid: 53441f3a-68d2-477c-906e-37c55dfcfb4d
topic_type:
- apiref
ms.openlocfilehash: fe7350e6d8e400ae37b5b8b7854a56f3c5c53ea7
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84491735"
---
# <a name="imetadataimportenummodulerefs-method"></a>Método IMetaDataImport::EnumModuleRefs
Enumera tokens ModuleRef que representam módulos importados.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT EnumModuleRefs (  
   [in, out] HCORENUM     *phEnum,  
   [out]     mdModuleRef  rModuleRefs[],  
   [in]      ULONG        cMax,  
   [out]     ULONG        *pcModuleRefs  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `phEnum`  
 [entrada, saída] Um ponteiro para o enumerador. Isso deve ser nulo para a primeira chamada deste método.  
  
 `rModuleRefs`  
 fora A matriz usada para armazenar os tokens de ModuleRef.  
  
 `cMax`  
 no O tamanho máximo da `rModuleRefs` matriz.  
  
 `pcModuleRefs`  
 fora O número de tokens de ModuleRef retornados em `rModuleRefs` .  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|`S_OK`|`EnumModuleRefs`retornado com êxito.|  
|`S_FALSE`|Não há tokens para enumerar. Nesse caso, `pcModuleRefs` é zero.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
