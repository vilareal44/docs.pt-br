---
title: Método IMetaDataImport::GetNativeCallConvFromSig
ms.date: 03/30/2017
api_name:
- IMetaDataImport.GetNativeCallConvFromSig
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetNativeCallConvFromSig
helpviewer_keywords:
- GetNativeCallConvFromSig method [.NET Framework metadata]
- IMetaDataImport::GetNativeCallConvFromSig method [.NET Framework metadata]
ms.assetid: 50e04026-4d4a-47d9-96c1-f4677d6d938b
topic_type:
- apiref
ms.openlocfilehash: 44439eda62f85c32893b73f17bd057195cf6b2e1
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503543"
---
# <a name="imetadataimportgetnativecallconvfromsig-method"></a>Método IMetaDataImport::GetNativeCallConvFromSig
Obtém a Convenção de chamada nativa para o método representado pelo ponteiro de assinatura especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetNativeCallConvFromSig (  
   [in]  void const  *pvSig,  
   [in]  ULONG       cbSig,  
   [out] ULONG       *pCallConv  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pvSig`  
 no Um ponteiro para a assinatura de metadados do método para o qual retornar a Convenção de chamada.  
  
 `cbSig`  
 no O tamanho em bytes de `pvSig` .  
  
 `pCallConv`  
 fora Um ponteiro para a Convenção de chamada nativa.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.Runtime.InteropServices.CallingConvention>
- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
