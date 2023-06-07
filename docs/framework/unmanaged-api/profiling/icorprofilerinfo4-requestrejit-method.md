---
title: Método ICorProfilerInfo4::RequestReJIT
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo4.RequestReJIT
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo4::RequestReJIT
helpviewer_keywords:
- RequestReJIT method, ICorProfilerInfo4 interface [.NET Framework profiling]
- ICorProfilerInfo4::RequestReJIT method [.NET Framework profiling]
ms.assetid: 781ed736-f30c-4816-920e-3552e36542c6
topic_type:
- apiref
ms.openlocfilehash: 7dd82f2dfab885070df4789fe5efc16a49d50e06
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84495795"
---
# <a name="icorprofilerinfo4requestrejit-method"></a>Método ICorProfilerInfo4::RequestReJIT
Solicita uma recompilação JIT de todas as instâncias das funções especificadas.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT RequestReJIT (  
   [in] ULONG    cFunctions,  
   [in, size_is(cFunctions)]  ModuleID    moduleIds[],  
   [in, size_is(cFunctions)]  mdMethodDef methodIds[]);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `cFunctions`  
 no O número de funções a serem recompiladas.  
  
 `moduleIds`  
 no Especifica a `moduleId` parte dos pares ( `module` , `methodDef` ) que identificam as funções a serem recompiladas.  
  
 `methodIds`  
 no Especifica a `methodId` parte dos pares ( `module` , `methodDef` ) que identificam as funções a serem recompiladas.  
  
## <a name="return-value"></a>Valor Retornado  
 Esse método retorna os HRESULTs específicos a seguir, bem como os erros de HRESULT que indicam falha de método.  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|Foi feita uma tentativa de marcar todos os métodos para a recompilação JIT. O criador de perfil deve implementar o método [ICorProfilerCallback4:: ReJITError](icorprofilercallback4-rejiterror-method.md) para determinar quais métodos foram marcados com êxito para recompilação JIT.|  
|CORPROF_E_CALLBACK4_REQUIRED|O criador de perfil deve implementar a interface [ICorProfilerCallback4](icorprofilercallback4-interface.md) para que essa chamada seja suportada.|  
|CORPROF_E_REJIT_NOT_ENABLED|A recompilação JIT não foi habilitada. Você deve habilitar a recompilação JIT durante a inicialização usando o método [ICorProfilerInfo:: SetEventMask](icorprofilerinfo-seteventmask-method.md) para definir o `COR_PRF_ENABLE_REJIT` sinalizador.|  
|E_INVALIDARG|`cFunctions`é 0, ou `moduleIds` ou `methodIds` é `NULL` .|  
|||  
|E_OUTOFMEMORY|O CLR não pôde concluir a solicitação porque ficou sem memória.|  
  
## <a name="remarks"></a>Comentários  
 Chame `RequestReJIT` para que o tempo de execução recompile um conjunto especificado de funções. Um criador de perfil de código pode usar a interface [ICorProfilerFunctionControl](icorprofilerfunctioncontrol-interface.md) para ajustar o código que é gerado quando as funções são recompiladas. Isso não afeta as funções atualmente em execução, apenas as invocações de função futuras. Se qualquer uma das funções especificadas tiver sido recompilada por JIT, a solicitação de uma recompilação será equivalente à reversão e à recompilação da função. Para preservar o inversão, quando o compilador JIT compila a versão original de uma função, ele considera apenas as versões originais dos seus chamadores para as decisões de inalinhamento. Quando o compilador JIT recompila uma função, ele considera as versões atuais (recompiladas ou originais) de seus chamadores para o inlining.  
  
 Um criador de perfil normalmente chama `RequestReJIT` em resposta à entrada do usuário solicitando que o criador de perfil tenha um ou mais métodos. `RequestReJIT`normalmente suspende o tempo de execução para fazer parte de seu trabalho e pode potencialmente disparar uma coleta de lixo. Como tal, o criador de perfil deve chamar `RequestReJIT` de um thread criado anteriormente, e não de um thread criado pelo CLR que está executando um retorno de chamada do profiler no momento.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo4](icorprofilerinfo4-interface.md)
- [Criação de perfil de interfaces](profiling-interfaces.md)
- [Criação de perfil](index.md)
