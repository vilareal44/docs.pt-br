---
title: Método ICorProfilerCallback::JITCachedFunctionSearchStarted
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.JITCachedFunctionSearchStarted
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::JITCachedFunctionSearchStarted
helpviewer_keywords:
- JITCachedFunctionSearchStarted method [.NET Framework profiling]
- ICorProfilerCallback::JITCachedFunctionSearchStarted method [.NET Framework profiling]
ms.assetid: 5cba642c-0d80-48ee-889d-198c5044d821
topic_type:
- apiref
ms.openlocfilehash: d8f4a04abea0bd5eb9bf38629a8fcaf76479bcc9
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500046"
---
# <a name="icorprofilercallbackjitcachedfunctionsearchstarted-method"></a>Método ICorProfilerCallback::JITCachedFunctionSearchStarted
Notifica o criador de perfil de que uma pesquisa foi iniciada para uma função que foi compilada anteriormente usando o gerador de imagem nativa (NGen. exe).  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT JITCachedFunctionSearchStarted(  
    [in]  FunctionID functionId,  
    [out] BOOL *pbUseCachedFunction);  
```  
  
## <a name="parameters"></a>Parâmetros

- `functionId`

  \[in] a ID da função para a qual a pesquisa está sendo executada.

- `pbUseCachedFunction`

  \[out] `true` se o mecanismo de execução deve usar a versão armazenada em cache de uma função (se disponível); caso contrário, `false` . Se o valor for `false` , o mecanismo de execução JIT compilará a função em vez de usar uma versão que não seja compilada por JIT.

## <a name="remarks"></a>Comentários  
 No .NET Framework versão 2,0, os `JITCachedFunctionSearchStarted` retornos de chamada do [método e ICorProfilerCallback:: JITCachedFunctionSearchFinished](icorprofilercallback-jitcachedfunctionsearchfinished-method.md) não serão feitos para todas as funções em imagens NGen regulares. Somente imagens NGen otimizadas para um perfil gerarão retornos de chamada para todas as funções na imagem. No entanto, devido à sobrecarga adicional, um criador de perfil deve solicitar imagens NGen otimizadas para o criador de perfil somente se pretender usar esses retornos de chamada para forçar uma função a ser compilada just-in-time (JIT). Caso contrário, o criador de perfil deve usar uma estratégia lenta para coletar informações de função.  
  
 Os profileres devem oferecer suporte a casos em que vários threads de um aplicativo de perfil criado chamem o mesmo método simultaneamente. Por exemplo, o thread A chama `JITCachedFunctionSearchStarted` e o criador de perfil responde definindo *PBUSECACHEDFUNCTION*como false para forçar a compilação JIT. O thread A chama [ICorProfilerCallback:: JITCompilationStarted](icorprofilercallback-jitcompilationstarted-method.md) e [ICorProfilerCallback:: JITCompilationFinished](icorprofilercallback-jitcompilationfinished-method.md).  
  
 Agora, o thread B chama `JITCachedFunctionSearchStarted` a mesma função. Mesmo que o criador de perfil tenha declarado sua intenção de compilar a função por JIT, o criador de perfil recebe o segundo retorno de chamada, pois o thread B envia um retorno de chamada antes que o criador de perfil tenha respondido às chamadas do thread a `JITCachedFunctionSearchStarted` . A ordem na qual os threads fazem chamadas depende de como os threads são agendados.  
  
 Quando o criador de perfil recebe retornos de chamada duplicados, ele deve definir o valor referenciado pelo `pbUseCachedFunction` para o mesmo valor para todos os retornos de chamada duplicados. Ou seja, quando `JITCachedFunctionSearchStarted` é chamado várias vezes com o mesmo `functionId` valor, o criador de perfil deve responder o mesmo a cada vez.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback](icorprofilercallback-interface.md)
