---
title: Fuslogvw.exe (Visualizador do Log de Associações de Assembly)
description: Use Fuslogvw.exe, o Visualizador de log de associação de assembly. Este visualizador mostra detalhes de associação de assembly, que ajuda a diagnosticar porque o .NET não pode encontrar um assembly em tempo de execução.
ms.date: 03/30/2017
helpviewer_keywords:
- failed assembly binds
- Fuslogvw.exe
- displaying failed assembly bind details
- assemblies [.NET Framework], failed assembly binds
- locating assemblies
- Assembly Binding Log Viewer
ms.assetid: e32fa443-0778-4cc3-bf36-5c8ea297d296
ms.openlocfilehash: 949f9cf98d5eb4e100be9837be120038f085cc40
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87167122"
---
# <a name="fuslogvwexe-assembly-binding-log-viewer"></a>Fuslogvw.exe (Visualizador do Log de Associações de Assembly)

O Visualizador de Log de Associação do Assembly exibe detalhes das associações de assembly. Essas informações ajudam a diagnosticar por que o .NET Framework não pode localizar um assembly no tempo de execução. Essas falhas normalmente são o resultado de um assembly implantado no local incorreto, de uma imagem nativa que não é mais válida ou de uma incompatibilidade em números de versão ou culturas. A falha no Common Language Runtime em localizar um assembly costuma aparecer como um <xref:System.TypeLoadException> em seu aplicativo.

> [!IMPORTANT]
> Você deve executar fuslogvw.exe com privilégios de administrador.

Essa ferramenta é instalada automaticamente com o Visual Studio. Para executar a ferramenta, use o Prompt de Comando do Desenvolvedor para Visual Studio (ou o Prompt de Comando do Visual Studio no Windows 7) com credenciais de administrador. Para obter mais informações, consulte [Prompts de Comando](developer-command-prompt-for-vs.md).

No prompt de comando, digite o seguinte:

```console
fuslogvw
```

O visualizador exibe uma entrada para cada associação do assembly com falha. Para cada falha, o visualizador descreve o aplicativo que iniciou a associação; o assembly de associação, incluindo nome, versão, cultura e chave pública; e a data e a hora da falha.

### <a name="to-change-the-log-location-view"></a>Para alterar a exibição do local do log

1. Selecione o botão de opção **Padrão** para exibir falhas de associação de todos os tipos de aplicativo. Por padrão, as entradas de log são armazenadas em diretórios por usuário em disco no cache de wininet.

2. Selecione o botão de opção **Personalizar** para exibir falhas de associação em um diretório personalizado especificado. Você deve especificar o local personalizado no qual deseja que o runtime armazene os logs definindo o local de log personalizado na caixa de diálogo **Configurações de Log** como um nome de diretório válido. Esse diretório deve estar limpo e conter apenas arquivos gerados pelo runtime. Se ele contiver um executável que gere uma falha a ser registrada em log, a falha não será registrada em log porque a ferramenta tenta criar um diretório com o mesmo nome do executável. Além disso, haverá falha em uma tentativa de executar um executável com base no local do log.

    > [!NOTE]
    > O local de associação padrão é preferível ao local de associação personalizado. O tempo de execução armazena o local de ligação padrão no cache do WinInet e, portanto, o limpa automaticamente. Se você especificar um local de ligação personalizado, será responsável por limpá-lo.

### <a name="to-view-details-about-a-specific-failure"></a>Para exibir detalhes sobre uma falha específica

1. Selecione o nome do aplicativo da entrada desejada no visualizador.

