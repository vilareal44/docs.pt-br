---
title: Estrutura COR_PRF_GC_GENERATION_RANGE
ms.date: 03/30/2017
api_name:
- COR_PRF_GC_GENERATION_RANGE
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- COR_PRF_GC_GENERATION_RANGE
helpviewer_keywords:
- COR_PRF_GC_GENERATION_RANGE structure [.NET Framework profiling]
ms.assetid: e7e07273-8d10-4a68-807e-59634e3f8c5e
topic_type:
- apiref
ms.openlocfilehash: 91fb902aab678e29c6e74380e3576fa5c4ae62d4
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500881"
---
# <a name="cor_prf_gc_generation_range-structure"></a>Estrutura COR_PRF_GC_GENERATION_RANGE
Descreve um intervalo (ou seja, um bloco) de memória que está passando por coleta de lixo.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef struct COR_PRF_GC_GENERATION_RANGE {  
    COR_PRF_GC_GENERATION generation;  
    ObjectID rangeStart;  
    UINT_PTR rangeLength;  
    UINT_PTR rangeLengthReserved;  
} COR_PRF_GC_GENERATION_RANGE;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`generation`|Um valor da enumeração [COR_PRF_GC_GENERATION](cor-prf-gc-generation-enumeration.md) que especifica a geração à qual o bloco de memória pertence.|  
|`rangeStart`|A ID de um objeto que especifica o local inicial do bloco de memória.|  
|`rangeLength`|Um ponteiro para um inteiro que especifica o tamanho da parte usada do bloco de memória (ou seja, a quantidade de memória usada dentro do bloco).|  
|`rangeLengthReserved`|Um ponteiro para um inteiro que especifica o tamanho do bloco de memória (ou seja, a quantidade de memória reservada para o bloco).|  
  
## <a name="remarks"></a>Comentários  
 `rangeLength`É garantido que o valor seja preciso apenas se [ICorProfilerInfo2:: GetGenerationBounds](icorprofilerinfo2-getgenerationbounds-method.md) ou [ICorProfilerInfo2:: GetObjectGeneration](icorprofilerinfo2-getobjectgeneration-method.md), ambos usam a `COR_PRF_GC_GENERATION_RANGE` estrutura, é chamado do método [ICorProfilerCallback2:: GarbageCollectionStarted](icorprofilercallback2-garbagecollectionstarted-method.md) ou [ICorProfilerCallback2:: GarbageCollectionFinished](icorprofilercallback2-garbagecollectionfinished-method.md) .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Estruturas de criação de perfil](profiling-structures.md)
