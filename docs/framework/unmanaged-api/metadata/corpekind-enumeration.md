---
title: Enumeração CorPEKind
ms.date: 03/30/2017
api_name:
- CorPEKind
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorPEKind
helpviewer_keywords:
- CorPEKind enumeration [.NET Framework metadata]
ms.assetid: 22dc6dea-b1b9-4982-a730-a022d586b117
topic_type:
- apiref
ms.openlocfilehash: 8b6eab8156f72847eb6dd3369950f9b46a3fc877
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84007553"
---
# <a name="corpekind-enumeration"></a>Enumeração CorPEKind
Contém valores que descrevem um arquivo executável portátil (PE), como retornado de uma chamada para [IMetaDataImport2:: GetPEKind](imetadataimport2-getpekind-method.md).  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum CorPEKind {  
  
    peNot           = 0x00000000,  
    peILonly        = 0x00000001,  
    pe32BitRequired = 0x00000002,  
    pe32Plus        = 0x00000004,  
    pe32Unmanaged   = 0x00000008,  
    pe32BitPreferred= 0x00000010  
  
} CorPEKind;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`peNot`|Indica que este não é um arquivo PE.|  
|`peILOnly`|Indica que este arquivo PE contém apenas código gerenciado.|  
|`pe32BitRequired`|Indica que esse arquivo PE faz chamadas Win32.|  
|`pe32Plus`|Indica que este arquivo PE é executado em uma plataforma de 64 bits.|  
|`pe32Unmanaged`|Indica que este arquivo PE é um código nativo.|  
|pe32BitPreferred|Indica que este arquivo PE é de plataforma neutra e prefere ser carregado em um ambiente de 32 bits.|  
  
## <a name="remarks"></a>Comentários  
 Esses valores podem ser usados em combinações de bits.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorHdr. h  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Enumerações de metadados](metadata-enumerations.md)
