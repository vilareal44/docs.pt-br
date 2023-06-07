---
title: Método IMetaDataEmit2::GetDeltaSaveSize
ms.date: 03/30/2017
api_name:
- IMetaDataEmit2.GetDeltaSaveSize
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit2::GetDeltaSaveSize
helpviewer_keywords:
- IMetaDataEmit2::GetDeltaSaveSize method [.NET Framework metadata]
- GetDeltaSaveSize method [.NET Framework metadata]
ms.assetid: 036db5e7-8211-4645-9a34-03d1a89be955
topic_type:
- apiref
ms.openlocfilehash: 24fe8fd65b36e133b767cd07c8602aa1ea7b9dfc
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84493063"
---
# <a name="imetadataemit2getdeltasavesize-method"></a>Método IMetaDataEmit2::GetDeltaSaveSize
Obtém um valor que indica qualquer alteração no tamanho dos metadados que resulta da sessão de edição e continuação atual.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetDeltaSaveSize (  
    [in]  CorSaveSize  fSave,  
    [out] DWORD        *pdwSaveSize  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `fSave`  
 no Um dos valores de [CorSaveSize](corsavesize-enumeration.md) , indicando o nível de precisão desejado. Para o .NET Framework versão 2,0, esse parâmetro é ignorado.  
  
 `pdwSaveSize`  
 fora A alteração no tamanho dos metadados.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataEmit2](imetadataemit2-interface.md)
- [Interface IMetaDataEmit](imetadataemit-interface.md)
