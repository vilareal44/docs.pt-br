---
title: Estrutura COR_PRF_FUNCTION_ARGUMENT_INFO
ms.date: 03/30/2017
api_name:
- COR_PRF_FUNCTION_ARGUMENT_INFO
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- COR_PRF_FUNCTION_ARGUMENT_INFO
helpviewer_keywords:
- COR_PRF_FUNCTION_ARGUMENT_INFO structure [.NET Framework profiling]
ms.assetid: 07cf3bab-e193-4991-8205-3f41cf2d67b3
topic_type:
- apiref
ms.openlocfilehash: 9fca75ae59b95a226b51768b3e1bfb220d9926f1
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500956"
---
# <a name="cor_prf_function_argument_info-structure"></a>Estrutura COR_PRF_FUNCTION_ARGUMENT_INFO
Representa os argumentos de uma função, em ordem da esquerda para a direita.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
typedef struct _COR_PRF_FUNCTION_ARGUMENT_INFO {  
    ULONG numRanges;  
    ULONG totalArgumentSize;  
    COR_PRF_FUNCTION_ARGUMENT_RANGE ranges[1];  
} COR_PRF_FUNCTION_ARGUMENT_INFO;  
```  
  
## <a name="members"></a>Membros  
  
|Membro|Descrição|  
|------------|-----------------|  
|`numRanges`|O número de blocos de argumentos. Ou seja, esse valor é o número de estruturas de [COR_PRF_FUNCTION_ARGUMENT_RANGE](cor-prf-function-argument-range-structure.md) na `ranges` matriz.|  
|`totalArgumentSize`|O tamanho total de todos os argumentos. Em outras palavras, esse valor é a soma dos comprimentos dos argumentos.|  
|`ranges`|Uma matriz de `COR_PRF_FUNCTION_ARGUMENT_RANGE` estruturas, cada uma representando um bloco de argumentos de função.|  
  
## <a name="remarks"></a>Comentários  
 Uma função pode ter muitos argumentos. Esses argumentos podem não ser armazenados de forma contígua na memória. Você pode ter um bloco de três argumentos em um único lugar, um bloco de dois argumentos em outro lugar e um bloco final de um argumento em um local diferente. Esses argumentos são todos para a mesma função; Eles são armazenados apenas em locais diferentes.  
  
 A `COR_PRF_FUNCTION_ARGUMENT_INFO` estrutura representa todos os argumentos de uma única função. Ele usa uma matriz para fazer referência a todos os blocos de argumentos de função. Portanto, para uma única função, você tem uma única `COR_PRF_FUNCTION_ARGUMENT_INFO` estrutura, que faz referência `COR_PRF_FUNCTION_ARGUMENT_RANGE` a várias estruturas, cada uma delas apontando para um ou mais argumentos de função.  
  
 Os argumentos armazenados em registros são despejados na memória para criar as estruturas.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Estruturas de criação de perfil](profiling-structures.md)
