---
title: Método IMetaDataImport::FindTypeRef
ms.date: 03/30/2017
api_name:
- IMetaDataImport.FindTypeRef
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::FindTypeRef
helpviewer_keywords:
- IMetaDataImport::FindTypeRef method [.NET Framework metadata]
- FindTypeRef method [.NET Framework metadata]
ms.assetid: 1b2bbf3f-943e-412e-b66c-e802431b055c
topic_type:
- apiref
ms.openlocfilehash: 545fe1e1d9e641d2225ad92c11453558dc4b97d1
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84491492"
---
# <a name="imetadataimportfindtyperef-method"></a>Método IMetaDataImport::FindTypeRef
Obtém um ponteiro para o token TypeRef para a <xref:System.Type> referência que está no escopo especificado e que tem o nome especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT FindTypeRef (  
   [in] mdToken        tkResolutionScope,  
   [in]  LPCWSTR       szName,  
   [out] mdTypeRef     *ptr  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `tkResolutionScope`  
 no Um token ModuleRef, AssemblyRef ou TypeRef que especifica o módulo, o assembly ou o tipo, respectivamente, no qual a referência de tipo é definida.  
  
 `szName`  
 no O nome da referência de tipo a ser pesquisada.  
  
 `ptr`  
 fora Um ponteiro para o token TypeRef correspondente.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
