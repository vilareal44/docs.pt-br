---
title: Interface IHostAssemblyManager
ms.date: 03/30/2017
api_name:
- IHostAssemblyManager
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostAssemblyManager
helpviewer_keywords:
- IHostAssemblyManager interface [.NET Framework hosting]
ms.assetid: dfec05bb-3cd7-4bd5-b396-a4f097c3a636
topic_type:
- apiref
ms.openlocfilehash: 4e32a36a4cf751bf7c5a2c918fde0122f21b7878
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501580"
---
# <a name="ihostassemblymanager-interface"></a>Interface IHostAssemblyManager
Fornece métodos que permitem que um host especifique conjuntos de assemblies que devem ser carregados pelo Common Language Runtime (CLR) ou pelo host.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método GetAssemblyStore](ihostassemblymanager-getassemblystore-method.md)|Obtém um ponteiro de interface para um [IHostAssemblyStore](ihostassemblystore-interface.md) que representa a lista de assemblies carregados pelo host.|  
|[Método GetNonHostStoreAssemblies](ihostassemblymanager-getnonhoststoreassemblies-method.md)|Obtém um ponteiro de interface para um [ICLRAssemblyReferenceList](iclrassemblyreferencelist-interface.md) que representa a lista de assemblies que o host espera que o CLR carregue.|  
  
## <a name="remarks"></a>Comentários  
 Não é necessário que o host implemente `IHostAssemblyManager` ou `IHostAssemblyStore` . Se o host implementar `IHostAssemblyManager` , ele também deverá implementar `IHostAssemblyStore` .  
  
 As consultas de tempo de execução para um `IHostAssemblyManager` chamando [IHostControl:: GetHostManager](ihostcontrol-gethostmanager-method.md) na inicialização com um `IID` de IID_IHostAssemblyManager.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICLRAssemblyReferenceList](iclrassemblyreferencelist-interface.md)
- [Interface IHostAssemblyStore](ihostassemblystore-interface.md)
- [Interface IHostControl](ihostcontrol-interface.md)
- [Interfaces de hospedagem](hosting-interfaces.md)
