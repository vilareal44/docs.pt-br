---
title: Interface IHostIoCompletionManager
ms.date: 03/30/2017
api_name:
- IHostIoCompletionManager
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostIoCompletionManager
helpviewer_keywords:
- IHostIoCompletionManager interface [.NET Framework hosting]
ms.assetid: c28d1983-83f7-46e2-990f-dbb9dc07c818
topic_type:
- apiref
ms.openlocfilehash: 095872f8d4bd4f7d3351b8b3e3f8f8445b615cd8
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501528"
---
# <a name="ihostiocompletionmanager-interface"></a>Interface IHostIoCompletionManager
Fornece métodos que permitem que o Common Language Runtime (CLR) interaja com portas de conclusão de e/s fornecidas pelo host.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método Bind](ihostiocompletionmanager-bind-method.md)|Associa um identificador a uma porta de conclusão de e/s.|  
|[Método CloseIoCompletionPort](ihostiocompletionmanager-closeiocompletionport-method.md)|Fecha uma porta que foi criada por meio de uma chamada anterior para `CreateIoCompletionPort` .|  
|[Método CreateIoCompletionPort](ihostiocompletionmanager-createiocompletionport-method.md)|Solicita que o host crie uma nova porta de conclusão de e/s.|  
|[Método GetAvailableThreads](ihostiocompletionmanager-getavailablethreads-method.md)|Obtém o número de threads de conclusão de e/s que não estão processando solicitações no momento.|  
|[Método GetHostOverlappedSize](ihostiocompletionmanager-gethostoverlappedsize-method.md)|Obtém o tamanho de qualquer dado personalizado que o host pretende acrescentar às solicitações de e/s.|  
|[Método GetMaxThreads](ihostiocompletionmanager-getmaxthreads-method.md)|Obtém o número máximo de threads que o host pode alocar para solicitações de e/s de serviço.|  
|[Método GetMinThreads](ihostiocompletionmanager-getminthreads-method.md)|Obtém o número mínimo de threads que o host fornece para atender solicitações de e/s.|  
|[Método InitializeHostOverlapped](ihostiocompletionmanager-initializehostoverlapped-method.md)|Fornece ao host uma oportunidade de inicializar quaisquer dados personalizados sobre uma solicitação de e/s.|  
|[Método SetCLRIoCompletionManager](ihostiocompletionmanager-setclriocompletionmanager-method.md)|Fornece ao host um ponteiro de interface para uma instância [ICLRIoCompletionManager](iclriocompletionmanager-interface.md) implementada pelo CLR.|  
|[Método SetMaxThreads](ihostiocompletionmanager-setmaxthreads-method.md)|Define o número máximo de threads que o host aloca para atender às solicitações de e/s.|  
|[Método SetMinThreads](ihostiocompletionmanager-setminthreads-method.md)|Define o número mínimo de threads que o host deve alocar para a conclusão de e/s.|  
  
## <a name="remarks"></a>Comentários  
 `IHostIoCompletionManager`corresponde à `ICLRIoCompletionManager` interface implementada pelo CLR. O CLR chama os métodos de `IHostIoCompletionManager` para associar identificadores às portas que o host fornece, e o host chama os métodos de `ICLRIoCompletionManager` para relatar a conclusão de solicitações de e/s.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interfaces de hospedagem](hosting-interfaces.md)
