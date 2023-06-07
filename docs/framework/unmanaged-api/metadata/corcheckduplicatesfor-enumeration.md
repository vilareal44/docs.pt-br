---
title: Enumeração CorCheckDuplicatesFor
ms.date: 03/30/2017
api_name:
- CorCheckDuplicatesFor
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorCheckDuplicatesFor
helpviewer_keywords:
- CorCheckDuplicatesFor enumeration [.NET Framework metadata]
ms.assetid: d8ec8d3c-70f7-4cc6-9957-68068fd8f49c
topic_type:
- apiref
ms.openlocfilehash: 2985c419b25b8bf76df8fee0f0f37ba9ebee3df7
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84007891"
---
# <a name="corcheckduplicatesfor-enumeration"></a>Enumeração CorCheckDuplicatesFor
Especifica os tokens de metadados que serão verificados em busca de duplicatas.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum CorCheckDuplicatesFor {  
  
    MDDupAll                    = 0xffffffff,  
    MDDupENC                    = MDDupAll,  
    MDNoDupChecks               = 0x00000000,  
    MDDupTypeDef                = 0x00000001,  
    MDDupInterfaceImpl          = 0x00000002,  
    MDDupMethodDef              = 0x00000004,  
    MDDupTypeRef                = 0x00000008,  
    MDDupMemberRef              = 0x00000010,  
    MDDupCustomAttribute        = 0x00000020,  
    MDDupParamDef               = 0x00000040,  
    MDDupPermission             = 0x00000080,  
    MDDupProperty               = 0x00000100,  
    MDDupEvent                  = 0x00000200,  
    MDDupFieldDef               = 0x00000400,  
    MDDupSignature              = 0x00000800,  
    MDDupModuleRef              = 0x00001000,  
    MDDupTypeSpec               = 0x00002000,  
    MDDupImplMap                = 0x00004000,  
    MDDupAssemblyRef            = 0x00008000,  
    MDDupFile                   = 0x00010000,  
    MDDupExportedType           = 0x00020000,  
    MDDupManifestResource       = 0x00040000,  
    MDDupGenericParam           = 0x00080000,  
    MDDupMethodSpec             = 0x00100000,  
    MDDupGenericParamConstraint = 0x00200000,  
  
    MDDupAssembly               = 0x10000000,  
  
    MDDupDefault =
        MDNoDupChecks | MDDupTypeRef | MDDupMemberRef |
        MDDupSignature | MDDupTypeSpec | MDDupMethodSpec  
  
} CorCheckDuplicatesFor;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`MDDupAll`|Verifique se há duplicatas em todos os tokens de metadados.|  
|`MDDupENC`|Não usado.|  
|`MDNoDupChecks`|Não verifique os tokens de metadados em busca de duplicatas.|  
|`MDDupTypeDef`|Verifique se há duplicatas de `mdTypeDef` tokens.|  
|`MDDupInterfaceImpl`|Verifique se há duplicatas de `mdInterfaceImpl` tokens.|  
|`MDDupMethodDef`|Verifique se há duplicatas de `mdMethodDef` tokens.|  
|`MDDupTypeRef`|Verifique se há duplicatas de `mdTypeRef` tokens.|  
|`MDDupMemberRef`|Verifique se há duplicatas de `mdMemberRef` tokens.|  
|`MDDupCustomAttribute`|Verifique se há duplicatas de `mdCustomAttribute` tokens.|  
|`MDDupParamDef`|Verifique se há duplicatas de `mdParamDef` tokens.|  
|`MDDupPermission`|Verifique se há duplicatas de `mdPermission` tokens.|  
|`MDDupProperty`|Verifique se há duplicatas de `mdProperty` tokens.|  
|`MDDupEvent`|Verifique se há duplicatas de `mdEvent` tokens.|  
|`MDDupFieldDef`|Verifique se há duplicatas de `mdFieldDef` tokens.|  
|`MDDupSignature`|Verifique se há duplicatas de `mdSignature` tokens.|  
|`MDDupModuleRef`|Verifique se há duplicatas de `mdModuleRef` tokens.|  
|`MDDupTypeSpec`|Verifique se há duplicatas de `mdTypeSpec` tokens.|  
|`MDDupImplMap`|Verifique se há duplicatas de `mdImplMap` tokens.|  
|`MDDupAssemblyRef`|Verifique se há duplicatas de `mdAssemblyRef` tokens.|  
|`MDDupFile`|Verifique se há duplicatas de `mdFile` tokens.|  
|`MDDupExportedType`|Verifique se há duplicatas de `mdExportedType` tokens.|  
|`MDDupManifestResource`|Verifique se há duplicatas de `mdManifestResource` tokens.|  
|`MDDupGenericParam`|Verifique se há duplicatas de `mdGenericParam` tokens.|  
|`MDDupMethodSpec`|Verifique se há duplicatas de `mdMethodSpec` tokens.|  
|`MDDupGenericParamConstraint`|Verifique se há duplicatas de `mdGenericParamConstraint` tokens.|  
|`MDDupAssembly`|Verifique se há duplicatas de `mdAssembly` tokens.|  
|`MDDupDefault`|Verifique se há duplicatas `mdMemberRef` de `mdTypeRef` tokens,,, `mdSignature` `mdTypeSpec` e `mdMethodSpec` .|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorHdr. h  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Enumerações de metadados](metadata-enumerations.md)