2. Clique no botão **Exibir Log**. Também é possível clicar duas vezes na entrada selecionada.

    A ferramenta exibe os seguintes detalhes sobre a falha de associação selecionada:

    - Um motivo específico para a falha na associação como, por exemplo, "arquivo não encontrado" ou "incompatibilidade versão".

    - Informações sobre o aplicativo que iniciou a associação, incluindo seu nome, o diretório raiz do aplicativo (AppBase) e uma descrição do caminho de pesquisa privado, se houver uma.

    - A identidade do assembly que a ferramenta está procurando.

    - Uma descrição de qualquer política da versão Aplicativo, Editor ou Administrador aplicada.

    - Se o assembly estava ou não no [cache de assembly global](../app-domains/gac.md).

    - Uma lista de todas as URLs de sondagem.

A entrada do log de exemplo a seguir mostra informações detalhadas sobre uma associação do assembly com falha.

```output
*** Assembly Binder Log Entry  (3/5/2007 @ 12:54:20 PM) ***

The operation failed.
Bind result: hr = 0x80070002. The system cannot find the file specified.

Assembly manager loaded from:  C:\WINNT\Microsoft.NET\Framework\v2.0.50727\fusion.dll
Running under executable  C:\Program Files\Microsoft.NET\FrameworkSDK\Samples\Tutorials\resourcesandlocalization\graphic\cs\graphicfailtest.exe
--- A detailed error log follows.

=== Pre-bind state information ===
LOG: DisplayName = graphicfailtest.resources, Version=0.0.0.0, Culture=en-US, PublicKeyToken=null
 (Fully-specified)
LOG: Appbase = C:\Program Files\Microsoft.NET\FrameworkSDK\Samples\Tutorials\resourcesandlocalization\graphic\cs\
LOG: Initial PrivatePath = NULL
LOG: Dynamic Base = NULL
LOG: Cache Base = NULL
LOG: AppName = NULL
Calling assembly : graphicfailtest, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null.
===

LOG: Processing DEVPATH.
LOG: DEVPATH is not set. Falling through to regular bind.
LOG: Policy not being applied to reference at this time (private, custom, partial, or location-based assembly bind).
LOG: Post-policy reference: graphicfailtest.resources, Version=0.0.0.0, Culture=en-US, PublicKeyToken=null
LOG: Attempting download of new URL file:///C:/Program Files/Microsoft.NET/FrameworkSDK/Samples/Tutorials/resourcesandlocalization/graphic/cs/graphicfailtest.resources.DLL.
LOG: Attempting download of new URL file:///C:/Program Files/Microsoft.NET/FrameworkSDK/Samples/Tutorials/resourcesandlocalization/graphic/cs/graphicfailtest.resources/graphicfailtest.resources.DLL.
LOG: Attempting download of new URL file:///C:/Program Files/Microsoft.NET/FrameworkSDK/Samples/Tutorials/resourcesandlocalization/graphic/cs/graphicfailtest.resources.EXE.
LOG: Attempting download of new URL file:///C:/Program Files/Microsoft.NET/FrameworkSDK/Samples/Tutorials/resourcesandlocalization/graphic/cs/graphicfailtest.resources/graphicfailtest.resources.EXE.
LOG: All probing URLs attempted and failed.
```

### <a name="to-delete-a-single-entry-from-the-log"></a>Para excluir uma única entrada do log

1. Selecione uma entrada no visualizador.

2. Clique no botão **Excluir Entrada**.

### <a name="to-delete-all-entries-from-the-log"></a>Para excluir todas as entradas do log

- Clique no botão **Excluir Tudo**.

### <a name="to-refresh-the-user-interface"></a>Para atualizar a interface do usuário

- Clique no botão **Atualizar** . O visualizador não detecta automaticamente novas entradas de log durante a execução. Você deve usar o botão **Atualizar** para exibi-las.

### <a name="to-change-the-log-settings"></a>Para alterar as configurações de log.

- Clique no botão **Configurações** para abrir a caixa de diálogo **Configurações de Log**.

### <a name="to-view-the-about-dialog"></a>Para exibir a caixa de diálogo Sobre

- Clique no botão **Sobre**.

## <a name="binding-logs-for-native-images"></a>Associando Logs para Imagens Nativas

