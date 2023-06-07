---
title: Função CorBindToRuntimeByCfg
ms.date: 03/30/2017
api_name:
- CorBindToRuntimeByCfg
api_location:
- mscoree.dll
api_type:
- DLLExport
f1_keywords:
- CorBindToRuntimeByCfg
helpviewer_keywords:
- CorBindToRuntimeByCfg function [.NET Framework hosting]
ms.assetid: ded1e492-a782-4185-9c66-709e421c1782
topic_type:
- apiref
ms.openlocfilehash: 8b7dcdcc6d9d0106af1bb83ee591cff76239b416
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504427"
---
# <a name="corbindtoruntimebycfg-function"></a>Função CorBindToRuntimeByCfg
Carrega o Common Language Runtime (CLR) em um processo usando informações de versão lidas de um arquivo XML.  
  
 Essa função foi preterida no .NET Framework 4.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT CorBindToRuntimeByCfg (  
    [in]  IStream     *pCfgStream,  
    [in]  DWORD        reserved,  
    [in]  DWORD        startupFlags,  
    [in]  REFCLSID     rclsid,  
    [in]  REFIID       riid,
    [out] LPVOID FAR*  ppv  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pCfgStream`  
 no Um ponteiro para um `IStream` objeto que lê o arquivo XML.  
  
 `reserved`  
 no Reservado para uso futuro. Use 0 (zero) como valor.  
  
 `startupFlags`  
 no Um valor da enumeração de [STARTUP_FLAGS](startup-flags-enumeration.md) que especifica o comportamento de inicialização do CLR.  
  
 `rclsid`  
 no O `CLSID` da coclass que implementa a interface [ICorRuntimeHost](icorruntimehost-interface.md) ou [ICLRRuntimeHost](iclrruntimehost-interface.md) . Os valores com suporte são CLSID_CorRuntimeHost ou CLSID_CLRRuntimeHost.  
  
 `riid`  
 no O `IID` de ou a `ICorRuntimeHost` `ICLRRuntimeHost` interface. Os valores com suporte são IID_ICorRuntimeHost ou IID_ICLRRuntimeHost.  
  
 `ppv`  
 fora Um ponteiro para o endereço da interface retornada.  
  
## <a name="remarks"></a>Comentários  
 O formato do arquivo XML é modelado após o arquivo de configuração de aplicativo padrão. Para obter mais informações sobre arquivos XML, consulte [esquema do arquivo de configuração](../../configure-apps/file-schema/index.md).  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Função CorBindToCurrentRuntime](corbindtocurrentruntime-function.md)
- [Função CorBindToRuntime](corbindtoruntime-function.md)
- [Função CorBindToRuntimeEx](corbindtoruntimeex-function.md)
- [Função CorBindToRuntimeHost](corbindtoruntimehost-function.md)
- [Interface ICorRuntimeHost](icorruntimehost-interface.md)
- [Funções de hospedagem CLR reprovadas](deprecated-clr-hosting-functions.md)
