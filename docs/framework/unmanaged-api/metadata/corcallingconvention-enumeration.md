---
title: Enumeração CorCallingConvention
ms.date: 03/30/2017
api_name:
- CorCallingConvention
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorCallingConvention
helpviewer_keywords:
- CorCallingConvention enumeration [.NET Framework metadata]
ms.assetid: 69156fbf-7219-43bf-b4b8-b13f1a2fcb86
topic_type:
- apiref
ms.openlocfilehash: 310319e8fefe80017c58706e2beaee5eb1e78422
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84007894"
---
# <a name="corcallingconvention-enumeration"></a>Enumeração CorCallingConvention
Contém valores que descrevem os tipos de convenções de chamada que são feitas em código gerenciado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum CorCallingConvention  
{  
    IMAGE_CEE_CS_CALLCONV_DEFAULT       = 0x0,  
  
    IMAGE_CEE_CS_CALLCONV_VARARG        = 0x5,  
    IMAGE_CEE_CS_CALLCONV_FIELD         = 0x6,  
    IMAGE_CEE_CS_CALLCONV_LOCAL_SIG     = 0x7,  
    IMAGE_CEE_CS_CALLCONV_PROPERTY      = 0x8,  
    IMAGE_CEE_CS_CALLCONV_UNMGD         = 0x9,  
    IMAGE_CEE_CS_CALLCONV_GENERICINST   = 0xa,  
    IMAGE_CEE_CS_CALLCONV_NATIVEVARARG  = 0xb,  
    IMAGE_CEE_CS_CALLCONV_MAX           = 0xc,  
  
    IMAGE_CEE_CS_CALLCONV_MASK          = 0x0f,  
    IMAGE_CEE_CS_CALLCONV_HASTHIS       = 0x20,  
    IMAGE_CEE_CS_CALLCONV_EXPLICITTHIS  = 0x40,  
    IMAGE_CEE_CS_CALLCONV_GENERIC       = 0x10  
  
} CorCallingConvention;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`IMAGE_CEE_CS_CALLCONV_DEFAULT`|Indica uma Convenção de chamada padrão.|  
|`IMAGE_CEE_CS_CALLCONV_VARARG`|Indica que o método usa um número variável de parâmetros.|  
|`IMAGE_CEE_CS_CALLCONV_FIELD`|Indica que a chamada é para um campo.|  
|`IMAGE_CEE_CS_CALLCONV_LOCAL_SIG`|Indica que a chamada é para um método local.|  
|`IMAGE_CEE_CS_CALLCONV_PROPERTY`|Indica que a chamada é para uma propriedade.|  
|`IMAGE_CEE_CS_CALLCONV_UNMGD`|Indica que a chamada não é gerenciada.|  
|`IMAGE_CEE_CS_CALLCONV_GENERICINST`|Indica uma instanciação de método genérico.|  
|`IMAGE_CEE_CS_CALLCONV_NATIVEVARARG`|Indica uma chamada de PInvoke de 64 bits para um método que usa um número variável de parâmetros.|  
|`IMAGE_CEE_CS_CALLCONV_MAX`|Descreve um valor inválido de 4 bits.|  
|`IMAGE_CEE_CS_CALLCONV_MASK`|Indica que a Convenção de chamada é descrita pelos quatro bits inferiores.|  
|`IMAGE_CEE_CS_CALLCONV_HASTHIS`|Indica que o bit superior descreve um `this` parâmetro.|  
|`IMAGE_CEE_CS_CALLCONV_EXPLICITTHIS`|Indica que um `this` parâmetro é descrito explicitamente na assinatura.|  
|`IMAGE_CEE_CS_CALLCONV_GENERIC`|Indica uma assinatura de método genérico com um número explícito de argumentos de tipo. Isso precede uma contagem de parâmetros comum.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorHdr. h  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Enumerações de metadados](metadata-enumerations.md)
