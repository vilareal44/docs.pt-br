---
title: Função StackSnapshotCallback
ms.date: 03/30/2017
api_name:
- StackSnapshotCallback
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- StackSnapshotCallback
helpviewer_keywords:
- StackSnapshotCallback function [.NET Framework profiling]
ms.assetid: d0f235b2-91fe-4f82-b7d5-e5c64186eea8
topic_type:
- apiref
ms.openlocfilehash: a0f5316900dedc6c8983f9e670f60767ed783a40
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84493975"
---
# <a name="stacksnapshotcallback-function"></a>Função StackSnapshotCallback
Fornece ao criador de perfil informações sobre cada quadro gerenciado e cada execução de quadros não gerenciados na pilha durante uma movimentação de pilha, que é iniciada pelo método [ICorProfilerInfo2::D ostacksnapshot](icorprofilerinfo2-dostacksnapshot-method.md) .  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT __stdcall StackSnapshotCallback (  
    [in] FunctionID funcId,  
    [in] UINT_PTR ip,  
    [in] COR_PRF_FRAME_INFO frameInfo,  
    [in] ULONG32 contextSize,  
    [in] BYTE context[],  
    [in] void *clientData  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `funcId`  
 no Se esse valor for zero, esse retorno de chamada será para uma execução de quadros não gerenciados; caso contrário, é o identificador de uma função gerenciada e esse retorno de chamada é para um quadro gerenciado.  
  
 `ip`  
 no O valor do ponteiro de instrução de código nativo no quadro.  
  
 `frameInfo`  
 no Um `COR_PRF_FRAME_INFO` valor que faz referência a informações sobre o registro de ativação. Esse valor é válido para uso somente durante este retorno de chamada.  
  
 `contextSize`  
 no O tamanho da `CONTEXT` estrutura, que é referenciada pelo `context` parâmetro.  
  
 `context`  
 no Um ponteiro para uma `CONTEXT` estrutura Win32 que representa o estado da CPU para esse quadro.  
  
 O `context` parâmetro será válido somente se o sinalizador de COR_PRF_SNAPSHOT_CONTEXT foi passado `ICorProfilerInfo2::DoStackSnapshot` .  
  
 `clientData`  
 no Um ponteiro para os dados do cliente, que são passados diretamente por meio de `ICorProfilerInfo2::DoStackSnapshot` .  
  
## <a name="remarks"></a>Comentários  
 A `StackSnapshotCallback` função é implementada pelo gravador do criador de perfil. Você deve limitar a complexidade do trabalho feito no `StackSnapshotCallback` . Por exemplo, ao usar `ICorProfilerInfo2::DoStackSnapshot` de forma assíncrona, o thread de destino pode estar mantendo bloqueios. Se o código dentro `StackSnapshotCallback` exigir os mesmos bloqueios, um deadlock poderia acontecer.  
  
 O `ICorProfilerInfo2::DoStackSnapshot` método chama a `StackSnapshotCallback` função uma vez por quadro gerenciado ou uma vez por execução de quadros não gerenciados. Se `StackSnapshotCallback` for chamado para uma execução de quadros não gerenciados, o criador de perfil poderá usar o contexto de registro (referenciado pelo `context` parâmetro) para executar sua própria movimentação de pilha não gerenciada. Nesse caso, a estrutura do Win32 `CONTEXT` representa o estado da CPU para o quadro enviado mais recentemente dentro da execução de quadros não gerenciados. Embora a `CONTEXT` estrutura Win32 inclua valores para todos os registros, você deve confiar apenas nos valores do registro de ponteiro de pilha, registro de ponteiro de quadro, registro de ponteiro de instrução e os registros inteiros não volátil (ou seja, preservados).  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Método DoStackSnapshot](icorprofilerinfo2-dostacksnapshot-method.md)
- [Criando perfil de funções estáticas globais](profiling-global-static-functions.md)
