---
title: Método IMetaDataImport::EnumMembersWithName
ms.date: 03/30/2017
api_name:
- IMetaDataImport.EnumMembersWithName
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::EnumMembersWithName
helpviewer_keywords:
- IMetaDataImport::EnumMembersWithName method [.NET Framework metadata]
- EnumMembersWithName method [.NET Framework metadata]
ms.assetid: 7c9e9120-3104-42f0-86ce-19a025f20dcc
topic_type:
- apiref
ms.openlocfilehash: ea451bdd645d2d4dea4c5dd00408e0bc51804803
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84492035"
---
# <a name="imetadataimportenummemberswithname-method"></a>Método IMetaDataImport::EnumMembersWithName
Enumera os tokens MemberDef que representam os membros do tipo especificado com o nome especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT EnumMembersWithName (  
   [in, out] HCORENUM    *phEnum,
   [in]      mdTypeDef   cl,
   [in]      LPCWSTR     szName,
   [out]     mdToken     rMembers[],
   [in]      ULONG       cMax,
   [out]     ULONG       *pcTokens  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `phEnum`  
 [entrada, saída] Um ponteiro para o enumerador.  
  
 `cl`  
 no Um token de TypeDef que representa o tipo com membros a serem enumerados.  
  
 `szName`  
 no O nome do membro que limita o escopo do enumerador.  
  
 `rMembers`  
 fora A matriz usada para armazenar os tokens MemberDef.  
  
 `cMax`  
 no O tamanho máximo da `rMembers` matriz.  
  
 `pcTokens`  
 fora O número real de tokens MemberDef retornados em `rMembers` .  
  
## <a name="remarks"></a>Comentários  
 Esse método enumera campos e métodos, mas não propriedades ou eventos. Ao contrário de [IMetaDataImport:: EnumMembers](imetadataimport-enummembers-method.md), `EnumMembersWithName` descarta todos os tokens de campo e membro que não têm o nome especificado.  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|`S_OK`|`EnumTypeDefs`retornado com êxito.|  
|`S_FALSE`|Não há tokens MemberDef para enumerar. Nesse caso, `pcTokens` é zero.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
