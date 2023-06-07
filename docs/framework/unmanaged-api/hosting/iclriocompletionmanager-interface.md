---
title: Interface ICLRIoCompletionManager
ms.date: 03/30/2017
api_name:
- ICLRIoCompletionManager
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRIoCompletionManager
helpviewer_keywords:
- ICLRIoCompletionManager interface [.NET Framework hosting]
ms.assetid: c6c3ace6-e5e7-4450-8cc5-a9a48208c493
topic_type:
- apiref
ms.openlocfilehash: 71afc5e9772f82b922e8f428e6d808e46d092704
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504206"
---
# <a name="iclriocompletionmanager-interface"></a>Interface ICLRIoCompletionManager
Implementa um método de retorno de chamada que permite ao host notificar o Common Language Runtime (CLR) do status de solicitações de e/s especificadas.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método OnComplete](iclriocompletionmanager-oncomplete-method.md)|Notifica o CLR sobre o status de uma solicitação de e/s que foi feita usando uma chamada para o método [IHostIoCompletionManager:: bind](ihostiocompletionmanager-bind-method.md) .|  
  
## <a name="remarks"></a>Comentários  
 O host implementa a abstração de conclusão de e/s usando a interface [IHostIoCompletionManager](ihostiocompletionmanager-interface.md) . O CLR faz solicitações de e/s por meio dessa interface, e o host notifica o tempo de execução do resultado de tais solicitações usando a `ICLRIoCompletionManager` interface.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IHostIoCompletionManager](ihostiocompletionmanager-interface.md)
- [Interface IHostThreadPoolManager](ihostthreadpoolmanager-interface.md)
- [Interfaces de hospedagem](hosting-interfaces.md)
