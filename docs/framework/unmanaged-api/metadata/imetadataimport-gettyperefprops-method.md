---
title: Método IMetaDataImport::GetTypeRefProps
ms.date: 03/30/2017
api_name:
- IMetaDataImport.GetTypeRefProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetTypeRefProps
helpviewer_keywords:
- IMetaDataImport::GetTypeRefProps method [.NET Framework metadata]
- GetTypeRefProps method [.NET Framework metadata]
ms.assetid: 01837955-ce1e-4068-b338-fd473bd77d1d
topic_type:
- apiref
ms.openlocfilehash: 273922e00c3e5319d5a03652cc77b69f4479ea67
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503517"
---
# <a name="imetadataimportgettyperefprops-method"></a>Método IMetaDataImport::GetTypeRefProps
Obtém os metadados associados com o <xref:System.Type> referido pelo token TypeRef especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetTypeRefProps (  
   [in]  mdTypeRef   tr,  
   [out] mdToken     *ptkResolutionScope,  
   [out] LPWSTR      szName,  
   [in]  ULONG       cchName,  
   [out] ULONG       *pchName  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `tr`  
 no O token TypeRef que representa o tipo para o qual retornar metadados.  
  
 `ptkResolutionScope`  
 fora Um ponteiro para o escopo no qual a referência é feita. Esse valor é um token AssemblyRef ou ModuleRef.  
  
 `szName`  
 fora Um buffer que contém o nome do tipo.  
  
 `cchName`  
 no O tamanho solicitado em caracteres largos de `szName` .  
  
 `pchName`  
 fora O tamanho retornado em caracteres largos de `szName` .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
