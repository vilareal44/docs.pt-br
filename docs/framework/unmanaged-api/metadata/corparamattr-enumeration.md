---
title: Enumeração CorParamAttr
ms.date: 03/30/2017
api_name:
- CorParamAttr
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorParamAttr
helpviewer_keywords:
- CorParamAttr enumeration [.NET Framework metadata]
ms.assetid: a7ff90ad-dad8-48e8-917d-4aa9a118cbc8
topic_type:
- apiref
ms.openlocfilehash: e8afcb972cab9757458c7032c3678d45c6418fac
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84007566"
---
# <a name="corparamattr-enumeration"></a>Enumeração CorParamAttr
Contém valores que descrevem os metadados de um parâmetro de método.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum CorParamAttr {  
  
    pdIn                        =   0x0001,  
    pdOut                       =   0x0002,  
    pdOptional                  =   0x0010,  
  
    pdReservedMask              =   0xf000,  
    pdHasDefault                =   0x1000,  
    pdHasFieldMarshal           =   0x2000,  
  
    pdUnused                    =   0xcfe0  
  
} CorParamAttr;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`pdIn`|Especifica que o parâmetro é passado para a chamada de método.|  
|`pdOut`|Especifica que o parâmetro é passado do retorno do método.|  
|`pdOptional`|Especifica que o parâmetro é opcional.|  
|`pdReservedMask`|Reservado para uso interno pelo Common Language Runtime.|  
|`pdHasDefault`|Especifica que o parâmetro tem um valor padrão.|  
|`pdHasFieldMarshal`|Especifica que o parâmetro tem informações de marshaling.|  
|`pdUnused`|Não utilizado.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorHdr. h  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Enumerações de metadados](metadata-enumerations.md)
