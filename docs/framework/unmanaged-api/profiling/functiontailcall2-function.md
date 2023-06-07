---
title: Função FunctionTailcall2
ms.date: 03/30/2017
api_name:
- FunctionTailcall2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- FunctionTailcall2
helpviewer_keywords:
- FunctionTailcall2 function [.NET Framework profiling]
ms.assetid: 249f9892-b5a9-41e1-b329-28a925904df6
topic_type:
- apiref
ms.openlocfilehash: cb7e21e0c6aad5ebb328ae5d1a993716f96e8d47
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500566"
---
# <a name="functiontailcall2-function"></a>Função FunctionTailcall2
Notifica o criador de perfil de que a função atualmente em execução está prestes a executar uma chamada tail para outra função e fornece informações sobre o registro de ativação.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp
void __stdcall FunctionTailcall2 (  
    [in] FunctionID         funcId,
    [in] UINT_PTR           clientData,
    [in] COR_PRF_FRAME_INFO func  
);  
```  
  
## <a name="parameters"></a>Parâmetros

- `funcId`

  \[in] o identificador da função atualmente em execução que está prestes a fazer uma chamada tail.

- `clientData`

  \[in] o identificador da função remapeada, que o criador de perfil especificou anteriormente via [FunctionIDMapper](functionidmapper-function.md), da função atualmente em execução que está prestes a fazer uma chamada final.
  
- `func`

  \[in] um `COR_PRF_FRAME_INFO` valor que aponta para informações sobre o registro de ativação.

  O criador de perfil deve tratar isso como um identificador opaco que pode ser passado de volta para o mecanismo de execução no método [ICorProfilerInfo2:: GetFunctionInfo2](icorprofilerinfo2-getfunctioninfo2-method.md) .

## <a name="remarks"></a>Comentários  
 A função de destino da chamada tail usará o quadro de pilhas atual e retornará diretamente para o chamador da função que fez a chamada final. Isso significa que um retorno de chamada [FunctionLeave2](functionleave2-function.md) não será emitido para uma função que seja o destino de uma chamada tail.  
  
 O valor do `func` parâmetro não é válido depois que a `FunctionTailcall2` função retorna, pois o valor pode ser alterado ou destruído.  
  
 A `FunctionTailcall2` função é um retorno de chamada; você deve implementá-la. A implementação deve usar o `__declspec` `naked` atributo de classe de armazenamento ().  
  
 O mecanismo de execução não salva nenhum registro antes de chamar essa função.  
  
- Na entrada, você deve salvar todos os registros que usar, incluindo aqueles na FPU (unidade de ponto flutuante).  
  
- Ao sair, você deve restaurar a pilha removendo todos os parâmetros que foram enviados por Push por seu chamador.  
  
 A implementação de `FunctionTailcall2` não deve bloquear, pois atrasará a coleta de lixo. A implementação não deve tentar uma coleta de lixo porque a pilha pode não estar em um estado amigável de coleta de lixo. Se for feita uma tentativa de coleta de lixo, o tempo de execução será bloqueado até o `FunctionTailcall2` retorno.  
  
 Além disso, a `FunctionTailcall2` função não deve chamar um código gerenciado ou, de qualquer forma, causar uma alocação de memória gerenciada.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Função FunctionEnter2](functionenter2-function.md)
- [Função FunctionLeave2](functionleave2-function.md)
- [Método SetEnterLeaveFunctionHooks2](icorprofilerinfo2-setenterleavefunctionhooks2-method.md)
- [Criando perfil de funções estáticas globais](profiling-global-static-functions.md)