Por padrão, Fuslogvw.exe registra em log solicitações de associação normais. Também é possível registrar em log associações de assembly para imagens nativas criadas usando o [Ngen.exe (Gerador de Imagens Nativas)](ngen-exe-native-image-generator.md).

#### <a name="to-log-assembly-binds-for-native-images"></a>Para registrar em log associações de assembly para imagens nativas

- No grupo **Categorias de Log**, selecione o botão de opção **Imagens Nativas**.

O log a seguir mostra uma falha causada por uma dependência que não existia quando a imagem nativa foi criada para o aplicativo. Se as dependências no tempo de execução forem diferentes das dependências durante a execução de Ngen.exe, a associação a uma imagem nativa não será permitida.

```output
*** Assembly Binder Log Entry  (12/8/2006 @ 5:22:07 PM) ***

The operation failed.
Bind result: hr = 0x80070002. The system cannot find the file specified.

Assembly manager loaded from:  E:\Windows\Microsoft.NET\Framework64\v2.0.50727\mscorwks.dll
Running under executable  E:\test\App.exe
--- A detailed error log follows.

LOG: Start binding of native image App, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null.
LOG: IL assembly loaded from E:\test\App.exe.
LOG: Start validating native image App, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null.
LOG: Start validating all the dependencies.
LOG: [Level 1]Start validating native image dependency mscorlib, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089.
LOG: Dependency evaluation succeeded.
LOG: [Level 1]Start validating IL dependency b, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null.
WRN: Dependency assembly was not found at ngen time, but is found at binding time. Disallow using this native image.
WRN: No matching native image found.
LOG: Bind to native image assembly did not succeed. Use IL image.
```

O log seguir mostra uma associação de imagem nativa com falha ocorrida porque as configurações de segurança no computador quando o aplicativo foi executado eram diferentes das configurações de segurança no momento em que a imagem nativa foi criada.

```output
*** Assembly Binder Log Entry  (12/8/2006 @ 5:29:09 PM) ***

The operation failed.
Bind result: hr = 0x80004005. Unspecified error

Assembly manager loaded from:  E:\Windows\Microsoft.NET\Framework64\v2.0.50727\mscorwks.dll
Running under executable  E:\test\Application101622.exe
--- A detailed error log follows.

LOG: Start binding of native image Application101622, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null.
LOG: IL assembly loaded from E:\test\Application101622.exe.
LOG: Start validating native image Application101622, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null.
LOG: Start validating all the dependencies.
LOG: [Level 1]Start validating native image dependency mscorlib, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089.
LOG: Dependency evaluation succeeded.
LOG: [Level 1]Start validating IL dependency Dependency101622, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null.
LOG: Dependency evaluation succeeded.
LOG: Validation of dependencies succeeded.
LOG: Start loading all the dependencies into load context.
LOG: Loading of dependencies succeeded.
LOG: Bind to native image succeeded.
Native image has correct version information.
Attempting to use native image E:\Windows\assembly\NativeImages_v2.0.50727_64\Application101622\1ac7fadabec4f72575d807501e9fdc72\Application101622.ni.exe.
Rejecting native image because it failed the security check. The assembly's permissions must have changed since the time it was ngenned, or it is running with a different security context.
Discarding native image.
```

## <a name="the-log-settings-dialog"></a>A caixa de diálogo Configurações de Log

É possível usar a caixa de diálogo **Configurações de Log** para executar as ações a seguir.

#### <a name="to-disable-logging"></a>Para desabilitar o registro em log

- Selecione o botão de opção **Log desabilitado**.  Essa opção não permanece selecionada por padrão.

#### <a name="to-log-assembly-binds-in-exceptions"></a>Para registrar em log associações de assembly em exceções

- Selecione o botão de opção **Registrar em log o texto de exceção**. Apenas as informações de log da fusão menos detalhadas são registradas em log em texto de exceção. Para exibir informações completas, use uma das outras configurações.

  Consulte a observação Importante a respeito de assemblies carregados como tendo domínio neutro.

