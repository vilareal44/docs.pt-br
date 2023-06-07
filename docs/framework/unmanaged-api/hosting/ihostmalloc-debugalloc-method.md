---
title: Método IHostMAlloc::DebugAlloc
ms.date: 03/30/2017
api_name:
- IHostMAlloc.DebugAlloc
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostMAlloc::DebugAlloc
helpviewer_keywords:
- DebugAlloc method [.NET Framework hosting]
- IHostMAlloc::DebugAlloc method [.NET Framework hosting]
ms.assetid: 0bfbc527-bea2-43ce-b041-69186f4440dd
topic_type:
- apiref
ms.openlocfilehash: 3f85e7c7fd54079ddce37f739a3a7bc0fa830d31
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84493286"
---
# <a name="ihostmallocdebugalloc-method"></a>Método IHostMAlloc::DebugAlloc
Solicita que o host aloque a quantidade especificada de memória do heap, além de controlar onde a memória foi alocada.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT DebugAlloc (  
    [in]  SIZE_T  cbSize,
    [in]  EMemoryCriticalLevel dwCriticalLevel,
    [in]  char*   pszFileName,
    [in]  int     iLineNo,
    [out] void**  ppMem  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `cbSize`  
 no O tamanho, em bytes, da solicitação de alocação de memória atual.  
  
 `dwCriticalLevel`  
 no Um dos valores de [EMemoryCriticalLevel](ememorycriticallevel-enumeration.md) , indicando o impacto de uma falha de alocação.  
  
 `pszFileName`  
 no O arquivo de código do executável que está sendo depurado.  
  
 `iLineNo`  
 no O número de linha em `pszFileName` que a alocação foi solicitada.  
  
 `ppMem`  
 fora Um ponteiro para a memória alocada ou NULL se a solicitação não puder ser concluída.  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|`DebugAlloc`retornado com êxito.|  
|HOST_E_CLRNOTAVAILABLE|O CLR não foi carregado em um processo ou o CLR está em um estado no qual não pode executar código gerenciado ou processar a chamada com êxito.|  
|HOST_E_TIMEOUT|A chamada atingiu o tempo limite.|  
|HOST_E_NOT_OWNER|O chamador não possui o bloqueio.|  
|HOST_E_ABANDONED|Um evento foi cancelado enquanto um thread ou uma fibra bloqueada estava esperando.|  
|E_FAIL|Ocorreu uma falha catastrófica desconhecida. Quando um método retorna E_FAIL, o CLR não é mais utilizável no processo. As chamadas subsequentes para métodos de hospedagem retornam HOST_E_CLRNOTAVAILABLE.|  
|E_OUTOFMEMORY|Não havia memória suficiente disponível para concluir a solicitação de alocação.|  
  
## <a name="remarks"></a>Comentários  
 O CLR Obtém um ponteiro de interface para uma instância [IHostMAlloc](ihostmalloc-interface.md) chamando o método [IHostMemoryManager:: CreateMAlloc](ihostmemorymanager-createmalloc-method.md) . `DebugAlloc`permite que o tempo de execução Obtenha informações de arquivo de código para uso durante a depuração.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IHostMemoryManager](ihostmemorymanager-interface.md)
- [Interface IHostMalloc](ihostmalloc-interface.md)
