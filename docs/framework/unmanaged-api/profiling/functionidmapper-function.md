---
title: Função FunctionIDMapper
ms.date: 03/30/2017
api_name:
- FunctionIDMapper
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- FunctionIDMapper
helpviewer_keywords:
- FunctionIDMapper function [.NET Framework profiling]
ms.assetid: b8205b60-1893-4303-8cff-7ac5a00892aa
topic_type:
- apiref
ms.openlocfilehash: afc818dfe625bfc329ceb1660539eb119702a90d
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500670"
---
# <a name="functionidmapper-function"></a>Função FunctionIDMapper
Notifica o criador de perfil de que o identificador fornecido de uma função pode ser remapeado para uma ID alternativa a ser usada nos retornos de chamada [FunctionEnter2](functionenter2-function.md), [FunctionLeave2](functionleave2-function.md)e [FunctionTailcall2](functiontailcall2-function.md) para essa função. `FunctionIDMapper`também permite que o criador de perfil indique se deseja receber retornos de chamada para essa função.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
UINT_PTR __stdcall FunctionIDMapper (  
    [in]  FunctionID  funcId,
    [out] BOOL       *pbHookFunction  
);  
```  
  
## <a name="parameters"></a>Parâmetros

- `funcId`

  \[no] o identificador de função a ser remapeado.

- `pbHookFunction`

  \[out] um ponteiro para um valor que o criador de perfil define `true` se deseja receber `FunctionEnter2` , `FunctionLeave2` e `FunctionTailcall2` retornos de chamada; caso contrário, ele define esse valor como `false` .

## <a name="return-value"></a>Valor Retornado  
 O criador de perfil retorna um valor que o mecanismo de execução usa como um identificador de função alternativo. O valor de retorno não pode ser nulo, a menos que `false` seja retornado em `pbHookFunction` . Caso contrário, um valor de retorno nulo produzirá resultados imprevisíveis, incluindo possivelmente interromper o processo.  
  
## <a name="remarks"></a>Comentários  
 A `FunctionIDMapper` função é um retorno de chamada. Ele é implementado pelo criador de perfil para remapear uma ID de função para algum outro identificador que seja mais útil para o criador de perfil. O `FunctionIDMapper` retorna a ID alternativa a ser usada para qualquer função especificada. Em seguida, o mecanismo de execução honra a solicitação do criador de perfil, passando essa ID alternativa, além da ID de função tradicional, de volta para o criador de perfil no `clientData` parâmetro de `FunctionEnter2` , `FunctionLeave2` e os `FunctionTailcall2` ganchos, para identificar a função para a qual o gancho está sendo chamado.  
  
 Você pode usar o método [ICorProfilerInfo:: SetFunctionIDMapper](icorprofilerinfo-setfunctionidmapper-method.md) para especificar a implementação da `FunctionIDMapper` função. Você pode chamar o `ICorProfilerInfo::SetFunctionIDMapper` método apenas uma vez e é recomendável fazer isso no retorno de chamada [ICorProfilerCallback:: Initialize](icorprofilercallback-initialize-method.md) .  
  
 Por padrão, supõe-se que um criador de perfil que define o sinalizador de COR_PRF_MONITOR_ENTERLEAVE usando [ICorProfilerInfo:: SetEventMask](icorprofilerinfo-seteventmask-method.md)e que define ganchos via [ICorProfilerInfo:: SetEnterLeaveFunctionHooks](icorprofilerinfo-setenterleavefunctionhooks-method.md) ou [ICorProfilerInfo2:: SetEnterLeaveFunctionHooks2](icorprofilerinfo2-setenterleavefunctionhooks2-method.md), deve receber os `FunctionEnter2` `FunctionLeave2` `FunctionTailcall2` retornos de chamada, e para cada função. No entanto, os profileres podem implementar `FunctionIDMapper` para evitar seletivamente o recebimento desses retornos de chamada para determinadas funções definindo `pbHookFunction` como `false` .  
  
 Os profileres devem ser tolerantes a casos em que vários threads de um aplicativo de perfil criado chamam o mesmo método/função simultaneamente. Nesses casos, o criador de perfil pode receber vários `FunctionIDMapper` retornos de chamada para o mesmo `FunctionID` . O criador de perfil deve ter certeza de retornar os mesmos valores desse retorno de chamada quando ele é chamado várias vezes com o mesmo `FunctionID` .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Método SetFunctionIDMapper](icorprofilerinfo-setfunctionidmapper-method.md)
- [Função FunctionIDMapper2](functionidmapper2-function.md)
- [Função FunctionEnter2](functionenter2-function.md)
- [Função FunctionLeave2](functionleave2-function.md)
- [Função FunctionTailcall2](functiontailcall2-function.md)
- [Criando perfil de funções estáticas globais](profiling-global-static-functions.md)
