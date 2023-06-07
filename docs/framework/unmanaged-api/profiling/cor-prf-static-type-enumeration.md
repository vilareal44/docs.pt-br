---
title: Enumeração COR_PRF_STATIC_TYPE
ms.date: 03/30/2017
api_name:
- COR_PRF_STATIC_TYPE
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- COR_PRF_STATIC_TYPE
helpviewer_keywords:
- COR_PRF_STATIC_TYPE enumeration [.NET Framework profiling]
ms.assetid: 441d7809-5b65-41a5-ba64-2910a8008315
topic_type:
- apiref
ms.openlocfilehash: 80d72aefc736054afcee152c55e941c0f8f3c6a8
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500761"
---
# <a name="cor_prf_static_type-enumeration"></a>Enumeração COR_PRF_STATIC_TYPE
Indica se um campo é estático e, em caso positivo, a qualidade estática aplicada ao campo. Esses valores podem ser combinados usando-se a operação OR de bits para indicar que o campo tem várias qualidades estáticas diferentes.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum {  
    COR_PRF_FIELD_NOT_A_STATIC = 0x0,  
    COR_PRF_FIELD_APP_DOMAIN_STATIC = 0x1,  
    COR_PRF_FIELD_THREAD_STATIC = 0x2,  
    COR_PRF_FIELD_CONTEXT_STATIC = 0x4,  
    COR_PRF_FIELD_RVA_STATIC = 0x8  
} COR_PRF_STATIC_TYPE;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`COR_PRF_FIELD_NOT_A_STATIC`|O campo não é estático.|  
|`COR_PRF_FIELD_APP_DOMAIN_STATIC`|O campo é um domínio de aplicativo estático.|  
|`COR_PRF_FIELD_THREAD_STATIC`|O campo é thread-estático.|  
|`COR_PRF_FIELD_CONTEXT_STATIC`|O campo é de contexto estático.|  
|`COR_PRF_FIELD_RVA_STATIC`|O campo é um endereço virtual relativo (RVA)-estático.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Criando perfil de enumerações](profiling-enumerations.md)
