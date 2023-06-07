---
title: Método ICorProfilerCallback::Shutdown
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.Shutdown
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::Shutdown
helpviewer_keywords:
- ICorProfilerCallback::Shutdown method [.NET Framework profiling]
- Shutdown method [.NET Framework profiling]
ms.assetid: 1ea194f0-a331-4855-a2ce-37393b8e5f84
topic_type:
- apiref
ms.openlocfilehash: f6873de1a864489d144a671b1a9e1349eaf77d15
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503170"
---
# <a name="icorprofilercallbackshutdown-method"></a>Método ICorProfilerCallback::Shutdown
Notifica o criador de perfil de que o aplicativo está sendo desligado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT Shutdown();  
```  
  
## <a name="remarks"></a>Comentários  
 O código do criador de perfil não pode chamar com segurança os métodos da interface [ICorProfilerInfo](icorprofilerinfo-interface.md) depois que o `Shutdown` método é chamado. Todas as chamadas para `ICorProfilerInfo` métodos resultam em um comportamento indefinido após o `Shutdown` método retornar. Determinados eventos imutáveis ainda podem ocorrer após o desligamento; o criador de perfil deve tomar cuidado para retornar imediatamente quando isso ocorrer.  
  
 O `Shutdown` método será chamado somente se o aplicativo gerenciado que está sendo criado para o perfil for iniciado como código gerenciado (ou seja, o quadro inicial na pilha de processo é gerenciado). Se o aplicativo foi iniciado como código não gerenciado, mas posteriormente entrou em código gerenciado, criando uma instância do Common Language Runtime (CLR) e, em seguida, `Shutdown` não será chamado. Nesses casos, o criador de perfil deve incluir em sua biblioteca uma `DllMain` rotina que usa o valor DLL_PROCESS_DETACH para liberar todos os recursos e executar o processamento de limpeza de seus dados, como a liberação de rastreamentos para o disco e assim por diante.  
  
 Em geral, o criador de perfil deve lidar com desligamentos inesperados. Por exemplo, um processo pode ser interrompido pelo método Win32's `TerminateProcess` (declarado em Winbase. h). Em outros casos, o CLR irá parar determinados threads gerenciados (threads em segundo plano) sem fornecer mensagens de destruição ordenada para eles.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback](icorprofilercallback-interface.md)
- [Método Initialize](icorprofilercallback-initialize-method.md)
