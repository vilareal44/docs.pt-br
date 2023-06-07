---
title: Método IMetaDataImport2::EnumGenericParams
ms.date: 03/30/2017
api_name:
- IMetaDataImport2.EnumGenericParams
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport2::EnumGenericParams
helpviewer_keywords:
- EnumGenericParams method [.NET Framework metadata]
- IMetaDataImport2::EnumGenericParams method [.NET Framework metadata]
ms.assetid: b50488a5-3cf0-483c-82dc-2892a3ec61ac
topic_type:
- apiref
ms.openlocfilehash: 093e3edf0a3c06222ebc56a4876fca08d1b7578f
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84490721"
---
# <a name="imetadataimport2enumgenericparams-method"></a>Método IMetaDataImport2::EnumGenericParams
Obtém um enumerador para uma matriz de tokens de parâmetro genéricos associados ao TypeDef ou token MethodDef especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp
HRESULT EnumGenericParams (  
   [in, out] HCORENUM     *phEnum,
   [in]  mdToken          tk,  
   [out] mdGenericParam   rGenericParams[],
   [in]  ULONG            cMax,
   [out] ULONG            *pcGenericParams  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `phEnum`  
 [entrada, saída] Um ponteiro para o enumerador.  
  
 `tk`  
 no O TypeDef ou o token MethodDef cujos parâmetros genéricos devem ser enumerados.  
  
 `rGenericParams`  
 fora A matriz de parâmetros genéricos a ser enumerada.  
  
 `cMax`  
 no O número máximo solicitado de tokens a serem colocados `rGenericParams` .  
  
 `pcGenericParams`  
 fora O número retornado de tokens colocados em `rGenericParams` .  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|`S_OK`|`EnumGenericParams`retornado com êxito.|  
|`S_FALSE`|`phEnum`Não tem elementos de membro. Nesse caso, `pcGenericParams` é definido como 0 (zero).|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport2](imetadataimport2-interface.md)
- [Interface IMetaDataImport](imetadataimport-interface.md)
