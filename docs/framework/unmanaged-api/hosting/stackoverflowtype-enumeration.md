---
title: Enumeração StackOverflowType
ms.date: 03/30/2017
api_name:
- StackOverflowType
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- StackOverflowType
helpviewer_keywords:
- StackOverflowType enumeration [.NET Framework hosting]
ms.assetid: dab648ad-972b-479c-b129-b4c1dcbd932e
topic_type:
- apiref
ms.openlocfilehash: f399d33dbe05cb5768aa45533ef30d28409e18e2
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84006449"
---
# <a name="stackoverflowtype-enumeration"></a>Enumeração StackOverflowType
Contém valores que indicam a causa subjacente de um evento de estouro de pilha.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum {  
    SO_Managed,  
    SO_ClrEngine,  
    SO_Other  
} StackOverflowType;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`SO_ClrEngine`|O estouro de pilha foi causado pelo mecanismo de execução.|  
|`SO_Managed`|O estouro de pilha foi causado por código gerenciado.|  
|`SO_Other`|O estouro de pilha foi causado por código não gerenciado.|  
  
## <a name="remarks"></a>Comentários  
 Essas informações são passadas para o host por meio de uma chamada para o método [IActionOnCLREvent:: OnEvent](iactiononclrevent-onevent-method.md) .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Hospedando enumerações](hosting-enumerations.md)
