---
title: Estrutura COR_PRF_FUNCTION_ARGUMENT_RANGE
ms.date: 03/30/2017
api_name:
- COR_PRF_FUNCTION_ARGUMENT_RANGE
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- COR_PRF_FUNCTION_ARGUMENT_RANGE
helpviewer_keywords:
- COR_PRF_FUNCTION_ARGUMENT_RANGE structure [.NET Framework profiling'
ms.assetid: 9f469eac-ac66-419b-8668-fe705bc1a51f
topic_type:
- apiref
ms.openlocfilehash: 8b3785955ec138bbf898e84aa4deb5ed2a6e6b53
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500943"
---
# <a name="cor_prf_function_argument_range-structure"></a>Estrutura COR_PRF_FUNCTION_ARGUMENT_RANGE
Representa um bloco de argumentos de função armazenados de forma contígua em ordem da esquerda para a direita na memória.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef struct _COR_PRF_FUNCTION_ARGUMENT_RANGE {  
    UINT_PTR startAddress;  
    ULONG length;  
} COR_PRF_FUNCTION_ARGUMENT_RANGE;  
```  
  
## <a name="members"></a>Membros  
  
|Membros|Descrição|  
|-------------|-----------------|  
|`startAddress`|O endereço inicial do bloco.|  
|`length`|O comprimento do bloco contíguo.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Estruturas de criação de perfil](profiling-structures.md)
