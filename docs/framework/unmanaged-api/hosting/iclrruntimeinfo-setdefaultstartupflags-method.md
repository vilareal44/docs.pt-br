---
title: Método ICLRRuntimeInfo::SetDefaultStartupFlags
ms.date: 03/30/2017
api_name:
- ICLRRuntimeInfo.SetDefaultStartupFlags
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRRuntimeInfo::SetDefaultStartupFlags
helpviewer_keywords:
- ICLRRuntimeInfo::SetDefaultStartupFlags method [.NET Framework hosting]
- SetDefaultStartupFlags method [.NET Framework hosting]
ms.assetid: 98ae174f-bff0-48f1-9e05-6cb63b451824
topic_type:
- apiref
ms.openlocfilehash: aa02d42511a863434fef236f90afae2c5417a78d
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504011"
---
# <a name="iclrruntimeinfosetdefaultstartupflags-method"></a>Método ICLRRuntimeInfo::SetDefaultStartupFlags
Define os sinalizadores de inicialização e o arquivo de configuração do host que será usado para iniciar o tempo de execução. Esse método substitui o uso do `startupFlags` parâmetro nas funções [CorBindToRuntimeEx](corbindtoruntimeex-function.md) e [CorBindToRuntimeHost](corbindtoruntimehost-function.md) .  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT SetDefaultStartupFlags(  
           [in]  DWORD dwStartupFlags,  
           [in]  LPCWSTR pwzHostConfigFile);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `dwStartupFlags`  
 no Os sinalizadores de inicialização do host a serem definidos. Use os mesmos sinalizadores que as funções [CorBindToRuntimeEx](corbindtoruntimeex-function.md) e [CorBindToRuntimeHost](corbindtoruntimehost-function.md) .  
  
 `pwzHostConfigFile`  
 no O caminho do diretório do arquivo de configuração do host a ser definido.  
  
## <a name="return-value"></a>Valor Retornado  
 Esse método retorna o HRESULT específico a seguir, bem como erros HRESULT que indicam falha de método.  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|S_OK|O método foi concluído com êxito.|  
  
## <a name="remarks"></a>Comentários  
 Um host multi-threaded deve sincronizar chamadas para esse método. Caso contrário, o thread A pode chamar o `SetStartupFlags` método depois que o thread b concluir uma chamada para `SetStartupFlags` e antes do thread b iniciar o tempo de execução.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MetaHost. h  
  
 **Biblioteca:** Incluído como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface ICLRRuntimeInfo](iclrruntimeinfo-interface.md)
- [Interfaces de hospedagem](hosting-interfaces.md)
- [Hosting](index.md)
