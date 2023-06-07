---
title: Interface ICorProfilerCallback7
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback7
api_location:
- mscorwks.dll
- corprof.idl
api_type:
- COM
ms.assetid: a0be019e-aaa1-4036-990f-565f114d4b5c
ms.openlocfilehash: 9a49d8e1ff31942c6564ab560d6726b9ede26466
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84499136"
---
# <a name="icorprofilercallback7-interface"></a>Interface ICorProfilerCallback7
[Com suporte no .NET Framework 4.6.1 e versões posteriores]  
  
 Uma subclasse de [ICorProfilerCallback6](icorprofilercallback6-interface.md) que fornece um método de retorno de chamada que o Common Language Runtime usa para notificar o criador de perfil de que o fluxo de símbolos associado a um módulo na memória é atualizado.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método ModuleInMemorySymbolsUpdated](icorprofilercallback7-moduleinmemorysymbolsupdated-method.md)|Notifica o criador de perfil de que o fluxo de símbolos associado a um módulo na memória é atualizado.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v461plus](../../../../includes/net-current-v461plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Criação de perfil de interfaces](profiling-interfaces.md)
