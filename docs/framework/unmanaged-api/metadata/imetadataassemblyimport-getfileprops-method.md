---
title: Método IMetaDataAssemblyImport::GetFileProps
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyImport.GetFileProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyImport::GetFileProps
helpviewer_keywords:
- GetFileProps method [.NET Framework metadata]
- IMetaDataAssemblyImport::GetFileProps method [.NET Framework metadata]
ms.assetid: c5e6216f-ae3d-4697-9688-66b69c1251ec
topic_type:
- apiref
ms.openlocfilehash: 78c192f10f629a0c1316ae7af7fc774819f4de8f
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84007475"
---
# <a name="imetadataassemblyimportgetfileprops-method"></a>Método IMetaDataAssemblyImport::GetFileProps
Obtém as propriedades do arquivo com a assinatura de metadados especificada.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetFileProps (  
    [in]  mdFile      mdf,
    [out] LPWSTR      szName,
    [in]  ULONG       cchName,
    [out] ULONG       *pchName,
    [out] const void  **ppbHashValue,
    [out] ULONG       *pcbHashValue,
    [out] DWORD       *pdwFileFlags  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `mdf`  
 no O `mdFile` token de metadados que representa o arquivo para o qual obter as propriedades.  
  
 `szName`  
 fora O nome simples do arquivo.  
  
 `cchName`  
 no O tamanho, em caracteres largos, de `szName` .  
  
 `pchName`  
 fora O número de caracteres largos realmente retornados em `szName` .  
  
 `ppbHashValue`  
 fora Um ponteiro para o valor de hash. Esse é o hash, usando o algoritmo SHA-1 do arquivo.  
  
 `pcbHashValue`  
 fora O número de caracteres largos no valor de hash retornado.  
  
 `pdwFileFlags`  
 fora Um ponteiro para os sinalizadores que descrevem os metadados aplicados a um arquivo. O valor de flags é uma combinação de um ou mais valores de [CorFileFlags](corfileflags-enumeration.md) .  
  
## <a name="requirements"></a>Requisitos  
 **Plataforma:** Consulte [requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataAssemblyImport](imetadataassemblyimport-interface.md)
