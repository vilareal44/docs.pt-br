---
title: Método ICorProfilerCallback::RemotingServerSendingReply
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.RemotingServerSendingReply
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::RemotingServerSendingReply
helpviewer_keywords:
- RemotingServerSendingReply method [.NET Framework profiling]
- ICorProfilerCallback::RemotingServerSendingReply method [.NET Framework profiling]
ms.assetid: dfe84a19-2e03-4be2-8b25-f02bad38e4a9
topic_type:
- apiref
ms.openlocfilehash: bf59d4e418223fd177bc5e19b173674b78e1f2ba
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84499916"
---
# <a name="icorprofilercallbackremotingserversendingreply-method"></a>Método ICorProfilerCallback::RemotingServerSendingReply
Notifica o criador de perfil de que o processo concluiu o processamento de uma solicitação de invocação de método remoto e está prestes a transmitir a resposta por meio de um canal.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT RemotingServerSendingReply(  
    [in] GUID *pCookie,  
    [in] BOOL fIsAsync);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pCookie`  
 no Um ponteiro para um GUID que corresponderá com o valor fornecido em [ICorProfilerCallback:: RemotingClientReceivingReply](icorprofilercallback-remotingclientreceivingreply-method.md) sob estas condições:  
  
- Os cookies de GUID de comunicação remota estão ativos.  
  
- O canal tem sucesso ao transmitir a mensagem.  
  
- Os cookies de GUID estão ativos no processo do lado do cliente.  
  
 Isso permite um emparelhamento fácil de chamadas remotas e a criação de uma pilha de chamadas lógica.  
  
 `fIsAsync`  
 no Um valor que é `true` se a chamada for assíncrona; caso contrário, `false` .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback](icorprofilercallback-interface.md)
