---
title: Como depurar problemas de ativação do CLR
description: Consulte como depurar problemas de ativação de Common Language Runtime (CLR) no .NET. Exiba e depure os logs de ativação do CLR, que podem ser úteis para determinar as causas raiz.
ms.date: 03/30/2017
helpviewer_keywords:
- CLR activation, debugging issues
ms.assetid: 4fe17546-d56e-4344-a930-6d8e4a545914
ms.openlocfilehash: 5215e82aebf93fa8d6d1937563ab348126a01d97
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85622608"
---
# <a name="how-to-debug-clr-activation-issues"></a>Como depurar problemas de ativação do CLR

Se você encontrar problemas em fazer com que seu aplicativo seja executado com a versão correta do CLR (Common Language Runtime), poderá exibir e depurar os logs de ativação do CLR. Esses logs podem ser muito úteis para determinar a causa raiz de um problema de ativação, quando o aplicativo carrega uma versão do CLR diferente da esperada ou não carrega o CLR de forma alguma. O [.NET Framework Initialization Errors: Managing the User Experience](initialization-errors-managing-the-user-experience.md) (Erros de inicialização do .NET Framework: gerenciando a experiência do usuário) discute a experiência quando nenhum CLR é encontrado para o aplicativo.

O registro em log de ativação do CLR pode ser habilitado em todo o sistema usando uma chave do Registro HKEY_LOCAL_MACHINE ou uma variável de ambiente do sistema. O log será gerado até que a entrada do Registro ou a variável de ambiente seja removida. Como alternativa, você pode usar uma variável de ambiente local de processo ou usuário para habilitar o registro em log com uma duração e um escopo diferente.

Os logs de ativação do CLR não devem ser confundidos com os [logs de associação de assembly](../tools/fuslogvw-exe-assembly-binding-log-viewer.md), que são totalmente diferentes.

## <a name="to-enable-clr-activation-logging"></a>Para habilitar o registro em log de ativação do CLR

### <a name="using-the-registry"></a>Usando o Registro

1. No Editor de Registro, navegue até a pasta HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\\.NETFramework (em um computador de 32 bits) ou a HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\\.NETFramework (em um computador de 64 bits).

2. Adicione um valor de cadeia de caracteres chamado `CLRLoadLogDir` e defina-o como o caminho completo de um diretório existente no qual você deseja armazenar os logs de ativação do CLR.

O registro em log da ativação continua habilitada até que você remova o valor da cadeia de caracteres.

### <a name="using-an-environment-variable"></a>Usando uma variável de ambiente

- Defina a variável de ambiente `COMPLUS_CLRLoadLogDir` para uma cadeia de caracteres que represente o caminho completo de um diretório existente em que você gostaria de armazenar os logs de ativação do CLR.

    Como você define a variável de ambiente determina seu escopo:

  - Se você a definir no nível do sistema, o registro em log da ativação estará habilitado para todos os aplicativos do .NET Framework nesse computador até a variável de ambiente ser removida.

  - Se você a definir no nível do usuário, o registro em log da ativação estará habilitado apenas para a conta de usuário atual. O registro em log continua até a variável de ambiente ser removida.

  - Se você a definir de dentro do processo antes de carregar o CLR, o registro em log da ativação estará habilitado até que o processo seja encerrado.

  - Se você a definir em um prompt de comando antes de executar um aplicativo, o registro em log da ativação estará habilitado para qualquer aplicativo executado desse prompt de comando.

    Por exemplo, para armazenar logs de ativação no diretório c:\clrloadlogs com escopo do nível de processo, abra uma janela de Prompt de Comando e digite o seguinte antes de executar o aplicativo:

    ```console
    set COMPLUS_CLRLoadLogDir=c:\clrloadlogs
    ```

## <a name="example"></a>Exemplo

Os logs de ativação do CLR fornecem uma grande quantidade de dados sobre a ativação do CLR e o uso das APIs de hospedagem do CLR. A maioria desses dados é usada internamente pela Microsoft, mas alguns dos dados também podem ser úteis para os desenvolvedores, conforme descrito neste artigo.

O log reflete a ordem na qual as APIs de hospedagem do CLR foram chamadas. Ele também inclui dados úteis sobre o conjunto de runtimes instalados detectados no computador. O formato de log de ativação do CLR não está documentado, mas pode ser usado para auxiliar os desenvolvedores que precisam resolver problemas de ativação do CLR.

