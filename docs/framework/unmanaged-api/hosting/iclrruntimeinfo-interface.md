---
title: Interface ICLRRuntimeInfo
ms.date: 03/30/2017
api_name:
- ICLRRuntimeInfo
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRRuntimeInfo
helpviewer_keywords:
- ICLRRuntimeInfo interface [.NET Framework hosting]
ms.assetid: 287e5ede-b3a7-4ef8-a756-4fca3f285a82
topic_type:
- apiref
ms.openlocfilehash: 71e2c7f6790f29872c051bb5cea50755068057e9
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504037"
---
# <a name="iclrruntimeinfo-interface"></a>Interface ICLRRuntimeInfo
Fornece métodos que retornam informações sobre um Common Language Runtime específico (CLR), incluindo versão, diretório e status de carregamento. Essa interface também fornece funcionalidade específica de tempo de execução sem inicializar o tempo de execução. Ele inclui o método [LoadLibrary](iclrruntimeinfo-loadlibrary-method.md) relativo ao tempo de execução, o método [GetProcAddress](iclrruntimeinfo-getprocaddress-method.md) específico do módulo de tempo de execução e interfaces fornecidas pelo tempo de execução por meio do método [GetInterface](iclrruntimeinfo-getinterface-method.md) .  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método BindAsLegacyV2Runtime](iclrruntimeinfo-bindaslegacyv2runtime-method.md)|Associa esse tempo de execução para todas as decisões de política de ativação do CLR versão 2 herdadas.|  
|[Método GetDefaultStartupFlags](iclrruntimeinfo-getdefaultstartupflags-method.md)|Obtém os sinalizadores de inicialização do CLR e o arquivo de configuração do host.|  
|[Método GetInterface](iclrruntimeinfo-getinterface-method.md)|Carrega o CLR no processo atual e retorna ponteiros de interface de tempo de execução, como [ICLRRuntimeHost](iclrruntimehost-interface.md), [ICLRStrongName](iclrstrongname-interface.md) e [IMetaDataDispenser](../metadata/imetadatadispenser-interface.md). Esse método substitui todas as `CorBindTo*` funções.|  
|[Método GetProcAddress](iclrruntimeinfo-getprocaddress-method.md)|Obtém o endereço de uma função especificada que foi exportada do CLR associado a esta interface. Esse método substitui o método [GetRealProcAddress](getrealprocaddress-function.md) .|  
|[Método GetRuntimeDirectory](iclrruntimeinfo-getruntimedirectory-method.md)|Obtém o diretório de instalação do CLR associado a esta interface. Esse método substitui o método [GetCORSystemDirectory](getcorsystemdirectory-function.md) .|  
|[Método GetVersionString](iclrruntimeinfo-getversionstring-method.md)|Obtém informações de versão Common Language Runtime (CLR) associadas a uma determinada interface [ICLRRuntimeInfo](iclrruntimeinfo-interface.md) . Esse método substitui os métodos [GetRequestedRuntimeInfo](getrequestedruntimeinfo-function.md) e [GetRequestedRuntimeVersion](getrequestedruntimeversion-function.md) .|  
|[Método IsLoadable](iclrruntimeinfo-isloadable-method.md)|Indica se o tempo de execução associado a essa interface pode ser carregado no processo atual, levando em conta outros tempos de execução que já podem estar carregados no processo.|  
|[Método IsLoaded](iclrruntimeinfo-isloaded-method.md)|Indica se o CLR associado à interface [ICLRRuntimeInfo](iclrruntimeinfo-interface.md) é carregado em um processo.|  
|[Método IsStarted](iclrruntimeinfo-isstarted-method.md)|Indica se o CLR associado à interface [ICLRRuntimeInfo](iclrruntimeinfo-interface.md) foi iniciado.|  
|[Método LoadErrorString](iclrruntimeinfo-loaderrorstring-method.md)|Traduz um valor HRESULT em uma mensagem de erro apropriada para a cultura especificada. Esse método substitui os métodos [LoadStringRC](loadstringrc-function.md) e [LoadStringRCEx](loadstringrcex-function.md) .|  
|[Método LoadLibrary](iclrruntimeinfo-loadlibrary-method.md)|Carrega uma biblioteca do diretório da estrutura do CLR representado por uma interface [ICLRRuntimeInfo](iclrruntimeinfo-interface.md) . Esse método substitui o método [LoadLibraryShim](loadlibraryshim-function.md) .|  
|[Método SetDefaultStartupFlags](iclrruntimeinfo-setdefaultstartupflags-method.md)|Define os sinalizadores de inicialização do CLR e o arquivo de configuração do host.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MetaHost. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interfaces de hospedagem](hosting-interfaces.md)
- [Hosting](index.md)
