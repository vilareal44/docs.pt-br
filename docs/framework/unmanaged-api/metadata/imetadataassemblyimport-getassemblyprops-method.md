---
title: Método IMetaDataAssemblyImport::GetAssemblyProps
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyImport.GetAssemblyProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyImport::GetAssemblyProps
helpviewer_keywords:
- GetAssemblyProps method [.NET Framework metadata]
- IMetaDataAssemblyImport::GetAssemblyProps method [.NET Framework metadata]
ms.assetid: 0eaa4aa9-9441-444a-920c-e4b2a2db899e
topic_type:
- apiref
ms.openlocfilehash: a90deaf3e9ddf326c6fca558cbb4681fc40e022d
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84009048"
---
# <a name="imetadataassemblyimportgetassemblyprops-method"></a>Método IMetaDataAssemblyImport::GetAssemblyProps
Obtém o conjunto de propriedades para o assembly com a assinatura de metadados especificada.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetAssemblyProps (  
    [in]  mdAssembly          mda,  
    [out] const void          **ppbPublicKey,
    [out] ULONG               *pcbPublicKey,  
    [out] ULONG               *pulHashAlgId,  
    [out] LPWSTR              szName,  
    [in] ULONG                cchName,  
    [out] ULONG               *pchName,  
    [out] ASSEMBLYMETADATA    *pMetaData,  
    [out] DWORD               *pdwAssemblyFlags  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `mda`  
 [in]. O `mdAssembly` token de metadados que representa o assembly para o qual obter as propriedades.  
  
 `ppbPublicKey`  
 fora Um ponteiro para a chave pública ou o token de metadados.  
  
 `pcbPublicKey`  
 fora O número de bytes na chave pública retornada.  
  
 `pulHashAlgId`  
 fora Um ponteiro para o algoritmo usado para fazer o hash dos arquivos no assembly.  
  
 `szName`  
 fora O nome simples do assembly.  
  
 `cchName`  
 no O tamanho, em caracteres largos, de `szName` .  
  
 `pchName`  
 fora O número de caracteres largos realmente retornados em `szName` .  
  
 `pMetaData`  
 fora Um ponteiro para uma estrutura ASSEMBLYMETADATA que contém os metadados do assembly.  
  
 `pdwAssemblyFlags`  
 fora Sinalizadores que descrevem os metadados aplicados a um assembly. Esse valor é uma combinação de um ou mais valores de [CorAssemblyFlags](corassemblyflags-enumeration.md) .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataAssemblyImport](imetadataassemblyimport-interface.md)
