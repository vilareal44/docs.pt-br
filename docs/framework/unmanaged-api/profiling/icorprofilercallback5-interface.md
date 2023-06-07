---
title: Interface ICorProfilerCallback5
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback5
api_location:
- Mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback5
helpviewer_keywords:
- ICorProfilerCallback5 interface [.NET Framework profiling]
ms.assetid: 61d2e9ef-5f1f-4771-8847-239616e35d84
topic_type:
- apiref
ms.openlocfilehash: 02a690503d7b6323f19dcb66247d8a552b760b1c
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84499201"
---
# <a name="icorprofilercallback5-interface"></a>Interface ICorProfilerCallback5
Complementa as informações para ajudar um criador de perfil a identificar o fechamento completo de objetos dinâmicos, quando usado com o método [ICorProfilerCallback:: RootReferences](icorprofilercallback-rootreferences-method.md) ou [ICorProfilerCallback2:: RootReferences2](icorprofilercallback2-rootreferences2-method.md) junto com os métodos [ICorProfilerCallback:: objectreferenciations](icorprofilercallback-objectreferences-method.md) e [ConditionalWeakTableElementReferences](icorprofilercallback5-conditionalweaktableelementreferences-method.md) .  
  
 O `ICorProfilerCallback5` deve ser implementado por um criador de perfil de memória gerenciada para assinar as notificações relacionadas aos identificadores dependentes.  
  
## <a name="remarks"></a>Comentários  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método ConditionalWeakTableElementReferences](icorprofilercallback5-conditionalweaktableelementreferences-method.md)|Identifica o fechamento transitivo de objetos referenciados por estas raízes por meio de referências diretas do campo de membro e de dependências `ConditionalWeakTable`.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Criação de perfil de interfaces](profiling-interfaces.md)
- [Interface ICorProfilerCallback2](icorprofilercallback2-interface.md)
