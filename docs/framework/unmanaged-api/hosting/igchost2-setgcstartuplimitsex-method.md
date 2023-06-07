---
title: Método IGCHost2::SetGCStartupLimitsEx
ms.date: 03/30/2017
api_name:
- IGCHost2.SetGCStartupLimitsEx
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IGCHost2::SetGCStartupLimitsEx
helpviewer_keywords:
- IGCHost2::SetGCStartupLimitsEx method [.NET Framework hosting]
- SetGCStartupLimitsEx method, IGCHost2 interface [.NET Framework hosting]
ms.assetid: bba941c2-1c57-46d3-bbf5-5fb92700c490
topic_type:
- apiref
ms.openlocfilehash: ca9566168b8aae361af8d61539066624697a2d04
ms.sourcegitcommit: d223616e7e6fe2139079052e6fcbe25413fb9900
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83805151"
---
# <a name="igchost2setgcstartuplimitsex-method"></a>Método IGCHost2::SetGCStartupLimitsEx
Define o tamanho do segmento e o tamanho máximo para a geração 0.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SetGCStartupLimitsEx (  
    [in] SIZE_T SegmentSize,  
    [in] SIZE_T MaxGen0Size  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `SegmentSize`  
 no O tamanho do segmento usado pelo sistema de coleta de lixo.  
  
 `MaxGen0Size`  
 no O tamanho máximo para a geração 0.  
  
## <a name="remarks"></a>Comentários  
 Os valores que `SetGCStartupLimitsEx` conjuntos só podem ser especificados antes do host ser iniciado. Esses valores não podem ser alterados posteriormente.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** GCHost. idl, GCHost. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IGCHost2](igchost2-interface.md)
