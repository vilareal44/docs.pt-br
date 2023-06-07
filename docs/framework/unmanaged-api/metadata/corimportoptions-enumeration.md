---
title: Enumeração CorImportOptions
ms.date: 03/30/2017
api_name:
- CorImportOptions
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorImportOptions
helpviewer_keywords:
- CorImportOptions enumeration [.NET Framework metadata]
ms.assetid: 4e5d03cb-97c9-4ff4-8dbd-17d94ee374d3
topic_type:
- apiref
ms.openlocfilehash: 3be8a004be752af8a8675a3499bdb6cbfd785840
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84009191"
---
# <a name="corimportoptions-enumeration"></a>Enumeração CorImportOptions
Contém valores de sinalizador que controlam o comportamento durante a importação de um assembly fora do escopo atual.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum CorImportOptions {  
  
    MDImportOptionDefault                = 0x00000000,  
    MDImportOptionAll                    = 0xFFFFFFFF,  
    MDImportOptionAllTypeDefs            = 0x00000001,  
    MDImportOptionAllMethodDefs          = 0x00000002,  
    MDImportOptionAllFieldDefs           = 0x00000004,  
    MDImportOptionAllProperties          = 0x00000008,  
    MDImportOptionAllEvents              = 0x00000010,  
    MDImportOptionAllCustomAttributes    = 0x00000020,  
    MDImportOptionAllExportedTypes       = 0x00000040  
  
} CorImportOptions;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`MDImportOptionDefault`|Indica o comportamento padrão, que é para ignorar os registros excluídos.|  
|`MDImportOptionAll`|Indica que todos os metadados devem ser enumerados.|  
|`MDImportOptionAllTypeDefs`|Indica que todos os TypeDefs, incluindo aqueles excluídos, devem ser enumerados.|  
|`MDImportOptionAllMethodDefs`|Indica que todos os MethodDefs, incluindo aqueles excluídos, devem ser enumerados.|  
|`MDImportOptionAllFieldDefs`|Indica que todos os FieldDefs, incluindo aqueles excluídos, devem ser enumerados.|  
|`MDImportOptionAllProperties`|Indica que todos os PropertyDefs, incluindo aqueles excluídos, devem ser enumerados.|  
|`MDImportOptionAllEvents`|Indica que todos os EventDefs, incluindo aqueles excluídos, devem ser enumerados.|  
|`MDImportOptionAllCustomAttributes`|Indica que todos os atributos personalizados, incluindo aqueles excluídos, devem ser enumerados.|  
|`MDImportOptionAllExportedTypes`|Indica que todos os tipos exportados, incluindo aqueles excluídos, devem ser enumerados.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorHdr. h  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Enumerações de metadados](metadata-enumerations.md)
