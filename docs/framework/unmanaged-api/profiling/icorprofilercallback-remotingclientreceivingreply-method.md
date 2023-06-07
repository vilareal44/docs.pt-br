---
title: Método ICorProfilerCallback::RemotingClientReceivingReply
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.RemotingClientReceivingReply
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::RemotingClientReceivingReply
helpviewer_keywords:
- ICorProfilerCallback::RemotingClientReceivingReply method [.NET Framework profiling]
- RemotingClientReceivingReply method [.NET Framework profiling]
ms.assetid: 15cfc300-8231-4ecb-9a04-19851c3eb484
topic_type:
- apiref
ms.openlocfilehash: 0a21924008bcbfa0894218f57aee559a564f8003
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84499968"
---
# <a name="icorprofilercallbackremotingclientreceivingreply-method"></a>Método ICorProfilerCallback::RemotingClientReceivingReply
Notifica o criador de perfil de que a parte do lado do servidor de uma chamada de comunicação remota foi concluída e que o cliente agora está recebendo e prestes a processar a resposta.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT RemotingClientReceivingReply(  
    [in] GUID *pCookie,  
    [in] BOOL fIsAsync);
```  
  
## <a name="parameters"></a>Parâmetros  
 `pCookie`  
 no Um valor que corresponderá com o valor fornecido em [ICorProfilerCallback:: RemotingServerSendingReply](icorprofilercallback-remotingserversendingreply-method.md) sob estas condições:  
  
- Os cookies de GUID de comunicação remota estão ativos.  
  
- O canal tem sucesso ao transmitir a mensagem.  
  
- Os cookies de GUID estão ativos no processo do lado do servidor.  
  
 Isso permite um emparelhamento fácil de chamadas remotas.  
  
 `fIsAsync`  
 no Um valor que é `true` se a chamada for assíncrona; caso contrário, `false` .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback](icorprofilercallback-interface.md)
