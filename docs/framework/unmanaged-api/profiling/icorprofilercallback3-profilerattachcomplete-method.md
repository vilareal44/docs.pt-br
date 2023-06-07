---
title: Método ICorProfilerCallback3::ProfilerAttachComplete
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback3.ProfilerAttachComplete Method
api_location:
- Mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback3::ProfilerAttachComplete
helpviewer_keywords:
- ProfilerAttachComplete method [.NET Framework profiling]
- ICorProfilerCallback3::ProfilerAttachComplete method [.NET Framework profiling]
ms.assetid: 257d6076-06e0-4d93-bb33-651fbb2b92d7
topic_type:
- apiref
ms.openlocfilehash: bcc938ff9322fca4f45366fdc695e0c3901484b5
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84499656"
---
# <a name="icorprofilercallback3profilerattachcomplete-method"></a>Método ICorProfilerCallback3::ProfilerAttachComplete
Chamado pelo Common Language Runtime (CLR) para indicar que o criador de perfil agora pode chamar os métodos de atualização [ICorProfilerInfo3:: EnumJITedFunctions](icorprofilerinfo3-enumjitedfunctions-method.md) e [ICorProfilerInfo3:: EnumModules](icorprofilerinfo3-enummodules-method.md) .  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT ProfilerAttachComplete ();  
```  
  
## <a name="remarks"></a>Comentários  
 O `ProfilerAttachComplete` retorno de chamada é emitido depois que o método [ICorProfilerCallback3:: InitializeForAttach](icorprofilercallback3-initializeforattach-method.md) é chamado. Isso indica o seguinte:  
  
- Os retornos de chamada que foram solicitados pelo criador de perfil no `InitializeForAttach` foram ativados.  
  
- O criador de perfil agora pode realizar a atualização nas IDs associadas sem se preocupar com notificações ausentes.  
  
 O CLR ignora o valor de retorno deste retorno de chamada.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback](icorprofilercallback-interface.md)
- [Interface ICorProfilerInfo3](icorprofilerinfo3-interface.md)
- [Criação de perfil de interfaces](profiling-interfaces.md)
- [Criação de perfil](index.md)
