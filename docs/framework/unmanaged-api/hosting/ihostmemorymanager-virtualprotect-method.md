---
title: Método IHostMemoryManager::VirtualProtect
ms.date: 03/30/2017
api_name:
- IHostMemoryManager.VirtualProtect
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostMemoryManager::VirtualProtect
helpviewer_keywords:
- IHostMemoryManager::VirtualProtect method [.NET Framework hosting]
- VirtualProtect method [.NET Framework hosting]
ms.assetid: 13be0299-df0d-4951-aabf-0676a30b385f
topic_type:
- apiref
ms.openlocfilehash: 473a52b55f793abc76883b0a5cd5b2a04756d9f7
ms.sourcegitcommit: d223616e7e6fe2139079052e6fcbe25413fb9900
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83804362"
---
# <a name="ihostmemorymanagervirtualprotect-method"></a>Método IHostMemoryManager::VirtualProtect
Serve como um wrapper lógico para a função Win32 correspondente. A implementação do Win32 de `VirtualProtect` altera a proteção em uma região de páginas confirmadas no espaço de endereço virtual do processo de chamada.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT VirtualProtect (  
    [in]  void*   lpAddress,  
    [in]  SIZE_T  dwSize,  
    [in]  DWORD   flNewProtect,  
    [out] DWORD*  pflOldProtect  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `lpAddress`  
 no Um ponteiro para o endereço base da memória virtual cujos atributos de proteção devem ser alterados.  
  
 `dwSize`  
 no O tamanho, em bytes, da região das páginas de memória a ser alterada.  
  
 `flNewProtect`  
 no O tipo de proteção de memória a ser aplicado.  
  
 `pflOldProtect`  
 fora Um ponteiro para o valor de proteção de memória anterior.  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|`VirtualProtect`retornado com êxito.|  
|HOST_E_CLRNOTAVAILABLE|O Common Language Runtime (CLR) não foi carregado em um processo ou o CLR está em um estado no qual não pode executar código gerenciado ou processar a chamada com êxito.|  
|HOST_E_TIMEOUT|A chamada atingiu o tempo limite.|  
|HOST_E_NOT_OWNER|O chamador não possui o bloqueio.|  
|HOST_E_ABANDONED|Um evento foi cancelado enquanto um thread ou uma fibra bloqueada estava esperando.|  
|E_FAIL|Ocorreu uma falha catastrófica desconhecida. Quando um método retorna E_FAIL, o CLR não é mais utilizável no processo. As chamadas subsequentes para métodos de hospedagem retornam HOST_E_CLRNOTAVAILABLE.|  
  
## <a name="remarks"></a>Comentários  
 Essa implementação de `VirtualProtect` retorna um valor HRESULT, enquanto a implementação do Win32 retorna um valor diferente de zero para indicar êxito e um valor zero para indicar falha. Para obter mais informações, consulte a documentação da plataforma Windows.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IHostMemoryManager](ihostmemorymanager-interface.md)
