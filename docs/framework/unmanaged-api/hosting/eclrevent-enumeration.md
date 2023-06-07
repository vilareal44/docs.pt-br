---
title: Enumeração EClrEvent
ms.date: 03/30/2017
api_name:
- EClrEvent
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- EClrEvent
helpviewer_keywords:
- EClrEvent enumeration [.NET Framework hosting]
ms.assetid: 7c36a7c2-75a2-4971-bc23-abf54c812154
topic_type:
- apiref
ms.openlocfilehash: 3ecaebb9d943a3cdbb231307012b5dc3aaf000f7
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84493377"
---
# <a name="eclrevent-enumeration"></a>Enumeração EClrEvent
Descreve os eventos de Common Language Runtime (CLR) para os quais o host pode registrar retornos de chamada.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum {  
    Event_ClrDisabled,  
    Event_DomainUnload,  
    Event_MDAFired,  
    Event_StackOverflow  
} EClrEvent;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`Event_ClrDisabled`|Especifica um erro fatal de CLR.|  
|`Event_DomainUnload`|Especifica o descarregamento de um específico <xref:System.AppDomain> .|  
|`Event_MDAFired`|Especifica que uma mensagem do MDA (Assistente de depuração gerenciada) foi gerada.|  
|`Event_StackOverflow`|Especifica que ocorreu um erro de estouro de pilha.|  
  
## <a name="remarks"></a>Comentários  
 O host pode registrar retornos de chamada para qualquer um dos tipos de eventos descritos pela `EClrEvent` chamada de métodos da interface [ICLROnEventManager](iclroneventmanager-interface.md) . O host obtém um ponteiro para essa interface chamando o método [ICLRControl:: GetCLRManager](iclrcontrol-getclrmanager-method.md) .  
  
 Os `Event_CLRDisabled` `Event_DomainUnload` eventos e podem ser gerados mais de uma vez e de threads diferentes para sinalizar um descarregamento ou a desabilitação do CLR.  
  
 O `Event_MDAFired` evento gera a criação de uma instância de [MDAInfo](mdainfo-structure.md) que contém os detalhes da mensagem do MDA. Para obter mais informações sobre MDAs, consulte [diagnosticando erros com assistentes de depuração gerenciada](../../debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants.md).  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IActionOnCLREvent](iactiononclrevent-interface.md)
- [Interface ICLRControl](iclrcontrol-interface.md)
- [Hospedando enumerações](hosting-enumerations.md)
