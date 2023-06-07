---
title: Funções de hospedagem CLR reprovadas
ms.date: 03/30/2017
helpviewer_keywords:
- .NET Framework 1.1, hosting global static functions
- global static functions [.NET Framework hosting], version 2.0
- .NET Framework 2.0, hosting global static functions
- hosting global static functions [.NET Framework], version 2.0
ms.assetid: 91fbbb35-e543-4814-b806-371cebae8c5a
ms.openlocfilehash: 083d0ff285abb4a99ad05c791bc504ff7f282c6a
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504362"
---
# <a name="deprecated-clr-hosting-functions"></a>Funções de hospedagem CLR reprovadas
Esta seção descreve as funções estáticas globais não gerenciadas que as versões anteriores da API de hospedagem usavam.  
  
 Com exceção das funções de infraestrutura ( `_Cor*` funções), que são usadas somente pelo .NET Framework, essas funções foram preteridas no .NET Framework 4.  
  
## <a name="activation-functions"></a>Funções de ativação  
 [Função ClrCreateManagedInstance](clrcreatemanagedinstance-function.md)  
 Preterido. Cria uma instância do tipo gerenciado especificado.  
  
 [Função CoInitializeCor](coinitializecor-function.md)  
 Obsoleto. Para inicializar o Common Language Runtime (CLR), use [CorBindToRuntimeEx](corbindtoruntimeex-function.md) ou [CorBindToCurrentRuntime](corbindtocurrentruntime-function.md).  
  
 [Função CoInitializeEE](coinitializeee-function.md)  
 Preterido. Garante que o mecanismo de execução do CLR seja carregado em um processo. Em vez disso, use o método [ICLRRuntimeHost:: Start](iclrruntimehost-start-method.md) .  
  
 [Função CorBindToCurrentRuntime](corbindtocurrentruntime-function.md)  
 Preterido. Carrega o Common Language Runtime (CLR) em um processo usando as informações de versão armazenadas em um arquivo XML.  
  
 [Função CorBindToRuntime](corbindtoruntime-function.md)  
 Preterido. Permite que hosts não gerenciados carreguem o CLR em um processo.  
  
 [Função CorBindToRuntimeByCfg](corbindtoruntimebycfg-function.md)  
 Preterido. Carrega o CLR em um processo usando informações de versão lidas de um arquivo XML.  
  
 [Função CorBindToRuntimeEx](corbindtoruntimeex-function.md)  
 Preterido. Permite que hosts não gerenciados carreguem o CLR em um processo e permite que você defina sinalizadores para especificar o comportamento do CLR.  
  
 [Função CorBindToRuntimeHost](corbindtoruntimehost-function.md)  
 Preterido. Permite que os hosts carreguem uma versão especificada do CLR em um processo.  
  
 [Função GetCORRequiredVersion](getcorrequiredversion-function.md)  
 Preterido. Obtém o número de versão do CLR necessário.  
  
 [Função GetCORSystemDirectory](getcorsystemdirectory-function.md)  
 Preterido. Retorna o diretório de instalação do CLR que é carregado no processo.  
  
 [Função GetRealProcAddress](getrealprocaddress-function.md)  
 Preterido. Obtém o endereço da função especificada que é exportada da última versão instalada do CLR.  
  
 [Função GetRequestedRuntimeInfo](getrequestedruntimeinfo-function.md)  
 Preterido. Obtém informações de versão e diretório sobre o CLR solicitado por um aplicativo.  
  
## <a name="clr-version-functions"></a>Funções de versão do CLR  
 As funções nesta seção retornam uma versão do CLR; Eles não ativam o CLR.  
  
 [Função GetCORVersion](getcorversion-function.md)  
 Preterido. Retorna o número de versão do CLR que está sendo executado no processo atual.  
  
 [Função GetFileVersion](getfileversion-function.md)  
 Preterido. Obtém as informações de versão do CLR do arquivo especificado, usando o buffer especificado.  
  
 [Função GetRequestedRuntimeVersion](getrequestedruntimeversion-function.md)  
 Preterido. Obtém o número de versão do CLR solicitado pelo aplicativo especificado. Se essa versão não estiver instalada, o obterá a versão mais recente instalada antes da versão solicitada.  
  
 [Função GetRequestedRuntimeVersionForCLSID](getrequestedruntimeversionforclsid-function.md)  
 Preterido. Obtém as informações de versão do CLR apropriadas para a classe com o CLSID especificado.  
  
 [Função GetVersionFromProcess](getversionfromprocess-function.md)  
 Preterido. Obtém o número de versão do CLR associado ao identificador de processo especificado.  
  
 [Função LockClrVersion](lockclrversion-function.md)  
 Preterido. Permite que o host determine qual versão do CLR será usada dentro do processo antes de inicializar explicitamente o CLR.  
  
