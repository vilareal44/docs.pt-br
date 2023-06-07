---
title: Método IMetaDataImport::EnumUnresolvedMethods
ms.date: 03/30/2017
api_name:
- IMetaDataImport.EnumUnresolvedMethods
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::EnumUnresolvedMethods
helpviewer_keywords:
- EnumUnresolvedMethods method [.NET Framework metadata]
- IMetaDataImport::EnumUnresolvedMethods method [.NET Framework metadata]
ms.assetid: eb3187d7-74cf-44b1-aeeb-7a8d2b60e3b7
topic_type:
- apiref
ms.openlocfilehash: c991c0af845bc6825db6b3bf258fe0809d5db804
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503686"
---
# <a name="imetadataimportenumunresolvedmethods-method"></a>Método IMetaDataImport::EnumUnresolvedMethods
Enumera os tokens MemberDef que representam os métodos não resolvidos no escopo de metadados atual.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT EnumUnresolvedMethods (  
   [in, out] HCORENUM    *phEnum,  
   [out]     mdToken     rMethods[],  
   [in]      ULONG       cMax,  
   [out]     ULONG       *pcTokens  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `phEnum`  
 [entrada, saída] Um ponteiro para o enumerador. Isso deve ser nulo para a primeira chamada deste método.  
  
 `rMethods`  
 fora A matriz usada para armazenar os tokens MemberDef.  
  
 `cMax`  
 no O tamanho máximo da `rMethods` matriz.  
  
 `pcTokens`  
 fora O número de tokens MemberDef retornados em `rMethods` .  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|`S_OK`|`EnumUnresolvedMethods`retornado com êxito.|  
|`S_FALSE`|Não há tokens para enumerar. Nesse caso, `pcTokens` é zero.|  
  
## <a name="remarks"></a>Comentários  
 Um método não resolvido é aquele que foi declarado mas não implementado. Um método será incluído na enumeração se o método estiver marcado `miForwardRef` e `mdPinvokeImpl` ou `miRuntime` estiver definido como zero. Em outras palavras, um método não resolvido é um método de classe marcado `miForwardRef` , mas que não é implementado em código não gerenciado (alcançado via PInvoke) nem implementado internamente pelo próprio tempo de execução  
  
 A enumeração exclui todos os métodos que são definidos no escopo do módulo (Globals) ou em interfaces ou classes abstratas.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
