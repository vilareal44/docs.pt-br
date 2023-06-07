---
title: Método IMetaDataEmit::DefineMemberRef
ms.date: 03/30/2017
api_name:
- IMetaDataEmit.DefineMemberRef
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit::DefineMemberRef
helpviewer_keywords:
- DefineMemberRef method [.NET Framework metadata]
- IMetaDataEmit::DefineMemberRef method [.NET Framework metadata]
ms.assetid: 21b5bcb8-ea75-4962-8acc-ad17584061e5
topic_type:
- apiref
ms.openlocfilehash: 576f4561ed782f091840ac378831110a1bfef9c6
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84004681"
---
# <a name="imetadataemitdefinememberref-method"></a>Método IMetaDataEmit::DefineMemberRef
Define uma referência a um membro de um módulo fora do escopo atual e Obtém um token para essa definição de referência.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT DefineMemberRef (
    [in]  mdToken           tkImport,
    [in]  LPCWSTR           szName,
    [in]  PCCOR_SIGNATURE   pvSigBlob,
    [in]  ULONG             cbSigBlob,
    [out] mdMemberRef       *pmr
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `tkImport`  
 no Token para a classe ou a interface do membro de destino, se o membro não for global; Se o membro for global, o `mdModuleRef` token para esse outro arquivo.  
  
 `szName`  
 no O nome do membro de destino.  
  
 `pvSigBlob`  
 no A assinatura do membro de destino.  
  
 `cbSigBlob`  
 no A contagem de bytes em `pvSigBlob` .  
  
 `pmr`  
 fora O `mdMemberRef` token atribuído.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataEmit](imetadataemit-interface.md)
- [Interface IMetaDataEmit2](imetadataemit2-interface.md)
