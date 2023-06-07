---
title: Método IMetaDataImport2::GetGenericParamProps
ms.date: 03/30/2017
api_name:
- IMetaDataImport2.GetGenericParamProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport2::GetGenericParamProps
helpviewer_keywords:
- IMetaDataImport2::GetGenericParamProps method [.NET Framework metadata]
- GetGenericParamProps method [.NET Framework metadata]
ms.assetid: dbb21e67-712b-49e7-a27c-a1e73ffd46c5
topic_type:
- apiref
ms.openlocfilehash: 7e97b2d4ad1fec4675d1484959b115a4d4b87e90
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84490608"
---
# <a name="imetadataimport2getgenericparamprops-method"></a>Método IMetaDataImport2::GetGenericParamProps
Obtém os metadados associados ao parâmetro genérico representado pelo token especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetGenericParamProps (  
   [in]  mdGenericParam  gp,  
   [out] ULONG           *pulParamSeq,  
   [out] DWORD           *pdwParamFlags,  
   [out] mdToken         *ptOwner,  
   [out] DWORD           *reserved,  
   [out] LPWSTR          wzName,  
   [in]  ULONG           cchName,  
   [out] ULONG           *pchName  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `gp`  
 no O token que representa o parâmetro genérico para o qual retornar metadados.  
  
 `pulParamSeq`  
 fora A posição ordinal do `Type` parâmetro no construtor ou método pai.  
  
 `pdwParamFlags`  
 fora Um valor da enumeração [CorGenericParamAttr](corgenericparamattr-enumeration.md) que descreve o `Type` para o parâmetro genérico.  
  
 `ptOwner`  
 fora Um TypeDef ou token MethodDef que representa o proprietário do parâmetro.  
  
 `reserved`  
 fora Reservado para extensibilidade futura.  
  
 `wzName`  
 fora O nome do parâmetro genérico.  
  
 `cchName`  
 no O tamanho do `wzName` buffer.  
  
 `pchName`  
 fora O tamanho retornado do nome, em caracteres largos.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport2](imetadataimport2-interface.md)
- [Interface IMetaDataImport](imetadataimport-interface.md)
