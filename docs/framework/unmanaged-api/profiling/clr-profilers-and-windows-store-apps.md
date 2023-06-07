---
title: Criadores de perfil CLR e aplicativos da Windows Store
ms.date: 03/30/2017
dev_langs:
- csharp
applies_to:
- Windows 10
- Windows 8
helpviewer_keywords:
- profiling API
- profiling API [.NET Framework]
- profiling managed code
- profiling managed code [Windows Store Apps]
ms.assetid: 1c8eb2e7-f20a-42f9-a795-71503486a0f5
ms.openlocfilehash: 6330a4c2733729da264065d1eec8c3c9eaf9f05c
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501021"
---
# <a name="clr-profilers-and-windows-store-apps"></a>Criadores de perfil CLR e aplicativos da Windows Store

Este tópico discute o que você precisa pensar ao escrever ferramentas de diagnóstico que analisam o código gerenciado em execução dentro de um aplicativo da Windows Store. Ele também fornece diretrizes para modificar suas ferramentas de desenvolvimento existentes para que continuem funcionando quando você executá-las em aplicativos da Windows Store. Para entender essas informações, é melhor se você estiver familiarizado com a API de criação de perfil do Common Language Runtime, já usou essa API em uma ferramenta de diagnóstico que é executada corretamente em aplicativos da área de trabalho do Windows, e agora você está interessado em modificar a ferramenta para ser executada corretamente em aplicativos da Windows Store.

## <a name="introduction"></a>Introdução

Se você o fez após o parágrafo introdutório, você está familiarizado com a API de criação de perfil do CLR. Você já escreveu uma ferramenta de diagnóstico que funciona bem em aplicativos de área de trabalho gerenciada. Agora você está curioso o que fazer para que sua ferramenta funcione com um aplicativo gerenciado da Windows Store. Talvez você já tenha tentado fazer isso, e descobriu que não é uma tarefa simples. Na verdade, há várias considerações que podem não ser óbvias para todos os desenvolvedores de ferramentas. Por exemplo:

- Os aplicativos da Windows Store são executados em um contexto com permissões extremamente reduzidas.

- Os arquivos de metadados do Windows têm características exclusivas quando comparados aos módulos gerenciados tradicionais.

- Os aplicativos da Windows Store têm o hábito de se suspender quando a interatividade ficar inativa.

- Os mecanismos de comunicação entre processos podem deixar de funcionar por vários motivos.

Este tópico lista as coisas que você precisa saber e como lidar com elas corretamente.

Se você for novo na API de criação de perfil do CLR, pule até os recursos no final deste tópico para encontrar melhores informações introdutórias.

Fornecer detalhes sobre APIs específicas do Windows e como elas devem ser usadas também está fora do escopo deste tópico. Considere este tópico um ponto de partida e consulte o MSDN para saber mais sobre todas as APIs do Windows referenciadas aqui.

## <a name="architecture-and-terminology"></a>Arquitetura e terminologia

Normalmente, uma ferramenta de diagnóstico tem uma arquitetura como a mostrada na ilustração a seguir. Ele usa o termo "Profiler", mas muitas dessas ferramentas vão muito além do desempenho típico ou da criação de perfil de memória em áreas como cobertura de código, estruturas de objeto fictícios, depuração de viagem de tempo, monitoramento de aplicativos e assim por diante. Para simplificar, este tópico continuará a se referir a todas essas ferramentas como profileres.

A seguinte terminologia é usada em todo este tópico:

**Aplicativo**

Esse é o aplicativo que o criador de perfil está analisando. Normalmente, o desenvolvedor deste aplicativo agora está usando o criador de perfil para ajudar a diagnosticar problemas com o aplicativo. Tradicionalmente, esse aplicativo seria um aplicativo de área de trabalho do Windows, mas, neste tópico, estamos examinando os aplicativos da Windows Store.

**DLL do criador de perfil**

Esse é o componente que é carregado no espaço de processo do aplicativo que está sendo analisado. Esse componente, também conhecido como o criador de perfil "Agent", implementa a interface [ICorProfilerCallback](icorprofilercallback-interface.md)[ICorProfilerCallback](icorprofilercallback-interface.md)(2, 3 etc.) interfaces e consome as interfaces [ICorProfilerInfo](icorprofilerinfo-interface.md)(2, 3 etc.) para coletar dados sobre o aplicativo analisado e potencialmente modificar aspectos do comportamento do aplicativo.

**IU do criador de perfil**

Esse é um aplicativo de área de trabalho com o qual o usuário do profiler interage. É responsável por exibir o status do aplicativo para o usuário e dar ao usuário o meio de controlar o comportamento do aplicativo analisado. Esse componente sempre é executado em seu próprio espaço de processo, separado do espaço de processo do aplicativo que está sendo criado. A interface do usuário do criador de perfil também pode atuar como "gatilho de anexação", que é o processo que chama o método [ICLRProfiling:: AttachProfiler](iclrprofiling-attachprofiler-method.md) , para fazer com que o aplicativo analisado carregue a DLL do criador de perfil nesses casos em que a DLL do criador de perfil não foi carregada na inicialização.

> [!IMPORTANT]
> Sua interface do usuário do profiler deve permanecer um aplicativo de área de trabalho do Windows, mesmo quando é usada para controlar e relatar um aplicativo da Windows Store. Não espere ser capaz de empacotar e enviar sua ferramenta de diagnóstico na Windows Store. Sua ferramenta precisa fazer coisas que os aplicativos da Windows Store não podem fazer, e muitas dessas coisas residem em sua interface do usuário do profiler.

