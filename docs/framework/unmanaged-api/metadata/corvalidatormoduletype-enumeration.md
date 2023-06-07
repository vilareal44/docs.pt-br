---
title: Enumeração CorValidatorModuleType
ms.date: 03/30/2017
api_name:
- CorValidatorModuleType
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorValidatorModuleType
helpviewer_keywords:
- CorValidatorModuleType enumeration [.NET Framework metadata]
ms.assetid: 748f1ab2-fbcb-4f55-89ec-8d23d81ebc80
topic_type:
- apiref
ms.openlocfilehash: 038e2ec20e5fd01edf9835080e0f7a15ec862fd9
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84008931"
---
# <a name="corvalidatormoduletype-enumeration"></a>Enumeração CorValidatorModuleType
Especifica o tipo de um módulo.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum  
{  
    ValidatorModuleTypeInvalid  = 0x0,  
    ValidatorModuleTypeMin      = 0x00000001,  
    ValidatorModuleTypePE       = 0x00000001,  
    ValidatorModuleTypeObj      = 0x00000002,  
    ValidatorModuleTypeEnc      = 0x00000003,  
    ValidatorModuleTypeIncr     = 0x00000004,  
    ValidatorModuleTypeMax      = 0x00000004  
} CorValidatorModuleType;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`ValidatorModuleTypeInvalid`|O módulo é um tipo inválido.|  
|`ValidatorModuleTypeMin`|O valor mínimo da `CorValidatorModuleType` enumeração.|  
|`ValidatorModuleTypePE`|O módulo é um arquivo executável portátil (PE).|  
|`ValidatorModuleTypeObj`|O módulo é um arquivo. obj.|  
|`ValidatorModuleTypeEnc`|O módulo é uma sessão do depurador de editar e continuar.|  
|`ValidatorModuleTypeIncr`|O módulo é um que foi compilado incrementalmente.|  
|`ValidatorModuleTypeMax`|O valor máximo da `CorValidatorModuleType` enumeração.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Enumerações de metadados](metadata-enumerations.md)
