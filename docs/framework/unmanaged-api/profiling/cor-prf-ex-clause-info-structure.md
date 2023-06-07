---
title: Estrutura COR_PRF_EX_CLAUSE_INFO
ms.date: 03/30/2017
api_name:
- COR_PRF_EX_CLAUSE_INFO
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- COR_PRF_EX_CLAUSE_INFO
helpviewer_keywords:
- COR_PRF_EX_CLAUSE_INFO structure [.NET Framework profiling]
ms.assetid: 7d0d6fb7-bc9d-40f0-8163-c0d162eaba7d
topic_type:
- apiref
ms.openlocfilehash: 5c764031f709eefe61022d0662f37bc5d3f3e281
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500995"
---
# <a name="cor_prf_ex_clause_info-structure"></a>Estrutura COR_PRF_EX_CLAUSE_INFO
Armazena informações sobre uma instância de cláusula de exceção específica e seu quadro associado.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef struct COR_PRF_EX_CLAUSE_INFO {  
    COR_PRF_CLAUSE_TYPE clauseType;  
    UINT_PTR programCounter;  
    UINT_PTR framePointer;  
    UINT_PTR shadowStackPointer;  
} COR_PRF_EX_CLAUSE_INFO;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`clauseType`|Um valor da enumeração de [COR_PRF_CLAUSE_TYPE](cor-prf-clause-type-enumeration.md) que especifica o tipo de cláusula de exceção que o código acabou de inserir ou à esquerda.|  
|`programCounter`|O ponto de entrada nativo do manipulador de cláusula — por exemplo, o conteúdo do registro de EIP x86.|  
|`framePointer`|O ponteiro para o quadro lógico para o manipulador de cláusula — por exemplo, o conteúdo do Registro EBP x86.|  
|`shadowStackPointer`|O ponteiro para a pilha de sombra. Esse valor é o conteúdo do registro BSP e se aplica somente a IA64.|  
  
## <a name="remarks"></a>Comentários  
 Quando uma notificação de exceção é recebida, [ICorProfilerInfo2:: GetNotifiedExceptionClauseInfo](icorprofilerinfo2-getnotifiedexceptionclauseinfo-method.md) pode ser usado para obter as informações de endereço nativo e quadro da cláusula de exceção ( `catch` / `finally` /Filter) que está prestes a ser executada ou que acabou de ser executada.  
  
 A execução de uma cláusula de exceção envolve esses retornos de chamada do Common Language Runtime (CLR):  
  
- [ICorProfilerCallback::ExceptionCatcherEnter](icorprofilercallback-exceptioncatcherenter-method.md)  
  
- [ICorProfilerCallback::ExceptionUnwindFinallyEnter](icorprofilercallback-exceptionunwindfinallyenter-method.md)  
  
- [ICorProfilerCallback::ExceptionSearchFilterEnter](icorprofilercallback-exceptionsearchfilterenter-method.md)  
  
- [ICorProfilerCallback::ExceptionCatcherLeave](icorprofilercallback-exceptioncatcherleave-method.md)  
  
- [ICorProfilerCallback::ExceptionUnwindFinallyLeave](icorprofilercallback-exceptionunwindfinallyleave-method.md)  
  
- [ICorProfilerCallback::ExceptionSearchFilterLeave](icorprofilercallback-exceptionsearchfilterleave-method.md)  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Estruturas de criação de perfil](profiling-structures.md)
