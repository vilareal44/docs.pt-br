---
title: Método IMetaDataAssemblyImport::GetExportedTypeProps
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyImport.GetExportedTypeProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyImport::GetExportedTypeProps
helpviewer_keywords:
- GetExportedTypeProps method [.NET Framework metadata]
- IMetaDataAssemblyImport::GetExportedTypeProps method [.NET Framework metadata]
ms.assetid: 25ca7623-5a55-4f09-b44a-36b03d142278
topic_type:
- apiref
ms.openlocfilehash: 944941c2356cae93ecc85f1714b4b29aefcb50ad
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84008398"
---
# <a name="imetadataassemblyimportgetexportedtypeprops-method"></a>Método IMetaDataAssemblyImport::GetExportedTypeProps
Obtém o conjunto de propriedades do tipo exportado com a assinatura de metadados especificada.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetExportedTypeProps (  
    [in]  mdExportedType    mdct,
    [out] LPWSTR            szName,
    [in]  ULONG             cchName,
    [out] ULONG             *pchName,
    [out] mdToken           *ptkImplementation,
    [out] mdTypeDef         *ptkTypeDef,
    [out] DWORD             *pdwExportedTypeFlags  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `mdct`  
 no Um `mdExportedType` token de metadados que representa o tipo exportado.  
  
 `szName`  
 fora O nome do tipo exportado.  
  
 `cchName`  
 no O tamanho, em caracteres largos, de `szName` .  
  
 `pchName`  
 fora O número de caracteres largos realmente retornados em`szName`  
  
 `ptkImplementation`  
 fora Um `mdFile` `mdAssemblyRef` token de metadados, ou `mdExportedType` que contém ou permite o acesso às propriedades do tipo exportado.  
  
 `ptkTypeDef`  
 fora Um ponteiro para um `mdTypeDef` token que representa um tipo no arquivo.  
  
 `pdwExportedTypeFlags`  
 fora Um ponteiro para os sinalizadores que descrevem os metadados aplicados ao tipo exportado. O valor de flags pode ser um ou mais valores de [CorTypeAttr](cortypeattr-enumeration.md) .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataAssemblyImport](imetadataassemblyimport-interface.md)
