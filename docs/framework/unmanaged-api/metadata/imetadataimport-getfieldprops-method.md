---
title: Método IMetaDataImport::GetFieldProps
ms.date: 03/30/2017
api_name:
- IMetaDataImport.GetFieldProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetFieldProps
helpviewer_keywords:
- IMetaDataImport::GetFieldProps method [.NET Framework metadata]
- GetFieldProps method [.NET Framework metadata]
ms.assetid: 7b0e9b10-8cef-4ba6-8432-40bf63e65ab1
topic_type:
- apiref
ms.openlocfilehash: 2bd05b49c3d51ac13865997910c99cc0cd5ca2d9
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84491222"
---
# <a name="imetadataimportgetfieldprops-method"></a>Método IMetaDataImport::GetFieldProps
Obtém os metadados associados ao campo referenciado pelo token FieldDef especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetFieldProps (  
   [in]  mdFieldDef        mb,
   [out] mdTypeDef         *pClass,  
   [out] LPWSTR            szField,  
   [in]  ULONG             cchField,
   [out] ULONG             *pchField,  
   [out] DWORD             *pdwAttr,  
   [in]  PCCOR_SIGNATURE   *ppvSigBlob,
   [out] ULONG             *pcbSigBlob,
   [out] DWORD             *pdwCPlusTypeFlag,
   [out] UVCP_CONSTANT     *ppValue,  
   [out] ULONG             *pcchValue  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `mb`  
 no Um token FieldDef que representa o campo para o qual obter metadados associados.  
  
 `pClass`  
 fora Um ponteiro para um token de TypeDef que representa o tipo da classe à qual o campo pertence.  
  
 `szField`  
 fora O nome do campo.  
  
 `cchField`  
 no O tamanho em caracteres largos do buffer para *szField*.  
  
 `pchField`  
 fora O tamanho real do buffer retornado.  
  
 `pdwAttr`  
 fora Sinalizadores associados aos metadados do campo.  
  
 `ppvSigBlob`  
 no Um ponteiro para o valor de metadados binários que descreve o campo.  
  
 `pcbSigBlob`  
 fora O tamanho em bytes de `ppvSigBlob` .  
  
 `pdwCPlusTypeFlag`  
 fora Um sinalizador que especifica o tipo de valor do campo.  
  
 `ppValue`  
 fora Um valor constante para o campo.  
  
 `pcchValue`  
 fora O tamanho em caracteres de `ppValue` , ou zero, se nenhuma cadeia de caracteres existir.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
