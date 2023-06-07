---
title: Método IMetaDataEmit2::DefineMethodSpec
ms.date: 03/30/2017
api_name:
- IMetaDataEmit2.DefineMethodSpec
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit2::DefineMethodSpec
helpviewer_keywords:
- DefineMethodSpec method [.NET Framework metadata]
- IMetaDataEmit2::DefineMethodSpec method [.NET Framework metadata]
ms.assetid: 3c24e552-fc69-4971-b65a-a3e4b5f7f1e8
topic_type:
- apiref
ms.openlocfilehash: 8e067dc4943e6847177c13a683703e3a649a49e4
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503808"
---
# <a name="imetadataemit2definemethodspec-method"></a>Método IMetaDataEmit2::DefineMethodSpec
Cria uma instância genérica de um método e Obtém um token para a definição.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT DefineMethodSpec (  
    [in]  mdToken           tkParent,
    [in]  PCCOR_SIGNATURE   pvSigBlob,
    [in]  ULONG             cbSigBlob,
    [out] mdMethodSpec      *pmi  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `tkParent`  
 no Um token para o método do qual criar a instância genérica. O token deve ser do tipo `mdMethodDef` ou `mdMemberRef` .  
  
 `pvSigBlob`  
 no Um ponteiro para a assinatura COM+ binária do método.  
  
 `cbSibBlob`  
 no O tamanho, em bytes, de `pvSigBlob` .  
  
 `pmi`  
 fora Um token para a definição de assinatura de metadados do método.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataEmit2](imetadataemit2-interface.md)
- [Interface IMetaDataEmit](imetadataemit-interface.md)
