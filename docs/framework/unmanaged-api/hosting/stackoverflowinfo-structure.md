---
title: Estrutura StackOverflowInfo
ms.date: 03/30/2017
api_name:
- StackOverflowInfo
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- StackOverflowInfo
helpviewer_keywords:
- StackOverflowInfo structure [.NET Framework hosting]
ms.assetid: 519389f2-0217-436c-99d4-93a76ebce5b5
topic_type:
- apiref
ms.openlocfilehash: 941093b9a0856c2b716ba359c854473f3c9ea26a
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84006513"
---
# <a name="stackoverflowinfo-structure"></a>Estrutura StackOverflowInfo
Armazena o tipo de estouro que ocorreu e informações sobre a exceção que foi lançada devido ao estouro.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef struct _StackOverflowInfo {  
    StackOverflowType   soType;  
    EXCEPTION_POINTERS  *pExceptionInfo;  
} StackOverflowInfo;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`soType`|Um valor da enumeração [StackOverflowType](stackoverflowtype-enumeration.md) que especifica o tipo de estouro.|  
|`pExceptionInfo`|Um ponteiro para um `EXCEPTION_POINTERS` objeto Win32, que contém um registro de exceção com uma descrição independente de computador de uma exceção e um registro de contexto com uma descrição dependente de computador do contexto do processador no momento da exceção.|  
  
## <a name="remarks"></a>Comentários  
 Um `StackOverflowInfo` objeto é passado para o método [IActionOnCLREvent:: OnEvent](iactiononclrevent-onevent-method.md) para `Event_StackOverflow` eventos.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. idl  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Estruturas de hospedagem](hosting-structures.md)
