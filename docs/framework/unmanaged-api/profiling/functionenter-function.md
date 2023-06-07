---
title: Função FunctionEnter
ms.date: 03/30/2017
api_name:
- FunctionEnter
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- FunctionEnter
helpviewer_keywords:
- FunctionEnter function [.NET Framework profiling]
ms.assetid: bf4ffa50-4506-4dd4-aa13-a0457b47ca74
topic_type:
- apiref
ms.openlocfilehash: 52870c7446987817ff00b90db26c3265bccdd096
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500722"
---
# <a name="functionenter-function"></a>Função FunctionEnter
Notifica o criador de perfil que o controle está sendo passado para uma função.  
  
> [!NOTE]
> A `FunctionEnter` função é preterida no .NET Framework versão 2,0, e seu uso incorrerá em uma penalidade de desempenho. Em vez disso, use a função [FunctionEnter2](functionenter2-function.md) .  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
void __stdcall FunctionEnter (  
    [in]  FunctionID funcID  
);  
```  
  
## <a name="parameters"></a>Parâmetros

- `funcID`

  \[in] o identificador da função à qual o controle é passado.

## <a name="remarks"></a>Comentários  
 A `FunctionEnter` função é um retorno de chamada; você deve implementá-la. A implementação deve usar o `__declspec` `naked` atributo de classe de armazenamento ().  
  
 O mecanismo de execução não salva nenhum registro antes de chamar essa função.  
  
- Na entrada, você deve salvar todos os registros que usar, incluindo aqueles na FPU (unidade de ponto flutuante).  
  
- Ao sair, você deve restaurar a pilha removendo todos os parâmetros que foram enviados por Push por seu chamador.  
  
 A implementação de `FunctionEnter` não deve bloquear, pois atrasará a coleta de lixo. A implementação não deve tentar uma coleta de lixo porque a pilha pode não estar em um estado amigável de coleta de lixo. Se for feita uma tentativa de coleta de lixo, o tempo de execução será bloqueado até o `FunctionEnter` retorno.  
  
 Além disso, a `FunctionEnter` função não deve chamar um código gerenciado ou, de qualquer forma, causar uma alocação de memória gerenciada.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl  
  
 **Biblioteca:** CorGuids.lib  
  
 **Versões do .NET Framework:** 1,1, 1,0  
  
## <a name="see-also"></a>Confira também

- [Função FunctionEnter2](functionenter2-function.md)
- [Função FunctionLeave2](functionleave2-function.md)
- [Função FunctionTailcall2](functiontailcall2-function.md)
- [Método SetEnterLeaveFunctionHooks2](icorprofilerinfo2-setenterleavefunctionhooks2-method.md)
- [Criando perfil de funções estáticas globais](profiling-global-static-functions.md)
