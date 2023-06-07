---
title: Método IMetaDataDispenserEx::GetOption
ms.date: 03/30/2017
api_name:
- IMetaDataDispenserEx.GetOption
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataDispenserEx::GetOption
helpviewer_keywords:
- GetOption method [.NET Framework metadata]
- IMetaDataDispenserEx::GetOption method [.NET Framework metadata]
ms.assetid: d7f794e5-8e25-4d65-850a-7c34fbfce87d
topic_type:
- apiref
ms.openlocfilehash: 832adacac4a6df9ccf21578538a1c557150f3ba1
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84008775"
---
# <a name="imetadatadispenserexgetoption-method"></a>Método IMetaDataDispenserEx::GetOption
Obtém o valor da opção especificada para o escopo de metadados atual. A opção controla como as chamadas para o escopo de metadados atual são tratadas.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetOption (  
    [in]  REFGUID         optionId,
    [out] const VARIANT   *pValue  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `optionId`  
 no Um ponteiro para um GUID que especifica a opção a ser recuperada. Consulte a seção comentários para obter uma lista de GUIDs com suporte.  
  
 `pValue`  
 fora O valor da opção retornada. O tipo desse valor será uma variante do tipo da opção especificada.  
  
## <a name="remarks"></a>Comentários  
 A lista a seguir mostra os GUIDs que têm suporte para esse método. Para obter descrições, consulte o método [IMetaDataDispenserEx:: SetOption](imetadatadispenserex-setoption-method.md) . Se `optionId` não estiver nessa lista, esse método retornará HRESULT `E_INVALIDARG` , indicando um parâmetro incorreto.  
  
- MetaDataCheckDuplicatesFor  
  
- MetaDataRefToDefCheck  
  
- MetaDataNotificationForTokenMovement  
  
- MetaDataSetENC  
  
- MetaDataErrorIfEmitOutOfOrder  
  
- MetaDataGenerateTCEAdapters  
  
- MetaDataLinkerOptions  
  
## <a name="requirements"></a>Requisitos  
 **Plataforma:** Consulte [requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataDispenserEx](imetadatadispenserex-interface.md)
- [Interface IMetaDataDispenser](imetadatadispenser-interface.md)
