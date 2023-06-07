---
title: Método ICorProfilerCallback::ExceptionUnwindFinallyLeave
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.ExceptionUnwindFinallyLeave
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::ExceptionUnwindFinallyLeave
helpviewer_keywords:
- ICorProfilerCallback::ExceptionUnwindFinallyLeave method [.NET Framework profiling]
- ExceptionUnwindFinallyLeave method [.NET Framework profiling]
ms.assetid: 2350351e-f253-4c0c-a191-f952bc5700e6
topic_type:
- apiref
ms.openlocfilehash: 8da9098e882dd4b4c1f60e4428ebe68421e629e1
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500137"
---
# <a name="icorprofilercallbackexceptionunwindfinallyleave-method"></a>Método ICorProfilerCallback::ExceptionUnwindFinallyLeave
Notifica o criador de perfil de que a fase de desenrolamento da manipulação de exceção saiu de uma `finally` cláusula.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT ExceptionUnwindFinallyLeave();  
```  
  
## <a name="remarks"></a>Comentários  
 O criador de perfil não deve bloquear durante essa chamada porque a pilha pode não estar em um estado que permita a coleta de lixo e, portanto, a coleta de lixo preemptiva não pode ser habilitada. Se o criador de perfil for bloqueado aqui e uma tentativa de coleta de lixo, o tempo de execução será bloqueado até que esse retorno de chamada seja retornado.  
  
 Além disso, durante essa chamada, o criador de perfil não deve chamar o código gerenciado ou, de qualquer forma, causar uma alocação de memória gerenciada.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerCallback](icorprofilercallback-interface.md)
- [Método ExceptionUnwindFinallyEnter](icorprofilercallback-exceptionunwindfinallyenter-method.md)
