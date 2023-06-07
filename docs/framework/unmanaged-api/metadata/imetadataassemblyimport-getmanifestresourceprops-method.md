---
title: Método IMetaDataAssemblyImport::GetManifestResourceProps
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyImport.GetManifestResourceProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyImport::GetManifestResourceProps
helpviewer_keywords:
- GetManifestResourceProps method [.NET Framework metadata]
- IMetaDataAssemblyImport::GetManifestResourceProps method [.NET Framework metadata]
ms.assetid: 00be4789-ac63-4397-b2ec-1629a5c5a585
topic_type:
- apiref
ms.openlocfilehash: c0b6d53ce3be3aed6a577bf6e38a281928499848
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84009022"
---
# <a name="imetadataassemblyimportgetmanifestresourceprops-method"></a>Método IMetaDataAssemblyImport::GetManifestResourceProps
Obtém o conjunto de propriedades do recurso de manifesto com a assinatura de metadados especificada.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetManifestResourceProps (  
    [in]  mdManifestResource   mdmr,
    [out] LPWSTR               szName,
    [in]  ULONG                cchName,
    [out] ULONG                *pchName,
    [out] mdToken              *ptkImplementation,
    [out] DWORD                *pdwOffset,
    [out] DWORD                *pdwResourceFlags  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `mdmr`  
 no Um `mdManifestResource` token que representa o recurso para o qual obter as propriedades.  
  
 `szName`  
 fora O nome do recurso.  
  
 `cchName`  
 no O tamanho, em caracteres largos, de `szName` .  
  
 `pchName`  
 fora Um ponteiro para o número de caracteres largos realmente retornados em `szName` .  
  
 `ptkImplementation`  
 fora Um ponteiro para um `mdFile` token ou um `mdAssemblyRef` token que representa o arquivo ou assembly, respectivamente, que contém o recurso.  
  
 `pdwOffset`  
 fora Um ponteiro para um valor que especifica o deslocamento para o início do recurso dentro do arquivo.  
  
 `pdwResourceFlags`  
 fora Um ponteiro para sinalizadores que descrevem os metadados aplicados a um recurso. O valor de flags é uma combinação de um ou mais valores de [CorManifestResourceFlags](cormanifestresourceflags-enumeration.md) .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataAssemblyImport](imetadataassemblyimport-interface.md)
