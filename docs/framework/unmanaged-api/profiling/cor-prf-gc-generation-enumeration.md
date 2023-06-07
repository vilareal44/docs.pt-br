---
title: Enumeração COR_PRF_GC_GENERATION
ms.date: 03/30/2017
api_name:
- COR_PRF_GC_GENERATION
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- COR_PRF_GC_GENERATION
helpviewer_keywords:
- COR_GC_GENERATION_FLAGS enumeration [.NET Framework profiling]
ms.assetid: d6ece160-26ad-4d39-abd7-05acd6f78c48
topic_type:
- apiref
ms.openlocfilehash: b7a068efcf20b2028e9c193567d15b59e582febf
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500917"
---
# <a name="cor_prf_gc_generation-enumeration"></a>Enumeração COR_PRF_GC_GENERATION
Identifica uma geração de coleta de lixo.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum {  
    COR_PRF_GC_GEN_0 = 0,  
    COR_PRF_GC_GEN_1 = 1,  
    COR_PRF_GC_GEN_2 = 2,  
    COR_PRF_GC_LARGE_OBJECT_HEAP = 3,
    COR_PRF_GC_PINNED_OBJECT_HEAP= 4
} COR_PRF_GC_GENERATION;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`COR_PRF_GC_GEN_0`|O objeto é armazenado como geração 0.|  
|`COR_PRF_GC_GEN_1`|O objeto é armazenado como geração 1.|  
|`COR_PRF_GC_GEN_2`|O objeto é armazenado como geração 2.|  
|`COR_PRF_GC_LARGE_OBJECT_HEAP`|O objeto é armazenado no heap de objeto grande.|  
|`COR_PRF_GC_PINNED_OBJECT_HEAP`|O objeto é armazenado no heap fixado-Object.|  
  
## <a name="remarks"></a>Comentários  
 O coletor de lixo melhora o desempenho de gerenciamento de memória dividindo objetos em gerações com base na idade. O coletor de lixo atualmente usa três gerações, numeradas de 0, 1 e 2 e dois segmentos de heap especiais, um para objetos grandes e outro para objetos fixados.
  
 Objetos cujo tamanho é maior do que um valor de limite são armazenados no heap de objeto grande. Os objetos fixados podem ser alocados para o heap de objeto fixado para evitar o custo de desempenho de alocá-los nos heaps normais. Outros objetos alocados começam a pertencer à geração 0. Todos os objetos que existem após a coleta de lixo ocorre na geração 0 são promovidos para a geração 1. Os objetos que existem após a coleta de lixo ocorrem na passagem de geração 1 para a geração 2.  
  
 O uso de gerações significa que o coletor de lixo tem que trabalhar apenas com um subconjunto dos objetos alocados a qualquer momento.  
  
 A `COR_PRF_GC_GENERATION` enumeração é usada pela estrutura de [COR_PRF_GC_GENERATION_RANGE](cor-prf-gc-generation-range-structure.md) .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Criando perfil de enumerações](profiling-enumerations.md)
