---
title: Interface ICorProfilerFunctionEnum
ms.date: 03/30/2017
api_name:
- ICorProfilerFunctionEnum
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerFunctionEnum
helpviewer_keywords:
- ICorProfilerFunctionEnum interface [.NET Framework profiling]
ms.assetid: 0a1d4a38-cd0b-4231-91df-13646218ae72
topic_type:
- apiref
ms.openlocfilehash: b69afa7676ad174725f13c1113ff3bd9972995f8
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503075"
---
# <a name="icorprofilerfunctionenum-interface"></a>Interface ICorProfilerFunctionEnum
Fornece métodos para iterar em sequência por meio de uma coleção de funções no Common Language Runtime.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método Clone](icorprofilerfunctionenum-clone-method.md)|Obtém um ponteiro de interface para uma cópia desta `ICorProfilerFunctionEnum` interface.|  
|[Método GetCount](icorprofilerfunctionenum-getcount-method.md)|Obtém o número de funções que foram carregadas pelo aplicativo ou carregadas de modo forçado pelo criador de perfil.|  
|[Método Next](icorprofilerfunctionenum-next-method.md)|Obtém o número especificado de funções contíguas de uma coleção sequencial de funções, começando na posição atual do enumerador na sequência.|  
|[Método Reset](icorprofilerfunctionenum-reset-method.md)|Move o cursor do enumerador para a posição inicial da sequência.|  
|[Método Skip](icorprofilerfunctionenum-skip-method.md)|Avança o cursor do enumerador de sua posição atual para que o número especificado de elementos seja ignorado.|  
  
## <a name="remarks"></a>Comentários  
 A `ICorProfilerFunctionEnum` interface é um enumerador. Ele permite que o destinatário de uma matriz Extraia elementos do remetente a uma taxa apropriada para o destinatário. Em outras palavras, o receptor é capaz de controlar explicitamente o fluxo de elementos de matriz, evitando, assim, os problemas associados à passagem de matrizes grandes como parâmetros de método.  
  
 `ICorProfilerFunctionEnum`enumera em funções que já foram compiladas por JIT, mas não inclui funções que são carregadas de imagens nativas geradas com NGen. exe.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)
- [Criação de perfil de interfaces](profiling-interfaces.md)
- [Método EnumJITedFunctions](icorprofilerinfo3-enumjitedfunctions-method.md)
