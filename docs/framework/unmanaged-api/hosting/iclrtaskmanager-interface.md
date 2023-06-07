---
title: Interface ICLRTaskManager
ms.date: 03/30/2017
api_name:
- ICLRTaskManager
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRTaskManager
helpviewer_keywords:
- ICLRTaskManager interface [.NET Framework hosting]
ms.assetid: 2bd55e0c-001b-41fd-b29d-f01670fe8216
topic_type:
- apiref
ms.openlocfilehash: f918d4e7b95922734d70ed832581e6c494c70b05
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501632"
---
# <a name="iclrtaskmanager-interface"></a>Interface ICLRTaskManager
Fornece métodos que permitem que o host solicite explicitamente que o Common Language Runtime (CLR) crie uma nova tarefa, obtenha a tarefa em execução no momento e defina o idioma geográfico e a cultura da tarefa.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método CreateTask](iclrtaskmanager-createtask-method.md)|Solicita explicitamente que o CLR crie uma nova instância de [ICLRTask](iclrtask-interface.md) .|  
|[Método GetCurrentTask](iclrtaskmanager-getcurrenttask-method.md)|Obtém a `ICLRTask` instância que representa a tarefa que está sendo executada no momento.|  
|[Método GetCurrentTaskType](iclrtaskmanager-getcurrenttasktype-method.md)|Obtém o tipo da tarefa que está sendo executada no momento.|  
|[Método SetLocale](iclrtaskmanager-setlocale-method.md)|Notifica o CLR de que o host modificou o identificador de localidade na tarefa em execução no momento.|  
|[Método SetUILocale](iclrtaskmanager-setuilocale-method.md)|Notifica o Common Language Runtime que o host modificou o identificador de localidade da interface do usuário na tarefa em execução no momento.|  
  
## <a name="remarks"></a>Comentários  
 Cada tarefa que está sendo executada em um ambiente hospedado tem representações no lado do host (uma instância de [IHostTask](ihosttask-interface.md)) e no lado do CLR (uma instância de [ICLRTask](iclrtask-interface.md)). O host ou o CLR podem iniciar a criação de uma tarefa, mas a representação no lado do host deve ser associada a uma representação do lado do CLR correspondente para garantir a comunicação bem-sucedida entre o host e o CLR em relação à tarefa. Os dois objetos devem ser criados e instanciados antes que o código gerenciado possa ser executado em um thread do sistema operacional.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICLRTask](iclrtask-interface.md)
- [Interface IHostTask](ihosttask-interface.md)
- [Interface IHostTaskManager](ihosttaskmanager-interface.md)
- [Interfaces de hospedagem](hosting-interfaces.md)
