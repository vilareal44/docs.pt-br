---
title: Interface ICorProfilerModuleEnum
ms.date: 03/30/2017
api_name:
- ICorProfilerModuleEnum
api_location:
- mscorwks.cll
api_type:
- COM
f1_keywords:
- ICorProfilerModuleEnum
helpviewer_keywords:
- ICorProfilerModuleEnum interface [.NET Framework profiling]
ms.assetid: 24d0fcfa-1601-4fba-868f-da8c97303467
topic_type:
- apiref
ms.openlocfilehash: fe11c0bbe273ae07cdae43f681a558e07a291ffb
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84494885"
---
# <a name="icorprofilermoduleenum-interface"></a>Interface ICorProfilerModuleEnum
Fornece métodos para iterar de forma sequencial por meio de uma coleção de módulos carregados pelo aplicativo ou pelo criador de perfis.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método Clone](icorprofilermoduleenum-clone-method.md)|Obtém um ponteiro de interface para uma cópia desta `ICorProfilerModuleEnum` interface.|  
|[Método GetCount](icorprofilermoduleenum-getcount-method.md)|Obtém o número de módulos gerenciados que foram carregados no aplicativo.|  
|[Método Next](icorprofilermoduleenum-next-method.md)|Obtém o número especificado de módulos contíguos de uma coleção sequencial de objetos, começando na posição atual do enumerador na sequência.|  
|[Método Reset](icorprofilermoduleenum-reset-method.md)|Move o cursor do enumerador para a posição inicial da sequência.|  
|[Método Skip](icorprofilermoduleenum-skip-method.md)|Avança a posição do cursor do enumerador para que o número especificado de elementos seja ignorado.|  
  
## <a name="remarks"></a>Comentários  
 A `ICorProfilerModuleEnum` interface é um enumerador. Ele permite que o destinatário de uma matriz Extraia elementos do remetente a uma taxa apropriada para o destinatário. Em outras palavras, o receptor é capaz de controlar explicitamente o fluxo de elementos de matriz, evitando, assim, os problemas associados à passagem de matrizes grandes como parâmetros de método.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)
- [Criação de perfil de interfaces](profiling-interfaces.md)
- [Método EnumModules](icorprofilerinfo3-enummodules-method.md)
