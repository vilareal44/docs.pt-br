---
title: Método ICorProfilerCallback::ExceptionSearchFunctionEnter
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.ExceptionSearchFunctionEnter
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::ExceptionSearchFunctionEnter
helpviewer_keywords:
- ExceptionSearchFunctionEnter method [.NET Framework profiling]
- ICorProfilerCallback::ExceptionSearchFunctionEnter method [.NET Framework profiling]
ms.assetid: bfd54573-b7e6-4bd1-a184-7f08a8b39fae
topic_type:
- apiref
ms.openlocfilehash: 244227aadb50720514f7511be563089d520b4bf5
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500189"
---
# <a name="icorprofilercallbackexceptionsearchfunctionenter-method"></a>Método ICorProfilerCallback::ExceptionSearchFunctionEnter
Notifica o criador de perfil de que a fase de pesquisa do tratamento de exceções começou a pesquisar uma função para localizar um manipulador para a exceção atual.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT ExceptionSearchFunctionEnter(  
    [in] FunctionID functionId);  
```  
  
## <a name="parameters"></a>Parâmetros

- `functionId`

  \[in] a ID da função que foi inserida.
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback](icorprofilercallback-interface.md)
- [Método ExceptionSearchFunctionLeave](icorprofilercallback-exceptionsearchfunctionleave-method.md)
