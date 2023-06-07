---
title: Método IMetaDataImport::GetMethodProps
ms.date: 03/30/2017
api_name:
- IMetaDataImport.GetMethodProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetMethodProps
helpviewer_keywords:
- GetMethodProps method [.NET Framework metadata]
- IMetaDataImport::GetMethodProps method [.NET Framework metadata]
ms.assetid: e0667ef7-1d31-4c89-a2d3-d426f023f8d2
topic_type:
- apiref
ms.openlocfilehash: 3c7c3525f2f8753241c9a206e4cf5e552bf06efe
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503613"
---
# <a name="imetadataimportgetmethodprops-method"></a>Método IMetaDataImport::GetMethodProps
Obtém os metadados associados ao método referenciado pelo token MethodDef especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetMethodProps (  
    [in]  mdMethodDef         mb,  
    [out] mdTypeDef           *pClass,  
    [out] LPWSTR              szMethod,  
    [in]  ULONG               cchMethod,  
    [out] ULONG               *pchMethod,  
    [out] DWORD               *pdwAttr,  
    [out] PCCOR_SIGNATURE     *ppvSigBlob,  
    [out] ULONG               *pcbSigBlob,  
    [out] ULONG               *pulCodeRVA,  
    [out] DWORD               *pdwImplFlags  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `mb`  
 no O token MethodDef que representa o método para o qual retornar metadados.  
  
 `pClass`  
 fora Um ponteiro para um token de TypeDef que representa o tipo que implementa o método.  
  
 `szMethod`  
 fora Um ponteiro para um buffer que tem o nome do método.  
  
 `cchMethod`  
 no O tamanho solicitado de `szMethod` .  
  
 `pchMethod`  
 fora Um ponteiro para o tamanho em caracteres largos de `szMethod` , ou no caso de truncamento, o número real de caracteres largos no nome do método.  
  
 `pdwAttr`  
 fora Um ponteiro para todos os sinalizadores associados ao método.  
  
 `ppvSigBlob`  
 fora Um ponteiro para a assinatura de metadados binários do método.  
  
 `pcbSigBlob`  
 fora Um ponteiro para o tamanho em bytes de `ppvSigBlob` .  
  
 `pulCodeRVA`  
 fora Um ponteiro para o endereço virtual relativo do método.  
  
 `pdwImplFlags`  
 fora Um ponteiro para qualquer sinalizador de implementação para o método.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
