---
title: Interface IHostTaskManager
ms.date: 03/30/2017
api_name:
- IHostTaskManager
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostTaskManager
helpviewer_keywords:
- IHostTaskManager interface [.NET Framework hosting]
ms.assetid: 4a0b05b9-3ef1-4607-b7c8-bd4dd43647a0
topic_type:
- apiref
ms.openlocfilehash: 190908c675b96b8ea2d81fb0203aa16a80d6a8b4
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501389"
---
# <a name="ihosttaskmanager-interface"></a>Interface IHostTaskManager
Fornece métodos que permitem que o Common Language Runtime (CLR) trabalhe com tarefas por meio do host em vez de usar as funções de fibra ou de Threading do sistema operacional padrão.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método BeginDelayAbort](ihosttaskmanager-begindelayabort-method.md)|Notifica o host de que o código gerenciado está entrando em um período no qual a tarefa atual não deve ser anulada.|  
|[Método BeginThreadAffinity](ihosttaskmanager-beginthreadaffinity-method.md)|Notifica o host de que o código gerenciado está entrando em um período no qual a tarefa atual não deve ser movida para outro thread do sistema operacional.|  
|[Método CallNeedsHostHook](ihosttaskmanager-callneedshosthook-method.md)|Permite que o host especifique se o Common Language Runtime pode embutir a chamada especificada para uma função não gerenciada.|  
|[Método CreateTask](ihosttaskmanager-createtask-method.md)|Solicita que o host crie uma nova tarefa.|  
|[Método EndDelayAbort](ihosttaskmanager-enddelayabort-method.md)|Notifica o host de que o código gerenciado está saindo do período no qual a tarefa atual não deve ser anulada, seguindo uma chamada anterior para `BeginDelayAbort` .|  
|[Método EndThreadAffinity](ihosttaskmanager-endthreadaffinity-method.md)|Notifica o host de que o código gerenciado está saindo do período no qual a tarefa atual não deve ser movida para outro thread do sistema operacional, seguindo uma chamada anterior para `BeginThreadAffinity` .|  
|[Método EnterRuntime](ihosttaskmanager-enterruntime-method.md)|Notifica o host de que uma chamada para um método não gerenciado, como um método de invocação de plataforma, está retornando o controle de execução para o CLR.|  
|[Método GetCurrentTask](ihosttaskmanager-getcurrenttask-method.md)|Obtém um ponteiro de interface para a tarefa que está atualmente em execução no thread do sistema operacional do qual essa chamada é feita.|  
|[Método GetStackGuarantee](ihosttaskmanager-getstackguarantee-method.md)|Obtém a quantidade de espaço de pilha que tem garantia de disponibilidade após a conclusão de uma operação de pilha, mas antes do fechamento de um processo.|  
|[Método LeaveRuntime](ihosttaskmanager-leaveruntime-method.md)|Notifica o host que o código gerenciado está prestes a fazer uma chamada para uma função não gerenciada.|  
|[Método ReverseEnterRuntime](ihosttaskmanager-reverseenterruntime-method.md)|Notifica o host de que uma chamada está sendo feita no Common Language Runtime (CLR) de código não gerenciado.|  
|[Método ReverseLeaveRuntime](ihosttaskmanager-reverseleaveruntime-method.md)|Notifica o host que o controle está deixando o CLR e inserindo uma função não gerenciada que, por sua vez, foi chamada a partir do código gerenciado.|  
|[Método SetCLRTaskManager](ihosttaskmanager-setclrtaskmanager-method.md)|Fornece ao host um ponteiro de interface para uma instância [ICLRTaskManager](iclrtaskmanager-interface.md) implementada pelo CLR.|  
|[Método SetLocale](ihosttaskmanager-setlocale-method.md)|Notifica o host de que o CLR alterou a localidade na tarefa atual.|  
|[Método SetStackGuarantee](ihosttaskmanager-setstackguarantee-method.md)|Reservado apenas para uso interno.|  
|[Método SetUILocale](ihosttaskmanager-setuilocale-method.md)|Notifica o host de que a localidade da interface do usuário foi alterada na tarefa atual.|  
|[Método Sleep](ihosttaskmanager-sleep-method.md)|Notifica o host que a tarefa atual vai suspender.|  
|[Método SwitchToTask](ihosttaskmanager-switchtotask-method.md)|Notifica o host de que ele deve desativar a tarefa atual.|  
  
## <a name="remarks"></a>Comentários  
 `IHostTaskManager`permite que o CLR crie e gerencie tarefas, para fornecer ganchos para o host tomar uma ação quando o controle transfere de um código gerenciado para não gerenciado e vice-versa, e para especificar determinadas ações que o host pode e não pode executar durante a execução do código.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICLRTask](iclrtask-interface.md)
- [Interface ICLRTaskManager](iclrtaskmanager-interface.md)
- [Interface IHostTask](ihosttask-interface.md)
- [Interfaces de hospedagem](hosting-interfaces.md)
