---
title: Interface ICorProfilerCallback
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback
helpviewer_keywords:
- ICorProfilerCallback interface [.NET Framework profiling]
ms.assetid: 4bae06f7-94d7-4ba8-b250-648b2da78674
topic_type:
- apiref
ms.openlocfilehash: 6a53b9b1b061c2ca07a469abc78c07ed9e710069
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500085"
---
# <a name="icorprofilercallback-interface"></a>Interface ICorProfilerCallback
Fornece métodos que são usados pelo Common Language Runtime (CLR) para notificar um criador de perfil de código quando os eventos para os quais o criador de perfil assinou ocorreram.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método AppDomainCreationFinished](icorprofilercallback-appdomaincreationfinished-method.md)|Notifica o criador de perfil de que um domínio de aplicativo foi criado.|  
|[Método AppDomainCreationStarted](icorprofilercallback-appdomaincreationstarted-method.md)|Notifica o criador de perfil de que um domínio de aplicativo está sendo criado.|  
|[Método AppDomainShutdownFinished](icorprofilercallback-appdomainshutdownfinished-method.md)|Notifica o criador de perfil de que um domínio de aplicativo foi descarregado de um processo.|  
|[Método AppDomainShutdownStarted](icorprofilercallback-appdomainshutdownstarted-method.md)|Notifica o criador de perfil de que um domínio de aplicativo está sendo descarregado de um processo.|  
|[Método AssemblyLoadFinished](icorprofilercallback-assemblyloadfinished-method.md)|Notifica o criador de perfil que concluiu o carregamento de um assembly.|  
|[Método AssemblyLoadStarted](icorprofilercallback-assemblyloadstarted-method.md)|Notifica o criador de perfil de que um assembly está sendo carregado.|  
|[Método AssemblyUnloadFinished](icorprofilercallback-assemblyunloadfinished-method.md)|Notifica o criador de perfil de que um assembly foi descarregado.|  
|[Método AssemblyUnloadStarted](icorprofilercallback-assemblyunloadstarted-method.md)|Notifica o criador de perfil de que um assembly está sendo descarregado.|  
|[Método ClassLoadFinished](icorprofilercallback-classloadfinished-method.md)|Notifica o criador de perfil que a conclusão do carregamento de uma classe.|  
|[Método ClassLoadStarted](icorprofilercallback-classloadstarted-method.md)|Notifica o criador de perfil de que uma classe está sendo carregada.|  
|[Método ClassUnloadFinished](icorprofilercallback-classunloadfinished-method.md)|Notifica o criador de perfil de que uma classe concluiu o descarregamento.|  
|[Método ClassUnloadStarted](icorprofilercallback-classunloadstarted-method.md)|Notifica o criador de perfil de que uma classe está sendo descarregada.|  
|[Método COMClassicVTableCreated](icorprofilercallback-comclassicvtablecreated-method.md)|Notifica o criador de perfil de que um RCW (Runtime Callable Wrapper) para o IID e a classe especificados foram criados.|  
|[Método COMClassicVTableDestroyed](icorprofilercallback-comclassicvtabledestroyed-method.md)|Notifica o criador de perfil de que um RCW está sendo destruído.|  
|[Método ExceptionCatcherEnter](icorprofilercallback-exceptioncatcherenter-method.md)|Notifica o criador de perfil que o controle está sendo passado para o `catch` bloco apropriado.|  
|[Método ExceptionCatcherLeave](icorprofilercallback-exceptioncatcherleave-method.md)|Notifica o criador de perfil que o controle está sendo passado para fora do `catch` bloco apropriado.|  
|[Método ExceptionCLRCatcherExecute](icorprofilercallback-exceptionclrcatcherexecute-method.md)|Obsoleto no .NET Framework versão 2,0.|  
|[Método ExceptionCLRCatcherFound](icorprofilercallback-exceptionclrcatcherfound-method.md)|Obsoleto no .NET Framework 2,0.|  
|[Método ExceptionOSHandlerEnter](icorprofilercallback-exceptionoshandlerenter-method.md)|Não implementado. Um criador de perfil que precisa de informações de exceção não gerenciadas deve obter essas informações por meio de outros meios.|  
|[Método ExceptionOSHandlerLeave](icorprofilercallback-exceptionoshandlerleave-method.md)|Não implementado. Um criador de perfil que precisa de informações de exceção não gerenciadas deve obter essas informações por meio de outros meios.|  
|[Método ExceptionSearchCatcherFound](icorprofilercallback-exceptionsearchcatcherfound-method.md)|Notifica o criador de perfil de que a fase de pesquisa do tratamento de exceções localizou um manipulador para a exceção que foi lançada.|  
|[Método ExceptionSearchFilterEnter](icorprofilercallback-exceptionsearchfilterenter-method.md)|Notifica o criador de perfil de que um filtro de usuário está sendo executado.|  
|[Método ExceptionSearchFilterLeave](icorprofilercallback-exceptionsearchfilterleave-method.md)|Notifica o criador de perfil de que um filtro de usuário acabou de executar a execução.|  
|[Método ExceptionSearchFunctionEnter](icorprofilercallback-exceptionsearchfunctionenter-method.md)|Notifica o criador de perfil de que a fase de pesquisa do tratamento de exceções inseriu uma função.|  
|[Método ExceptionSearchFunctionLeave](icorprofilercallback-exceptionsearchfunctionleave-method.md)|Notifica o criador de perfil de que a fase de pesquisa do tratamento de exceções concluiu a pesquisa de uma função.|  
|[Método ExceptionThrown](icorprofilercallback-exceptionthrown-method.md)|Notifica o criador de perfil de que uma exceção foi lançada.|  
|[Método ExceptionUnwindFinallyEnter](icorprofilercallback-exceptionunwindfinallyenter-method.md)|Notifica o criador de perfil de que a fase de desenrolamento da manipulação de exceção está inserindo uma `finally` cláusula contida na função especificada.|  
|[Método ExceptionUnwindFinallyLeave](icorprofilercallback-exceptionunwindfinallyleave-method.md)|Notifica o criador de perfil de que a fase de desenrolamento da manipulação de exceção saiu de uma `finally` cláusula.|  
|[Método ExceptionUnwindFunctionEnter](icorprofilercallback-exceptionunwindfunctionenter-method.md)|Notifica o criador de perfil de que a fase de desenrolar da manipulação de exceções inseriu uma função.|  
|[Método ExceptionUnwindFunctionLeave](icorprofilercallback-exceptionunwindfunctionleave-method.md)|Notifica o criador de perfil de que a fase de desenrolamento da manipulação de exceções concluiu o desenrolamento de uma função.|  
|[Método FunctionUnloadStarted](icorprofilercallback-functionunloadstarted-method.md)|Notifica o criador de perfil de que o tempo de execução começou a descarregar uma função.|  
|[Método Initialize](icorprofilercallback-initialize-method.md)|Chamado para inicializar o criador de perfil sempre que um novo aplicativo CLR é iniciado.|  
|[Método JITCachedFunctionSearchFinished](icorprofilercallback-jitcachedfunctionsearchfinished-method.md)|Notifica o criador de perfil de que uma pesquisa foi concluída para uma função que foi compilada anteriormente usando NGen. exe.|  
|[Método JITCachedFunctionSearchStarted](icorprofilercallback-jitcachedfunctionsearchstarted-method.md)|Notifica o criador de perfil de que uma pesquisa foi iniciada para uma função que foi compilada anteriormente usando NGen. exe.|  
|[Método JITCompilationFinished](icorprofilercallback-jitcompilationfinished-method.md)|Notifica o criador de perfil de que o compilador JIT concluiu a compilação de uma função.|  
|[Método JITCompilationStarted](icorprofilercallback-jitcompilationstarted-method.md)|Notifica o criador de perfil de que o compilador JIT (just-in-time) começou a compilar uma função.|  
|[Método JITFunctionPitched](icorprofilercallback-jitfunctionpitched-method.md)|Notifica o criador de perfil de que uma função que foi compilada por JIT foi removida da memória.|  
|[Método JITInlining](icorprofilercallback-jitinlining-method.md)|Notifica o criador de perfil de que o compilador JIT está prestes a inserir uma função em linha com outra função.|  
|[Método ManagedToUnmanagedTransition](icorprofilercallback-managedtounmanagedtransition-method.md)|Notifica o criador de perfil de que uma transição do código gerenciado para o código não gerenciado ocorreu.|  
|[Método ModuleAttachedToAssembly](icorprofilercallback-moduleattachedtoassembly-method.md)|Notifica o criador de perfil de que um módulo está sendo anexado a seu assembly pai.|  
|[Método ModuleLoadFinished](icorprofilercallback-moduleloadfinished-method.md)|Notifica o criador de perfil que concluiu o carregamento de um módulo.|  
|[Método ModuleLoadStarted](icorprofilercallback-moduleloadstarted-method.md)|Notifica o criador de perfil de que um módulo está sendo carregado.|  
|[Método ModuleUnloadFinished](icorprofilercallback-moduleunloadfinished-method.md)|Notifica o criador de perfil de que um módulo concluiu o descarregamento.|  
|[Método ModuleUnloadStarted](icorprofilercallback-moduleunloadstarted-method.md)|Notifica o criador de perfil de que um módulo está sendo descarregado.|  
|[Método MovedReferences](icorprofilercallback-movedreferences-method.md)|Notifica o criador de perfil sobre referências de objeto que foram movidas durante a coleta de lixo.|  
|[Método ObjectAllocated](icorprofilercallback-objectallocated-method.md)|Notifica o criador de perfil de que a memória dentro do heap foi alocada para um objeto.|  
|[Método ObjectReferences](icorprofilercallback-objectreferences-method.md)|Notifica o criador de perfil sobre objetos na memória referenciados pelo objeto especificado.|  
|[Método ObjectsAllocatedByClass](icorprofilercallback-objectsallocatedbyclass-method.md)|Notifica o criador de perfil sobre o número de instâncias de cada classe especificada que foram criadas desde a coleta de lixo anterior.|  
|[Método RemotingClientInvocationFinished](icorprofilercallback-remotingclientinvocationfinished-method.md)|Notifica o criador de perfil de que uma chamada de comunicação remota foi executada até a conclusão no cliente.|  
|[Método RemotingClientInvocationStarted](icorprofilercallback-remotingclientinvocationstarted-method.md)|Notifica o criador de perfil de que uma chamada de comunicação remota foi iniciada.|  
|[Método RemotingClientReceivingReply](icorprofilercallback-remotingclientreceivingreply-method.md)|Notifica o criador de perfil de que a parte do lado do servidor de uma chamada de comunicação remota foi concluída e que o cliente agora está recebendo e prestes a processar a resposta.|  
|[Método RemotingClientSendingMessage](icorprofilercallback-remotingclientsendingmessage-method.md)|Notifica o criador de perfil de que o cliente está enviando uma solicitação para o servidor.|  
|[Método RemotingServerInvocationReturned](icorprofilercallback-remotingserverinvocationreturned-method.md)|Notifica o criador de perfil de que o processo concluiu a invocação de um método em resposta a uma solicitação de invocação de método remoto.|  
|[Método RemotingServerInvocationStarted](icorprofilercallback-remotingserverinvocationstarted-method.md)|Notifica o criador de perfil de que o processo está invocando um método em resposta a uma solicitação de invocação de método remoto.|  
|[Método RemotingServerReceivingMessage](icorprofilercallback-remotingserverreceivingmessage-method.md)|Notifica o criador de perfil de que o processo está recebendo uma invocação de método remoto ou uma solicitação de ativação.|  
|[Método RemotingServerSendingReply](icorprofilercallback-remotingserversendingreply-method.md)|Notifica o criador de perfil de que o processo concluiu o processamento de uma solicitação de invocação de método remoto e está prestes a transmitir a resposta por meio de um canal.|  
|[Método RootReferences](icorprofilercallback-rootreferences-method.md)|Notifica o criador de perfil com informações sobre referências raiz após a coleta de lixo.|  
|[Método RuntimeResumeFinished](icorprofilercallback-runtimeresumefinished-method.md)|Notifica o criador de perfil de que o tempo de execução retomou todos os threads de tempo de execução e retornou à operação normal.|  
|[Método RuntimeResumeStarted](icorprofilercallback-runtimeresumestarted-method.md)|Notifica o criador de perfil de que o tempo de execução está retomando todos os threads em tempo de execução.|  
|[Método RuntimeSuspendAborted](icorprofilercallback-runtimesuspendaborted-method.md)|Notifica o criador de perfil de que o tempo de execução anulou a suspensão de runtime que estava ocorrendo.|  
|[Método RuntimeSuspendFinished](icorprofilercallback-runtimesuspendfinished-method.md)|Notifica o criador de perfil de que o tempo de execução concluiu a suspensão de todos os threads em tempo de execução.|  
|[Método RuntimeSuspendStarted](icorprofilercallback-runtimesuspendstarted-method.md)|Notifica o criador de perfil de que o tempo de execução está prestes a suspender todos os threads em tempo de execução.|  
|[Método RuntimeThreadResumed](icorprofilercallback-runtimethreadresumed-method.md)|Notifica o criador de perfil de que o thread especificado foi retomado após ser suspenso.|  
|[Método RuntimeThreadSuspended](icorprofilercallback-runtimethreadsuspended-method.md)|Notifica o criador de perfil de que o thread especificado foi ou está prestes a ser suspenso.|  
|[Método Shutdown](icorprofilercallback-shutdown-method.md)|Notifica o criador de perfil de que o aplicativo está sendo desligado.|  
|[Método ThreadAssignedToOSThread](icorprofilercallback-threadassignedtoosthread-method.md)|Notifica o criador de perfil de que um thread gerenciado está sendo implementado usando um determinado thread de sistema operacional (SO).|  
|[Método ThreadCreated](icorprofilercallback-threadcreated-method.md)|Notifica o criador de perfil de que um thread foi criado.|  
|[Método ThreadDestroyed](icorprofilercallback-threaddestroyed-method.md)|Notifica o criador de perfil de que um thread foi destruído.|  
|[Método UnmanagedToManagedTransition](icorprofilercallback-unmanagedtomanagedtransition-method.md)|Notifica o criador de perfil de que uma transição de código não gerenciado para código gerenciado ocorreu.|  
  