## <a name="hosting-functions"></a>Funções de hospedagem  
 [Função CallFunctionShim](callfunctionshim-function.md)  
 Preterido. Faz uma chamada para a função que tem o nome e os parâmetros especificados na biblioteca especificada.  
  
 [Função CoEEShutDownCOM](coeeshutdowncom-function.md)  
 Preterido. Descarrega um assembly COM do processo.  
  
 [Função CorExitProcess](corexitprocess-function.md)  
 Preterido. Desliga o processo atual não gerenciado.  
  
 [Função CorLaunchApplication](corlaunchapplication-function.md)  
 Preterido. Inicia o aplicativo no caminho de rede especificado, usando os manifestos especificados e outros dados de aplicativo.  
  
 [Função CorMarkThreadInThreadPool](cormarkthreadinthreadpool-function.md)  
 Preterido. Marca o thread do pool de threads em execução no momento para a execução do código gerenciado. A partir do .NET Framework versão 2,0, essa função não tem nenhum efeito. Ela não é necessária e pode ser removida do seu código.  
  
 [Função CoUninitializeCor](couninitializecor-function.md)  
 Obsoleto. O CLR não pode ser descarregado de um processo.  
  
 [Função CoUninitializeEE](couninitializeee-function.md)  
 Obsoleto.  
  
 [Função CreateDebuggingInterfaceFromVersion](createdebugginginterfacefromversion-function.md)  
 Preterido. Cria um objeto [ICorDebug](../debugging/icordebug-interface.md) com base nas informações de versão especificadas.  
  
 [Função CreateICeeFileGen](createiceefilegen-function.md)  
 Preterido. Cria um objeto [ICeeFileGen](iceefilegen-class.md) .  
  
 [Função DestroyICeeFileGen](destroyiceefilegen-function.md)  
 Preterido. Destrói um objeto [ICeeFileGen](iceefilegen-class.md) .  
  
 [Ponteiro de função FExecuteInAppDomainCallback](fexecuteinappdomaincallback-function-pointer.md)  
 Preterido. Aponta para uma função que o CLR chama para executar código gerenciado.  
  
 [Ponteiro de função FLockClrVersionCallback](flockclrversioncallback-function-pointer.md)  
 Preterido. Aponta para uma função que o CLR chama para notificar o host de que a inicialização foi iniciada ou concluída.  
  
 [Função GetCLRIdentityManager](getclridentitymanager-function.md)  
 Preterido. Obtém um ponteiro para uma interface que permite que o CLR gerencie identidades.  
  
 [Função LoadLibraryShim](loadlibraryshim-function.md)  
 Preterido. Carrega uma versão especificada de uma .NET Framework DLL.  
  
 [Função LoadStringRC](loadstringrc-function.md)  
 Preterido. Traduz um valor HRESULT em uma mensagem de erro usando a cultura padrão do thread atual.  
  
 [Função LoadStringRCEx](loadstringrcex-function.md)  
 Preterido. Traduz um valor HRESULT para uma mensagem de erro apropriada para a cultura especificada.  
  
 [Ponteiro de função LPOVERLAPPED_COMPLETION_ROUTINE](lpoverlapped-completion-routine-function-pointer.md)  
 Preterido. Aponta para uma função que notifica o host quando uma e/s sobreposta (ou seja, assíncrona) para um dispositivo for concluída.  
  
 [Ponteiro de função LPTHREAD_START_ROUTINE](lpthread-start-routine-function-pointer.md)  
 Preterido. Aponta para uma função que notifica o host que um thread começou a ser executado.  
  
 [Função RunDll32ShimW](rundll32shimw-function.md)  
 Preterido. Executa o comando especificado.  
  
 [Ponteiro de função WAITORTIMERCALLBACK](waitortimercallback-function-pointer.md)  
 Preterido. Aponta para uma função que notifica o host de que um identificador de espera foi sinalizado ou atingiu o tempo limite.  
  
## <a name="infrastructure-functions"></a>Funções de infraestrutura  
 As funções nesta seção são para uso apenas pelo .NET Framework.  
  
 [Função _CorDllMain](cordllmain-function.md)  
 Inicializa o CLR, localiza o ponto de entrada gerenciado no cabeçalho CLR do assembly de DLL e começa a execução.  
  
 [Função _CorExeMain](corexemain-function.md)  
 Inicializa o CLR, localiza o ponto de entrada gerenciado no cabeçalho CLR do assembly executável e começa a execução.  
  
 [Função _CorExeMain2](corexemain2-function.md)  
 Executa o ponto de entrada no código mapeado de memória especificado. Essa função é chamada pelo carregador do sistema operacional.  
  
 [Função _CorImageUnloading](corimageunloading-function.md)  
 Notifica o carregador quando as imagens do módulo gerenciado são descarregadas.  
  
 [Função _CorValidateImage](corvalidateimage-function.md)  
 Valida as imagens de módulo gerenciado e notifica o carregador do sistema operacional depois que elas tiverem sido carregadas.  
  
## <a name="see-also"></a>Confira também

- [.NET Framework 4 hospedando funções estáticas globais](net-framework-4-hosting-global-static-functions.md)
