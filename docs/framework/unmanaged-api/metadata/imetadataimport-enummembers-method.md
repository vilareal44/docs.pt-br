---
title: Método IMetaDataImport::EnumMembers
ms.date: 03/30/2017
api_name:
- IMetaDataImport.EnumMembers
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::EnumMembers
helpviewer_keywords:
- IMetaDataImport::EnumMembers method [.NET Framework metadata]
- EnumMembers method [.NET Framework metadata]
ms.assetid: 3fb8e178-342b-4c89-9bcf-f7f834e6cb77
topic_type:
- apiref
ms.openlocfilehash: cc3bc5140da0634b5172f6253de3de37bff487f1
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84492023"
---
# <a name="imetadataimportenummembers-method"></a>Método IMetaDataImport::EnumMembers
Enumera os tokens MemberDef que representam os membros do tipo especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT EnumMembers (
   [in, out]  HCORENUM    *phEnum,
   [in]  mdTypeDef   cl,
   [out] mdToken     rMembers[],
   [in]  ULONG       cMax,
   [out] ULONG       *pcTokens  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `phEnum`  
 [entrada, saída] Um ponteiro para o enumerador.  
  
 `cl`  
 no Um token de TypeDef que representa o tipo cujos membros devem ser enumerados.  
  
 `rMembers`  
 fora A matriz usada para manter os tokens MemberDef.  
  
 `cMax`  
 no O tamanho máximo da `rMembers` matriz.  
  
 `pcTokens`  
 fora O número real de tokens MemberDef retornados em `rMembers` .  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|`S_OK`|`EnumMembers`retornado com êxito.|  
|`S_FALSE`|Não há tokens MemberDef para enumerar. Nesse caso, `pcTokens` é zero.|  
  
## <a name="remarks"></a>Comentários  
 Ao enumerar coleções de membros para uma classe, `EnumMembers` retorna somente Membros (campos e métodos, mas **não** Propriedades ou eventos) definidos diretamente na classe. Ele não retorna nenhum membro herdado pela classe, mesmo que a classe forneça uma implementação para esses membros herdados. Para enumerar membros herdados, o chamador deve percorrer a cadeia de herança explicitamente. Observe que as regras para a cadeia de herança podem variar dependendo do idioma ou do compilador que emitiu os metadados originais.

 Propriedades e eventos não são enumerados pelo `EnumMembers` . Para enumerar, use [EnumProperties](imetadataimport-enumproperties-method.md) ou [EnumEvents](imetadataimport-enumevents-method.md).
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
