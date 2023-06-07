---
title: Método IMetaDataImport::EnumCustomAttributes
ms.date: 03/30/2017
api_name:
- IMetaDataImport.EnumCustomAttributes
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::EnumCustomAttributes
helpviewer_keywords:
- EnumCustomAttributes method [.NET Framework metadata]
- IMetaDataImport::EnumCustomAttributes method [.NET Framework metadata]
ms.assetid: 798513a0-68b1-4d04-bc5b-782a4445ea68
topic_type:
- apiref
ms.openlocfilehash: 9b0da8a06259fe99da52497da3011da94289d301
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84492311"
---
# <a name="imetadataimportenumcustomattributes-method"></a>Método IMetaDataImport::EnumCustomAttributes
Enumera os tokens de definição de atributo personalizados associados ao tipo ou membro especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT EnumCustomAttributes (
   [in, out] HCORENUM      *phEnum,  
   [in]  mdToken            tk,
   [in]  mdToken            tkType,
   [out] mdCustomAttribute  rCustomAttributes[],
   [in]  ULONG              cMax,  
   [out, optional] ULONG   *pcCustomAttributes  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `phEnum`  
 [entrada, saída] Um ponteiro para o enumerador retornado.  
  
 `tk`  
 no Um token para o escopo da enumeração ou zero para todos os atributos personalizados.  
  
 `tkType`  
 no Um token para o construtor do tipo dos atributos a serem enumerados ou `null` para todos os tipos.  
  
 `rCustomAttributes`  
 fora Uma matriz de tokens de atributo personalizados.  
  
 `cMax`  
 no O tamanho máximo da `rCustomAttributes` matriz.  
  
 `pcCustomAttributes`  
 [saída, opcional] O número real de valores de token retornados em `rCustomAttributes` .  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|`S_OK`|`EnumCustomAttributes`retornado com êxito.|  
|`S_FALSE`|Não há atributos personalizados para enumerar. Nesse caso, `pcCustomAttributes` é zero.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
