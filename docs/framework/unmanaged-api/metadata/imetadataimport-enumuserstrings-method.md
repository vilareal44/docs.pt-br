---
title: Método IMetaDataImport::EnumUserStrings
ms.date: 03/30/2017
api_name:
- IMetaDataImport.EnumUserStrings
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::EnumUserStrings
helpviewer_keywords:
- IMetaDataImport::EnumUserStrings method [.NET Framework metadata]
- EnumUserStrings method [.NET Framework metadata]
ms.assetid: 2b1f1418-4be8-4cdb-b418-b3abccc527a7
topic_type:
- apiref
ms.openlocfilehash: cd164008098c053e7d6506a6eef7d3bc8e4274b6
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503688"
---
# <a name="imetadataimportenumuserstrings-method"></a>Método IMetaDataImport::EnumUserStrings
Enumera os tokens de cadeia de caracteres que representam cadeias embutidas em código no escopo de metadados atual.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT EnumUserStrings (  
   [in, out]  HCORENUM    *phEnum,  
   [out]  mdString        rStrings[],  
   [in]   ULONG           cMax,  
   [out]  ULONG           *pcStrings  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `phEnum`  
 [entrada, saída] Um ponteiro para o enumerador. Isso deve ser nulo para a primeira chamada deste método.  
  
 `rStrings`  
 fora A matriz usada para armazenar os tokens de cadeia de caracteres.  
  
 `cMax`  
 no O tamanho máximo da `rStrings` matriz.  
  
 `pcStrings`  
 fora O número de tokens de cadeia de caracteres retornados em `rStrings` .  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|`S_OK`|`EnumUserStrings`retornado com êxito.|  
|`S_FALSE`|Não há tokens para enumerar. Nesse caso, `pcStrings` é zero.|  
  
## <a name="remarks"></a>Comentários  
 Os tokens de cadeia de caracteres são criados pelo método [IMetaDataEmit::D efineuserstring](imetadataemit-defineuserstring-method.md) . Esse método é projetado para ser usado por um navegador de metadados em vez de por um compilador.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
