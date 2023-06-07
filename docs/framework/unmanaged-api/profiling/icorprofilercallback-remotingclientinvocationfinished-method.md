---
title: Método ICorProfilerCallback::RemotingClientInvocationFinished
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.RemotingClientInvocationFinished
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::RemotingClientInvocationFinished
helpviewer_keywords:
- RemotingClientInvocationFinished method [.NET Framework profiling]
- ICorProfilerCallback::RemotingClientInvocationFinished method [.NET Framework profiling]
ms.assetid: ea4b283b-1210-4f41-a7a2-c398b1adde4e
topic_type:
- apiref
ms.openlocfilehash: f5786db1f17e8a463dc78f9c93464145be3a8f32
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84499981"
---
# <a name="icorprofilercallbackremotingclientinvocationfinished-method"></a>Método ICorProfilerCallback::RemotingClientInvocationFinished
Notifica o criador de perfil de que uma chamada de comunicação remota foi executada até a conclusão no cliente.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT RemotingClientInvocationFinished();  
```  
  
## <a name="remarks"></a>Comentários  
 Se a chamada remota tiver sido síncrona, ela também será executada para conclusão no servidor. Se a chamada remota tiver sido assíncrona, uma resposta ainda poderá ser esperada quando a chamada for tratada. Se uma resposta for esperada, ela ocorrerá como uma chamada para [ICorProfilerCallback:: RemotingClientReceivingReply](icorprofilercallback-remotingclientreceivingreply-method.md) e uma chamada adicional para `RemotingClientInvocationFinished` para indicar o processamento secundário necessário de uma chamada assíncrona.  
  
 Cada um dos seguintes pares de retornos de chamada ocorrerá no mesmo thread:  
  
- `RemotingClientInvocationStarted`e [ICorProfilerCallback:: RemotingClientSendingMessage](icorprofilercallback-remotingclientsendingmessage-method.md)  
  
- [ICorProfilerCallback:: RemotingClientReceivingReply](icorprofilercallback-remotingclientreceivingreply-method.md) e [ICorProfilerCallback:: RemotingClientInvocationFinished](icorprofilercallback-remotingclientinvocationfinished-method.md)  
  
- [ICorProfilerCallback:: RemotingServerInvocationReturned](icorprofilercallback-remotingserverinvocationreturned-method.md) e [ICorProfilerCallback:: RemotingServerSendingReply](icorprofilercallback-remotingserversendingreply-method.md)  
  
 Você deve estar ciente dos seguintes problemas com os retornos de chamada remotos:  
  
- A execução de uma função de comunicação remota não é refletida pela API do criador de perfil, portanto, as notificações para funções que são chamadas do cliente e executadas no servidor não são recebidas corretamente. A invocação real ocorre por meio de um objeto proxy; para o criador de perfil, parece que determinadas funções são compiladas por JIT, mas nunca são usadas.  
  
- O criador de perfil não recebe notificações precisas para eventos de comunicação remota assíncrona.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback](icorprofilercallback-interface.md)
