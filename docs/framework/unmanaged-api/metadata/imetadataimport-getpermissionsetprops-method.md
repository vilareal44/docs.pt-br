---
title: Método IMetaDataImport::GetPermissionSetProps
ms.date: 03/30/2017
api_name:
- IMetaDataImport.GetPermissionSetProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetPermissionSetProps
helpviewer_keywords:
- GetPermissionSetProps method [.NET Framework metadata]
- IMetaDataImport::GetPermissionSetProps method [.NET Framework metadata]
ms.assetid: 9855f0e4-12c0-4d3d-ab5d-d6bc52d25eae
topic_type:
- apiref
ms.openlocfilehash: 54c75156c32e5b40aa933ef6530b2cc33edf7de4
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84490985"
---
# <a name="imetadataimportgetpermissionsetprops-method"></a>Método IMetaDataImport::GetPermissionSetProps
Obtém os metadados associados ao <xref:System.Security.PermissionSet?displayProperty=nameWithType> representado pelo token de permissão especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetPermissionSetProps (  
   [in]  mdPermission      pm,  
   [out] DWORD             *pdwAction,
   [out] void const        **ppvPermission,
   [out] ULONG             *pcbPermission  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pm`  
 no O token de metadados de permissão que representa o conjunto de permissões para obter as propriedades de metadados.  
  
 `pdwAction`  
 fora Um ponteiro para o conjunto de permissões.  
  
 `ppvPermission`  
 fora Um ponteiro para a assinatura de metadados binários do conjunto de permissões.  
  
 `pcbPermission`  
 fora O tamanho em bytes de `ppvPermission` .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.Security.PermissionSet>
- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
