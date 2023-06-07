---
title: Método ICorProfilerInfo2::GetNotifiedExceptionClauseInfo
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo2.GetNotifiedExceptionClauseInfo
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo2::GetNotifiedExceptionClauseInfo
helpviewer_keywords:
- ICorProfilerInfo2::GetNotifiedExceptionCaluseInfo method [.NET Framework profiling]
- GetNotifiedExceptionCaluseInfo method [.NET Framework profiling]
ms.assetid: f9594a7e-cb0c-4c48-accb-29f762aa0c21
topic_type:
- apiref
ms.openlocfilehash: 08337a118a80d213f16e2a16f744b96f5dde2e7f
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84496978"
---
# <a name="icorprofilerinfo2getnotifiedexceptionclauseinfo-method"></a>Método ICorProfilerInfo2::GetNotifiedExceptionClauseInfo
Obtém o endereço nativo e as informações do quadro para a cláusula de exceção ( `catch` / `finally` / `filter` ) que está prestes a ser executada ou que acabou de ser executada.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetNotifiedExceptionClauseInfo(  
    [out] COR_PRF_EX_CLAUSE_INFO *pinfo);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pinfo`  
 fora Um ponteiro para uma estrutura de [COR_PRF_EX_CLAUSE_INFO](cor-prf-ex-clause-info-structure.md) que descreve a instância da cláusula de exceção atual e seu quadro associado.  
  
## <a name="remarks"></a>Comentários  
 Quando uma notificação de exceção é recebida, `GetNotifiedExceptionClauseInfo` pode ser usado para obter as informações de endereço nativo e de quadro para a cláusula de exceção ( `catch` / `finally` / `filter` ) que está prestes a ser executada ([ICorProfilerCallback:: ExceptionCatcherEnter](icorprofilercallback-exceptioncatcherenter-method.md), [ICorProfilerCallback:: ExceptionUnwindFinallyEnter](icorprofilercallback-exceptionunwindfinallyenter-method.md)ou [ICorProfilerCallback:: ExceptionSearchFilterEnter](icorprofilercallback-exceptionsearchfilterenter-method.md) callback é recebida pelo criador de perfil) ou acabou de ser executada ([ICorProfilerCallback:: ExceptionCatcherLeave](icorprofilercallback-exceptioncatcherleave-method.md), [ICorProfilerCallback:: ExceptionUnwindFinallyLeave](icorprofilercallback-exceptionunwindfinallyleave-method.md)ou [ICorProfilerCallback:: ExceptionSearchFilterLeave](icorprofilercallback-exceptionsearchfilterleave-method.md) callback é recebido pelo criador de perfil).  
  
 Essa chamada pode ser feita a qualquer momento após um dos retornos de chamada Enter acima até que o retorno de chamada Leave correspondente seja recebido ou uma exceção aninhada seja gerada na cláusula Current; nesse caso, não há nenhuma notificação de licença para essa cláusula. Observe que não é possível que uma exceção gerada escape uma cláusula de `filter` exceção, portanto, sempre há uma notificação de licença nesse caso.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)
- [Interface ICorProfilerInfo2](icorprofilerinfo2-interface.md)
