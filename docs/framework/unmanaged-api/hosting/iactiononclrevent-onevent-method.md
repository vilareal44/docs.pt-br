---
title: Método IActionOnCLREvent::OnEvent
ms.date: 03/30/2017
api_name:
- IActionOnCLREvent.OnEvent
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IActionOnCLREvent::OnEvent
helpviewer_keywords:
- OnEvent method [.NET Framework hosting]
- IActionOnCLREvent::OnEvent method [.NET Framework hosting]
ms.assetid: 0970f10c-4304-4c12-91c0-83e51455afb4
topic_type:
- apiref
ms.openlocfilehash: bbf5e299285071ba6d43fd2c40fc724d19bc7b2a
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504336"
---
# <a name="iactiononclreventonevent-method"></a>Método IActionOnCLREvent::OnEvent
Executa retornos de chamada em eventos que foram registrados usando uma chamada para o método [ICLROnEventManager:: RegisterActionOnEvent](iclroneventmanager-registeractiononevent-method.md) .  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT OnEvent (  
    [in] EClrEvent event,  
    [in] PVOID     data  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `event`  
 no Um dos valores de [EClrEvent](eclrevent-enumeration.md) , que indica o tipo de evento.  
  
 `data`  
 no Um ponteiro para um objeto que contém detalhes sobre `event` .  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|`OnEvent`retornado com êxito.|  
|HOST_E_CLRNOTAVAILABLE|O Common Language Runtime (CLR) não foi carregado em um processo ou o CLR está em um estado no qual não pode executar código gerenciado ou processar a chamada com êxito.|  
|HOST_E_TIMEOUT|A chamada atingiu o tempo limite.|  
|HOST_E_NOT_OWNER|O chamador não possui o bloqueio.|  
|HOST_E_ABANDONED|Um evento foi cancelado enquanto um thread ou uma fibra bloqueada estava esperando.|  
|E_FAIL|Ocorreu uma falha catastrófica desconhecida. Se um método retornar E_FAIL, o CLR não poderá mais ser usado no processo. As chamadas subsequentes para qualquer método de hospedagem retornam HOST_E_CLRNOTAVAILABLE.|  
  
## <a name="remarks"></a>Comentários  
 O `data` parâmetro é um ponteiro para um objeto de tipo não especificado. Se o `event` parâmetro for `Event_DomainUnload` , `data` é o identificador numérico para o <xref:System.AppDomain> que foi descarregado. O host pode tomar a ação apropriada usando esse identificador como uma chave.  
  
 Se `event` for `Event_MDAFired` , `data` é um ponteiro para uma instância [MDAInfo](mdainfo-structure.md) que contém a saída da mensagem de um MDA (Assistente de depuração gerenciada). MDAs são um recurso do CLR que ajuda os desenvolvedores na depuração, gerando mensagens XML sobre eventos que, de outra forma, são difíceis de interceptar. Essas mensagens podem ser especialmente úteis na depuração de transições entre códigos gerenciados e não gerenciados. Para obter mais informações, consulte [diagnosticando erros com assistentes de depuração gerenciada](../../debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants.md).  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Diagnosticando erros com assistentes para depuração gerenciada](../../debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants.md)
- [Enumeração EClrEvent](eclrevent-enumeration.md)
- [Interface IActionOnCLREvent](iactiononclrevent-interface.md)
- [Interface ICLRControl](iclrcontrol-interface.md)
- [Interface ICLROnEventManager](iclroneventmanager-interface.md)
- [Estrutura MDAInfo](mdainfo-structure.md)
