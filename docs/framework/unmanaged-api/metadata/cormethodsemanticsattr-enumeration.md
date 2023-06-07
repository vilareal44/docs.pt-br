---
title: Enumeração CorMethodSemanticsAttr
ms.date: 03/30/2017
api_name:
- CorMethodSemanticsAttr
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorMethodSemanticsAttr
helpviewer_keywords:
- CorMethodSemanticsAttr enumeration [.NET Framework metadata]
ms.assetid: ca2af325-eb9d-4a91-90e4-267e45b98611
topic_type:
- apiref
ms.openlocfilehash: 1572c206f4a5a5fe0fd189ca84d0bcda2249c6d4
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84007644"
---
# <a name="cormethodsemanticsattr-enumeration"></a>Enumeração CorMethodSemanticsAttr
Contém valores que descrevem a relação entre um método e uma propriedade ou evento associado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum CorMethodSemanticsAttr {  
  
    msSetter    =   0x0001,  
    msGetter    =   0x0002,  
    msOther     =   0x0004,  
    msAddOn     =   0x0008,  
    msRemoveOn  =   0x0010,  
    msFire      =   0x0020,  
  
} CorMethodSemanticsAttr;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`msSetter`|Especifica que o método é um `set` acessador para uma propriedade.|  
|`msGetter`|Especifica que o método é um `get` acessador para uma propriedade.|  
|`msOther`|Especifica que o método tem uma relação com uma propriedade ou um evento diferente daqueles definidos aqui.|  
|`msAddOn`|Especifica que o método adiciona métodos de manipulador para um evento.|  
|`msRemoveOn`|Especifica que o método Remove métodos de manipulador para um evento.|  
|`msFire`|Especifica que o método gera um evento.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorHdr. h  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Enumerações de metadados](metadata-enumerations.md)
