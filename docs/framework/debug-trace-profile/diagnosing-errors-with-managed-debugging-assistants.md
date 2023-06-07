---
title: Diagnosticando erros com assistentes de depuração gerenciados
description: Diagnosticar erros no .NET com assistentes de depuração gerenciados. MDAs são ferramentas de depuração que funcionam em conjunto com o CLR para fornecer informações de estado de tempo de execução.
ms.date: 08/14/2018
f1_keywords:
- EHMDA
helpviewer_keywords:
- run-time error debugging
- managed code, run-time debugging
- resource debugging
- registry, MDAs
- common language runtime, debugging
- MDAs (managed debugging assistants)
- configuration files [.NET Framework], debugging runtime events
- messages, managed debugging assistants
- application configuration files, managed debugging assistants
- debugging [.NET Framework], managed debugging assistants
- environment variables, MDAs
- access violation debugging [.NET Framework]
- diagnostics, managed debugging assistants
- unmanaged code, run-time debugging
- default debug output stream
- memory, debugging
- app.config files, managed debugging assistants
- managed debugging assistants (MDAs)
- debugging [.NET Framework], run-time errors
- trapping events
- runtime, error debugging
- disabling MDAs
- output, managed debugging assistants
- errors [.NET Framework], managed debugging assistants
ms.assetid: 76994ee6-9fa9-4059-b813-26578d24427c
ms.openlocfilehash: ac6fdc09fb057cc55659ce076d37ab96fe2354d1
ms.sourcegitcommit: a2c8b19e813a52b91facbb5d7e3c062c7188b457
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85416090"
---
# <a name="diagnose-errors-with-managed-debugging-assistants"></a>Diagnosticar erros com assistentes de depuração gerenciados

Os MDAs (Assistentes para depuração gerenciada) são recursos de depuração que trabalham com o CLR (Common Language Runtime) para fornecer informações sobre o estado do tempo de execução. Os assistentes geram mensagens informativas sobre eventos de runtime que não podem ser interceptados de outro modo. É possível usar MDAs para isolar bugs de aplicativos difíceis de encontrar que ocorrem durante a transição entre código gerenciado e não gerenciado.

