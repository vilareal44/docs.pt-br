---
title: Método IHostAssemblyStore::ProvideAssembly
ms.date: 03/30/2017
api_name:
- IHostAssemblyStore.ProvideAssembly
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostAssemblyStore::ProvideAssembly
helpviewer_keywords:
- ProvideAssembly method [.NET Framework hosting]
- IHostAssemblyStore::ProvideAssembly method [.NET Framework hosting]
ms.assetid: 625c3dd5-a3f0-442c-adde-310dadbb5054
topic_type:
- apiref
ms.openlocfilehash: 162def0d703ea81efc3df3ea5ee08b58e34822e6
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501567"
---
# <a name="ihostassemblystoreprovideassembly-method"></a>Método IHostAssemblyStore::ProvideAssembly
Obtém uma referência a um assembly que não é referenciado pelo [ICLRAssemblyReferenceList](iclrassemblyreferencelist-interface.md) que é retornado de [IHostAssemblyManager:: GetNonHostStoreAssemblies](ihostassemblymanager-getnonhoststoreassemblies-method.md). As chamadas de Common Language Runtime (CLR) `ProvideAssembly` para cada assembly que não aparecem na lista.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT ProvideAssembly (  
    [in]  AssemblyBindInfo *pBindInfo,  
    [out] UINT64           *pAssemblyId,  
    [out] UINT64           *pHostContext,  
    [out] IStream          **ppStmAssemblyImage,  
    [out] IStream          **ppStmPDB  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pBindInfo`  
 no Um ponteiro para uma instância [AssemblyBindInfo](assemblybindinfo-structure.md) que o host usa para determinar determinadas características de ligação, incluindo a presença ou a ausência de qualquer política de controle de versão e a qual assembly associar.  
  
 `pAssemblyId`  
 fora Um ponteiro para um identificador exclusivo para o assembly solicitado para isso `IStream` .  
  
 `pHostContext`  
 fora Um ponteiro para dados específicos de host que é usado para determinar a evidência do assembly solicitado sem a necessidade de uma chamada de invocação de plataforma. `pHostContext`corresponde à <xref:System.Reflection.Assembly.HostContext%2A> propriedade da classe gerenciada <xref:System.Reflection.Assembly> .  
  
 `ppStmAssemblyImage`  
 fora Um ponteiro para o endereço de um `IStream` que contém a imagem executável portátil (PE) a ser carregada, ou NULL se o assembly não foi encontrado.  
  
 `ppStmPDB`  
 fora Um ponteiro para o endereço de um `IStream` que contém as informações de depuração do programa (PDB) ou NULL se não foi possível encontrar o arquivo. pdb.  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|`ProvideAssembly`retornado com êxito.|  
|HOST_E_CLRNOTAVAILABLE|O CLR não foi carregado em um processo ou o CLR está em um estado no qual não pode executar código gerenciado ou processar a chamada com êxito.|  
|HOST_E_TIMEOUT|A chamada atingiu o tempo limite.|  
|HOST_E_NOT_OWNER|O chamador não possui o bloqueio.|  
|HOST_E_ABANDONED|Um evento foi cancelado enquanto um thread ou uma fibra bloqueada estava esperando.|  
|E_FAIL|Ocorreu uma falha catastrófica desconhecida. Quando um método retorna E_FAIL, o CLR não é mais utilizável no processo. As chamadas subsequentes para métodos de hospedagem retornam HOST_E_CLRNOTAVAILABLE.|  
|COR_E_FILENOTFOUND (0x80070002)|Não foi possível localizar o assembly solicitado.|  
|E_NOT_SUFFICIENT_BUFFER|O tamanho do buffer especificado por `pAssemblyId` não é grande o suficiente para conter o identificador que o host deseja retornar.|  
  
## <a name="remarks"></a>Comentários  
 O valor de identidade retornado para `pAssemblyId` é especificado pelo host. Os identificadores devem ser exclusivos dentro do tempo de vida de um processo. O CLR usa esse valor como um identificador exclusivo para o fluxo. Ele verifica cada valor em relação aos valores para `pAssemblyId` retornados por outras chamadas para `ProvideAssembly` . Se o host retornar o mesmo `pAssemblyId` valor para outro `IStream` , o CLR verificará se o conteúdo desse fluxo já foi mapeado. Nesse caso, o tempo de execução carrega a cópia existente da imagem em vez de mapear uma nova.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICLRAssemblyReferenceList](iclrassemblyreferencelist-interface.md)
- [Interface IHostAssemblyManager](ihostassemblymanager-interface.md)
- [Interface IHostAssemblyStore](ihostassemblystore-interface.md)
