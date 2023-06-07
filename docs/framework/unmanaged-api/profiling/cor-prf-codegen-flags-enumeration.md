---
title: Enumeração COR_PRF_CODEGEN_FLAGS
ms.date: 03/30/2017
api_name:
- COR_PRF_CODEGEN_FLAGS
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- COR_PRF_CODEGEN_FLAGS
helpviewer_keywords:
- COR_PRF_CODEGEN_FLAGS enumeration [.NET Framework profiling]
ms.assetid: 3e184022-0247-4824-a23d-6b29593d8d01
topic_type:
- apiref
ms.openlocfilehash: c2c7ae7a8930949c79b5e24e2da75f3b4649e7f6
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500982"
---
# <a name="cor_prf_codegen_flags-enumeration"></a>Enumeração COR_PRF_CODEGEN_FLAGS
Define os sinalizadores de geração de código que podem ser definidos com o método [ICorProfilerFunctionControl:: SetCodegenFlags](icorprofilerfunctioncontrol-setcodegenflags-method.md) .  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum {  
    COR_PRF_CODEGEN_DISABLE_INLINING =          0x0001,  
    COR_PRF_CODEGEN_DISABLE_ALL_OPTIMIZATIONS = 0x0002,  
} COR_PRF_CODEGEN_FLAGS;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`COR_PRF_CODEGEN_DISABLE_INLINING`|Nenhuma função será embutida no corpo de uma dessas funções. No entanto, a própria função pode ser embutida em seus chamadores.|  
|`COR_PRF_CODEGEN_DISABLE_ALL_OPTIMIZATIONS`|Todas as otimizações serão desabilitadas para o corpo da função. No entanto, a função em si ainda pode ser embutida em seus chamadores.|  
  
## <a name="remarks"></a>Comentários  
 A `COR_PRF_CODEGEN_FLAGS` enumeração é usada pelo método [ICorProfilerFunctionControl:: SetCodegenFlags](icorprofilerfunctioncontrol-setcodegenflags-method.md) para permitir que o criador de perfil controle a geração de código para a função JIT-recompilada.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Criando perfil de enumerações](profiling-enumerations.md)
