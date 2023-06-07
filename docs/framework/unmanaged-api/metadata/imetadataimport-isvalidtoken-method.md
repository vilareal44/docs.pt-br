---
title: Método IMetaDataImport::IsValidToken
ms.date: 03/30/2017
api_name:
- IMetaDataImport.IsValidToken
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::IsValidToken
helpviewer_keywords:
- IMetaDataImport::IsValidToken method [.NET Framework metadata]
- IsValidToken method [.NET Framework metadata]
ms.assetid: aeb0fc63-9eff-4384-9284-cb9900572d74
topic_type:
- apiref
ms.openlocfilehash: 9190d021b801be951d214406dde7e6d76da15608
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503431"
---
# <a name="imetadataimportisvalidtoken-method"></a>Método IMetaDataImport::IsValidToken
Obtém um valor que indica se o token especificado contém uma referência válida a um objeto de código.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
BOOL IsValidToken (  
   [in] mdToken     tk  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `tk`  
 no O token para verificar a validade de referência.  
  
## <a name="return-value"></a>Valor Retornado  
 `true`Se `tk` é um token de metadados válido dentro do escopo atual. Caso contrário, `false`.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
