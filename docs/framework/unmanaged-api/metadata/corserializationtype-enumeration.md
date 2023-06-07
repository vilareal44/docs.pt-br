---
title: Enumeração CorSerializationType
ms.date: 03/30/2017
api_name:
- CorSerializationType
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorSerializationType
helpviewer_keywords:
- CorSerializationType enumeration [.NET Framework metadata]
ms.assetid: 6b1fcd11-c7fb-4be2-8910-abc862d4caf4
topic_type:
- apiref
ms.openlocfilehash: 649a9159f99afa64615c40c23a98a80318ae0d7f
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84009165"
---
# <a name="corserializationtype-enumeration"></a>Enumeração CorSerializationType
Especifica como um objeto é serializado pelo Common Language Runtime.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum CorSerializationType {  
  
    SERIALIZATION_TYPE_UNDEFINED     = 0,  
    SERIALIZATION_TYPE_BOOLEAN       = ELEMENT_TYPE_BOOLEAN,  
    SERIALIZATION_TYPE_CHAR          = ELEMENT_TYPE_CHAR,  
    SERIALIZATION_TYPE_I1            = ELEMENT_TYPE_I1,  
    SERIALIZATION_TYPE_U1            = ELEMENT_TYPE_U1,  
    SERIALIZATION_TYPE_I2            = ELEMENT_TYPE_I2,  
    SERIALIZATION_TYPE_U2            = ELEMENT_TYPE_U2,  
    SERIALIZATION_TYPE_I4            = ELEMENT_TYPE_I4,  
    SERIALIZATION_TYPE_U4            = ELEMENT_TYPE_U4,  
    SERIALIZATION_TYPE_I8            = ELEMENT_TYPE_I8,  
    SERIALIZATION_TYPE_U8            = ELEMENT_TYPE_U8,  
    SERIALIZATION_TYPE_R4            = ELEMENT_TYPE_R4,  
    SERIALIZATION_TYPE_R8            = ELEMENT_TYPE_R8,  
    SERIALIZATION_TYPE_STRING        = ELEMENT_TYPE_STRING,  
    SERIALIZATION_TYPE_SZARRAY       = ELEMENT_TYPE_SZARRAY,  
    SERIALIZATION_TYPE_TYPE          = 0x50,  
    SERIALIZATION_TYPE_TAGGED_OBJECT = 0x51,  
    SERIALIZATION_TYPE_FIELD         = 0x53,  
    SERIALIZATION_TYPE_PROPERTY      = 0x54,  
    SERIALIZATION_TYPE_ENUM          = 0x55  
  
} CorSerializationType;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`SERIALIZATION_TYPE_UNDEFINED`|A serialização do objeto está indefinida.|  
|`SERIALIZATION_TYPE_BOOLEAN`|O objeto é serializado como um tipo booliano|  
|`SERIALIZATION_TYPE_CHAR`|O objeto é serializado como um tipo de caractere.|  
|`SERIALIZATION_TYPE_I1`|O objeto é serializado como um inteiro de 1 byte assinado.|  
|`SERIALIZATION_TYPE_U1`|O objeto é serializado como um inteiro de 1 byte não assinado.|  
|`SERIALIZATION_TYPE_I2`|O objeto é serializado como um inteiro de 2 bytes assinado.|  
|`SERIALIZATION_TYPE_U2`|O objeto é serializado como um inteiro de 2 bytes não assinado.|  
|`SERIALIZATION_TYPE_I4`|O objeto é serializado como um inteiro de 4 bytes assinado.|  
|`SERIALIZATION_TYPE_U4`|O objeto é serializado como um inteiro de 4 bytes não assinado.|  
|`SERIALIZATION_TYPE_I8`|O objeto é serializado como um inteiro de 8 bytes assinado.|  
|`SERIALIZATION_TYPE_U8`|O objeto é serializado como um inteiro de 8 bytes não assinado.|  
|`SERIALIZATION_TYPE_R4`|O objeto é serializado como um ponto flutuante de 4 bytes.|  
|`SERIALIZATION_TYPE_R8`|O objeto é serializado como um ponto flutuante de 8 bytes.|  
|`SERIALIZATION_TYPE_STRING`|O objeto é serializado como um tipo System. String.|  
|`SERIALIZATION_TYPE_SZARRAY`|O objeto é serializado como uma matriz unidimensional de limite inferior de zero.|  
|`SERIALIZATION_TYPE_TYPE`|O objeto é serializado como um tipo genérico.|  
|`SERIALIZATION_TYPE_TAGGED_OBJECT`|O objeto é serializado como um objeto marcado.|  
|`SERIALIZATION_TYPE_FIELD`|O objeto é serializado como um campo.|  
|`SERIALIZATION_TYPE_PROPERTY`|O objeto é serializado como uma propriedade.|  
|`SERIALIZATION_TYPE_ENUM`|O objeto é serializado como uma enumeração.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorHdr. h  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Enumerações de metadados](metadata-enumerations.md)
