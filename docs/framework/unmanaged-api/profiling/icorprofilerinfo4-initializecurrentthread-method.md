---
title: Método ICorProfilerInfo4::InitializeCurrentThread
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo4::InitializeCurrentThread
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo4::InitializeCurrentThread
helpviewer_keywords:
- ICorProfilerInfo4::InitializeCurrentThread method [.NET Framework profiling]
- InitializeCurrentThread method, ICorProfilerInfo4 interface [.NET Framework profiling]
ms.assetid: 18a3335c-8c75-476c-b6de-72c0bfedae5d
topic_type:
- apiref
ms.openlocfilehash: 1f3ff3e9b68aa30f424f4b2fe6c7cacd2cddd544
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84495925"
---
# <a name="icorprofilerinfo4initializecurrentthread-method"></a>Método ICorProfilerInfo4::InitializeCurrentThread
Inicializa o thread atual antes das chamadas de API do criador de perfil subsequentes no mesmo thread, para que o deadlock possa ser evitado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT InitializeCurrentThread ();  
```  
  
## <a name="remarks"></a>Comentários  
 Recomendamos que você chame `InitializeCurrentThread` em qualquer thread que chamará uma API do profiler enquanto houver threads suspensos. Esse método é normalmente usado por infilers de amostragem que criam seu próprio thread para chamar o método [ICorProfilerInfo2::D ostacksnapshot](icorprofilerinfo2-dostacksnapshot-method.md) para executar movimentações de pilha enquanto o thread-alvo é suspenso. Chamando `InitializeCurrentThread` uma vez quando o criador de perfil cria primeiro o thread de amostragem, os profileres podem garantir a inicialização lenta por thread que o CLR executaria, de outra forma, durante a primeira chamada para `DoStackSnapshot` pode ocorrer com segurança quando nenhum outro thread for suspenso.  
  
> [!NOTE]
> `InitializeCurrentThread`faz a inicialização com antecedência para concluir as tarefas que usam bloqueios e pode travar. Chame `InitializeCurrentThread` somente quando não houver threads suspensos.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo4](icorprofilerinfo4-interface.md)
- [Criação de perfil de interfaces](profiling-interfaces.md)
- [Criação de perfil](index.md)
