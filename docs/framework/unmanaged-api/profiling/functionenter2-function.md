---
title: Função FunctionEnter2
ms.date: 03/30/2017
api_name:
- FunctionEnter2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- FunctionEnter2
helpviewer_keywords:
- FunctionEnter2 function [.NET Framework profiling]
ms.assetid: ce7a21f9-0ca3-4b92-bc4b-bb803cae3f51
topic_type:
- apiref
ms.openlocfilehash: 8c88e97f8187ac347f4ff39890c8d87ee80c8f9e
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500709"
---
# <a name="functionenter2-function"></a>Função FunctionEnter2
Notifica o criador de perfil que o controle está sendo passado para uma função e fornece informações sobre o quadro de pilha e os argumentos de função. Essa função substitui a função [FunctionEnter](functionenter-function.md) .  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
void __stdcall FunctionEnter2 (  
    [in]  FunctionID                       funcId,
    [in]  UINT_PTR                         clientData,
    [in]  COR_PRF_FRAME_INFO               func,
    [in]  COR_PRF_FUNCTION_ARGUMENT_INFO  *argumentInfo  
);  
```  
  
## <a name="parameters"></a>Parâmetros

- `funcId`

  \[in] o identificador da função à qual o controle é passado.

- `clientData`

  \[in] o identificador da função remapeada, que o criador de perfil especificou anteriormente usando a função [FunctionIDMapper](functionidmapper-function.md) .
  
- `func`

  \[in] um `COR_PRF_FRAME_INFO` valor que aponta para informações sobre o registro de ativação.
  
  O criador de perfil deve tratar isso como um identificador opaco que pode ser passado de volta para o mecanismo de execução no método [ICorProfilerInfo2:: GetFunctionInfo2](icorprofilerinfo2-getfunctioninfo2-method.md) .  
  
- `argumentInfo`

  \[in] um ponteiro para uma estrutura de [COR_PRF_FUNCTION_ARGUMENT_INFO](cor-prf-function-argument-info-structure.md) que especifica os locais na memória dos argumentos da função.

  Para acessar informações de argumento, o `COR_PRF_ENABLE_FUNCTION_ARGS` sinalizador deve ser definido. O criador de perfil pode usar o método [ICorProfilerInfo:: SetEventMask](icorprofilerinfo-seteventmask-method.md) para definir os sinalizadores de evento.

## <a name="remarks"></a>Comentários  
 Os valores dos `func` parâmetros e `argumentInfo` não são válidos após a `FunctionEnter2` função retornar, pois os valores podem ser alterados ou destruídos.  
  
 A `FunctionEnter2` função é um retorno de chamada; você deve implementá-la. A implementação deve usar o `__declspec` `naked` atributo de classe de armazenamento ().  
  
 O mecanismo de execução não salva nenhum registro antes de chamar essa função.  
  
- Na entrada, você deve salvar todos os registros que usar, incluindo aqueles na FPU (unidade de ponto flutuante).  
  
- Ao sair, você deve restaurar a pilha removendo todos os parâmetros que foram enviados por Push por seu chamador.  
  
 A implementação de `FunctionEnter2` não deve bloquear, pois atrasará a coleta de lixo. A implementação não deve tentar uma coleta de lixo porque a pilha pode não estar em um estado amigável de coleta de lixo. Se for feita uma tentativa de coleta de lixo, o tempo de execução será bloqueado até o `FunctionEnter2` retorno.  
  
 Além disso, a `FunctionEnter2` função não deve chamar um código gerenciado ou, de qualquer forma, causar uma alocação de memória gerenciada.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Função FunctionLeave2](functionleave2-function.md)
- [Função FunctionTailcall2](functiontailcall2-function.md)
- [Método SetEnterLeaveFunctionHooks2](icorprofilerinfo2-setenterleavefunctionhooks2-method.md)
- [Criando perfil de funções estáticas globais](profiling-global-static-functions.md)
