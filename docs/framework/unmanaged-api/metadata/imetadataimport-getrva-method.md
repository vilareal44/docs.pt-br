---
title: Método IMetaDataImport::GetRVA
ms.date: 03/30/2017
api_name:
- IMetaDataImport.GetRVA
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetRVA
helpviewer_keywords:
- GetRVA method [.NET Framework metadata]
- IMetaDataImport::GetRVA method [.NET Framework metadata]
ms.assetid: ea422217-988b-4acd-b2db-c55357938275
topic_type:
- apiref
ms.openlocfilehash: 58ab9ee9381fce4d7af1910df6c8d3bb813bcf13
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84490885"
---
# <a name="imetadataimportgetrva-method"></a>Método IMetaDataImport::GetRVA
Obtém o endereço virtual relativo (RVA) e os sinalizadores de implementação do método ou do campo representado pelo token especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetRVA (  
   [in]  mdToken     tk,
   [out] ULONG       *pulCodeRVA,
   [out]  DWORD      *pdwImplFlags  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `tk`  
 no Um token de metadados MethodDef ou FieldDef que representa o objeto de código para o qual retornar o RVA. Se o token for um FieldDef, o campo deverá ser uma variável global.  
  
 `pulCodeRVA`  
 fora Um ponteiro para o endereço virtual relativo do objeto de código representado pelo token.  
  
 `pdwImplFlags`  
 fora Um ponteiro para os sinalizadores de implementação do método. Esse valor é um bitmask da enumeração [CorMethodImpl](cormethodimpl-enumeration.md) . O valor de `pdwImplFlags` será válido somente se `tk` for um token MethodDef.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