> [!NOTE]
> Não é possível abrir um log de ativação até que o processo que usa o CLR seja finalizado.

> [!NOTE]
> Logs de ativação do CLR não são localizados, eles são gerados sempre em inglês.

No exemplo a seguir de um log de ativação, as informações mais úteis são realçadas e descritas após o log.

```output
532,205950.367,CLR Loading log for C:\Tests\myapp.exe
532,205950.367,Log started at 4:26:12 PM on 10/6/2011
532,205950.367,-----------------------------------
532,205950.382,FunctionCall: _CorExeMain
532,205950.382,FunctionCall: ClrCreateInstance, Clsid: {2EBCD49A-1B47-4A61-B13A-4A03701E594B}, Iid: {E2190695-77B2-492E-8E14-C4B3A7FDD593}
532,205950.382,MethodCall: ICLRMetaHostPolicy::GetRequestedRuntime. Version: (null), Metahost Policy Flags: 0x168, Binary: (null), Iid: {BD39D1D2-BA2F-486A-89B0-B4B0CB466891}
532,205950.382,Installed Runtime: v4.0.30319. VERSION_ARCHITECTURE: 0
532,205950.382,Input values for ComputeVersionString follow this line
532,205950.382,-----------------------------------
532,205950.382,Default Application Name: C:\Tests\myapp.exe
532,205950.382,IsLegacyBind is: 0
532,205950.382,IsCapped is 0
532,205950.382,SkuCheckFlags are 0
532,205950.382,ShouldEmulateExeLaunch is 0
532,205950.382,LegacyBindRequired is 0
532,205950.382,-----------------------------------
532,205950.382,Parsing config file: C:\Tests\myapp.exe
532,205950.382,UseLegacyV2RuntimeActivationPolicy is set to 0
532,205950.382,LegacyFunctionCall: GetFileVersion. Filename: C:\Tests\myapp.exe
532,205950.382,LegacyFunctionCall: GetFileVersion. Filename: C:\Tests\myapp.exe
532,205950.382,C:\Tests\myapp.exe was built with version: v2.0.50727
532,205950.382,ERROR: Version v2.0.50727 is not present on the machine.
532,205950.398,SEM_FAILCRITICALERRORS is set to 0
532,205950.398,Launching feature-on-demand installation. CmdLine: C:\Windows\system32\fondue.exe /enable-feature:NetFx3
532,205950.398,FunctionCall: RealDllMain. Reason: 0
532,205950.398,FunctionCall: OnShimDllMainCalled. Reason: 0
```

- **CLR Loading log** fornece o caminho para o executável que iniciou o processo que carregou o código gerenciado. Observe que isso poderia ser um host nativo.

    ```output
    532,205950.367,CLR Loading log for C:\Tests\myapp.exe
    ```

- **Installed Runtime** é o conjunto das versões do CLR instaladas no computador que são candidatas para a solicitação de ativação.

    ```output
    532,205950.382,Installed Runtime: v4.0.30319. VERSION_ARCHITECTURE: 0
    ```

- **built with version** é a versão do CLR que foi usada para criar o binário que foi fornecido para um método como [ICLRMetaHostPolicy::GetRequestedRuntime](../unmanaged-api/hosting/iclrmetahostpolicy-getrequestedruntime-method.md).

    ```output
    532,205950.382,C:\Tests\myapp.exe was built with version: v2.0.50727
    ```

- **feature-on-demand installation** refere-se a habilitar o .NET Framework 3.5 no Windows 8. Consulte [.NET Framework Initialization Errors: Managing the User Experience](initialization-errors-managing-the-user-experience.md) (Erros de inicialização do .NET Framework: gerenciando a experiência do usuário) para obter mais informações sobre esse cenário.

    ```output
    532,205950.398,Launching feature-on-demand installation. CmdLine: C:\Windows\system32\fondue.exe /enable-feature:NetFx3
    ```

## <a name="see-also"></a>Consulte também

- [Implantação](index.md)
- [Como configurar um aplicativo para dar suporte a .NET Framework 4 ou versões posteriores](../migration-guide/how-to-configure-an-app-to-support-net-framework-4-or-4-5.md)
