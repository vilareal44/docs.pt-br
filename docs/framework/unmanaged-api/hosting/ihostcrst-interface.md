---
title: Interface IHostCrst
ms.date: 03/30/2017
api_name:
- IHostCrst
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostCrst
helpviewer_keywords:
- IHostCrst interface [.NET Framework hosting]
ms.assetid: ac298ebd-0815-47e4-a823-30b31baab903
topic_type:
- apiref
ms.openlocfilehash: e8cb1486ccea11ba6edcf7bbb781a9bf210b496d
ms.sourcegitcommit: d223616e7e6fe2139079052e6fcbe25413fb9900
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83804910"
---
# <a name="ihostcrst-interface"></a>Interface IHostCrst
Serve como a representação do host de uma seção crítica para Threading.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método Enter](ihostcrst-enter-method.md)|Insere a seção crítica.|  
|[Método Leave](ihostcrst-leave-method.md)|Deixa a seção crítica.|  
|[Método SetSpinCount](ihostcrst-setspincount-method.md)|Define a contagem de rotação para a seção crítica.|  
|[Método TryEnter](ihostcrst-tryenter-method.md)|Tenta inserir a seção crítica e relata êxito ou falha imediatamente.|  
  
## <a name="remarks"></a>Comentários  
 `IHostCrst`permite que o Common Language Runtime (CLR) se comunique diretamente com a representação do host de uma seção crítica, em vez de usar funções do Win32, como `EnterCriticalSection` ou `LeaveCriticalSection` .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICLRSyncManager](iclrsyncmanager-interface.md)
- [Interface IHostSyncManager](ihostsyncmanager-interface.md)
- [Interfaces de hospedagem](hosting-interfaces.md)
