---
title: Interface ICLRTask
ms.date: 03/30/2017
api_name:
- ICLRTask
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRTask
helpviewer_keywords:
- ICLRTask interface [.NET Framework hosting]
ms.assetid: b3a44df3-578a-4451-b55e-70c8e7695f5e
topic_type:
- apiref
ms.openlocfilehash: b1327e13006ca4b3f9074c1348b1817c9a1b3728
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503946"
---
# <a name="iclrtask-interface"></a>Interface ICLRTask
Fornece métodos que permitem ao host fazer solicitações do Common Language Runtime (CLR) ou fornecer notificação ao CLR sobre a tarefa associada.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método Abort](iclrtask-abort-method.md)|Solicita que o CLR anule a tarefa que a `ICLRTask` instância atual representa.|  
|[Método ExitTask](iclrtask-exittask-method.md)|Notifica o CLR que a tarefa associada à `ICLRTask` instância atual está terminando e tenta desligar a tarefa normalmente.|  
|[Método GetMemStats](iclrtask-getmemstats-method.md)|Obtém informações estatísticas sobre o uso de recursos de memória pela tarefa representada pela instância atual `ICLRTask` .|  
|[Método LocksHeld](iclrtask-locksheld-method.md)|Obtém o número de bloqueios atualmente mantidos na tarefa.|  
|[Método NeedsPriorityScheduling](iclrtask-needspriorityscheduling-method.md)|Obtém um valor que indica se o host deve atribuir uma prioridade alta para reagendar a tarefa representada pela `ICLRTask` instância atual.|  
|[Método Reset](iclrtask-reset-method.md)|Informa ao CLR que o host concluiu uma tarefa e permite que o CLR reutilize a `ICLRTask` instância atual para representar outra tarefa.|  
|[Método RudeAbort](iclrtask-rudeabort-method.md)|Faz com que o CLR anule a tarefa representada pela `ICLRTask` instância atual imediatamente, sem uma garantia de que os finalizadores serão executados.|  
|[Método SetTaskIdentifier](iclrtask-settaskidentifier-method.md)|Define um identificador exclusivo para a tarefa representada pela instância atual `ICLRTask` , para uso na depuração.|  
|[Método SwitchIn](iclrtask-switchin-method.md)|Notifica o CLR que a tarefa representada pela `ICLRTask` instância atual está em um estado operável.|  
|[Método SwitchOut](iclrtask-switchout-method.md)|Notifica o CLR que a tarefa representada pela `ICLRTask` instância atual não está mais em um estado operável.|  
|[Método YieldTask](iclrtask-yieldtask-method.md)|Solicita que o CLR torne o tempo do processador disponível para outras tarefas. O CLR não garante que a tarefa seja colocada em um estado em que possa gerar o tempo de processamento.|  
  
## <a name="remarks"></a>Comentários  
 Um `ICLRTask` é a representação de uma tarefa para o CLR. A qualquer momento durante a execução de código, uma tarefa pode ser descrita como em execução ou aguardando para ser executada. O host chama o `ICLRTask::SwitchIn` método para notificar o CLR que a tarefa que a `ICLRTask` instância atual representa agora está em um estado operável. Após uma chamada para `ICLRTask::SwitchIn` , o host pode agendar a tarefa em qualquer thread do sistema operacional, exceto nos casos em que o tempo de execução requer afinidade de thread, conforme especificado por chamadas para os métodos [IHostTaskManager:: BeginThreadAffinity](ihosttaskmanager-beginthreadaffinity-method.md) e [IHostTaskManager:: EndThreadAffinity](ihosttaskmanager-endthreadaffinity-method.md) . Algum tempo depois, o sistema operacional pode decidir remover a tarefa do thread e colocá-la em um estado de não execução. Por exemplo, isso pode ocorrer sempre que a tarefa é bloqueada em primitivos de sincronização ou aguarda a conclusão de operações de e/s. O host chama o [SwitchOut](iclrtask-switchout-method.md) para notificar o CLR que a tarefa representada pela `ICLRTask` instância atual não está mais em um estado operável.  
  
 Normalmente, uma tarefa é encerrada no final da execução do código. Nesse momento, o host chama `ICLRTask::ExitTask` para destruir o associado `ICLRTask` . No entanto, as tarefas também podem ser recicladas usando uma chamada para `ICLRTask::Reset` , o que permite que a `ICLRTask` instância seja usada novamente. Essa abordagem impede a sobrecarga de criar e destruir instâncias repetidamente.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICLRTaskManager](iclrtaskmanager-interface.md)
- [Interface IHostTask](ihosttask-interface.md)
- [Interface IHostTaskManager](ihosttaskmanager-interface.md)
- [Interfaces de hospedagem](hosting-interfaces.md)
- [Interface ICLRTask2](iclrtask2-interface.md)
