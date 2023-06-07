---
title: Interface IMetaDataEmit2
ms.date: 03/30/2017
api_name:
- IMetaDataEmit2
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit2
helpviewer_keywords:
- IMetaDataEmit2 interface [.NET Framework metadata]
ms.assetid: 866dc96b-bbfc-4c0f-80c2-38ce93072106
topic_type:
- apiref
ms.openlocfilehash: 5a232f30da8812c6f3bd94647d74151312a8593b
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84493014"
---
# <a name="imetadataemit2-interface"></a>Interface IMetaDataEmit2
Estende a interface [IMetaDataEmit](imetadataemit-interface.md) principalmente para fornecer a capacidade de trabalhar com tipos genéricos.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método DefineGenericParam](imetadataemit2-definegenericparam-method.md)|Cria uma definição para um parâmetro de tipo genérico e Obtém um token para esse parâmetro de tipo genérico.|  
|[Método DefineMethodSpec](imetadataemit2-definemethodspec-method.md)|Cria uma instância genérica de um método e Obtém um token para a definição.|  
|[Método GetDeltaSaveSize](imetadataemit2-getdeltasavesize-method.md)|Obtém um valor que indica a diferença no tamanho dos dados necessários para expressar as alterações da sessão de edição e continuação atual.|  
|[Método ResetENCLog](imetadataemit2-resetenclog-method.md)|Redefine o log de edição e continuação e inicia uma nova sessão.|  
|[Método SaveDelta](imetadataemit2-savedelta-method.md)|Salva as alterações da sessão de edição e continuação atual para o arquivo especificado.|  
|[Método SaveDeltaToMemory](imetadataemit2-savedeltatomemory-method.md)|Salva as alterações da sessão de edição e continuação atual na memória.|  
|[Método SaveDeltaToStream](imetadataemit2-savedeltatostream-method.md)|Salva as alterações da sessão de edição e continuação atual para o fluxo especificado.|  
|[Método SetGenericParamProps](imetadataemit2-setgenericparamprops-method.md)|Define valores de propriedade para a definição de parâmetro genérico referenciada pelo token especificado.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interfaces de metadados](metadata-interfaces.md)
- [Interface IMetaDataEmit](imetadataemit-interface.md)
