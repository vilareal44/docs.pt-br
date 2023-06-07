---
title: Método IMetaDataEmit2::SaveDeltaToMemory
ms.date: 03/30/2017
api_name:
- IMetaDataEmit2.SaveDeltaToMemory
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit2::SaveDeltaToMemory
helpviewer_keywords:
- SaveDeltaToMemory method [.NET Framework metadata]
- IMetaDataEmit2::SaveDeltaToMemory method [.NET Framework metadata]
ms.assetid: e2146726-0084-4c9e-a2d2-e8d461b13b21
topic_type:
- apiref
ms.openlocfilehash: 8a6f11996425c92d9b0e3123ee2d3a064739454b
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84492738"
---
# <a name="imetadataemit2savedeltatomemory-method"></a>Método IMetaDataEmit2::SaveDeltaToMemory
Salva as alterações da sessão de edição e continuação atual na memória.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SaveDeltaToMemory (  
    [out] void        *pbData,
    [in]  ULONG       cbData  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pbData`  
 fora O endereço no qual começar a gravar o Delta de metadados.  
  
 `cbData`  
 no O tamanho das alterações. Use [IMetaDataEmit2:: GetDeltaSaveSize](imetadataemit2-getdeltasavesize-method.md) para determinar o tamanho.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataEmit2](imetadataemit2-interface.md)
- [Interface IMetaDataEmit](imetadataemit-interface.md)