#### <a name="to-log-assembly-bind-failures"></a>Para registrar em log falhas na associação do assembly

- Selecione o botão de opção **Registrar falhas na associação em disco**.

  Consulte a observação Importante a respeito de assemblies carregados como tendo domínio neutro.

#### <a name="to-log-all-assembly-binds"></a>Para registrar em log todas as associações de assembly

- Selecione o botão de opção **Registrar todas as associações em disco**.

  Consulte a observação Importante a respeito de assemblies carregados como tendo domínio neutro.

> [!IMPORTANT]
> Quando um assembly é carregado como tendo domínio neutro, por exemplo, definindo-se a propriedade <xref:System.AppDomainSetup.LoaderOptimization%2A> como <xref:System.LoaderOptimization.MultiDomain?displayProperty=nameWithType> ou a <xref:System.LoaderOptimization.MultiDomainHost?displayProperty=nameWithType>, a ativação do registro em log pode causar perda de memória em alguns casos. Isso poderá acontecer se uma entrada de log for feita quando um módulo de domínio neutro for carregado em um domínio do aplicativo e, posteriormente, o domínio do aplicativo for descarregado. A entrada de log talvez não seja liberada até o término do processo. Alguns depuradores ativam o registro em log automaticamente.

#### <a name="to-enable-a-custom-log-path"></a>Para habilitar um caminho de log personalizado

1. Selecione o botão de opção **Habilitar caminho de log personalizado**.

2. Digite o caminho na caixa de texto **Caminho de log personalizado**.

> [!NOTE]
> O [Visualizador de Log de Associação de Assembly (Fuslogvw.exe)](fuslogvw-exe-assembly-binding-log-viewer.md) usa o cache do IE (Internet Explorer) para armazenar seu log de associação. Devido a um dano ocasional no cache do IE, o [Visualizador de Log de Associação de Assembly (Fuslogvw.exe)](fuslogvw-exe-assembly-binding-log-viewer.md) às vezes pode parar de mostrar os novos logs de associação na janela de exibição. Por conta desse dano, a infraestrutura de associação do .NET (fusão) não pode gravar no ou ler do log de associação. (Esse problema não será encontrado se você usar um caminho de log personalizado.)  Para corrigir o dano e permitir que a fusão mostre logs de associação novamente, limpe o cache do IE excluindo arquivos de Internet temporários de dentro da caixa de diálogo Opções da Internet do IE.
>
> Se o aplicativo não gerenciado hospedar o Common Language Runtime implementando as interfaces `IHostAssemblyManager` e `IHostAssemblyStore`, as entradas de log não poderão ser armazenadas no cache de wininet.  Para exibir entradas de log para hosts personalizadas que implementam essas interfaces, você deve especificar um caminho de log alternativo.

#### <a name="to-enable-logging-for-apps-running-in-the-windows-app-container"></a>Para habilitar o registro em log para aplicativos em execução no contêiner do aplicativo do Windows

1. Habilite um caminho de log personalizado, conforme descrito no procedimento anterior. Por padrão, os aplicativos em execução no contêiner do aplicativo do Windows têm acesso limitado no disco rígido. O diretório especificado terá acesso de leitura/gravação em todos os aplicativos no contêiner do aplicativo.

2. Marque a caixa de seleção **Habilitar registro em log imersivo**.

    > [!NOTE]
    > Essa caixa só está habilitada no Windows 8 ou posterior.

## <a name="see-also"></a>Confira também

- <xref:System.TypeLoadException>
- [Ferramentas](index.md)
- [Cache de assemblies global](../app-domains/gac.md)
- [Como o runtime localiza assemblies](../deployment/how-the-runtime-locates-assemblies.md)
- [Prompts de comando](developer-command-prompt-for-vs.md)