Em todo este documento, o código de exemplo pressupõe que:

- A DLL do criador de perfil é escrita em C++, porque deve ser uma DLL nativa, de acordo com os requisitos da API de criação de perfil do CLR.

- Sua interface do usuário do profiler é escrita em C#. Isso não é necessário, mas porque não há requisitos de linguagem para o processo da interface do usuário do criador de perfil, por que não escolher uma linguagem concisa e simples?

### <a name="windows-rt-devices"></a>Dispositivos Windows RT

Os dispositivos Windows RT estão bem bloqueados. Os profileres de terceiros simplesmente não podem ser carregados nesses dispositivos. Este documento se concentra em PCs com o Windows 8.

## <a name="consuming-windows-runtime-apis"></a>Consumindo APIs de Windows Runtime

Em vários cenários discutidos nas seções a seguir, seu aplicativo de área de trabalho do profiler precisa consumir algumas novas APIs de Windows Runtime. Convém consultar a documentação para entender quais Windows Runtime APIs podem ser usadas em aplicativos de desktop e se seu comportamento é diferente quando chamado de aplicativos da área de trabalho e aplicativos da Windows Store.

Se sua interface do usuário do Profiler for escrita em código gerenciado, haverá algumas etapas que você precisará fazer para facilitar o consumo dessas APIs de Windows Runtime. Para obter mais informações, consulte o artigo [aplicativos e Windows Runtime de área de trabalho gerenciada](https://docs.microsoft.com/previous-versions/windows/apps/jj856306(v=win.10)) .

## <a name="loading-the-profiler-dll"></a>Carregando a DLL do criador de perfil

Esta seção descreve como a interface do usuário do profiler faz com que o aplicativo da Windows Store carregue sua DLL do criador de perfil. O código discutido nesta seção pertence ao seu aplicativo de desktop da interface do usuário do Profiler e, portanto, envolve o uso de APIs do Windows que são seguras para aplicativos da área de trabalho, mas não são necessariamente seguros para aplicativos da Windows Store

Sua interface do usuário do profiler pode fazer com que a DLL do criador de perfil seja carregada no espaço de processo do aplicativo de duas maneiras:

- Na inicialização do aplicativo, conforme controlado pelas variáveis de ambiente.

- Anexando-se ao aplicativo após a conclusão da inicialização chamando o método [ICLRProfiling:: AttachProfiler](iclrprofiling-attachprofiler-method.md) .

Um dos seus primeiros obstáculos será obter carga de inicialização e carregar o carregamento de sua DLL do criador de perfil para funcionar corretamente com os aplicativos da Windows Store. Ambas as formas de carregamento compartilham algumas considerações especiais em comum, portanto, vamos começar com elas.

### <a name="common-considerations-for-startup-and-attach-loads"></a>Considerações comuns para inicialização e carregamentos de anexos

**Assinando sua DLL do criador de perfil**

Quando o Windows tenta carregar sua DLL do criador de perfil, ele verifica se a DLL do criador de perfil está corretamente assinada. Caso contrário, a carga falhará por padrão. Existem duas maneiras de fazer isso:

- Verifique se a DLL do criador de perfil está assinada.

- Informe ao usuário que ele deve instalar uma licença de desenvolvedor em seu computador com Windows 8 antes de usar sua ferramenta. Isso pode ser feito automaticamente do Visual Studio ou manualmente a partir de um prompt de comando. Para obter mais informações, consulte [obter uma licença de desenvolvedor](https://docs.microsoft.com/previous-versions/windows/apps/hh974578(v=win.10)).

**Permissões do sistema de arquivos**

O aplicativo da Windows Store deve ter permissão para carregar e executar a DLL do criador de perfil do local no sistema de arquivos no qual residesBy padrão, o aplicativo da Windows Store não tem essa permissão na maioria dos diretórios e qualquer tentativa com falha para carregar a DLL do criador de perfil produzirá uma entrada no log de eventos do aplicativo do Windows semelhante a esta :

```output
NET Runtime version 4.0.30319.17929 - Loading profiler failed during CoCreateInstance.  Profiler CLSID: '{xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}'.  HRESULT: 0x80070005.  Process ID (decimal): 4688.  Message ID: [0x2504].
```

Em geral, os aplicativos da Windows Store só têm permissão para acessar um conjunto limitado de locais no disco. Cada aplicativo da Windows Store pode acessar suas próprias pastas de dados de aplicativo, bem como algumas outras áreas no sistema de arquivos para as quais todos os aplicativos da Windows Store têm acesso concedido. É melhor instalar sua DLL do criador de perfil e suas dependências em algum lugar em arquivos de programas ou arquivos de programas (x86), pois todos os aplicativos da Windows Store têm permissões de leitura e execução ali por padrão.

### <a name="startup-load"></a>Carga de inicialização

Normalmente, em um aplicativo de área de trabalho, sua interface do usuário do profiler solicita uma carga de inicialização de sua DLL do criador de perfil inicializando um bloco de ambiente que contém as variáveis de ambiente de API de criação de perfil do CLR necessárias (ou seja,,, `COR_PROFILER` `COR_ENABLE_PROFILING` e `COR_PROFILER_PATH` ) e, em seguida, criando um novo processo com esse bloco de ambiente. O mesmo se aplica a aplicativos da Windows Store, mas os mecanismos são diferentes.

**Não executar com privilégios elevados**

Se o processo tentar gerar o processo de aplicativo da Windows Store B, o processo A deverá ser executado no nível de integridade médio, não no nível de integridade alta (ou seja, não elevado). Isso significa que a interface do usuário do criador de perfil deve estar em execução no nível de integridade médio ou deve gerar outro processo de área de trabalho no nível de integridade médio para cuidar da inicialização do aplicativo da Windows Store.

**Escolhendo um aplicativo da Windows Store para o perfil**

Primeiro, você desejará solicitar ao usuário do profiler que o aplicativo da Windows Store seja iniciado. Para aplicativos da área de trabalho, talvez você tenha mostrado uma caixa de diálogo de procura de arquivo e o usuário encontraria e selecionaria um arquivo. exe. Mas os aplicativos da Windows Store são diferentes, e usar uma caixa de diálogo de procura não faz sentido. Em vez disso, é melhor mostrar ao usuário uma lista de aplicativos da Windows Store instalados para que o usuário selecione.

Você pode usar a <xref:Windows.Management.Deployment.PackageManager> classe para gerar essa lista. `PackageManager`o é uma classe Windows Runtime que está disponível para aplicativos da área de trabalho e, na verdade, *só* está disponível para aplicativos da área de trabalho.

O exemplo de código a seguir de uma IU hipotética do criador de perfil escrito como um aplicativo de área de trabalho em C# usa o `PackageManager` para gerar uma lista de aplicativos do Windows:

```csharp
string currentUserSID = WindowsIdentity.GetCurrent().User.ToString();
IAppxFactory appxFactory = (IAppxFactory) new AppxFactory();
PackageManager packageManager = new PackageManager();
IEnumerable<Package> packages = packageManager.FindPackagesForUser(currentUserSID);
```

**Especificando o bloco de ambiente personalizado**

Uma nova interface COM, [IPackageDebugSettings](/windows/desktop/api/shobjidl_core/nn-shobjidl_core-ipackagedebugsettings), permite que você personalize o comportamento de execução de um aplicativo da Windows Store para facilitar ainda mais as formas de diagnóstico. Um dos seus métodos, [EnableDebugging](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-ipackagedebugsettings-enabledebugging), permite que você passe um bloco de ambiente para o aplicativo da Windows Store quando ele é iniciado, juntamente com outros efeitos úteis, como desabilitar a suspensão automática de processos. O bloco de ambiente é importante porque é onde você precisa especificar as variáveis de ambiente ( `COR_PROFILER` , `COR_ENABLE_PROFILING` e `COR_PROFILER_PATH)` ) usadas pelo CLR para carregar a DLL do criador de perfil.

Considere o seguinte snippet de código:

```csharp
IPackageDebugSettings pkgDebugSettings = new PackageDebugSettings();
pkgDebugSettings.EnableDebugging(packageFullName, debuggerCommandLine,
                                                                 (IntPtr)fixedEnvironmentPzz);
```

Há alguns itens que você precisará obter, certo:

- `packageFullName`pode ser determinado durante a iteração dos pacotes e da captura `package.Id.FullName` .

- `debuggerCommandLine`é um pouco mais interessante. Para passar o bloco de ambiente personalizado para o aplicativo da Windows Store, você precisa escrever seu próprio depurador fictício e simplista. O Windows gera o aplicativo da Windows Store suspenso e, em seguida, anexa o depurador iniciando o depurador com uma linha de comando como neste exemplo:

    ```console
    MyDummyDebugger.exe -p 1336 -tid 1424
    ```

     onde `-p 1336` significa que o aplicativo da Windows Store tem a ID de processo 1336 e significa que a `-tid 1424` ID de thread 1424 é o thread suspenso. O depurador fictício analisaria o ThreadID da linha de comando, retomaria esse thread e, em seguida, sairá.

     Aqui está um exemplo de código C++ para fazer isso (certifique-se de adicionar verificação de erro!):

    ```cpp
    int wmain(int argc, wchar_t* argv[])
    {
        // …
        // Parse command line here
        // …

        HANDLE hThread = OpenThread(THREAD_SUSPEND_RESUME,
                                                                  FALSE /* bInheritHandle */, nThreadID);
        ResumeThread(hThread);
        CloseHandle(hThread);
        return 0;
    }
    ```

     Você precisará implantar esse depurador fictício como parte da instalação da ferramenta de diagnóstico e, em seguida, especificar o caminho para esse depurador no `debuggerCommandLine` parâmetro.

**Iniciando o aplicativo da Windows Store**

O momento de iniciar o aplicativo da Windows Store chegou finalmente. Se você já tentou fazer isso por conta própria, talvez tenha notado que [CreateProcess](/windows/desktop/api/processthreadsapi/nf-processthreadsapi-createprocessa) não é como criar um processo de aplicativo da Windows Store. Em vez disso, você precisará usar o método [IApplicationActivationManager:: ActivateApplication](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-iapplicationactivationmanager-activateapplication) . Para fazer isso, você precisará obter a ID do modelo de usuário do aplicativo do aplicativo da Windows Store que você está iniciando. E isso significa que você precisará fazer um pouco se aprofundando no manifesto.

Ao fazer a iteração em seus pacotes (consulte "escolhendo um aplicativo da Windows Store para o perfil" na seção de [carregamento de inicialização](#startup-load) anterior), você desejará obter o conjunto de aplicativos contidos no manifesto do pacote atual:

```csharp
string manifestPath = package.InstalledLocation.Path + "\\AppxManifest.xml";

AppxPackaging.IStream manifestStream;
SHCreateStreamOnFileEx(
                    manifestPath,
                    0x00000040,     // STGM_READ | STGM_SHARE_DENY_NONE
                    0,              // file creation attributes
                    false,          // fCreate
                    null,           // reserved
                    out manifestStream);

IAppxManifestReader manifestReader = appxFactory.CreateManifestReader(manifestStream);

IAppxManifestApplicationsEnumerator appsEnum = manifestReader.GetApplications();
```

Sim, um pacote pode ter vários aplicativos, e cada aplicativo tem sua própria ID de modelo de usuário de aplicativo. Portanto, você desejará solicitar ao usuário qual aplicativo criar perfil e obter a ID do modelo de usuário do aplicativo nesse aplicativo específico:

```csharp
while (appsEnum.GetHasCurrent() != 0)
{
    IAppxManifestApplication app = appsEnum.GetCurrent();
    string appUserModelId = app.GetAppUserModelId();
    //...
}
```

Por fim, agora você tem o que precisa para iniciar o aplicativo da Windows Store:

```csharp
IApplicationActivationManager appActivationMgr = new ApplicationActivationManager();
appActivationMgr.ActivateApplication(appUserModelId, appArgs, ACTIVATEOPTIONS.AO_NONE, out pid);
```

**Lembre-se de chamar DisableDebugging**

Quando você chamou [IPackageDebugSettings:: EnableDebugging](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-ipackagedebugsettings-enabledebugging), fez uma promessa que seria limpa depois de chamar o método [IPackageDebugSettings::D isabledebugging](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-ipackagedebugsettings-disabledebugging) , portanto, certifique-se de fazer isso quando a sessão de criação de perfil terminar.

### <a name="attach-load"></a>Anexar carregamento

Quando a interface do usuário do profiler deseja anexar sua DLL do criador de perfil a um aplicativo que já começou a ser executado, ele usa [ICLRProfiling:: AttachProfiler](iclrprofiling-attachprofiler-method.md). O mesmo se aplica a aplicativos da Windows Store. Mas, além das considerações comuns listadas anteriormente, verifique se o aplicativo da Windows Store de destino não está suspenso.

**EnableDebugging**

Assim como acontece com o carregamento de inicialização, chame o método [IPackageDebugSettings:: EnableDebugging](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-ipackagedebugsettings-enabledebugging) . Você não precisa dele para passar um bloco de ambiente, mas precisa de um de seus outros recursos: desabilitar a suspensão automática de processos. Caso contrário, quando a interface do usuário do profiler chamar [AttachProfiler](iclrprofiling-attachprofiler-method.md), o aplicativo da Windows Store de destino poderá ser suspenso. Na verdade, isso é provável se o usuário agora estiver interagindo com sua interface do usuário do Profiler e o aplicativo da Windows Store não estiver ativo em nenhuma das telas. E se o aplicativo da Windows Store for suspenso, ele não poderá responder a nenhum sinal que o CLR envie a ele para anexar a DLL do criador de perfil.

Portanto, você vai querer fazer algo assim:

```csharp
IPackageDebugSettings pkgDebugSettings = new PackageDebugSettings();
pkgDebugSettings.EnableDebugging(packageFullName, null /* debuggerCommandLine */,
                                                                 IntPtr.Zero /* environment */);
```

Essa é a mesma chamada que você faria para o caso de carregamento de inicialização, exceto que você não especifica uma linha de comando do depurador ou um bloco de ambiente.

**DisableDebugging**

Como sempre, não se esqueça de chamar [IPackageDebugSettings::D isabledebugging](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-ipackagedebugsettings-disabledebugging) quando a sessão de criação de perfil for concluída.

## <a name="running-inside-the-windows-store-app"></a>Executando dentro do aplicativo da Windows Store

Portanto, o aplicativo da Windows Store, por fim, carregou a DLL do criador de perfil. Agora, a DLL do criador de perfil deve ser ensinada a reproduzir as diferentes regras exigidas pelos aplicativos da Windows Store, incluindo quais APIs são permitidas e como executar com permissões reduzidas.

### <a name="stick-to-the-windows-store-app-apis"></a>Aderir às APIs do aplicativo da Windows Store

Ao navegar pela API do Windows, você observará que cada API está documentada como aplicável a aplicativos de área de trabalho, aplicativos da Windows Store ou ambos. Por exemplo, a seção **requisitos** da documentação para a função [InitializeCriticalSectionAndSpinCount](/windows/desktop/api/synchapi/nf-synchapi-initializecriticalsectionandspincount) indica que a função se aplica somente a aplicativos da área de trabalho. Por outro lado, a função [InitializeCriticalSectionEx](/windows/desktop/api/synchapi/nf-synchapi-initializecriticalsectionex) está disponível para aplicativos da área de trabalho e para aplicativos da Windows Store.

Ao desenvolver sua DLL do criador de perfil, trate-a como se fosse um aplicativo da Windows Store e use apenas as APIs que estão documentadas como disponíveis para aplicativos da Windows Store. Analise suas dependências (por exemplo, você pode executar `link /dump /imports` em sua DLL do criador de perfil para auditoria) e, em seguida, pesquise os documentos para ver quais das suas dependências estão ok e quais não são. Na maioria dos casos, suas violações podem ser corrigidas simplesmente substituindo-as por uma forma mais recente da API que está documentada como segura (por exemplo, substituindo [InitializeCriticalSectionAndSpinCount](/windows/desktop/api/synchapi/nf-synchapi-initializecriticalsectionandspincount) por [InitializeCriticalSectionEx](/windows/desktop/api/synchapi/nf-synchapi-initializecriticalsectionex)).

Você pode observar que a DLL do criador de perfil chama algumas APIs que se aplicam apenas a aplicativos da área de trabalho e, ainda assim, eles parecem funcionar mesmo quando a DLL do criador de perfil é carregada dentro de um aplicativo da Windows Store. Lembre-se de que é arriscado usar qualquer API não documentada para uso com aplicativos da Windows Store em sua DLL do criador de perfil quando carregada em um processo de aplicativo da Windows Store:

- Não há garantia de que essas APIs funcionem quando chamado no contexto exclusivo em que os aplicativos da Windows Store são executados.

- Essas APIs podem não funcionar consistentemente quando chamadas de dentro de diferentes processos de aplicativo da Windows Store.

- Essas APIs talvez pareçam funcionar bem em aplicativos da Windows Store na versão atual do Windows, mas podem ser interrompidas ou desativadas em versões futuras do Windows.

O melhor conselho é corrigir todas as suas violações e evitar o risco.

Você pode achar que não é possível fazer sem uma API específica e não encontrar uma substituição adequada para aplicativos da Windows Store. Nesse caso, no mínimo:

- Teste, teste, teste o horário de verão para fora do seu uso dessa API.

- Entenda que a API pode repentinamente interromper ou desaparecer se chamada de dentro de aplicativos da Windows Store em versões futuras do Windows. Isso não será considerado uma preocupação de compatibilidade com a Microsoft e o suporte ao seu uso não será uma prioridade.

### <a name="reduced-permissions"></a>Permissões reduzidas

Está fora do escopo deste tópico para listar todas as formas pelas quais as permissões de aplicativo da Windows Store diferem dos aplicativos da área de trabalho. Mas certamente o comportamento será diferente sempre que sua DLL do criador de perfil (quando carregada em um aplicativo da Windows Store em comparação com um aplicativo de desktop) tentar acessar os recursos. O sistema de arquivos é o exemplo mais comum. Há, porém, alguns locais no disco que um determinado aplicativo da Windows Store tem permissão para acessar (consulte [acesso a arquivos e permissões (Windows Runtime aplicativos](https://docs.microsoft.com/previous-versions/windows/apps/hh967755(v=win.10))) e sua DLL do criador de perfil estará sob as mesmas restrições. Teste seu código completamente.

### <a name="inter-process-communication"></a>Comunicação entre processos

Conforme mostrado no diagrama no início deste documento, a DLL do criador de perfil (carregada no espaço de processo do aplicativo da Windows Store) provavelmente precisará se comunicar com sua interface do usuário do Profiler (em execução em um espaço de processo do aplicativo de área de trabalho separado) por meio de seu próprio canal de IPC (comunicação entre processos) personalizado. A interface do usuário do profiler envia sinais para a DLL do criador de perfil para modificar seu comportamento, e a DLL do criador de perfil envia dados do aplicativo da Windows Store analisado de volta para a interface do usuário do Profiler para pós-processamento e exibição para a do profiler

A maioria dos profileres precisa trabalhar dessa forma, mas suas escolhas para mecanismos IPC são mais limitadas quando a DLL do criador de perfil é carregada em um aplicativo da Windows Store. Por exemplo, os pipes nomeados não fazem parte do SDK do aplicativo da Windows Store, portanto, você não pode usá-los.

Mas, é claro, os arquivos ainda estão em, embora de maneira mais limitada. Os eventos também estão disponíveis.

**Comunicando-se por meio de arquivos**

A maioria dos seus dados provavelmente passará entre a DLL do criador de perfil e a interface do usuário do profiler via arquivos. A chave é escolher um local de arquivo que a DLL do criador de perfil (no contexto de um aplicativo da Windows Store) e a interface do usuário do profiler tenham acesso de leitura e gravação ao. Por exemplo, o caminho da pasta temporária é um local que a DLL do criador de perfil e a interface do usuário do profiler podem acessar, mas nenhum outro pacote de aplicativo da Windows Store pode acessar (protegendo assim qualquer informação que você registrar de outros pacotes de aplicativos da Windows Store).

A interface do usuário do Profiler e a DLL do criador de perfil podem determinar esse caminho independentemente. Sua interface do usuário do Profiler, quando itera por meio de todos os pacotes instalados para a usuária atual (consulte o código de exemplo anterior), obtém acesso à `PackageId` classe, da qual o caminho da pasta temporária pode ser derivado com um código semelhante a este trecho. (Como sempre, a verificação de erros é omitida para fins de brevidade.)

```csharp
// C# code for the Profiler UI.
ApplicationData appData =
    ApplicationDataManager.CreateForPackageFamily(
        packageId.FamilyName);

tempDir = appData.TemporaryFolder.Path;
```

Enquanto isso, a DLL do criador de perfil pode fazer basicamente a mesma coisa, embora possa obter mais facilmente a <xref:Windows.Storage.ApplicationData> classe usando a propriedade [applicationData. Current](xref:Windows.Storage.ApplicationData.Current%2A) .

**Comunicando-se por eventos**

Se você quiser uma semântica de sinalização simples entre sua interface do usuário do Profiler e a DLL do criador de perfil, poderá usar eventos dentro de aplicativos da Windows Store, bem como aplicativos da área de trabalho.

Na sua DLL do criador de perfil, você pode simplesmente chamar a função [CreateEventEx](/windows/desktop/api/synchapi/nf-synchapi-createeventexa) para criar um evento nomeado com qualquer nome que desejar. Por exemplo:

```cpp
// Profiler DLL in Windows Store app (C++).
CreateEventEx(
    NULL,  // Not inherited
    "MyNamedEvent"
    CREATE_EVENT_MANUAL_RESET, /* explicit ResetEvent() required; leave initial state unsignaled */
    EVENT_ALL_ACCESS);
```

Em seguida, sua interface do usuário do profiler precisa encontrar esse evento nomeado no namespace do aplicativo da Windows Store. Por exemplo, sua interface do usuário do Profiler poderia chamar [CreateEventEx](/windows/desktop/api/synchapi/nf-synchapi-createeventexa), especificando o nome do evento como

`AppContainerNamedObjects\<acSid>\MyNamedEvent`

`<acSid>`é o SID do AppContainer do aplicativo da Windows Store. Uma seção anterior deste tópico mostrou como iterar sobre os pacotes instalados para o usuário atual. A partir desse código de exemplo, você pode obter o PackageID. E, a partir da PackageID, você pode obter o `<acSid>` código com semelhante ao seguinte:

```csharp
IntPtr acPSID;
DeriveAppContainerSidFromAppContainerName(packageId.FamilyName, out acPSID);

string acSid;
ConvertSidToStringSid(acPSID, out acSid);

string acDir;
GetAppContainerFolderPath(acSid, out acDir);
```

### <a name="no-shutdown-notifications"></a>Nenhuma notificação de desligamento

Ao ser executado dentro de um aplicativo da Windows Store, sua DLL do criador de perfil não deve depender de [ICorProfilerCallback:: Shutdown](icorprofilercallback-shutdown-method.md) ou mesmo [DllMain](/windows/desktop/Dlls/dllmain) (with `DLL_PROCESS_DETACH` ) ser chamado para notificar sua DLL do criador de perfil que o aplicativo da Windows Store está sendo encerrado. Na verdade, você deve esperar que nunca será chamado. Historicamente, muitas DLLs do criador de perfil usaram essas notificações como locais convenientes para liberar caches para o disco, fechar arquivos, enviar notificações de volta para a interface do usuário do criador de perfil, etc. Mas agora a DLL do criador de perfil precisa ser organizada de um modo um pouco diferente.

A DLL do criador de perfil deve estar registrando em log as informações conforme ele vai. Por motivos de desempenho, talvez você queira lote de informações na memória e liberá-las para o disco à medida que o lote cresce em um tamanho além do limite. Mas suponha que todas as informações ainda não liberadas para o disco possam ser perdidas. Isso significa que você desejará escolher seu limite de forma inteligente e que sua interface do usuário do profiler precisa ser protegida para lidar com informações incompletas gravadas pela DLL do criador de perfil.

## <a name="windows-runtime-metadata-files"></a>Windows Runtime arquivos de metadados

Ele está fora do escopo deste documento para entrar em detalhes sobre o que são arquivos de Windows Runtime de metadados (WinMD). Esta seção é limitada a como a API de criação de perfil do CLR reage quando os arquivos WinMD são carregados pelo aplicativo da Windows Store que sua DLL do criador de perfil está analisando.

### <a name="managed-and-non-managed-winmds"></a>WinMDs gerenciados e não gerenciados

Se um desenvolvedor usar o Visual Studio para criar um novo projeto de componente Windows Runtime, uma compilação desse projeto produzirá um arquivo WinMD que descreve os metadados (as descrições de tipo das classes, interfaces, etc.) criados pelo desenvolvedor. Se esse projeto for um projeto de linguagem gerenciado escrito em C# ou Visual Basic, esse mesmo arquivo WinMD também conterá a implementação desses tipos (o que significa que ele contém todo o IL compilado do código-fonte do desenvolvedor). Esses arquivos são conhecidos como arquivos WinMD gerenciados. Eles são interessantes, pois contêm tanto Windows Runtime metadados quanto a implementação subjacente.

Por outro lado, se um desenvolvedor cria um projeto de componente Windows Runtime para C++, uma compilação desse projeto produz um arquivo WinMD que contém apenas metadados e a implementação é compilada em uma DLL nativa separada. Da mesma forma, os arquivos WinMD que acompanham o SDK do Windows contêm apenas metadados, com a implementação compilada em DLLs nativas separadas que são fornecidas como parte do Windows.

As informações a seguir se aplicam a WinMDs gerenciados, que contêm metadados e implementação, e a WinMDs não gerenciada, que contêm apenas metadados.

### <a name="winmd-files-look-like-clr-modules"></a>Arquivos WinMD parecem módulos CLR

No que diz respeito ao CLR, todos os arquivos WinMD são módulos. A API de criação de perfil do CLR, portanto, informa à DLL do criador de perfil quando os arquivos WinMD são carregados e quais são seus ModuleIDs, da mesma maneira que para outros módulos gerenciados.

A DLL do criador de perfil pode distinguir arquivos WinMD de outros módulos chamando o método [ICorProfilerInfo3:: GetModuleInfo2](icorprofilerinfo3-getmoduleinfo2-method.md) e inspecionando o `pdwModuleFlags` parâmetro de saída para o sinalizador [COR_PRF_MODULE_WINDOWS_RUNTIME](cor-prf-module-flags-enumeration.md) . (Ela é definida se e somente se ModuleID representar um WinMD.)

### <a name="reading-metadata-from-winmds"></a>Lendo metadados do WinMDs

Os arquivos WinMD, como os módulos regulares, contêm metadados que podem ser lidos por meio das [APIs de metadados](../metadata/index.md). No entanto, o CLR mapeia tipos de Windows Runtime para .NET Framework tipos quando lê arquivos WinMD para que os desenvolvedores que programam em código gerenciado e consumam o arquivo WinMD possam ter uma experiência de programação mais natural. Para obter alguns exemplos desses mapeamentos, consulte [suporte de .NET Framework para aplicativos da Windows Store e Windows Runtime](../../../standard/cross-platform/support-for-windows-store-apps-and-windows-runtime.md).

Então, qual exibição o criador de perfil obterá quando usar as APIs de metadados: a exibição Windows Runtime bruta ou a exibição .NET Framework mapeada?  A resposta: cabe a você.

Quando você chama o método [ICorProfilerInfo:: GetModuleMetaData](icorprofilerinfo-getmodulemetadata-method.md) em um WinMD para obter uma interface de metadados, como [IMetaDataImport](../metadata/imetadataimport-interface.md), você pode optar por definir [ofNoTransform](../metadata/coropenflags-enumeration.md) no `dwOpenFlags` parâmetro para desativar esse mapeamento. Caso contrário, por padrão, o mapeamento será habilitado. Normalmente, um criador de perfil manterá o mapeamento habilitado, de forma que as cadeias de caracteres que a DLL do criador de perfil obtenha dos metadados de WinMD (por exemplo, nomes de tipos) pareçam familiares e naturais para o usuário do profiler.

### <a name="modifying-metadata-from-winmds"></a>Modificando metadados de WinMDs

Não há suporte para a modificação de metadados em WinMDs. Se você chamar o método [ICorProfilerInfo:: GetModuleMetaData](icorprofilerinfo-getmodulemetadata-method.md) para um arquivo WinMD e especificar [ofWrite](../metadata/coropenflags-enumeration.md) no `dwOpenFlags` parâmetro ou solicitar uma interface de metadados gravável, como [IMetaDataEmit](../metadata/imetadataemit-interface.md), [GetModuleMetaData](icorprofilerinfo-getmodulemetadata-method.md) falhará. Isso é de particular importância para os profileres de reescrita de IL, que precisam modificar metadados para dar suporte à sua instrumentação (por exemplo, para adicionar Refsdeassembly ou novos métodos). Portanto, você deve verificar [COR_PRF_MODULE_WINDOWS_RUNTIME](cor-prf-module-flags-enumeration.md) primeiro (conforme discutido na seção anterior) e evitar que o solicite interfaces de metadados graváveis nesses módulos.

### <a name="resolving-assembly-references-with-winmds"></a>Resolvendo referências de assembly com WinMDs

Muitos profileres precisam resolver referências de metadados manualmente para auxiliar na instrumentação ou na inspeção de tipo. Esses profileres precisam estar cientes de como o CLR resolve referências de assembly que apontam para WinMDs, pois essas referências são resolvidas de forma completamente diferente das referências de assembly padrão.

## <a name="memory-profilers"></a>Profileres de memória

O coletor de lixo e o heap gerenciado não são fundamentalmente diferentes em um aplicativo da Windows Store e em um aplicativo de área de trabalho. No entanto, há algumas diferenças sutis que os autores do criador de perfil precisam conhecer.

### <a name="forcegc-creates-a-managed-thread"></a>ForceGC cria um thread gerenciado

Ao fazer a criação de perfil de memória, a DLL do criador de perfil normalmente cria um thread separado do qual chamar o método do [método ForceGC](icorprofilerinfo-forcegc-method.md) . Isso não é nada novo. Mas o que pode ser surpreendente é que o ato de fazer uma coleta de lixo dentro de um aplicativo da Windows Store pode transformar seu thread em um thread gerenciado (por exemplo, uma API de criação de perfil ThreadID será criada para esse thread).

Para entender as consequências disso, é importante entender as diferenças entre chamadas síncronas e assíncronas, conforme definido pela API de criação de perfil do CLR. Observe que isso é muito diferente do conceito de chamadas assíncronas em aplicativos da Windows Store. Consulte a postagem de blog [por que temos CORPROF_E_UNSUPPORTED_CALL_SEQUENCE](https://docs.microsoft.com/archive/blogs/davbr/why-we-have-corprof_e_unsupported_call_sequence) para obter mais informações.

O ponto relevante é que as chamadas feitas em threads criados por seu criador de perfil são sempre consideradas síncronas, mesmo que essas chamadas sejam feitas de fora de uma implementação de um dos métodos [ICorProfilerCallback](icorprofilercallback-interface.md) da dll do criador de perfil. Pelo menos, isso costumava ser o caso. Agora que o CLR transformou o thread do criador de perfil em um thread gerenciado devido à sua chamada para o [método ForceGC](icorprofilerinfo-forcegc-method.md), esse thread não é mais considerado o thread do criador de perfil. Assim, o CLR impõe uma definição mais estrita do que é qualificado como síncrono para esse thread – ou seja, uma chamada deve se originar de dentro de um dos métodos [ICorProfilerCallback](icorprofilercallback-interface.md) da dll do criador de perfil para se qualificar como síncrona.

O que isso significa na prática? A maioria dos métodos de [ICorProfilerInfo](icorprofilerinfo-interface.md) só é segura para ser chamada de forma síncrona e, caso contrário, falhará imediatamente. Portanto, se a DLL do criador de perfil reutilizasse seu thread do [método ForceGC](icorprofilerinfo-forcegc-method.md) para outras chamadas normalmente feitas em threads criados pelo criador de perfil (por exemplo, para [RequestProfilerDetach](icorprofilerinfo3-requestprofilerdetach-method.md), [RequestReJIT](icorprofilerinfo4-requestrejit-method.md)ou [RequestRevert](icorprofilerinfo4-requestrevert-method.md)), você terá problemas. Até mesmo uma função assíncrona segura, como o [DoStackSnapshot](icorprofilerinfo2-dostacksnapshot-method.md) , tem regras especiais quando chamada de threads gerenciados. (Consulte a postagem de blog [pilha do criador de perfil: Noções básicas e além](https://docs.microsoft.com/archive/blogs/davbr/profiler-stack-walking-basics-and-beyond) de mais informações.)

Portanto, é recomendável que qualquer thread que sua DLL do criador de perfil crie para chamar o [método ForceGC](icorprofilerinfo-forcegc-method.md) deve ser usado *apenas* com a finalidade de disparar GCS e, em seguida, responder aos retornos de chamada do GC. Ele não deve chamar a API de criação de perfil para executar outras tarefas, como amostragem de pilha ou desanexação.

### <a name="conditionalweaktablereferences"></a>ConditionalWeakTableReferences

A partir do .NET Framework 4,5, há um novo retorno de chamada de GC, [ConditionalWeakTableElementReferences](icorprofilercallback5-conditionalweaktableelementreferences-method.md), que fornece ao criador de perfil informações mais completas sobre *identificadores dependentes*. Esses identificadores efetivamente adicionam uma referência de um objeto de origem a um objeto de destino para fins de gerenciamento de vida útil de GC. Os identificadores dependentes não são novos e os desenvolvedores que programam em código gerenciado têm sido capazes de criar seus próprios identificadores dependentes usando a <xref:System.Runtime.CompilerServices.ConditionalWeakTable%602?displayProperty=nameWithType> classe mesmo antes do Windows 8 e do .NET Framework 4,5.

No entanto, os aplicativos XAML gerenciados da Windows Store agora fazem uso intensivo de identificadores dependentes. Em particular, o CLR os utiliza para auxiliar no gerenciamento de ciclos de referência entre objetos gerenciados e objetos Windows Runtime não gerenciados. Isso significa que é mais importante do que nunca para os infileres de memória serem informados sobre esses identificadores dependentes, para que possam ser visualizados junto com o restante das bordas no grafo de heap. A DLL do criador de perfil deve usar [RootReferences2](icorprofilercallback2-rootreferences2-method.md), [objectreferenciations](icorprofilercallback-objectreferences-method.md)e [ConditionalWeakTableElementReferences](icorprofilercallback5-conditionalweaktableelementreferences-method.md) juntos para formar uma exibição completa do grafo de heap.

## <a name="conclusion"></a>Conclusão

É possível usar a API de criação de perfil do CLR para analisar o código gerenciado em execução dentro dos aplicativos da Windows Store. Na verdade, você pode usar um criador de perfil existente que está desenvolvendo e fazer algumas alterações específicas para que ele possa direcionar os aplicativos da Windows Store. Sua interface do usuário do profiler deve usar as novas APIs para ativar o aplicativo da Windows Store no modo de depuração. Verifique se a DLL do criador de perfil consome apenas as APIs aplicáveis para aplicativos da Windows Store. O mecanismo de comunicação entre a DLL do criador de perfil e a interface do usuário do profiler deve ser escrito com as restrições da API do aplicativo da Windows Store em mente e com a conscientização das permissões restritas em vigor para aplicativos da Windows Store Sua DLL do criador de perfil deve estar ciente de como o CLR trata WinMDs e como o comportamento do coletor de lixo é diferente em relação a threads gerenciados.

## <a name="resources"></a>Recursos

**O Common Language Runtime**

- [Criação de perfil (referência de API não gerenciada)](index.md)

- [Metadados (referência de API não gerenciada)](../metadata/index.md)

**A interação do CLR com o Windows Runtime**

- [Suporte do .NET Framework para Aplicativos da Windows Store e Windows Runtime](../../../standard/cross-platform/support-for-windows-store-apps-and-windows-runtime.md)

**Aplicativos da Windows Store**

- [Acesso a arquivos e permissões (Windows Runtime aplicativos](https://docs.microsoft.com/previous-versions/windows/apps/hh967755%28v=win.10%29)

- [Obter uma licença de desenvolvedor](https://docs.microsoft.com/previous-versions/windows/apps/hh974578%28v=win.10%29)

- [Interface IPackageDebugSettings](/windows/desktop/api/shobjidl_core/nn-shobjidl_core-ipackagedebugsettings)