## <a name="remarks"></a>Comentários  
 O CLR chama um método na `ICorProfilerCallback` interface (ou [ICorProfilerCallback2](icorprofilercallback2-interface.md)) para notificar o criador de perfil quando um evento, ao qual o criador de perfil assinou, ocorre. Essa é a interface de retorno de chamada principal por meio da qual o CLR se comunica com o criador de perfil de código.  
  
 Um criador de perfil de código deve implementar os métodos da `ICorProfilerCallback` interface. Para o .NET Framework versão 2,0 ou posterior, o criador de perfil também deve implementar os `ICorProfilerCallback2` métodos. Cada implementação de método deve retornar um HRESULT que tenha um valor de S_OK em caso de êxito ou E_FAIL em caso de falha. Atualmente, o CLR ignora o HRESULT que é retornado por cada retorno de chamada, exceto [ICorProfilerCallback:: Objectreferenciations](icorprofilercallback-objectreferences-method.md).  
  
 No registro do Microsoft Windows, um criador de perfil de código deve registrar seu objeto de Component Object Model (COM) que implementa as `ICorProfilerCallback` `ICorProfilerCallback2` interfaces e. Um criador de perfil de código assina os eventos para os quais deseja receber notificação chamando [ICorProfilerInfo:: SetEventMask](icorprofilerinfo-seteventmask-method.md). Isso geralmente é feito na implementação do criador de perfil de [ICorProfilerCallback:: Initialize](icorprofilercallback-initialize-method.md). Em seguida, o criador de perfil é capaz de receber a notificação do tempo de execução quando um evento está prestes a ocorrer ou ocorreu apenas em um processo de tempo de execução.  
  
> [!NOTE]
> O criador de perfil registra um único objeto COM. Se o criador de perfil estiver direcionando para o .NET Framework versão 1,0 ou 1,1, esse objeto COM precisará implementar apenas os métodos de `ICorProfilerCallback` . Se ele estiver direcionando .NET Framework versão 2,0 ou posterior, o objeto COM também deverá implementar os métodos de `ICorProfilerCallback2` .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Criação de perfil de interfaces](profiling-interfaces.md)
- [Interface ICorProfilerCallback2](icorprofilercallback2-interface.md)
- [Interface ICorProfilerCallback3](icorprofilercallback3-interface.md)
- [Interface ICorProfilerCallback4](icorprofilercallback4-interface.md)
