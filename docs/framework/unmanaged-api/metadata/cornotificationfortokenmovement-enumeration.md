---
title: Enumeração CorNotificationForTokenMovement
ms.date: 03/30/2017
api_name:
- CorNotificationForTokenMovement
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorNotificationForTokenMovement
helpviewer_keywords:
- CorNotificationForTokenMovement enumeration [.NET Framework metadata]
ms.assetid: 1edd1670-976a-4fc8-bef7-7c41e60ad989
topic_type:
- apiref
ms.openlocfilehash: e8065a5492884a4b7f5d662737e4beddc6fca5b3
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84007592"
---
# <a name="cornotificationfortokenmovement-enumeration"></a>Enumeração CorNotificationForTokenMovement
Especifica as notificações que serão enviadas para o cliente da API de metadados quando ocorrer um remapeamento de token.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum CorNotificationForTokenMovement {  
  
    MDNotifyDefault             = 0x0000000f,  
    MDNotifyAll                 = 0xffffffff,  
    MDNotifyNone                = 0x00000000,  
    MDNotifyMethodDef           = 0x00000001,  
    MDNotifyMemberRef           = 0x00000002,  
    MDNotifyFieldDef            = 0x00000004,  
    MDNotifyTypeRef             = 0x00000008,  
  
    MDNotifyTypeDef             = 0x00000010,  
    MDNotifyParamDef            = 0x00000020,  
    MDNotifyInterfaceImpl       = 0x00000040,  
    MDNotifyProperty            = 0x00000080,  
    MDNotifyEvent               = 0x00000100,  
    MDNotifySignature           = 0x00000200,  
    MDNotifyTypeSpec            = 0x00000400,  
    MDNotifyCustomAttribute     = 0x00000800,  
    MDNotifySecurityValue       = 0x00001000,  
    MDNotifyPermission          = 0x00002000,  
    MDNotifyModuleRef           = 0x00004000,  
  
    MDNotifyNameSpace           = 0x00008000,  
  
    MDNotifyAssemblyRef         = 0x01000000,  
    MDNotifyFile                = 0x02000000,  
    MDNotifyExportedType        = 0x04000000,  
    MDNotifyResource            = 0x08000000  
  
} CorNotificationForTokenMovement;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`MDNotifyDefault`|Notifique quando `mdTypeRef` os `mdMethodDef` `mdMemberRef` `mdFieldDef` tokens são movidos.|  
|`MDNotifyAll`|Notificar quando qualquer token for movido.|  
|`MDNotifyNone`|Não notificar quando os tokens forem movidos.|  
|`MDNotifyMethodDef`|Notificar quando um `mdMethodDef` token se mover.|  
|`MDNotifyMemberRef`|Notificar quando um `mdMemberRef` token se mover.|  
|`MDNotifyFieldDef`|Notificar quando um `mdFieldDef` token se mover.|  
|`MDNotifyTypeRef`|Notificar quando um `mdTypeRef` token se mover.|  
|`MDNotifyTypeDef`|Notificar quando um `mdTypeDef` token se mover.|  
|`MDNotifyParamDef`|Notificar quando um `mdParamDef` token se mover.|  
|`MDNotifyInterfaceImpl`|Notificar quando um `mdInterfaceImpl` token se mover.|  
|`MDNotifyProperty`|Notificar quando um `mdProperty` token se mover.|  
|`MDNotifyEvent`|Notificar quando um `mdEvent` token se mover.|  
|`MDNotifySignature`|Notificar quando um `mdSignature` token se mover.|  
|`MDNotifyTypeSpec`|Notificar quando um `mdTypeSpec` token se mover.|  
|`MDNotifyCustomAttribute`|Notificar quando um `mdCustomAttribute` token se mover.|  
|`MDNotifySecurityValue`|Notificar quando um `mdSecurityValue` token se mover.|  
|`MDNotifyPermission`|Notificar quando um `mdPermission` token se mover.|  
|`MDNotifyModuleRef`|Notificar quando um `mdModuleRef` token se mover.|  
|`MDNotifyNameSpace`|Notificar quando um `mdNameSpace` token se mover.|  
|`MDNotifyAssemblyRef`|Notificar quando um `mdAssemblyRef` token se mover.|  
|`MDNotifyFile`|Notificar quando um `mdFile` token se mover.|  
|`MDNotifyExportedType`|Notificar quando um `mdExportedType` token se mover.|  
|`MDNotifyResource`|Notificar quando um `mdManifestResource` token se mover.|  
  
## <a name="remarks"></a>Comentários  
 Um token pode ser mapeado novamente (ou seja, movido) durante uma mesclagem de metadados.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorHdr. h  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Enumerações de metadados](metadata-enumerations.md)
