---
title: Método IHostMemoryManager::AcquiredVirtualAddressSpace
ms.date: 03/30/2017
api_name:
- IHostMemoryManager.AcquiredVirtualAddressSpace
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostMemoryManager::AcquiredVirtualAddressSpace
helpviewer_keywords:
- IHostMemoryManager::AcquiredVirtualAddressSpace method [.NET Framework hosting]
- AcquiredVirtualAddressSpace method [.NET Framework hosting]
ms.assetid: ef2f83c2-127e-4c38-8385-306c03cd2167
topic_type:
- apiref
ms.openlocfilehash: f5469a6f35826bcb06fe821e3748861dbf3682f3
ms.sourcegitcommit: d223616e7e6fe2139079052e6fcbe25413fb9900
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83804538"
---
# <a name="ihostmemorymanageracquiredvirtualaddressspace-method"></a>Método IHostMemoryManager::AcquiredVirtualAddressSpace
Notifica o host de que o Common Language Runtime (CLR) adquiriu a memória especificada do sistema operacional.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT AcquiredVirtualAddressSpace(  
    [in] LPVOID  startAddress,  
    [in] SIZE_T  size  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `startAddress`  
 no O endereço inicial da memória.  
  
 `size`  
 no O tamanho, em bytes, da memória.  
  
## <a name="remarks"></a>Comentários  
 O `AcquiredVirtualAddressSpace` método é um método de retorno de chamada e deve ser implementado pelo gravador do aplicativo de hospedagem. Ele é chamado pelo CLR.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IHostMemoryManager](ihostmemorymanager-interface.md)
