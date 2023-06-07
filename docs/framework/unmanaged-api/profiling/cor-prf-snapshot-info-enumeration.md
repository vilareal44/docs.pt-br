---
title: Enumeração COR_PRF_SNAPSHOT_INFO
ms.date: 03/30/2017
api_name:
- COR_PRF_SNAPSHOT_INFO
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- COR_PRF_SNAPSHOT_INFO
helpviewer_keywords:
- COR_PRF_SNAPSHOT_INFO enumeration [.NET Framework profiling]
ms.assetid: a5906b2a-ad4a-4cc6-a421-2d7d8adf7468
topic_type:
- apiref
ms.openlocfilehash: 6168c5b27868a261871b292e17ca02b04ae89917
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500774"
---
# <a name="cor_prf_snapshot_info-enumeration"></a>Enumeração COR_PRF_SNAPSHOT_INFO
Especifica a quantidade de dados a serem passados com um instantâneo de pilha em cada chamada para a função [StackSnapshotCallback](stacksnapshotcallback-function.md) do criador de perfil.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum _COR_PRF_SNAPSHOT_INFO {  
    COR_PRF_SNAPSHOT_DEFAULT = 0x0,  
    COR_PRF_SNAPSHOT_REGISTER_CONTEXT = 0x1,  
    COR_PRF_SNAPSHOT_X86_OPTIMIZED = 0X2  
} COR_PRF_SNAPSHOT_INFO;  
```  
  
## <a name="members"></a>Membros  
  
|Membros|Descrição|  
|-------------|-----------------|  
|`COR_PRF_SNAPSHOT_DEFAULT`|Indica que os valores devem ser passados para todos os `StackSnapshotCallback` parâmetros, exceto o `context` parâmetro.|  
|`COR_PRF_SNAPSHOT_REGISTER_CONTEXT`|Indica que os valores devem ser passados para todos os `StackSnapshotCallback` parâmetros, incluindo o `context` parâmetro.|  
|`COR_PRF_SNAPSHOT_X86_OPTIMIZED`|Indica que um algoritmo de movimentação de pilha mais simples e alternativo será usado.|  
  
## <a name="remarks"></a>Comentários  
 Os valores fornecidos pela `COR_PRF_SNAPSHOT_INFO` enumeração são passados como parâmetros para o método [DoStackSnapshot](icorprofilerinfo2-dostacksnapshot-method.md) .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Método DoStackSnapshot](icorprofilerinfo2-dostacksnapshot-method.md)
- [Criando perfil de enumerações](profiling-enumerations.md)
