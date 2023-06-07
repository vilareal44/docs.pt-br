---
title: Enumeração COINITIEE
ms.date: 03/30/2017
api_name:
- COINITIEE
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- COINITIEE
helpviewer_keywords:
- COINITIEE enumeration [.NET Framework metadata]
ms.assetid: 64264238-3b68-4bac-a887-36b552426a6c
topic_type:
- apiref
ms.openlocfilehash: f4d1c2f105abb64bf196d0dd8371c2788c97336e
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84005902"
---
# <a name="coinitiee-enumeration"></a>Enumeração COINITIEE
Especifica constantes usadas por [CoInitialize](../hosting/coinitializeee-function.md) ao inicializar o Common Language Runtime.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum tagCOINITEE {  
   COINITEE_DEFAULT = 0x0,  
   COINITEE_DLL     = 0x1,  
   COINITEE_MAIN    = 0x2  
} COINITIEE;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`COINITEE_DEFAULT`|Modo de inicialização padrão. Isso inicializa o tempo de execução e cria o padrão <xref:System.AppDomain> .|  
|`COINITEE_DLL`|Inicializa para executar uma DLL gerenciada.|  
|`COINITEE_MAIN`|Inicializa para executar um EXE gerenciado. Isso inicializa o tempo de execução, mas não cria o padrão <xref:System.AppDomain> , que é criado após a inserção da rotina principal do exe.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Incluído como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Enumerações de metadados](metadata-enumerations.md)
