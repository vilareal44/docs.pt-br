---
title: Método IMetaDataAssemblyImport::FindExportedTypeByName
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyImport.FindExportedTypeByName
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyImport::FindExportedTypeByName
helpviewer_keywords:
- FindExportedTypeByName method [.NET Framework metadata]
- IMetaDataAssemblyImport::FindExportedTypeByName method [.NET Framework metadata]
ms.assetid: 46264b2c-574d-4dde-aafc-77187a104fdd
topic_type:
- apiref
ms.openlocfilehash: ac6de9a16fad6ba9d14f3960ddd28c42c111f254
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84009386"
---
# <a name="imetadataassemblyimportfindexportedtypebyname-method"></a>Método IMetaDataAssemblyImport::FindExportedTypeByName
Obtém um ponteiro para um tipo exportado, dado seu nome e tipo de delimitador.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT FindExportedTypeByName (  
    [in]  LPCWSTR           szName,
    [in]  mdToken           mdtExportedType,
    [out] mdExportedType    *ptkExportedType  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `szName`  
 no O nome do tipo exportado.  
  
 `mdtExportedType`  
 no O token de metadados para a classe de circunscrição do tipo exportado. Esse valor é `mdExportedTypeNil` se o tipo exportado solicitado não for um tipo aninhado.  
  
 `ptkExportedType`  
 fora Um ponteiro para o `mdExportedType` token que representa o tipo exportado.  
  
## <a name="remarks"></a>Comentários  
 O `FindExportedTypeByName` método usa as regras padrão empregadas pelo Common Language Runtime para resolver referências.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataAssemblyImport](imetadataassemblyimport-interface.md)
- [Como o runtime localiza assemblies](../../deployment/how-the-runtime-locates-assemblies.md)
