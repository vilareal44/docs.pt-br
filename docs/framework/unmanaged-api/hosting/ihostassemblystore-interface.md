---
title: Interface IHostAssemblyStore
ms.date: 03/30/2017
api_name:
- IHostAssemblyStore
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostAssemblyStore
helpviewer_keywords:
- IHostAssemblyStore interface [.NET Framework hosting]
ms.assetid: cccb650f-abe0-41e2-9fd1-b383788eb1f6
topic_type:
- apiref
ms.openlocfilehash: cca73eec663b9afd12ecea5ab9d7073ea0168d33
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501549"
---
# <a name="ihostassemblystore-interface"></a>Interface IHostAssemblyStore
Fornece métodos que permitem que um host carregue assemblies e módulos independentemente do Common Language Runtime (CLR).  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método ProvideAssembly](ihostassemblystore-provideassembly-method.md)|Obtém uma referência a um assembly que não é referenciado pelo [ICLRAssemblyReferenceList](iclrassemblyreferencelist-interface.md) retornado de uma chamada para [IHostAssemblyManager:: GetNonHostStoreAssemblies](ihostassemblymanager-getnonhoststoreassemblies-method.md).|  
|[Método ProvideModule](ihostassemblystore-providemodule-method.md)|Resolve um módulo dentro de um assembly ou um arquivo de recurso vinculado (não incorporado).|  
  
## <a name="remarks"></a>Comentários  
 `IHostAssemblyStore`fornece uma maneira para um host carregar assemblies com eficiência com base na identidade do assembly. O host carrega assemblies retornando `IStream` instâncias que apontam diretamente nos bytes.  
  
 O CLR determina se um host foi implementado `IHostAssemblyStore` chamando `IHostAssemblyManager::GetNonHostAssemblyStores` na inicialização. Isso permite que o host, por exemplo, controle a associação a assemblies de usuário, mas dependa do tempo de execução para associar a assemblies de .NET Framework.  
  
> [!NOTE]
> No fornecimento de uma implementação do `IHostAssemblyStore` , o host especifica sua intenção de resolver todos os assemblies que não são referenciados pelo `ICLRAssemblyReferenceList` retornado de `IHostAssemblyManager::GetNonHostStoreAssemblies` .  
  
> [!NOTE]
> A versão 2,0 do .NET Framework não fornece uma maneira para o host carregar a imagem nativa de um assembly, conforme fornecido pelo utilitário do [gerador de imagem nativa (NGen. exe)](../../tools/ngen-exe-native-image-generator.md) .  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICLRAssemblyReferenceList](iclrassemblyreferencelist-interface.md)
- [Interface IHostAssemblyManager](ihostassemblymanager-interface.md)
- [Interfaces de hospedagem](hosting-interfaces.md)
