---
title: Enumeração CorMethodImpl
ms.date: 03/30/2017
api_name:
- CorMethodImpl
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorMethodImpl
helpviewer_keywords:
- CorMethodImpl enumeration [.NET Framework metadata]
ms.assetid: ffbb3caf-20da-4a4b-8983-77376e72b990
topic_type:
- apiref
ms.openlocfilehash: b32e8f0b03ef6d550c384f3d932cc295a7270028
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84007657"
---
# <a name="cormethodimpl-enumeration"></a>Enumeração CorMethodImpl
Contém valores que descrevem os recursos de implementação do método.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum CorMethodImpl {  
  
    miCodeTypeMask      =   0x0003,  
    miIL                =   0x0000,  
    miNative            =   0x0001,  
    miOPTIL             =   0x0002,  
    miRuntime           =   0x0003,  
  
    miManagedMask       =   0x0004,  
    miUnmanaged         =   0x0004,  
    miManaged           =   0x0000,  
  
    miForwardRef        =   0x0010,  
    miPreserveSig       =   0x0080,  
  
    miInternalCall      =   0x1000,  
    miSynchronized      =   0x0020,  
    miNoInlining        =   0x0008,  
    miAggressiveInlining =  0x0100,  
    miNoOptimization     =  0x0040,  
    miMaxMethodImplVal  =   0xffff  
  
} CorMethodImpl;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`miCodeTypeMask`|Sinalizadores que descrevem o tipo de código.|  
|`miIL`|Especifica que a implementação do método é MSIL (Microsoft Intermediate Language).|  
|`miNative`|Especifica que a implementação do método é nativa.|  
|`miOPTIL`|Especifica que a implementação do método é OPTIl.|  
|`miRuntime`|Especifica que a implementação do método é fornecida pelo Common Language Runtime.|  
|`miManagedMask`|Sinalizadores que indicam se o código é gerenciado ou não gerenciado.|  
|`miUnmanaged`|Especifica que a implementação do método não é gerenciada.|  
|`miManaged`|Especifica que a implementação do método é gerenciada.|  
|`miForwardRef`|Especifica que o método está definido. Esse sinalizador é usado principalmente em cenários de mesclagem.|  
|`miPreserveSig`|Especifica que a assinatura do método não pode ser desconfigurado para uma conversão HRESULT.|  
|`miInternalCall`|Reservado para uso interno pelo Common Language Runtime.|  
|`miSynchronized`|Especifica que o método é de thread único por meio de seu corpo.|  
|`miNoInlining`|Especifica que o método não pode estar em linha.|  
|`miAggressiveInlining`|Especifica que o método deve ser embutido, se possível.|  
|`miNoOptimization`|Especifica que o método não deve ser otimizado.|  
|`miMaxMethodImplVal`|O valor máximo válido para um `CorMethodImpl` .|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorHdr. h  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Enumerações de metadados](metadata-enumerations.md)
