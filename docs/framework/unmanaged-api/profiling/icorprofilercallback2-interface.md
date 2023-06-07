---
title: Interface ICorProfilerCallback2
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback2
helpviewer_keywords:
- ICorProfilerCallback2 interface [.NET Framework profiling]
ms.assetid: 4a261dba-450d-4f1f-8d98-865b58bfc992
topic_type:
- apiref
ms.openlocfilehash: 3b0e60602d2f36552c3e0e85ec51205b4128486b
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84499760"
---
# <a name="icorprofilercallback2-interface"></a>Interface ICorProfilerCallback2
Fornece métodos que são usados pelo Common Language Runtime (CLR) para notificar um criador de perfil de código quando os eventos para os quais o criador de perfil assinou ocorreram. A `ICorProfilerCallback2` interface é uma extensão da interface [ICorProfilerCallback](icorprofilercallback-interface.md) . Ou seja, ele fornece novos retornos de chamada introduzidos na versão .NET Framework 2,0.  
  
> [!NOTE]
> Cada implementação de método deve retornar um HRESULT com um valor de S_OK em caso de êxito ou E_FAIL em caso de falha. Atualmente, o CLR ignora o HRESULT que é retornado por cada retorno de chamada, exceto [ICorProfilerCallback:: Objectreferenciations](icorprofilercallback-objectreferences-method.md).  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método FinalizeableObjectQueued](icorprofilercallback2-finalizeableobjectqueued-method.md)|Notifica o criador de perfil de código de que um objeto com um finalizador foi enfileirado para o thread do finalizador para execução de seu `Finalize` método.|  
|[Método GarbageCollectionFinished](icorprofilercallback2-garbagecollectionfinished-method.md)|Notifica o criador de perfil de que uma coleta de lixo foi concluída e todos os retornos de chamada de coleta de lixo foram emitidos para ela.|  
|[Método GarbageCollectionStarted](icorprofilercallback2-garbagecollectionstarted-method.md)|Notifica o criador de perfil de código de que uma coleta de lixo foi iniciada.|  
|[Método HandleCreated](icorprofilercallback2-handlecreated-method.md)|Notifica o criador de perfil de código de que um identificador de coleta de lixo foi criado.|  
|[Método HandleDestroyed](icorprofilercallback2-handledestroyed-method.md)|Notifica o criador de perfil de código de que um identificador de coleta de lixo foi destruído.|  
|[Método RootReferences2](icorprofilercallback2-rootreferences2-method.md)|Notifica o criador de perfil sobre referências de raiz após a ocorrência de uma coleta de lixo. Esse método é uma extensão do método [ICorProfilerCallback:: RootReferences](icorprofilercallback-rootreferences-method.md) .|  
|[Método SurvivingReferences](icorprofilercallback2-survivingreferences-method.md)|Notifica o criador de perfil sobre referências de objeto que têm sobreviveram uma coleta de lixo.|  
|[Método ThreadNameChanged](icorprofilercallback2-threadnamechanged-method.md)|Notifica o criador de perfil de código de que o nome de um thread foi alterado.|  
  
## <a name="remarks"></a>Comentários  
 O CLR chama um método na `ICorProfilerCallback` interface (ou `ICorProfilerCallback2` ) para notificar o criador de perfil quando um evento, ao qual o criador de perfil tinha se inscrito, ocorre. Essa é a interface de retorno de chamada principal por meio da qual o CLR se comunica com o criador de perfil de código.  
  
 Um criador de perfil de código deve implementar os métodos da `ICorProfilerCallback` interface. Para o .NET Framework 2,0 e versões posteriores, o criador de perfil também deve implementar os `ICorProfilerCallback2` métodos. Cada implementação de método deve retornar um HRESULT com um valor de S_OK em caso de êxito ou E_FAIL em caso de falha. Atualmente, o CLR ignora o HRESULT que é retornado por cada retorno de chamada, exceto [ICorProfilerCallback:: Objectreferenciations](icorprofilercallback-objectreferences-method.md).  
  
 Um criador de perfil de código deve se registrar no registro do Microsoft Windows, seu objeto COM que implementa as `ICorProfilerCallback` `ICorProfilerCallback2` interfaces e. Um criador de perfil de código assina os eventos para os quais deseja receber notificação chamando [ICorProfilerInfo:: SetEventMask](icorprofilerinfo-seteventmask-method.md). Isso geralmente é feito na implementação do criador de perfil de [ICorProfilerCallback:: Initialize](icorprofilercallback-initialize-method.md). Em seguida, o criador de perfil é capaz de receber a notificação do tempo de execução quando um evento está prestes a ocorrer ou ocorreu apenas em um processo de tempo de execução.  
  
> [!NOTE]
> O criador de perfil registra um único objeto COM. Se o criador de perfil estiver direcionando .NET Framework versão 1,0 ou 1,1, esse objeto COM precisará implementar apenas os métodos de `ICorProfilerCallback` . Se ele estiver direcionando .NET Framework versão 2,0 e posterior, o objeto COM também deverá implementar os métodos de `ICorProfilerCallback2` .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Criação de perfil de interfaces](profiling-interfaces.md)
- [Interface ICorProfilerCallback](icorprofilercallback-interface.md)
- [Interface ICorProfilerCallback3](icorprofilercallback3-interface.md)
- [Interface ICorProfilerCallback4](icorprofilercallback4-interface.md)
