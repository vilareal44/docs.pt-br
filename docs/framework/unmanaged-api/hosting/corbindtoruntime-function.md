---
title: Função CorBindToRuntime
ms.date: 03/30/2017
api_name:
- CorBindToRuntime
api_location:
- mscoree.dll
- mscoreei.dll
api_type:
- DLLExport
f1_keywords:
- CorBindToRuntime
helpviewer_keywords:
- CorBindToRuntime function [.NET Framework hosting]
ms.assetid: 799740aa-46ec-4532-95da-6444565b4971
topic_type:
- apiref
ms.openlocfilehash: 52594c36c54c74941371f9950fbc6fb543b86de0
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84493546"
---
# <a name="corbindtoruntime-function"></a>Função CorBindToRuntime
Permite que hosts não gerenciados carreguem o Common Language Runtime (CLR) em um processo.  
  
 Essa função foi preterida no .NET Framework 4.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT CorBindToRuntime (  
    [in]  LPCWSTR     pwszVersion,
    [in]  LPCWSTR     pwszBuildFlavor,
    [in]  REFCLSID    rclsid,
    [in]  REFIID      riid,
    [out] LPVOID FAR  *ppv  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pwszVersion`  
 no Uma cadeia de caracteres que descreve a versão do CLR que você deseja carregar.  
  
 Um número de versão no .NET Framework consiste em quatro partes separadas por pontos: *Major. Minor. Build. Revision*. A cadeia de caracteres passada como `pwszVersion` deve começar com o caractere "v" seguido pelas três primeiras partes do número de versão (por exemplo, "v 1.0.1529").  
  
 Algumas versões do CLR são instaladas com uma declaração de política que especifica a compatibilidade com versões anteriores do CLR. Por padrão, o Shim de inicialização `pwszVersion` é avaliado em relação a instruções de política e carrega a versão mais recente do tempo de execução que é compatível com a versão que está sendo solicitada. Um host pode forçar o Shim a ignorar a avaliação da política e carregar a versão exata especificada no `pwszVersion` passando um valor de `STARTUP_LOADER_SAFEMODE` para o `flags` parâmetro, conforme descrito abaixo.  
  
 Se o chamador especificar NULL para `pwszVersion` , a versão mais recente do tempo de execução será carregada. A passagem de NULL fornece ao host nenhum controle sobre qual versão do tempo de execução está carregada. Embora essa abordagem possa ser apropriada em alguns cenários, é altamente recomendável que o host forneça uma versão específica para carregar.  
  
 `pwszBuildFlavor`  
 no Uma cadeia de caracteres que especifica se o servidor ou a compilação da estação de trabalho do CLR deve ser carregada. Os valores válidos são `svr` e `wks`. A compilação do servidor é otimizada para aproveitar vários processadores para coletas de lixo e a compilação da estação de trabalho é otimizada para aplicativos cliente em execução em um computador com um único processador.  
  
 Se `pwszBuildFlavor` é definido como NULL, a compilação da estação de trabalho é carregada. Durante a execução em um computador com um único processador, a compilação da estação de trabalho é sempre carregada, mesmo se `pwszBuildFlavor` estiver definida como `svr` . No entanto, se `pwszBuildFlavor` for definido como `svr` e a coleta de lixo simultânea for especificada (consulte a descrição do `flags` parâmetro), a compilação do servidor será carregada.  
  
 `rclsid`  
 no O `CLSID` da coclass que implementa a interface [ICorRuntimeHost](icorruntimehost-interface.md) ou [ICLRRuntimeHost](iclrruntimehost-interface.md) . Os valores com suporte são CLSID_CorRuntimeHost ou CLSID_CLRRuntimeHost.  
  
 `riid`  
 no O `IID` da interface solicitada de `rclsid` . Os valores com suporte são IID_ICorRuntimeHost ou IID_ICLRRuntimeHost.  
  
 `ppv`  
 fora O ponteiro de interface retornado para `riid` .  
  
## <a name="remarks"></a>Comentários  
 Se `pwszVersion` o especificar uma versão de tempo de execução que não existe, `CorBindToRuntimeEx` o retornará um valor HRESULT de CLR_E_SHIM_RUNTIMELOAD.  
  
## <a name="execution-context-and-flow-of-windows-identity"></a>Contexto de execução e fluxo de identidade do Windows  
 Na versão 1 do CLR, o <xref:System.Security.Principal.WindowsIdentity> objeto não flui em pontos assíncronos, como novos threads, pools de threads ou retornos de chamada de temporizador. Na versão 2,0 do CLR, um <xref:System.Threading.ExecutionContext> objeto encapsula algumas informações sobre o thread em execução no momento e o flui em qualquer ponto assíncrono, mas não entre os limites do domínio do aplicativo. Da mesma forma, o <xref:System.Security.Principal.WindowsIdentity> objeto também flui em qualquer ponto assíncrono. Portanto, a representação atual no thread, se houver, flui também.  
  
 Você pode alterar o fluxo de duas maneiras:  
  
1. Modificando as <xref:System.Threading.ExecutionContext> configurações para suprimir o fluxo em uma base por thread (consulte os <xref:System.Threading.ExecutionContext.SuppressFlow%2A> <xref:System.Security.SecurityContext.SuppressFlow%2A> métodos, e <xref:System.Security.SecurityContext.SuppressFlowWindowsIdentity%2A> ).  
  
2. Alterando o modo padrão do processo para o modo de compatibilidade da versão 1, em que o <xref:System.Security.Principal.WindowsIdentity> objeto não flui em nenhum ponto assíncrono, independentemente das <xref:System.Threading.ExecutionContext> configurações no thread atual. A maneira como você altera o modo padrão depende se você usa um executável gerenciado ou uma interface de hospedagem não gerenciada para carregar o CLR:  
  
    1. Para executáveis gerenciados, você deve definir o `enabled` atributo do [\<legacyImpersonationPolicy>](../../configure-apps/file-schema/runtime/legacyimpersonationpolicy-element.md) elemento como `true` .  
  
    2. Para interfaces de hospedagem não gerenciadas, defina o `STARTUP_LEGACY_IMPERSONATION` sinalizador no `flags` parâmetro ao chamar a `CorBindToRuntimeEx` função.  
  
     O modo de compatibilidade da versão 1 se aplica a todo o processo e a todos os domínios de aplicativo no processo.  
  
## <a name="remarks"></a>Comentários  
 [CorBindToRuntimeEx](corbindtoruntimeex-function.md) e `CorBindToRuntime` executam a mesma operação, mas a `CorBindToRuntimeEx` função permite que você defina sinalizadores para especificar o comportamento do CLR.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. h  
  
 **Biblioteca:** MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Função CorBindToCurrentRuntime](corbindtocurrentruntime-function.md)
- [Função CorBindToRuntimeByCfg](corbindtoruntimebycfg-function.md)
- [Função CorBindToRuntimeEx](corbindtoruntimeex-function.md)
- [Função CorBindToRuntimeHost](corbindtoruntimehost-function.md)
- [Interface ICorRuntimeHost](icorruntimehost-interface.md)
- [Funções de hospedagem CLR reprovadas](deprecated-clr-hosting-functions.md)
