---
title: Enumeração EClrFailure
ms.date: 03/30/2017
api_name:
- EClrFailure
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- EClrFailure
helpviewer_keywords:
- EClrFailure enumeration [.NET Framework hosting]
ms.assetid: 37b95cce-9bfb-4ecf-a00b-33dcba782c67
topic_type:
- apiref
ms.openlocfilehash: fa2b5052a1d569487f0c6c72699ff9ab571beefc
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504388"
---
# <a name="eclrfailure-enumeration"></a>Enumeração EClrFailure
Descreve o conjunto de falhas para as quais um host pode definir ações de política.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef enum {  
    FAIL_NonCriticalResource,  
    FAIL_CriticalResource,  
    FAIL_FatalRuntime,  
    FAIL_OrphanedLock  
    FAIL_StackOverflow  
    FAIL_AccessViolation  
    FAIL_CodeContract  
} EClrFailure;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`FAIL_NonCriticalResource`|Ocorreu uma falha durante uma tentativa de alocar um recurso (como um thread, um bloco de memória ou um bloqueio) em uma região não crítica de código.|  
|`FAIL_CriticalResource`|Ocorreu uma falha durante uma tentativa de alocar um recurso (como um thread, um bloco de memória ou um bloqueio) em uma região crítica de código.|  
|`FAIL_FatalRuntime`|O Common Language Runtime (CLR) não é mais capaz de executar código gerenciado no processo. Daqui em diante, chamadas para qualquer função de hospedagem retornam um valor HRESULT de HOST_E_CLRNOTAVAILABLE.|  
|`FAIL_OrphanedLock`|Um thread não pôde liberar um bloqueio ao retornar de um <xref:System.AppDomain> objeto. O host não pode definir essa falha para fazer com que um thread seja anulado.|  
|`FAIL_StackOverflow`|Ocorreu um estouro de pilha.|  
|`FAIL_AccessViolation`|Foi feita uma tentativa de ler ou gravar na memória protegida. Sem suporte no .NET Framework 4.|  
|`FAIL_CodeContract`|Ocorreu uma falha de contrato de código. Consulte [contratos de código](../../debug-trace-profile/code-contracts.md).|  
  
## <a name="remarks"></a>Comentários  
 Consulte o método [ICLRPolicyManager:: SetActionOnFailure](iclrpolicymanager-setactiononfailure-method.md) para obter uma lista de valores [EPolicyAction](epolicyaction-enumeration.md) que o host pode usar para especificar as ações de política para condições de falha. Para obter mais informações sobre regiões críticas e não críticas de código, consulte [EClrOperation](eclroperation-enumeration.md).  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICLRPolicyManager](iclrpolicymanager-interface.md)
- [Método SetActionOnFailure](iclrpolicymanager-setactiononfailure-method.md)
- [Interface IHostPolicyManager](ihostpolicymanager-interface.md)
- [Hospedando enumerações](hosting-enumerations.md)
