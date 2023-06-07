---
title: Método IMetaDataImport2::EnumMethodSpecs
ms.date: 03/30/2017
api_name:
- IMetaDataImport2.EnumMethodSpecs
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport2::EnumMethodSpecs
helpviewer_keywords:
- IMetaDataImport2::EnumMethodSpecs method [.NET Framework metadata]
- EnumMethodSpecs method [.NET Framework metadata]
ms.assetid: b3fc1e6c-bcb6-4915-baf8-7dc0a31b8724
topic_type:
- apiref
ms.openlocfilehash: 8f6fbc570e7ea85aca5b365611d58a1700fb27cd
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84490712"
---
# <a name="imetadataimport2enummethodspecs-method"></a>Método IMetaDataImport2::EnumMethodSpecs
Obtém um enumerador para uma matriz de tokens de MethodSpec associados ao token MethodDef ou MemberRef especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT EnumMethodSpecs (  
    [in, out] HCORENUM      *phEnum,
    [in]      mdToken       tk,  
    [out]     mdMethodSpec  rMethodSpecs[],  
    [in]      ULONG         cMax,  
    [out]     ULONG         *pcMethodSpecs  
);
```  
  
## <a name="parameters"></a>Parâmetros  
 `phEnum`  
 [entrada, saída] Um ponteiro para o enumerador para `rMethodSpecs` .  
  
 `tk`  
 no O token MemberRef ou MethodDef que representa o método cujos tokens de MethodSpec devem ser enumerados. Se o valor de `tk` for 0 (zero), todos os tokens de MethodSpec no escopo serão enumerados.  
  
 `rMethodSpecs`  
 fora A matriz de tokens de MethodSpec a ser enumerada.  
  
 `cMax`  
 no O número máximo solicitado de tokens a serem colocados `rMethodSpecs` .  
  
 `pcMethodSpecs`  
 fora O número retornado de tokens colocados em `rMethodSpecs` .  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|`S_OK`|`EnumMethodSpecs`retornado com êxito.|  
|`S_FALSE`|`phEnum`Não tem elementos de membro. Nesse caso, `pcMethodSpecs` é definido como 0 (zero).|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport2](imetadataimport2-interface.md)
- [Interface IMetaDataImport](imetadataimport-interface.md)
