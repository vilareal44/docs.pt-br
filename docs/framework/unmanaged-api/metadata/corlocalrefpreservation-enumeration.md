---
title: Enumeração CorLocalRefPreservation
ms.date: 03/30/2017
api_name:
- CorLocalRefPreservation
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorLocalRefPreservation
helpviewer_keywords:
- CorLocalRefPreservation enumeration [.NET Framework metadata]
ms.assetid: 44757163-1228-4213-a4c4-d4de503cc75d
topic_type:
- apiref
ms.openlocfilehash: 42cb4e76bb77aebcee3b28035635a877513cdc04
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84008983"
---
# <a name="corlocalrefpreservation-enumeration"></a>Enumeração CorLocalRefPreservation
Contém valores de sinalizador para o tratamento de referências locais.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum CorLocalRefPreservation  
{  
    MDPreserveLocalRefsNone     =   0x00000000,  
    MDPreserveLocalTypeRef      =   0x00000001,  
    MDPreserveLocalMemberRef    =   0x00000002  
} CorLocalRefPreservation;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`MDPreserveLocalRefsNone`|Não preservar referências locais.|  
|`MDPreserveLocalTypeRef`|Preserve referências de tipo local.|  
|`MDPreserveLocalMemberRef`|Preserve referências de membro local.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorHdr. h  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Enumerações de metadados](metadata-enumerations.md)
