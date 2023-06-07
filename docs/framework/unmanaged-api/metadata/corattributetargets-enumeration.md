---
title: Enumeração CorAttributeTargets
ms.date: 03/30/2017
api_name:
- CorAttributeTargets
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorAttributeTargets
helpviewer_keywords:
- CorAttributeTargets enumeration [.NET Framework metadata]
ms.assetid: 694c0fa0-7011-41a9-9dfd-f0e16ea574b5
topic_type:
- apiref
ms.openlocfilehash: f1836f26af99f91ab1765107573f6b067edd5e95
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84007917"
---
# <a name="corattributetargets-enumeration"></a>Enumeração CorAttributeTargets
Especifica os elementos do aplicativo no qual ele é válido para aplicar um atributo.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum CorAttributeTargets  
{  
    catAssembly            = 0x0001,  
    catModule              = 0x0002,  
    catClass               = 0x0004,  
    catStruct              = 0x0008,  
    catEnum                = 0x0010,  
    catConstructor         = 0x0020,  
    catMethod              = 0x0040,  
    catProperty            = 0x0080,  
    catField               = 0x0100,  
    catEvent               = 0x0200,  
    catInterface           = 0x0400,  
    catParameter           = 0x0800,  
    catDelegate            = 0x1000,  
    catGenericParameter    = 0x4000,  
  
    catAll                 =
        catAssembly | catModule | catClass | catStruct |
        catEnum | catConstructor | catMethod | catProperty |
        catField | catEvent | catInterface | catParameter |
        catDelegate | catGenericParameter,  
  
    catClassMembers        =
        catClass | catStruct | catEnum | catConstructor |
        catMethod | catProperty | catField | catEvent |
        catDelegate | catInterface  
  
} CorAttributeTargets;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`catAssembly`|O atributo pode ser aplicado a um assembly.|  
|`catModule`|O atributo pode ser aplicado a um módulo executável portátil (. dll ou. exe).|  
|`catClass`|O atributo pode ser aplicado a uma classe.|  
|`catStruct`|Atributo pode ser aplicado a uma estrutura; ou seja, um tipo de valor.|  
|`catEnum`|O atributo pode ser aplicado a uma enumeração.|  
|`catConstructor`|O atributo pode ser aplicado a um construtor.|  
|`catMethod`|O atributo pode ser aplicado a um método.|  
|`catProperty`|O atributo pode ser aplicado a uma propriedade.|  
|`catField`|O atributo pode ser aplicado a um campo.|  
|`catEvent`|O atributo pode ser aplicado a um evento.|  
|`catInterface`|O atributo pode ser aplicado a uma interface.|  
|`catParameter`|O atributo pode ser aplicado a um parâmetro.|  
|`catDelegate`|O atributo pode ser aplicado a um delegado.|  
|`catGenericParameter`|O atributo pode ser aplicado a um parâmetro genérico.|  
|`catAll`|O atributo pode ser aplicado a qualquer elemento de aplicativo.|  
|`catClassMembers`|O atributo pode ser aplicado a um membro de uma classe.|  
  
## <a name="remarks"></a>Comentários  
 Os `CorAttributeTargets` valores de enumeração podem ser combinados com uma operação OR bit a bit para obter a combinação preferida.  
  
 O `CorAttributeTargets` paraleliza a enumeração gerenciada <xref:System.AttributeTargets?displayProperty=nameWithType> .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorHdr. h  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Enumerações de metadados](metadata-enumerations.md)
