---
title: Método IMetaDataImport::GetScopeProps
ms.date: 03/30/2017
api_name:
- IMetaDataImport.GetScopeProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetScopeProps
helpviewer_keywords:
- IMetaDataImport::GetScopeProps method [.NET Framework metadata]
- GetScopeProps method [.NET Framework metadata]
ms.assetid: c8ba42d2-d9fa-43cb-bbc0-f33e1e592cb6
topic_type:
- apiref
ms.openlocfilehash: 0916b6382bb9352616d85e21f423301dc6aa9fa9
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84490842"
---
# <a name="imetadataimportgetscopeprops-method"></a>Método IMetaDataImport::GetScopeProps
Obtém o nome e, opcionalmente, o identificador de versão do assembly ou módulo no escopo de metadados atual.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetScopeProps (  
   [out] LPWSTR           szName,  
   [in]  ULONG            cchName,  
   [out] ULONG            *pchName,  
   [out, optional] GUID   *pmvid  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `szName`  
 fora Um buffer para o assembly ou o nome do módulo.  
  
 `cchName`  
 no O tamanho em caracteres largos de `szName` .  
  
 `pchName`  
 fora O número de caracteres largos retornados em `szName` .  
  
 `pmvid`  
 [saída, opcional] Um ponteiro para um GUID que identifica exclusivamente a versão do assembly ou módulo.  
  
## <a name="remarks"></a>Comentários  
 O método [IMetaDataEmit:: SetModuleProps](imetadataemit-setmoduleprops-method.md) é usado para definir essas propriedades.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataImport](imetadataimport-interface.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
