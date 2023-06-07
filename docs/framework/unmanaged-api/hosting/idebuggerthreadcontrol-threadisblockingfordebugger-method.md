---
title: Método IDebuggerThreadControl::ThreadIsBlockingForDebugger
ms.date: 03/30/2017
api_name:
- IDebuggerThreadControl.ThreadIsBlockingForDebugger
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ThreadIsBlockingForDebugger
helpviewer_keywords:
- ThreadIsBlockingForDebugger method [.NET Framework hosting]
- IDebuggerThreadControl::ThreadIsBlockingForDebugger method [.NET Framework hosting]
ms.assetid: d4d7cb2d-69da-48b3-879a-1a8a68c9bfa8
topic_type:
- apiref
ms.openlocfilehash: f7cdc3fe9d52db56d0280bc602d3a9f2f54e8246
ms.sourcegitcommit: d223616e7e6fe2139079052e6fcbe25413fb9900
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83805261"
---
# <a name="idebuggerthreadcontrolthreadisblockingfordebugger-method"></a>Método IDebuggerThreadControl::ThreadIsBlockingForDebugger
Notifica o host que o thread que está enviando esse retorno de chamada está prestes a ser bloqueado nos serviços de depuração.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT ThreadIsBlockingForDebugger ( );  
```  
  
## <a name="remarks"></a>Comentários  
 O `ThreadIsBlockingForDebugger` método sempre será chamado em um thread de tempo de execução.  
  
 O `ThreadIsBlockingForDebugger` método dá ao host uma oportunidade de executar outra ação enquanto o thread é bloqueado.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **Versões do .NET Framework:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IDebuggerThreadControl](idebuggerthreadcontrol-interface.md)
