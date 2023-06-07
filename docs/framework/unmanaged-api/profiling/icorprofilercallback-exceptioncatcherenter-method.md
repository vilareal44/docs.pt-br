---
title: Método ICorProfilerCallback::ExceptionCatcherEnter
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.ExceptionCatcherEnter
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::ExceptionCatcherEnter
helpviewer_keywords:
- ICorProfilerCallback::ExceptionCatcherEnter method [.NET Framework profiling]
- ExceptionCatcherEnter method [.NET Framework profiling]
ms.assetid: 41462329-a648-46f0-ae6d-728b94c31aa9
topic_type:
- apiref
ms.openlocfilehash: 9d0ef4da4ba6c8db49bcb0b40911756f7d9db66d
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500306"
---
# <a name="icorprofilercallbackexceptioncatcherenter-method"></a>Método ICorProfilerCallback::ExceptionCatcherEnter
Notifica o criador de perfil que o controle está sendo passado para o `catch` bloco apropriado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT ExceptionCatcherEnter(  
    [in] FunctionID functionId,  
    [in] ObjectID   objectId);  
```  
  
## <a name="parameters"></a>Parâmetros

- `functionId`

  \[in] o identificador da função que contém o `catch` bloco.
  
- `objectId`

  \[in] o identificador da exceção que está sendo manipulada.

## <a name="remarks"></a>Comentários  
 O `ExceptionCatcherEnter` método será chamado somente se o ponto de captura estiver no código compilado com o compilador JIT (just-in-time). Uma exceção que é capturada no código não gerenciado ou no código interno do tempo de execução não chamará essa notificação. O `objectId` valor é passado novamente, pois uma coleta de lixo pode ter movido o objeto desde a `ExceptionThrown` notificação.  
  
 O criador de perfil não deve bloquear em sua implementação desse método porque a pilha pode não estar em um estado que permita a coleta de lixo e, portanto, a coleta de lixo preemptiva não pode ser habilitada. Se o criador de perfil for bloqueado aqui e a coleta de lixo for tentada, o tempo de execução será bloqueado até que esse retorno de chamada seja retornado.  
  
 A implementação do criador de perfil desse método não deve chamar o código gerenciado ou, de qualquer forma, causar uma alocação de memória gerenciada.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback](icorprofilercallback-interface.md)
- [Método ExceptionCatcherLeave](icorprofilercallback-exceptioncatcherleave-method.md)
