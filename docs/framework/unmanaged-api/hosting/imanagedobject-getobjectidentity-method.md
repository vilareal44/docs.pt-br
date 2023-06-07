---
title: Método IManagedObject::GetObjectIdentity
ms.date: 03/30/2017
api_name:
- IManagedObject.GetObjectIdentity
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- GetObjectIdentity
helpviewer_keywords:
- GetObjectIdentity method [.NET Framework hosting]
- IManagedObject::GetObjectIdentity method [.NET Framework hosting]
ms.assetid: b862ff3e-e480-4cdf-84e2-e1013334a467
topic_type:
- apiref
ms.openlocfilehash: 1b40ed8e107d30c22b4ade25d29376b1b74583d1
ms.sourcegitcommit: e5772b3ddcc114c80b4c9767ffdb3f6c7fad8f05
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83842407"
---
# <a name="imanagedobjectgetobjectidentity-method"></a>Método IManagedObject::GetObjectIdentity
Obtém a identidade deste objeto gerenciado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetObjectIdentity (  
    [out] BSTR*   pBSTRGUID,  
    [out] int*    AppDomainID,  
    [out] CCW_PTR pCCW  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pBSTRGUID`  
 fora Um ponteiro para o GUID do processo no qual o objeto reside.  
  
 `AppDomainID`  
 fora Um ponteiro para a ID do domínio do aplicativo do objeto.  
  
 `pCCW`  
 fora Um ponteiro para o índice do objeto na tabela v clássica COM.  
  
## <a name="remarks"></a>Comentários  
 A identidade de um objeto gerenciado inclui GUID do processo, ID de domínio do aplicativo e o índice do objeto na tabela v clássica do COM.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Consulte também

- [Interface IManagedObject](imanagedobject-interface.md)
