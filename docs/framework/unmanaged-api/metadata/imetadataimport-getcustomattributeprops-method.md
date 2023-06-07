---
title: Método IMetaDataImport::GetCustomAttributeProps
ms.date: 03/30/2017
api_name:
- IMetaDataImport.GetCustomAttributeProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetCustomAttributeProps
helpviewer_keywords:
- GetCustomAttributeProps method [.NET Framework metadata]
- IMetaDataImport::GetCustomAttributeProps method [.NET Framework metadata]
ms.assetid: 6eefb243-a281-41c1-bcdc-7e17513bc446
topic_type:
- apiref
ms.openlocfilehash: 320cfae93f8aae94f9315e8e20ed6cf7f9cced7c
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84491310"
---
# <a name="imetadataimportgetcustomattributeprops-method"></a>Método IMetaDataImport::GetCustomAttributeProps
Obtém o valor do atributo personalizado, dado seu token de metadados.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetCustomAttributeProps (  
   [in]            mdCustomAttribute   cv,  
   [out, optional] mdToken             *ptkObj,  
   [out, optional] mdToken             *ptkType,  
   [out, optional] void const          **ppBlob,  
   [out, optional] ULONG               *pcbSize  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `cv`  
 no Um token de metadados que representa o atributo personalizado a ser recuperado.  
  
 `ptkObj`  
 [saída, opcional] Um token de metadados que representa o objeto que o atributo personalizado modifica. Esse valor pode ser qualquer tipo de token de metadados, exceto `mdCustomAttribute` .  
  
 `ptkType`  
 [saída, opcional] Um `mdMethodDef` `mdMemberRef` token de metadados ou que representa o <xref:System.Type> do atributo personalizado retornado.  
  
 `ppBlob`  
 [saída, opcional] Um ponteiro para uma matriz de dados que é o valor do atributo personalizado.  
  
 `pcbSize`  
 [saída, opcional] O tamanho em bytes dos dados retornados em * `ppBlob` .  
  
## <a name="remarks"></a>Comentários  
 Um atributo personalizado é armazenado como uma matriz de dados, o formato que é compreendido pelo mecanismo de metadados.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
