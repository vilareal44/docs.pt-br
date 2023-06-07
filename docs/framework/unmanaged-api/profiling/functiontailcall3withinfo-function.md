---
title: Função FunctionTailcall3WithInfo
ms.date: 03/30/2017
api_name:
- FunctionTailcall3WithInfo
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- FunctionTailcall3WithInfo
helpviewer_keywords:
- FunctionTailcall3WithInfo function [.NET Framework profiling]
ms.assetid: 46380fcc-0198-43ae-a1f5-2d4939425886
topic_type:
- apiref
ms.openlocfilehash: f076044b44859cc39d90be528ee6648f5eaa626c
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500579"
---
# <a name="functiontailcall3withinfo-function"></a>Função FunctionTailcall3WithInfo
Notifica o criador de perfil de que a função atualmente em execução está prestes a executar uma chamada tail para outra função e fornece um identificador que pode ser passado para o [método ICorProfilerInfo3:: GetFunctionTailcall3Info](icorprofilerinfo3-getfunctiontailcall3info-method.md) para recuperar o quadro de pilha.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
void __stdcall FunctionTailcall3WithInfo(  
               [in] FunctionIDOrClientID functionIDOrClientID,  
               [in] COR_PRF_ELT_INFO eltInfo);  
```  
  
## <a name="parameters"></a>Parâmetros  

- `functionIDOrClientID`

  \[in] o identificador da função atualmente em execução que está prestes a fazer uma chamada tail.

- `eltInfo`

  \[in] um identificador opaco que representa informações sobre um determinado registro de ativação. Esse identificador é válido somente durante o retorno de chamada ao qual é passado.

## <a name="remarks"></a>Comentários  
 O `FunctionTailcall3WithInfo` método de retorno de chamada notifica o criador de perfil conforme as funções são chamadas e permite que o criador de perfil Use o [método ICorProfilerInfo3:: GetFunctionTailcall3Info](icorprofilerinfo3-getfunctiontailcall3info-method.md) para inspecionar o quadro de pilha. Para acessar as informações do registro de ativação, o `COR_PRF_ENABLE_FRAME_INFO` sinalizador precisa ser definido. O criador de perfil pode usar o [método ICorProfilerInfo:: SetEventMask](icorprofilerinfo-seteventmask-method.md) para definir os sinalizadores de evento e, em seguida, usar o [método ICorProfilerInfo3:: SetEnterLeaveFunctionHooks3WithInfo](icorprofilerinfo3-setenterleavefunctionhooks3withinfo-method.md) para registrar sua implementação dessa função.  
  
 A `FunctionTailcall3WithInfo` função é um retorno de chamada; você deve implementá-la. A implementação deve usar o `__declspec(naked)` atributo de classe de armazenamento.  
  
 O mecanismo de execução não salva nenhum registro antes de chamar essa função.  
  
- Na entrada, você deve salvar todos os registros que usar, incluindo aqueles na FPU (unidade de ponto flutuante).  
  
- Ao sair, você deve restaurar a pilha removendo todos os parâmetros que foram enviados por Push por seu chamador.  
  
 A implementação de `FunctionTailcall3WithInfo` não deve bloquear, pois atrasará a coleta de lixo. A implementação não deve tentar uma coleta de lixo, pois a pilha pode não estar em um estado amigável de coleta de lixo. Se for feita uma tentativa de coleta de lixo, o tempo de execução será bloqueado até o `FunctionTailcall3WithInfo` retorno.  
  
 Além disso, a função FunctionTailcall3WithInfo não deve chamar um código gerenciado ou causar uma alocação de memória gerenciada de alguma forma.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [FunctionEnter3](functionenter3-function.md)
- [FunctionLeave3](functionleave3-function.md)
- [FunctionTailcall3](functiontailcall3-function.md)
- [FunctionEnter3WithInfo](functiontailcall3-function.md)
- [FunctionLeave3WithInfo](functionleave3withinfo-function.md)
- [SetEnterLeaveFunctionHooks3](icorprofilerinfo3-setenterleavefunctionhooks3-method.md)
- [SetEnterLeaveFunctionHooks3WithInfo](icorprofilerinfo3-setenterleavefunctionhooks3withinfo-method.md)
- [SetFunctionIDMapper](icorprofilerinfo-setfunctionidmapper-method.md)
- [SetFunctionIDMapper2](icorprofilerinfo3-setfunctionidmapper2-method.md)
- [Criando perfil de funções estáticas globais](profiling-global-static-functions.md)
