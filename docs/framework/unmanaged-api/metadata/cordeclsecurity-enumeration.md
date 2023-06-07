---
title: Enumeração CorDeclSecurity
ms.date: 03/30/2017
api_name:
- CorDeclSecurity
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorDeclSecurity
helpviewer_keywords:
- CorDeclSecurity enumeration [.NET Framework metadata]
ms.assetid: 864f1267-d267-4696-8df7-1f83f8444d6f
topic_type:
- apiref
ms.openlocfilehash: ffbc9a10ff48b3dfd59b95c0f6b9ecab80b6a49c
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84007878"
---
# <a name="cordeclsecurity-enumeration"></a>Enumeração CorDeclSecurity
Especifica as ações de segurança que podem ser executadas usando a segurança declarativa.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum CorDeclSecurity {  
  
    dclActionMask               =   0x001f,  
    dclActionNil                =   0x0000,  
    dclRequest                  =   0x0001,  
    dclDemand                   =   0x0002,  
    dclAssert                   =   0x0003,  
    dclDeny                     =   0x0004,  
    dclPermitOnly               =   0x0005,  
    dclLinktimeCheck            =   0x0006,  
    dclInheritanceCheck         =   0x0007,  
    dclRequestMinimum           =   0x0008,  
    dclRequestOptional          =   0x0009,  
    dclRequestRefuse            =   0x000a,  
    dclPrejitGrant              =   0x000b,  
    dclPrejitDenied             =   0x000c,  
    dclNonCasDemand             =   0x000d,  
    dclNonCasLinkDemand         =   0x000e,  
    dclNonCasInheritance        =   0x000f,  
    dclLinkDemandChoice         =   0x0010,  
    dclInheritanceDemandChoice  =   0x0011,  
    dclDemandChoice             =   0x0012,  
    dclMaximumValue             =   0x0012  
  
} CorDeclSecurity;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`dclActionMask`|Reservado.|  
|`dclActionNil`|Reservado.|  
|`dclRequest`|Reservado.|  
|`dclDemand`|Todos os chamadores na pilha de chamadas deverão ter a permissão especificada pelo objeto de permissão atual.|  
|`dclAssert`|O código de chamada pode acessar o recurso identificado pelo objeto de permissão atual, mesmo que os chamadores mais altos na pilha não tenham recebido permissão para acessar o recurso|  
|`dclDeny`|A capacidade de acessar o recurso especificado pelo objeto de permissão atual é negada aos chamadores, mesmo que eles tenham recebido permissão para acessá-lo.|  
|`dclPermitOnly`|Somente os recursos especificados por esse objeto de permissão poderão ser acessados, mesmo que o código tenha recebido permissão para acessar outros recursos.|  
|`dclLinktimeCheck`|É necessário que o chamador imediato tenha recebido a permissão especificada para um determinado período de tempo.|  
|`dclInheritanceCheck`|A classe derivada que herda outra classe ou a substituição de um método é necessária para ter recebido a permissão especificada.|  
|`dclRequestMinimum`|O chamador pode solicitar as permissões mínimas necessárias para que o código seja executado. Esta ação só pode ser usada no escopo do assembly.|  
|`dclRequestOptional`|O chamador pode solicitar permissões adicionais que são opcionais (não é necessário executar). Essa solicitação recusa implicitamente todas as outras permissões não solicitadas especificamente. Esta ação só pode ser usada no escopo do assembly.|  
|`dclRequestRefuse`|A solicitação do chamador para permissões que podem ser usadas não será concedida. Esta ação só pode ser usada no escopo do assembly.|  
|`dclPrejitGrant`|Reservado.|  
|`dclPrejitDenied`|Reservado.|  
|`dclNonCasDemand`|Reservado.|  
|`dclNonCasLinkDemand`|O chamador imediato deverá ter recebido a permissão especificada.|  
|`dclNonCasInheritance`|Reservado.|  
|`dclLinkDemandChoice`|Reservado.|  
|`dclInheritanceDemandChoice`|Reservado.|  
|`dclDemandChoice`|Reservado.|  
|`dclMaximumValue`|Reservado.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorHdr. h  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Enumerações de metadados](metadata-enumerations.md)
