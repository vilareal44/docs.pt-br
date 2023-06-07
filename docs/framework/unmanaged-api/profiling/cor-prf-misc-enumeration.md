---
title: Enumeração COR_PRF_MISC
ms.date: 03/30/2017
api_name:
- COR_PRF_MISC
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- COR_PRF_MISC
helpviewer_keywords:
- COR_PRF_MISC enumeration [.NET Framework profiling]
ms.assetid: 619bb5de-e309-48b6-a3af-32d935a0ff46
topic_type:
- apiref
ms.openlocfilehash: 7b8f2845589a8372f62c95ef1a82eae3ed602c1f
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500826"
---
# <a name="cor_prf_misc-enumeration"></a>Enumeração COR_PRF_MISC
Contém valores constantes que especificam identificadores especiais.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum {  
    PROFILER_PARENT_UNKNOWN = 0xFFFFFFFD,  
    PROFILER_GLOBAL_CLASS   = 0xFFFFFFFE,  
    PROFILER_GLOBAL_MODULE  = 0xFFFFFFFF  
} COR_PRF_MISC;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`PROFILER_PARENT_UNKNOWN`|O identificador padrão usado por [ICorProfilerInfo:: GetModuleInfo](icorprofilerinfo-getmoduleinfo-method.md) para um módulo que ainda não foi anexado a um assembly.|  
|`PROFILER_GLOBAL_CLASS`|O identificador de classe padrão para constantes globais que não pertencem a uma classe.|  
|`PROFILER_GLOBAL_MODULE`|O identificador de módulo padrão para objetos globais que não pertencem a um módulo.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Criando perfil de enumerações](profiling-enumerations.md)
