---
title: Método ICorProfilerCallback::JITInlining
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.JITInlining
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::JITInlining
helpviewer_keywords:
- JITInlining method [.NET Framework profiling]
- ICorProfilerCallback::JITInlining method [.NET Framework profiling]
ms.assetid: c2f45801-dd38-4b78-b6b7-64397dc73f83
topic_type:
- apiref
ms.openlocfilehash: 13ce44c9c7a04b870278bc8926dd9fe5daf10bc3
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500007"
---
# <a name="icorprofilercallbackjitinlining-method"></a>Método ICorProfilerCallback::JITInlining
Notifica o criador de perfil de que o compilador JIT (just-in-time) está prestes a inserir uma função em linha com outra função.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT JITInlining(  
    [in]  FunctionID callerId,  
    [in]  FunctionID calleeId,  
    [out] BOOL      *pfShouldInline);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `callerId`  
 no A ID da função na qual a `calleeId` função será inserida.  
  
 `calleeId`  
 no A ID da função a ser inserida.  
  
 `pfShouldInline`  
 [fora] `true` para permitir que a inserção ocorra; caso contrário, `false` .  
  
## <a name="remarks"></a>Comentários  
 O criador de perfil pode definir `pfShouldInline` como `false` para impedir que a `calleeId` função seja inserida na `callerId` função. Além disso, o criador de perfil pode desabilitar a inserção embutida globalmente usando o valor COR_PRF_DISABLE_INLINING da enumeração [COR_PRF_MONITOR](cor-prf-monitor-enumeration.md) .  
  
 Funções inseridas embutidas não geram eventos para entrar ou sair. Portanto, o criador de perfil deve definir `pfShouldInline` como para `false` produzir um chamadas preciso. `pfShouldInline`A configuração para `false` afetará o desempenho, pois a inserção embutida normalmente aumenta a velocidade e reduz o número de eventos de compilação JIT separados para o método inserido.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback](icorprofilercallback-interface.md)
