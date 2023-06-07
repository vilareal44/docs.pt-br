---
title: Interface ICLRDebugManager
ms.date: 03/30/2017
api_name:
- ICLRDebugManager
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRDebugManager
helpviewer_keywords:
- ICLRDebugManager interface [.NET Framework hosting]
ms.assetid: e835062c-c7d6-4945-8a44-2de7ebf3928e
topic_type:
- apiref
ms.openlocfilehash: 653c8d1d3edd38e646b4e90c0e48dbe15bed102a
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504258"
---
# <a name="iclrdebugmanager-interface"></a>Interface ICLRDebugManager
Fornece métodos que permitem que um host associe um conjunto de tarefas a um identificador e um nome amigável.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método BeginConnection](iclrdebugmanager-beginconnection-method.md)|Estabelece uma nova conexão entre o host e o depurador para associar tarefas a um identificador e um nome amigável.|  
|[Método EndConnection](iclrdebugmanager-endconnection-method.md)|Remove a associação entre uma lista de tarefas e um identificador e um nome amigável.|  
|[Método GetDacl](iclrdebugmanager-getdacl-method.md)|Este método não está implementado.|  
|[Método IsDebuggerAttached](iclrdebugmanager-isdebuggerattached-method.md)|Obtém um valor que indica se um depurador está anexado ao processo.|  
|[Método SetConnectionTasks](iclrdebugmanager-setconnectiontasks-method.md)|Associa uma lista de instâncias [ICLRTask](iclrtask-interface.md) com um identificador e um nome amigável.|  
|[Método SetDacl](iclrdebugmanager-setdacl-method.md)|Este método não está implementado.|  
|[Método SetSymbolReadingPolicy](iclrdebugmanager-setsymbolreadingpolicy-method.md)|Define a política para ler arquivos de banco de dados do programa (PDB). A política determina se as informações sobre números de linha e arquivos estão incluídas em pilhas de chamadas.|  
  
## <a name="remarks"></a>Comentários  
 Em cenários de depuração, um host pode querer agrupar tarefas de acordo com sua própria lógica de programação. Por exemplo, um agrupamento permitiria que um desenvolvedor vêsse apenas as tarefas exigidas pelas APIs do desenvolvedor, em vez de ver cada tarefa em execução no processo. `ICLRDebugManager`permite que o host implemente esse tipo de agrupamento.  
  
> [!IMPORTANT]
> Três `ICLRDebugManager` métodos, `BeginConnection` `SetConnectionTasks` e `EndConnection` , dependem uns dos outros. Eles devem ser chamados na ordem especificada para funcionar conforme o esperado.  
  
 O agrupamento e os identificadores e nomes amigáveis que o host atribui ao agrupamento não têm significado para o Common Language Runtime (CLR). O CLR simplesmente passa as informações ao depurador.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interfaces de hospedagem](hosting-interfaces.md)
