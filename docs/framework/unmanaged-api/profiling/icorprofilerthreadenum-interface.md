---
title: Interface ICorProfilerThreadEnum
ms.date: 03/30/2017
api_name:
- ICorProfilerThreadEnum
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerThreadEnum
helpviewer_keywords:
- ICorProfilerThreadEnum interface [.NET Framework profiling]
ms.assetid: 1e35031b-e095-4c14-9644-8deeb3081e0b
topic_type:
- apiref
ms.openlocfilehash: d28991254fba73de7a55135844d16417580d8792
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84494378"
---
# <a name="icorprofilerthreadenum-interface"></a>Interface ICorProfilerThreadEnum
Fornece métodos para iterar em sequência por meio de uma coleção de threads no Common Language Runtime.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método Clone](icorprofilerthreadenum-clone-method.md)|Obtém um ponteiro de interface para uma cópia desta `ICorProfilerThreadEnum` interface.|  
|[Método GetCount](icorprofilerthreadenum-getcount-method.md)|Obtém o número de threads que são usados pelo aplicativo.|  
|[Método Next](icorprofilerthreadenum-next-method.md)|Obtém o número especificado de threads contíguos de uma coleção sequencial de threads, começando na posição atual do enumerador na sequência.|  
|[Método Reset](icorprofilerthreadenum-reset-method.md)|Move o cursor do enumerador para a posição inicial da sequência.|  
|[Método Skip](icorprofilerthreadenum-skip-method.md)|Avança o cursor do enumerador de sua posição atual para ignorar o número especificado de elementos.|  
  
## <a name="remarks"></a>Comentários  
 A `ICorProfilerThreadEnum` interface é um enumerador. Ele permite que o destinatário de uma matriz Extraia elementos do remetente a uma taxa apropriada para o destinatário. Em outras palavras, o receptor é capaz de controlar explicitamente o fluxo de elementos de matriz, evitando, assim, os problemas associados à passagem de matrizes grandes como parâmetros de método.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)
- [Criação de perfil de interfaces](profiling-interfaces.md)
