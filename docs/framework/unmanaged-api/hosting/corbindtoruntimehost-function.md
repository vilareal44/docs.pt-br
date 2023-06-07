---
title: Função CorBindToRuntimeHost
ms.date: 03/30/2017
api_name:
- CorBindToRuntimeHost
api_location:
- mscoree.dll
api_type:
- DLLExport
f1_keywords:
- CorBindToRuntimeHost
helpviewer_keywords:
- CorBindToRuntimeHost function [.NET Framework hosting]
ms.assetid: 5c826ba3-8258-49bc-a417-78807915fcaf
topic_type:
- apiref
ms.openlocfilehash: 9d1c7f4f5b881f7f55539602c152b557a7950472
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504401"
---
# <a name="corbindtoruntimehost-function"></a>Função CorBindToRuntimeHost
Permite que os hosts carreguem uma versão especificada do Common Language Runtime (CLR) em um processo.  
  
 Essa função foi preterida no .NET Framework 4.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT CorBindToRuntimeHost (  
    [in] LPCWSTR       pwszVersion,
    [in] LPCWSTR       pwszBuildFlavor,
    [in] LPCWSTR       pwszHostConfigFile,
    [in] VOID*         pReserved,
    [in] DWORD         startupFlags,
    [in] REFCLSID      rclsid,
    [in] REFIID        riid,
    [out] LPVOID FAR  *ppv  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `pwszVersion`  
 no Uma cadeia de caracteres que descreve a versão do CLR que você deseja carregar.  
  
 Um número de versão no .NET Framework consiste em quatro partes separadas por pontos: *Major. Minor. Build. Revision*. A cadeia de caracteres passada como `pwszVersion` deve começar com o caractere "v" seguido pelas três primeiras partes do número de versão (por exemplo, "v 1.0.1529").  
  
 Algumas versões do CLR são instaladas com uma declaração de política que especifica a compatibilidade com versões anteriores do CLR. Por padrão, o Shim de inicialização `pwszVersion` é avaliado em relação a instruções de política e carrega a versão mais recente do tempo de execução que é compatível com a versão que está sendo solicitada. Um host pode forçar o Shim a ignorar a avaliação da política e carregar a versão exata especificada no `pwszVersion` passando um valor de STARTUP_LOADER_SAFEMODE para o `startupFlags` parâmetro.  
  
 Se `pwszVersion` for, `null,` o método não carregará nenhuma versão do CLR. Em vez disso, ele retorna CLR_E_SHIM_RUNTIMELOAD, que indica que houve falha ao carregar o tempo de execução.  
  
 `pwszBuildFlavor`  
 no Uma cadeia de caracteres que especifica se o servidor ou a compilação da estação de trabalho do CLR deve ser carregada. Os valores válidos são `svr` e `wks`. A compilação do servidor é otimizada para aproveitar vários processadores para coletas de lixo e a compilação da estação de trabalho é otimizada para aplicativos cliente em execução em um computador com um único processador.  
  
 Se `pwszBuildFlavor` é definido como NULL, a compilação da estação de trabalho é carregada. Durante a execução em um computador com um único processador, a compilação da estação de trabalho é sempre carregada, mesmo se `pwszBuildFlavor` estiver definida como `svr` . No entanto, se `pwszBuildFlavor` for definido como `svr` e a coleta de lixo simultânea for especificada (consulte a descrição do `startupFlags` parâmetro), a compilação do servidor será carregada.  
  
> [!NOTE]
> Não há suporte para a coleta de lixo simultânea em aplicativos que executam o emulador x86 WOW64 em sistemas de 64 bits que implementam a arquitetura Intel Itanium (anteriormente chamada IA-64). Para obter mais informações sobre como usar o WOW64 em sistemas Windows de 64 bits, consulte [executando aplicativos de 32 bits](/windows/desktop/WinProg64/running-32-bit-applications).  
  
 `pwszHostConfigFile`  
 no O nome de um arquivo de configuração de host que especifica a versão do CLR a ser carregada. Se o nome do arquivo não incluir um caminho totalmente qualificado, o arquivo será considerado no mesmo diretório que o executável que está fazendo a chamada.  
  
 `pReserved`  
 no Reservado para extensibilidade futura.  
  
 `startupFlags`  
 no Um conjunto de sinalizadores que controla a coleta de lixo simultânea, o código neutro de domínio e o comportamento do `pwszVersion` parâmetro. O padrão é domínio único se nenhum sinalizador for definido. Para obter uma lista de valores com suporte, consulte a [enumeração STARTUP_FLAGS](startup-flags-enumeration.md).  
  
 `rclsid`  
 no O `CLSID` da coclass que implementa a interface [ICorRuntimeHost](icorruntimehost-interface.md) ou [ICLRRuntimeHost](iclrruntimehost-interface.md) . Os valores com suporte são CLSID_CorRuntimeHost ou CLSID_CLRRuntimeHost.  
  
 `riid`  
 no O `IID` da interface que você está solicitando. Os valores com suporte são IID_ICorRuntimeHost ou IID_ICLRRuntimeHost.  
  
 `ppv`  
 fora Um ponteiro de interface para a versão do tempo de execução que foi carregado.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** MSCorEE. idl  
  
 **Biblioteca:** MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Função CorBindToCurrentRuntime](corbindtocurrentruntime-function.md)
- [Função CorBindToRuntime](corbindtoruntime-function.md)
- [Função CorBindToRuntimeByCfg](corbindtoruntimebycfg-function.md)
- [Função CorBindToRuntimeEx](corbindtoruntimeex-function.md)
- [Interface ICorRuntimeHost](icorruntimehost-interface.md)
- [Funções de hospedagem CLR reprovadas](deprecated-clr-hosting-functions.md)
