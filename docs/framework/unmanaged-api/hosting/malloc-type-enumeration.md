---
title: Enumeração MALLOC_TYPE
ms.date: 03/30/2017
api_name:
- MALLOC_TYPE
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- MALLOC_TYPE
helpviewer_keywords:
- MALLOC_TYPE Enumeration
ms.assetid: c02476f9-23a2-4af7-9282-aa9c42c7429b
topic_type:
- apiref
ms.openlocfilehash: 630fe4e79b369bfdefc19be72780f1893090895e
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84008450"
---
# <a name="malloc_type-enumeration"></a>Enumeração MALLOC_TYPE
Contém valores que especificam as características da memória que está sendo alocada.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum {  
    MALLOC_THREADSAFE = 0x1,  
    MALLOC_EXECUTABLE = 0x2,  
} MALLOC_TYPE;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`MALLOC_EXECUTABLE`|A memória alocada pode conter um arquivo executável.|  
|`MALLOC_THREADSAFE`|A memória alocada é thread-safe. Ou seja, a memória pode ser acessada por vários threads sem nenhuma sincronização.<br /><br /> Se esse sinalizador não for definido, as chamadas no objeto deverão ser serializadas.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Hospedando enumerações](hosting-enumerations.md)
