---
title: Estrutura MDAInfo
ms.date: 03/30/2017
api_name:
- MDAInfo
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- MDAInfo
helpviewer_keywords:
- MDAInfo structure [.NET Framework hosting]
ms.assetid: fb8c14f7-d461-43d1-8b47-adb6723b9b93
topic_type:
- apiref
ms.openlocfilehash: 517e0ae7fb5d5151f94f82d9146ebbf40bad2ef9
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503855"
---
# <a name="mdainfo-structure"></a>Estrutura MDAInfo
Fornece detalhes sobre o `Event_MDAFired` evento, que dispara a criação de um MDA (Assistente de depuração gerenciada).  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef struct _MDAInfo {  
    LPCWSTR  lpMDACaption;  
    LPCWSTR  lpMDAMessage  
} MDAInfo;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`lpMDACaption`|O título do MDA atual. O título descreve o tipo de falha que disparou o `Event_MDAFired` evento.|  
|`lpMDAMessage`|A mensagem de saída fornecida pelo MDA atual.|  
  
## <a name="remarks"></a>Comentários  
 Os assistentes de depuração gerenciada (MDAs) são ferramentas de depuração que funcionam em conjunto com o Common Language Runtime (CLR) para executar tarefas como identificar condições inválidas no mecanismo de execução de tempo de execução ou despejar informações adicionais sobre o estado do mecanismo. MDAs gerar mensagens XML sobre eventos que, de outra forma, são difíceis de interceptar. Eles são especialmente úteis para a depuração de transições entre códigos gerenciados e não gerenciados.  
  
 O tempo de execução executa as seguintes etapas quando um evento que dispara a criação de um MDA é acionado:  
  
- Se o host não tiver registrado uma instância de [IActionOnCLREvent](iactiononclrevent-interface.md) chamando [ICLROnEventManager:: RegisterActionOnEvent](iclroneventmanager-registeractiononevent-method.md) para ser notificado de um `Event_MDAFired` evento, o tempo de execução continuará com seu comportamento padrão não hospedado.  
  
- Se o host tiver registrado um manipulador para esse evento, o tempo de execução verificará se um depurador está anexado ao processo. Se for, o tempo de execução será interrompido no depurador. Quando o depurador continuar, ele chamará o host. Se nenhum depurador estiver anexado, o tempo de execução chamará `IActionOnCLREvent::OnEvent` e passará um ponteiro para uma `MDAInfo` instância como o `data` parâmetro.  
  
 O host pode optar por ativar o MDAs e ser notificado quando um MDA for ativado. Isso dá ao host uma oportunidade de substituir o comportamento padrão e anular o thread gerenciado que disparou o evento, para impedir que ele corrompa o estado do processo. Para obter mais informações sobre como usar o MDAs, consulte [diagnosticando erros com assistentes de depuração gerenciada](../../debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants.md).  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. idl  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Estruturas de hospedagem](hosting-structures.md)
- [Diagnosticando erros com assistentes para depuração gerenciada](../../debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants.md)