Você pode [habilitar ou desabilitar](#enable-and-disable-mdas) todos os MDAs adicionando uma chave ao registro do Windows ou configurando uma variável de ambiente. Você pode habilitar MDAs específicos usando definições de configuração do aplicativo. É possível definir configurações adicionais para alguns MDAs individuais no arquivo de configuração do aplicativo. Como esses arquivos de configuração são analisados quando o runtime é carregado, você deve habilitar o MDA antes da inicialização do aplicativo gerenciado. Não será possível habilitá-lo para aplicativos já iniciados.

A tabela a seguir lista os MDAs que acompanham o .NET Framework:

|||
|-|-|
|[asynchronousThreadAbort](asynchronousthreadabort-mda.md)|[bindingFailure](bindingfailure-mda.md)|
|[callbackOnCollectedDelegate](callbackoncollecteddelegate-mda.md)|[contextSwitchDeadlock](contextswitchdeadlock-mda.md)|
|[dangerousThreadingAPI](dangerousthreadingapi-mda.md)|[dateTimeInvalidLocalFormat](datetimeinvalidlocalformat-mda.md)|
|[dirtyCastAndCallOnInterface](dirtycastandcalloninterface-mda.md)|[disconnectedContext](disconnectedcontext-mda.md)|
|[dllMainReturnsFalse](dllmainreturnsfalse-mda.md)|[exceptionSwallowedOnCallFromCom](exceptionswallowedoncallfromcom-mda.md)|
|[failedQI](failedqi-mda.md)|[fatalExecutionEngineError](fatalexecutionengineerror-mda.md)|
|[gcManagedToUnmanaged](gcmanagedtounmanaged-mda.md)|[gcUnmanagedToManaged](gcunmanagedtomanaged-mda.md)|
|[illegalPrepareConstrainedRegion](illegalprepareconstrainedregion-mda.md)|[invalidApartmentStateChange](invalidapartmentstatechange-mda.md)|
|[invalidCERCall](invalidcercall-mda.md)|[invalidFunctionPointerInDelegate](invalidfunctionpointerindelegate-mda.md)|
|[invalidGCHandleCookie](invalidgchandlecookie-mda.md)|[invalidIUnknown](invalidiunknown-mda.md)|
|[invalidMemberDeclaration](invalidmemberdeclaration-mda.md)|[invalidOverlappedToPinvoke](invalidoverlappedtopinvoke-mda.md)|
|[invalidVariant](invalidvariant-mda.md)|[jitCompilationStart](jitcompilationstart-mda.md)|
|[loaderLock](loaderlock-mda.md)|[loadFromContext](loadfromcontext-mda.md)|
|[marshalCleanupError](marshalcleanuperror-mda.md)|[marshaling](marshaling-mda.md)|
|[memberInfoCacheCreation](memberinfocachecreation-mda.md)|[moduloObjectHashcode](moduloobjecthashcode-mda.md)|
|[nonComVisibleBaseClass](noncomvisiblebaseclass-mda.md)|[notMarshalable](notmarshalable-mda.md)|
|[openGenericCERCall](opengenericcercall-mda.md)|[overlappedFreeError](overlappedfreeerror-mda.md)|
|[pInvokeLog](pinvokelog-mda.md)|[pInvokeStackImbalance](pinvokestackimbalance-mda.md)|
|[raceOnRCWCleanup](raceonrcwcleanup-mda.md)|[reentrância](reentrancy-mda.md)|
|[releaseHandleFailed](releasehandlefailed-mda.md)|[reportAvOnComRelease](reportavoncomrelease-mda.md)|
|[streamWriterBufferedDataLost](streamwriterbuffereddatalost-mda.md)|[virtualCERCall](virtualcercall-mda.md)|

Por padrão, o .NET Framework ativa um subconjunto de MDAs para todos os depuradores gerenciados. Você pode exibir o conjunto padrão no Visual Studio escolhendo configurações de exceção do **Windows**  >  **Exception Settings** no menu **depurar** e, em seguida, expandindo a lista de **assistentes de depuração gerenciada** .

![Janela de configurações de exceção no Visual Studio](./media/diagnosing-errors-with-managed-debugging-assistants/exception-settings-mdas.png)

## <a name="enable-and-disable-mdas"></a>Habilitar e desabilitar MDAs

É possível habilitar e desabilitar MDAs usando uma chave do Registro, uma variável de ambiente e as definições de configuração do aplicativo. Você deve habilitar a chave do Registro ou a variável de ambiente para usar as definições de configuração do aplicativo.

> [!TIP]
> Em vez de desabilitar o MDAs, você pode impedir que o Visual Studio exiba a caixa de diálogo MDA sempre que uma notificação do MDA é recebida. Para fazer isso, escolha **Windows**  >  **configurações de exceção** do Windows no menu **depurar** , expanda a lista **assistentes de depuração gerenciada** e marque ou desmarque a caixa de seleção **interromper quando for gerada** para o MDA individual.

### <a name="registry-key"></a>Chave do Registro

Para habilitar o MDAs, adicione o **HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft \\ . **Subchave NETFramework\MDA (tipo REG_SZ, valor 1) no registro do Windows. Copie o exemplo a seguir em um arquivo de texto chamado *MDAEnable. reg*. Abra o editor do registro do Windows (RegEdit.exe) e, no menu **arquivo** , escolha **importar**. Selecione o arquivo *MDAEnable. reg* para habilitar o MDAs nesse computador. A definição da subchave para o valor de cadeia de caracteres **1** (não o valor DWORD de **1**) permite a leitura das configurações de MDA do arquivo *ApplicationName. Suffix*.mda.config. Por exemplo, o arquivo de configuração do MDA para o bloco de notas seria nomeado notepad.exe.mda.config.

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework]
"MDA"="1"
```

Se o computador estiver executando um aplicativo de 32 bits em um sistema operacional de 64 bits, a chave do MDA deverá ser definida da seguinte forma:

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\.NETFramework]
"MDA"="1"
```

