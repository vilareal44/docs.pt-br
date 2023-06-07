---
title: Método IMetaDataImport::EnumMethodSemantics
ms.date: 03/30/2017
api_name:
- IMetaDataImport.EnumMethodSemantics
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::EnumMethodSemantics
helpviewer_keywords:
- EnumMethodSemantics method [.NET Framework metadata]
- IMetaDataImport::EnumMethodSemantics method [.NET Framework metadata]
ms.assetid: e7e3c630-9691-46d6-94df-b5593a7bb08a
topic_type:
- apiref
ms.openlocfilehash: 213cbd955e3d47a49abde579a54af48641e225ec
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84491908"
---
# <a name="imetadataimportenummethodsemantics-method"></a>Método IMetaDataImport::EnumMethodSemantics
Enumera as propriedades e os eventos de alteração de propriedade aos quais o método especificado está relacionado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT EnumMethodSemantics (  
   [in, out] HCORENUM    *phEnum,  
   [in]  mdMethodDef     mb,
   [out] mdToken         rEventProp[],  
   [in]  ULONG           cMax,  
   [out] ULONG           *pcEventProp  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `phEnum`  
 [entrada, saída] Um ponteiro para o enumerador. Isso deve ser nulo para a primeira chamada deste método.  
  
 `mb`  
 no Um token MethodDef que limita o escopo da enumeração.  
  
 `rEventProp`  
 fora A matriz usada para armazenar os eventos ou as propriedades.  
  
 `cMax`  
 no O tamanho máximo da `rEventProp` matriz.  
  
 `pcEventProp`  
 fora O número de eventos ou Propriedades retornados em `rEventProp` .  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|`S_OK`|`EnumMethodSemantics`retornado com êxito.|  
|`S_FALSE`|Não há eventos ou propriedades para enumerar. Nesse caso, `pcEventProp` é zero.|  
  
## <a name="remarks"></a>Comentários  
 Muitos tipos de Common Language Runtime definem eventos de *Propriedade* `Changed` e `On` *Property* `Changed` métodos de propriedade relacionados a suas propriedades. Por exemplo, o <xref:System.Windows.Forms.Control?displayProperty=nameWithType> tipo define uma <xref:System.Windows.Forms.Control.Font%2A> propriedade, um <xref:System.Windows.Forms.Control.FontChanged> evento e um <xref:System.Windows.Forms.Control.OnFontChanged%2A> método. O método do acessador set do <xref:System.Windows.Forms.Control.Font%2A> método de chamadas de propriedade <xref:System.Windows.Forms.Control.OnFontChanged%2A> que, por sua vez, gera o <xref:System.Windows.Forms.Control.FontChanged> evento. Você chamaria `EnumMethodSemantics` usando o MethodDef para <xref:System.Windows.Forms.Control.OnFontChanged%2A> para obter referências à <xref:System.Windows.Forms.Control.Font%2A> propriedade e ao <xref:System.Windows.Forms.Control.FontChanged> evento.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