Consulte [definições de configuração específicas do aplicativo](#application-specific-configuration-settings) para obter mais informações. A configuração do Registro pode ser substituída pela variável de ambiente COMPLUS_MDA. Consulte [variável de ambiente](#environment-variable) para obter mais informações.

Para desabilitar o MDAs, defina a subchave MDA como **0** (zero) usando o editor do registro do Windows.

Por padrão, alguns MDAs são habilitados quando você executa um aplicativo anexado a um depurador, mesmo sem adicionar a chave do Registro. Você pode desabilitar esses assistentes executando o arquivo *MDADisable. reg* , conforme descrito anteriormente nesta seção.

### <a name="environment-variable"></a>Variável de ambiente

A ativação do MDA também pode ser controlada pela variável de ambiente COMPLUS_MDA, que substitui a chave do Registro. A cadeia de caracteres COMPLUS_MDA é uma lista delimitada por ponto-e-vírgula, que não diferencia maiúsculas de minúsculas, de nomes de MDA ou outras cadeias de caracteres de controle especiais. A inicialização em um depurador gerenciado ou não gerenciado habilita um conjunto de MDAs por padrão. Isso é feito implicitamente precedendo a lista separada por ponto-e-vírgula de MDAs habilitados por padrão nos depuradores ao valor da variável de ambiente ou da chave do Registro. As cadeias de caracteres de controle especiais são as seguintes:

- `0` – Desativa todos os MDAs.

- `1` – Lê as configurações do MDA por meio de *ApplicationName*.mda.config.

- `managedDebugger` – Ativa explicitamente todos os MDAs ativados implicitamente quando um executável gerenciado é iniciado em um depurador.

- `unmanagedDebugger` – Ativa explicitamente todos os MDAs ativados implicitamente quando um executável não gerenciado é iniciado em um depurador.

Se houver configurações conflitantes, as mais recentes substituirão as anteriores:

- `COMPLUS_MDA=0` desabilita todos os MDAs, incluindo aqueles habilitados implicitamente em um depurador.

- `COMPLUS_MDA=gcUnmanagedToManaged` habilita `gcUnmanagedToManaged`, além de todos os MDAs habilitados implicitamente em um depurador.

- `COMPLUS_MDA=0;gcUnmanagedToManaged` habilita `gcUnmanagedToManaged`, mas desabilita os MDAs que seriam, de outro modo, habilitados implicitamente em um depurador.

### <a name="application-specific-configuration-settings"></a>Definições de configuração específicas do aplicativo

É possível habilitar, desabilitar e configurar alguns assistentes individualmente no arquivo de configuração do MDA para o aplicativo. Para habilitar o uso de um arquivo de configuração de aplicativo para configurar MDAs, a chave do Registro do MDA ou a variável de ambiente COMPLUS_MDA deve estar definida. O arquivo de configuração de aplicativo costuma ficar localizado no mesmo diretório do arquivo executável do aplicativo (.exe). O nome do arquivo usa o formulário *ApplicationName*.mda.config; por exemplo, notepad.exe.mda.config. Os assistentes que estão habilitados no arquivo de configuração de aplicativo podem ter atributos ou elementos especificamente projetados para controlar o comportamento desse assistente.

O exemplo a seguir mostra como habilitar e configurar o [marshaling](marshaling-mda.md):

```xml
<mdaConfig>
  <assistants>
    <marshaling>
      <methodFilter>
        <match name="*"/>
      </methodFilter>
      <fieldFilter>
        <match name="*"/>
      </fieldFilter>
    </marshaling>
  </assistants>
</mdaConfig>
```

O MDA `Marshaling` emite informações sobre o tipo gerenciado em que o marshaling está sendo realizado para um tipo não gerenciado para a transição gerenciado/não gerenciado no aplicativo. O `Marshaling` MDA também pode filtrar os nomes dos campos de método e estrutura fornecidos nos elementos filho **MethodFilter** e **fieldFilter** , respectivamente.

O exemplo a seguir mostra como habilitar vários MDAs usando suas configurações padrão:

```xml
<mdaConfig>
  <assistants>
    <illegalPrepareConstrainedRegion />
    <invalidCERCall />
    <openGenericCERCall />
    <virtualCERCall />
  </assistants>
</mdaConfig>
```

> [!IMPORTANT]
> Ao especificar mais de um assistente em um arquivo de configuração, você deve listá-los em ordem alfabética. Por exemplo, se quiser habilitar os MDAs `virtualCERCall` e `invalidCERCall`, você deverá adicionar a entrada `<invalidCERCall />` antes da entrada `<virtualCERCall />`. Se as entradas não estiverem em ordem alfabética, será exibida uma mensagem de exceção do arquivo de configuração inválido sem tratamento.

## <a name="mda-exceptions"></a>Exceções do MDA

Quando um MDA é habilitado, ele fica ativo mesmo quando seu código não está em execução em um depurador. Se um evento de MDA for acionado quando um depurador não estiver presente, a mensagem do evento será apresentada em uma caixa de diálogo de exceção sem tratamento, ainda que não seja uma exceção sem tratamento. Para evitar a caixa de diálogo, remova as configurações de habilitação de MDA quando o código não estiver em execução em um ambiente de depuração.

Quando seu código é executado no IDE (ambiente de desenvolvimento integrado) do Visual Studio, você pode evitar a caixa de diálogo de exceção que aparece para eventos específicos do MDA. Para fazer isso, no menu **depurar** , escolha configurações de exceção do **Windows**  >  **Exception Settings**. Na janela **configurações de exceção** , expanda a lista **assistentes de depuração gerenciada** e desmarque a caixa de seleção **interromper quando lançado** para o MDA individual. Você também pode usar essa caixa de diálogo para *habilitar* a exibição das caixas de diálogo de exceção do MDA.

## <a name="mda-output"></a>Saída do MDA

A saída do MDA é semelhante ao exemplo a seguir, que mostra a saída do `PInvokeStackImbalance` MDA:

**Uma chamada para a função PInvoke ' MDATest! ' MDATest. Program:: StdCall ' desbalanceou a pilha. Isso é provável porque a assinatura do PInvoke gerenciada não corresponde à assinatura de destino não gerenciada. Verifique se a Convenção de chamada e os parâmetros da assinatura PInvoke correspondem à assinatura não gerenciada de destino.**

## <a name="see-also"></a>Veja também

- [Depuração, rastreamento e criação de perfil](index.md)
