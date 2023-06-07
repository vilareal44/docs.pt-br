---
title: Novidades do .NET Core 3.0
description: Conheça os novos recursos encontrados no .NET Core 3.0.
dev_langs:
- csharp
author: adegeo
ms.author: adegeo
ms.date: 01/27/2020
ms.openlocfilehash: bf712e88d96a5c2c80c3ff50283d44e9c7717abb
ms.sourcegitcommit: cbb19e56d48cf88375d35d0c27554d4722761e0d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88608213"
---
# <a name="whats-new-in-net-core-30"></a>Novidades do .NET Core 3.0

Este artigo descreve o que há de novo no .NET Core 3,0. Um dos maiores avanços é o suporte para aplicativos Windows de área de trabalho (somente Windows). Usando a Área de Trabalho do Windows do componente de SDK do .NET Core 3.0, você pode portar seus aplicativos Windows Forms e WPF (Windows Presentation Foundation). Para deixar claro, o componente Windows Desktop só é compatível com o Windows e só é incluído nele. Para obter mais informações, consulte a seção [Área de Trabalho do Windows](#windows-desktop) mais adiante neste artigo.

O .NET Core 3.0 adiciona suporte para C# 8.0. É altamente recomendável que você use o [Visual Studio 2019 versão 16,3](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) ou mais recente, [Visual Studio para Mac 8,3](/visualstudio/mac/install-preview) ou mais recente ou [Visual Studio Code](https://code.visualstudio.com/) com a **extensão C#** mais recente.

[Baixe e comece a usar o .NET Core 3,0](https://aka.ms/netcore3download) agora no Windows, no MacOS ou no Linux.

Para obter mais informações sobre a versão, consulte o [anúncio do .NET Core 3,0](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-0/).

O .NET Core RC1 foi considerado pronto para produção pela Microsoft e foi totalmente suportado. Se você estiver usando uma versão de visualização, deverá mover para a versão RTM para obter suporte contínuo.

## <a name="language-improvements-c-80"></a>Aprimoramentos na linguagem C# 8,0

O C# 8,0 também faz parte desta versão, que inclui o recurso de [tipos de referência anulável](../../csharp/tutorials/nullable-reference-types.md) , [fluxos assíncronos](../../csharp/tutorials/generate-consume-asynchronous-stream.md)e [mais padrões](../../csharp/tutorials/pattern-matching.md). Para obter mais informações sobre recursos do C# 8.0, consulte [Novidades do C# 8.0](../../csharp/whats-new/csharp-8.md).

Foram adicionados aprimoramentos de linguagem para dar suporte aos seguintes recursos de API detalhados abaixo:

- [Intervalos e índices](#ranges-and-indices)
- [Fluxos assíncronos](#async-streams)

## <a name="net-standard-21"></a>.NET Standard 2.1

O .NET Core 3,0 implementa **.NET Standard 2,1**. No entanto, o `dotnet new classlib` modelo padrão gera um projeto que ainda tem como alvo **.net Standard 2,0**. Para direcionar ao **.NET Standard 2.1**, edite seu arquivo de projeto e altere a propriedade `TargetFramework` para `netstandard2.1`:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>netstandard2.1</TargetFramework>
  </PropertyGroup>

</Project>
```

Se você estiver usando o Visual Studio, precisará do [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019), já que o Visual Studio 2017 não dá suporte ao **.NET Standard 2.1** nem ao **.NET Core 3.0**.

## <a name="compiledeploy"></a>Compilar/implantar

### <a name="default-executables"></a>Executáveis por padrão

O .NET Core agora compila [executáveis dependentes de estrutura](../deploying/index.md#publish-framework-dependent) por padrão. Esse comportamento é novo para aplicativos que usam uma versão do .NET Core instalada globalmente. Anteriormente, apenas [implantações autocontidas](../deploying/index.md#publish-self-contained) produziam um executável.

Durante `dotnet build` ou `dotnet publish` , um executável (conhecido como **appHost**) é criado que corresponde ao ambiente e à plataforma do SDK que você está usando. Você pode esperar desses executáveis o mesmo que de outros executáveis nativos, como:

- Você pode clicar duas vezes no arquivo executável.
- Você pode iniciar o aplicativo diretamente de um prompt de comando, como `myapp.exe` no Windows e `./myapp` no Linux e macOS.

### <a name="macos-apphost-and-notarization"></a>macOS appHost e notarization

*somente macOS*

A partir do notarized SDK do .NET Core 3,0 para macOS, a configuração para produzir um executável padrão (conhecida como appHost) é desabilitada por padrão. Para obter mais informações, consulte [o notarization Catalina do MacOS e o impacto sobre os downloads e projetos do .NET Core](../install/macos-notarization-issues.md).

Quando a configuração appHost está habilitada, o .NET Core gera um executável de Mach-O nativo quando você cria ou publica. Seu aplicativo é executado no contexto do appHost quando ele é executado do código-fonte com o `dotnet run` comando ou iniciando o executável de Mach-o diretamente.

Sem o appHost, a única maneira como um usuário pode iniciar um aplicativo [dependente da estrutura](../deploying/index.md#publish-framework-dependent) é com o `dotnet <filename.dll>` comando. Um appHost é sempre criado quando você publica [seu aplicativo independente](../deploying/index.md#publish-self-contained).

Você pode configurar o appHost no nível do projeto ou alternar o appHost para um `dotnet` comando específico com o `-p:UseAppHost` parâmetro:

- Arquivo de projeto

  ```xml
  <PropertyGroup>
    <UseAppHost>true</UseAppHost>
  </PropertyGroup>
  ```

- Parâmetro de linha de comando

  ```dotnetcli
  dotnet run -p:UseAppHost=true
  ```

Para obter mais informações sobre a `UseAppHost` configuração, consulte [Propriedades do MSBuild para Microsoft. net. SDK](../project-sdk/msbuild-props.md#useapphost).

### <a name="single-file-executables"></a>Executáveis de arquivo único

O comando `dotnet publish` dá suporte ao empacotamento de seu aplicativo em um executável de arquivo único específico da plataforma. O executável é autoextraível e contém todas as dependências (incluindo nativas) necessárias para a execução do aplicativo. Quando o aplicativo é executado pela primeira vez, o aplicativo é extraído para um diretório com base no nome do aplicativo e no identificador do build. A inicialização é mais rápida quando o aplicativo é executado novamente. O aplicativo não precisará autoextrair uma segunda vez, a menos que uma versão nova tenha sido usada.

Para publicar um único arquivo executável, defina o `PublishSingleFile` em seu projeto ou na linha de comando com o comando `dotnet publish`:

```xml
<PropertyGroup>
  <RuntimeIdentifier>win10-x64</RuntimeIdentifier>
  <PublishSingleFile>true</PublishSingleFile>
</PropertyGroup>
```

-ou-

```dotnetcli
dotnet publish -r win10-x64 -p:PublishSingleFile=true
```

Para obter mais informações sobre a publicação de arquivo único, consulte o [documento de design de empacotador de arquivo único](https://github.com/dotnet/designs/blob/master/accepted/2020/single-file/design.md).

### <a name="assembly-linking"></a>Vinculação de assembly

O SDK do .NET Core 3.0 vem com uma ferramenta que pode reduzir o tamanho dos aplicativos analisando a IL e cortando assemblies não utilizados.

Os aplicativos autossuficientes incluem todos os componentes necessários para executar seu código, sem exigir que o .NET seja instalado no computador host. No entanto, muitas vezes o aplicativo requer apenas um pequeno subconjunto da estrutura para funcionar, e outras bibliotecas não utilizadas podem ser removidas.

O .NET Core agora inclui uma configuração que usará a ferramenta[Vinculador de IL](https://github.com/mono/linker) para verificar a IL do seu aplicativo. Essa ferramenta detecta o código necessário e, em seguida, corta as bibliotecas não utilizadas. Ela pode reduzir significativamente o tamanho da implantação de alguns aplicativos.

Para habilitá-la, adicione a configuração `<PublishTrimmed>` ao seu projeto e publique um aplicativo autossuficiente:

```xml
<PropertyGroup>
  <PublishTrimmed>true</PublishTrimmed>
</PropertyGroup>
```

```dotnetcli
dotnet publish -r <rid> -c Release
```

Por exemplo, o modelo básico de novo projeto de console "hello world" incluído, quando publicado, atinge cerca de 70 MB de tamanho. Usando `<PublishTrimmed>`, esse tamanho é reduzido para cerca de 30 MB.

É importante considerar que os aplicativos ou estruturas (incluindo ASP.NET Core e WPF) que usam reflexão ou recursos dinâmicos relacionados, geralmente são interrompidos quando cortados. Essa interrupção ocorre porque o vinculador não tem ciência desse comportamento dinâmico e não pode determinar quais tipos de estrutura são necessários para reflexão. A ferramenta Vinculador de IL pode ser configurada para estar ciente deste cenário.

Antes de mais nada, teste seu aplicativo depois de cortar.

Para saber mais sobre a ferramenta Vinculador de IL, confira a [documentação](https://aka.ms/dotnet-illink) ou visite o repositório [mono/linker]( https://github.com/mono/linker).

### <a name="tiered-compilation"></a>Compilação em camadas

A TC ([compilação em camadas](https://github.com/dotnet/runtime/blob/master/docs/design/features/tiered-compilation.md)) está ativa por padrão com o .NET Core 3.0. Esse recurso permite que o tempo de execução use de forma mais adaptável o compilador JIT (just-in-time) para obter um melhor desempenho.

O principal benefício da compilação em camadas é fornecer duas maneiras de métodos de jitting: em uma camada de qualidade inferior, mas mais rápida, ou em uma camada de qualidade superior, mas mais lenta. A qualidade refere-se ao quão bem o método é otimizado. O TC ajuda a melhorar o desempenho de um aplicativo à medida que passa por vários estágios de execução, desde a inicialização até o estado estável. Quando a compilação em camadas é desabilitada, cada método é compilado de uma única maneira que é ajustada ao desempenho de estado estável no desempenho de inicialização.

Quando TC está habilitado, o comportamento a seguir se aplica à compilação do método quando um aplicativo é inicializado:

- Se o método tiver código com compilação antecipada de tempo ou [ReadyToRun](#readytorun-images), o código gerado previamente será usado.
- Caso contrário, o método é JIT. Normalmente, esses métodos são genéricos sobre tipos de valor.
  - O *Quick JIT* produz código mais rápido (ou menos otimizado) com mais rapidez. No .NET Core 3,0, o JIT rápido é habilitado por padrão para métodos que não contêm loops e são preferenciais durante a inicialização.
  - O JIT de Otimização total produz um código de qualidade mais alta (ou mais otimizado) mais lentamente. Para métodos em que o JIT rápido não seria usado (por exemplo, se o método for atribuído com <xref:System.Runtime.CompilerServices.MethodImplOptions.AggressiveOptimization?displayProperty=nameWithType> ), o JIT de Otimização total será usado.

Para métodos chamados com frequência, o compilador just-in-time, eventualmente, cria código totalmente otimizado em segundo plano. Em seguida, o código otimizado substitui o código pré-compilado para esse método.

O código gerado pelo Quick JIT pode ser executado mais lentamente, alocar mais memória ou usar mais espaço de pilha. Se houver problemas, você poderá desabilitar o JIT rápido usando essa propriedade do MSBuild no arquivo de projeto:

```xml
<PropertyGroup>
  <TieredCompilationQuickJit>false</TieredCompilationQuickJit>
</PropertyGroup>
```

Para desabilitar completamente o TC, use essa propriedade do MSBuild em seu arquivo de projeto:

```xml
<PropertyGroup>
  <TieredCompilation>false</TieredCompilation>
</PropertyGroup>
```

> [!TIP]
> Se você alterar essas configurações no arquivo de projeto, talvez seja necessário executar uma compilação limpa para que as novas configurações sejam refletidas (exclua `obj` os `bin` diretórios e e recompile).

Para obter mais informações sobre como configurar a compilação em tempo de execução, consulte [Opções de configuração de tempo de execução para compilação](../run-time-config/compilation.md).

### <a name="readytorun-images"></a>Imagens ReadyToRun

Você pode melhorar o tempo de inicialização do seu aplicativo .NET Core compilando seus assemblies de aplicativos como o formato ReadyToRun (R2R). R2R é uma forma de compilação antecipada (AOT).

Os binários R2R melhoram o desempenho de inicialização reduzindo a quantidade de trabalho que o compilador just-in-time (JIT) precisa fazer à medida que seu aplicativo é carregado. Os binários contêm código nativo similar comparado ao que o JIT produziria. Entretanto, os binários R2R são maiores porque contêm código de IL (linguagem intermediária), que ainda é necessário para alguns cenários, e a versão nativa do mesmo código. O R2R só está disponível quando você publica um aplicativo autocontido que tenha como alvo um RID (Runtime Environment) específico, como o Linux x64 ou o Windows x64.

Para compilar seu projeto como ReadyToRun, faça o seguinte:

01. Adicione a `<PublishReadyToRun>` configuração ao seu projeto:

    ```xml
    <PropertyGroup>
      <PublishReadyToRun>true</PublishReadyToRun>
    </PropertyGroup>
    ```

01. Publique um aplicativo autossuficiente. Por exemplo, esse comando cria um aplicativo autossuficiente para a versão de 64 bits do Windows:

    ```dotnetcli
    dotnet publish -c Release -r win-x64 --self-contained
    ```

#### <a name="cross-platformarchitecture-restrictions"></a>Restrições de plataforma cruzada/arquitetura

O compilador ReadyToRun atualmente não tem suporte para o direcionamento cruzado. Você precisa compilar em determinado destino. Por exemplo, se você quiser imagens R2R para Windows x64, será necessário executar o comando Publicar nesse ambiente.

Exceções ao direcionamento cruzado:

- O Windows x64 pode ser usado para compilar imagens do Windows ARM32, ARM64 e x86.
- O Windows x86 pode ser usado para compilar imagens do Windows ARM32.
- O Linux x64 pode ser usado para compilar imagens do Linux ARM32 e ARM64.

## <a name="runtimesdk"></a>Tempo de execução/SDK

### <a name="major-version-runtime-roll-forward"></a>Roll forward de tempo de execução de versão principal

O .NET Core 3.0 introduz um recurso opcional que permite que seu aplicativo efetue roll forward para a versão principal mais recente do .NET Core. Adicionalmente, foi adicionada uma nova configuração para controlar como o roll forward é aplicado ao seu aplicativo. Isso pode ser configurado das seguintes maneiras:

- Propriedade do arquivo de projeto: `RollForward`
- Propriedade do arquivo de configuração de tempo de execução: `rollForward`
- Variável de ambiente: `DOTNET_ROLL_FORWARD`
- Argumento de linha de comando: `--roll-forward`

Um dos valores a seguir precisa ser especificado. Se a configuração for omitida, **Secundária** será o padrão.

- **LatestPatch**\
Efetuar roll forward para a versão de patch mais recente. Isso desabilita o roll forward da versão secundária.
- **Secundária**\
Se a versão secundária solicitada estiver ausente, efetue roll forward para a menor versão secundária mais alta. Se a versão secundária solicitada estiver presente, a política **LatestPatch** será usada.
- **Primária**\
Se a versão principal solicitada estiver ausente, efetuar roll forward para a versão principal mais alta e a versão secundária mais baixa. Se a versão principal solicitada está presente, a política **Secundária** é usada.
- **LatestMinor**\
Efetuar roll forward para a versão secundária mais recente, mesmo se a versão secundária solicitada estiver presente. Destinado a cenários de hospedagem de componente.
- **LatestMajor**\
Efetuar roll forward para a versão principal e a secundária mais altas, mesmo se a principal solicitada estiver presente. Destinado a cenários de hospedagem de componente.
- **Desativar**\
Não efetuar roll forward. Associar somente à versão especificada. Essa política não é recomendada para uso geral, pois ela desabilita a capacidade de efetuar roll forward para os patches mais recentes. Esse valor só é recomendado para teste.

Com a exceção da configuração **Desabilitar**, todas as configurações usarão a versão de patch mais recente disponível.

Por padrão, se a versão solicitada (conforme especificado em `.runtimeconfig.json` para o aplicativo) for uma versão de lançamento, somente as versões de lançamento serão consideradas para roll-forward. Todas as versões de pré-lançamento são ignoradas. Se não houver nenhuma versão de lançamento correspondente, as versões de pré-lançamento serão levadas em conta. Esse comportamento pode ser alterado por meio da configuração `DOTNET_ROLL_FORWARD_TO_PRERELEASE=1` , caso em que todas as versões são sempre consideradas.

### <a name="build-copies-dependencies"></a>O build copia dependências

O comando `dotnet build` agora copia as dependências do NuGet para seu aplicativo do cache NuGet para a pasta de saída de build. Anteriormente, as dependências eram copiadas apenas como parte de `dotnet publish`.

Há algumas operações, como vinculação e publicação de página do razor, que ainda exigem publicação.

### <a name="local-tools"></a>Ferramentas locais

O .NET Core 3.0 apresenta ferramentas locais. As ferramentas locais são semelhantes às [ferramentas globais](../tools/global-tools.md) , mas são associadas a um local específico no disco. Ferramentas locais não estão disponíveis globalmente e são distribuídas como pacotes NuGet.

> [!WARNING]
> Se você tiver tentado ferramentas locais no .NET Core 3.0 Versão Prévia 1, tais como executar `dotnet tool restore` ou `dotnet tool install`, exclua a pasta de cache local de ferramentas. Caso contrário, as ferramentas locais não funcionarão em nenhuma versão mais recente. Essa pasta está localizada em:
>
> No macOS ou Linux: `rm -r $HOME/.dotnet/toolResolverCache`
>
> No Windows: `rmdir /s %USERPROFILE%\.dotnet\toolResolverCache`

As ferramentas locais dependem de um nome de arquivo de manifesto `dotnet-tools.json` no seu diretório atual. Esse arquivo de manifesto define as ferramentas que estarão disponíveis nessa pasta e abaixo. Você pode distribuir o arquivo de manifesto com o seu código para garantir que qualquer pessoa que trabalha com o seu código possa restaurar e usar as mesmas ferramentas.

Para ferramentas globais e locais, é necessária uma versão compatível do runtime. Muitas ferramentas que estão atualmente em NuGet.org direcionam para o runtime do .NET Core 2.1. Para instalar essas ferramentas, de forma global ou local, você ainda precisará instalar o [Runtime do NET Core 2.1](https://dotnet.microsoft.com/download/dotnet-core/2.1).

### <a name="new-globaljson-options"></a>Novas global.jsem opções

O *global.jsno* arquivo tem novas opções que fornecem mais flexibilidade quando você está tentando definir qual versão do SDK do .NET Core é usada. As novas opções são:

- `allowPrerelease`: Indica se o resolvedor do SDK deve considerar versões de pré-lançamento ao selecionar a versão do SDK a ser usada.
- `rollForward`: Indica a política de roll forward a ser usada ao selecionar uma versão do SDK, seja como um fallback quando uma versão específica do SDK estiver ausente ou como uma diretiva para usar uma versão superior.

Para obter mais informações sobre as alterações, incluindo valores padrão, valores com suporte e novas regras de correspondência, consulte [global.jsem visão geral](../tools/global-json.md).

### <a name="smaller-garbage-collection-heap-sizes"></a>Tamanhos menores de heap de coleta de lixo

O tamanho do heap do coletor de lixo padrão foi reduzido, resultando em menor uso de memória pelo .NET Core. Essa alteração se alinha melhor com o orçamento de alocação de geração 0 com os tamanhos de cache de processadores modernos.

### <a name="garbage-collection-large-page-support"></a>Suporte de página grande de coleta de lixo

Páginas grandes (também conhecidas como páginas enormes no Linux) é um recurso em que o sistema operacional é capaz de estabelecer regiões de memória maiores do que o tamanho da página nativo (geralmente 4K) para melhorar o desempenho do aplicativo que está solicitando essas páginas grandes.

O coletor de lixo agora pode ser configurado com a configuração **GCLargePages** como um recurso opcional a ser escolhido para alocar páginas grandes no Windows.

## <a name="windows-desktop--com"></a>Windows Desktop & COM

### <a name="net-core-sdk-windows-installer"></a>Windows Installer do SDK do .NET Core

O instalador MSI para Windows foi alterado do .NET Core 3.0 em diante. Os instaladores de SDK agora atualizarão versões de faixa de recurso do SDK no local. Faixas de recurso são definidas nos grupos de *centenas* na seção *patch* do número de versão. Por exemplo, **3.0._101_** e **3.0._201_** são versões em duas faixas de recurso diferentes, enquanto **3.0._101_** e **3.0._199_** estão na mesma faixa de recurso. Além disso, quando o SDK do .NET Core **3.0._101_** for instalado, o SDK do .NET Core **3.0._100_** será removido do computador se ele existir. Quando o SDK do .NET Core **3.0._200_** for instalado no mesmo computador, o SDK do .NET Core **3.0._101_** não será removido.

Para obter mais informações sobre controle de versão, consulte [Visão geral de como é o controle de versão no .NET Core](../versions/index.md).

### <a name="windows-desktop"></a>Área de trabalho do Windows

O .NET Core 3.0 dá suporte a aplicativos da Área de Trabalho do Windows usando o Windows Forms e o WPF (Windows Presentation Foundation). Essas estruturas também oferecem suporte ao uso de controles modernos e no estilo Fluent da biblioteca XAML da interface do usuário do Windows (WinUI) por meio de [Ilhas XAML](/windows/uwp/xaml-platform/xaml-host-controls).

O componente Windows Desktop faz parte do SDK do .NET Core 3.0 do Windows.

É possível criar um novo aplicativo de WPF ou Windows Forms com os seguintes comandos `dotnet`:

```dotnetcli
dotnet new wpf
dotnet new winforms
```

O Visual Studio 2019 adiciona modelos de **Novo Projeto** ao Windows Forms e WPF no .NET Core 3.0.

Para obter mais informações sobre como portar um aplicativo existente do .NET Framework, consulte [Portar projetos do WPF](../../desktop-wpf/migration/convert-project-from-net-framework.md) e [Portar projetos do Windows Forms](../porting/winforms.md).

#### <a name="winforms-high-dpi"></a>WinForms com DPI alto

Aplicativos do Windows Forms do .NET Core podem definir o modo de DPI alto com <xref:System.Windows.Forms.Application.SetHighDpiMode(System.Windows.Forms.HighDpiMode)?displayProperty=nameWithType>. O método `SetHighDpiMode` define o modo de DPI alto correspondente, a menos que a configuração tenha sido definida por outros meios, tais como `App.Manifest` ou P/Invoke antes de `Application.Run`.

Os valores `highDpiMode` possíveis, conforme expressos pelo enum <xref:System.Windows.Forms.HighDpiMode?displayProperty=nameWithType>, são:

- `DpiUnaware`
- `SystemAware`
- `PerMonitor`
- `PerMonitorV2`
- `DpiUnawareGdiScaled`

Para obter mais informações sobre os modos de DPI alto, confira [Desenvolvimento de aplicativos de área de trabalho de DPI alto no Windows](/windows/desktop/hidpi/high-dpi-desktop-application-development-on-windows).

### <a name="create-com-components"></a>Criar componentes COM

No Windows, agora você pode criar componentes gerenciados que podem ser chamados por COM. Essa funcionalidade é fundamental para usar o .NET Core com modelos de suplemento do COM e também para fornecer paridade com o .NET Framework.

Ao contrário do .NET Framework em que o *mscoree. dll* foi usado como o servidor COM, o .NET Core adicionará uma dll de inicializador nativa ao diretório *bin* quando você compilar o componente COM.

Para obter um exemplo de como criar um componente COM e consumi-lo, veja a [demonstração de COM](https://github.com/dotnet/samples/tree/master/core/extensions/COMServerDemo).

### <a name="windows-native-interop"></a>Interoperabilidade nativa do Windows

O Windows oferece uma API nativa rica na forma de APIs C simples, COM e WinRT. Embora o .NET Core dê suporte a **P/Invoke**, o .NET Core 3.0 adiciona a capacidade de **criar conjuntamente APIs COM** e **ativar APIs WinRT**. Para obter um exemplo de código, consulte a [demonstração do Excel](https://github.com/dotnet/samples/tree/master/core/extensions/ExcelDemo).

### <a name="msix-deployment"></a>Implantação do MSIX

[MSIX](https://docs.microsoft.com/windows/msix/) é um novo formato de pacote de aplicativos do Windows. Ele pode ser usado para implantar aplicativos da área de trabalho do .NET Core 3.0 no Windows 10.

O [Projeto de Empacotamento de Aplicativos do Windows](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-packaging-dot-net), disponível no Visual Studio 2019, permite criar pacotes MSIX com aplicativos .NET Core [autossuficientes](../deploying/index.md#publish-self-contained).

O arquivo de projeto do .NET Core precisa especificar os runtimes compatíveis na propriedade `<RuntimeIdentifiers>`:

```xml
<RuntimeIdentifiers>win-x86;win-x64</RuntimeIdentifiers>
```

## <a name="linux-improvements"></a>Aprimoramentos do Linux

### <a name="serialport-for-linux"></a>SerialPort para Linux

O .NET Core 3.0 fornece suporte básico para <xref:System.IO.Ports.SerialPort?displayProperty=nameWithType> no Linux.

Anteriormente, o .NET Core só dava suporte ao uso de `SerialPort` no Windows.

Para saber mais sobre o suporte limitado para a porta serial no Linux, confira o [Problema do GitHub nº 33146](https://github.com/dotnet/corefx/issues/33146).

### <a name="docker-and-cgroup-memory-limits"></a>Limites de memória do Docker e cgroup

A execução do .NET Core 3,0 no Linux com o Docker funciona melhor com limites de memória CGroup. Executar um contêiner do Docker com limites de memória, tais como `docker run -m`, altera o comportamento do .NET Core.

- Tamanho de heap do GC (coletor de lixo) padrão: máximo de 20 MB ou 75% do limite de memória no contêiner.
- O tamanho explícito pode ser definido como um número absoluto ou um percentual do limite de cgroup.
- O tamanho mínimo do segmento reservado por heap de GC é de 16 MB. Esse tamanho reduz o número de heaps que são criados em computadores.

### <a name="gpio-support-for-raspberry-pi"></a>Suporte de GPIO para o Raspberry Pi

Foram lançados dois pacotes para o NuGet que você pode usar para programação de GPIO:

- [System.Device.Gpio](https://www.nuget.org/packages/System.Device.Gpio)
- [Iot.Device.Bindings](https://www.nuget.org/packages/Iot.Device.Bindings)

Os pacotes GPIO incluem as APIs para dispositivos *GPIO*, *SPI*, *I2C* e *PWM*. O pacote de associações de IoT inclui associações de dispositivo. Para obter mais informações, veja o [repositório GitHub de dispositivos](https://github.com/dotnet/iot/blob/master/src/devices/).

### <a name="arm64-linux-support"></a>Suporte a ARM64 no Linux

O .NET Core 3.0 adiciona suporte para ARM64 para Linux. O principal caso de uso para ARM64 atualmente é em cenários de IoT. Para obter mais informações, consulte [Status do .NET Core ARM64](https://github.com/dotnet/announcements/issues/82).

[Imagens do Docker para o .NET Core ARM64](https://hub.docker.com/r/microsoft/dotnet/) estão disponíveis para Alpine, Debian e Ubuntu.

> [!NOTE]
> O suporte do Windows a **ARM64** ainda não está disponível.

## <a name="security"></a>Segurança

### <a name="tls-13--openssl-111-on-linux"></a>TLS 1.3 e OpenSSL 1.1.1 no Linux

O .NET Core agora faz proveito do [suporte a protocolo TLS 1.3 no OpenSSL 1.1.1](https://www.openssl.org/blog/blog/2018/09/11/release111/) quando esse protocolo está disponível em um determinado ambiente. Com o TLS 1.3:

- Tempos de conexão são aprimorados com um menor número de viagens de ida e volta necessárias entre o cliente e o servidor.
- Segurança aprimorada devido à remoção de vários algoritmos criptográficos obsoletos e não seguros.

Quando disponíveis, o .NET Core 3.0 usa **OpenSSL 1.1.1**, **1.1.0** ou **1.0.2** em um sistema Linux. Quando o **OpenSSL 1.1.1** está disponível, ambos os tipos <xref:System.Net.Security.SslStream?displayProperty=nameWithType> e <xref:System.Net.Http.HttpClient?displayProperty=nameWithType> usarão o **protocolo TLS 1.3** (supondo que o cliente e o servidor deem suporte ao **protocolo TLS 1.3**).

> [!IMPORTANT]
> Windows e macOS ainda não oferecem suporte a **TLS 1.3**. O .NET Core 3.0 será compatível com **TLS 1.3** nesses sistemas operacionais quando o suporte for disponibilizado.

O exemplo de C# 8.0 a seguir demonstra o .NET Core 3.0 no Ubuntu 18.10 conectando-se a <https://www.cloudflare.com>:

[!code-csharp[TLSExample](./snippets/dotnet-core-3-0/csharp/TLS.cs#TLS)]

### <a name="cryptography-ciphers"></a>Cifras de criptografia

O .NET 3.0 adiciona o suporte para as criptografias **AES-GCM** e **AES-CCM**, implementadas com <xref:System.Security.Cryptography.AesGcm?displayProperty=nameWithType> e <xref:System.Security.Cryptography.AesCcm?displayProperty=nameWithType>, respectivamente. Esses algoritmos são ambos do tipo [AEAD (criptografia autenticada com os dados de associação)](https://en.wikipedia.org/wiki/Authenticated_encryption).

O código a seguir demonstra como usar criptografia `AesGcm` para criptografar e descriptografar dados aleatórios.

[!code-csharp[AesGcm](./snippets/dotnet-core-3-0/csharp/Cipher.cs#AesGcm)]

### <a name="cryptographic-key-importexport"></a>Importar/exportar chave de criptografia

O .NET Core 3.0 dá suporte à importação e exportação de chaves públicas e privadas assimétricas de formatos padrão. Você não precisa usar um certificado X.509.

Todos os tipos principais, tais como *RSA*, *DSA*, *ECDsa* e *ECDiffieHellman*, dão suporte aos seguintes formatos:

- **Chave pública**
  - X.509 SubjectPublicKeyInfo

- **Chave privada**
  - PKCS nº 8 PrivateKeyInfo
  - PKCS nº 8 EncryptedPrivateKeyInfo

Chaves RSA também dão suporte a:

- **Chave pública**
  - PKCS nº 1 RSAPublicKey

- **Chave privada**
  - PKCS nº 1 RSAPrivateKey

Os métodos de exportação produzem dados binários codificados em DER e os métodos de importação esperam o mesmo. Se uma chave for armazenada no formato PEM compatível com texto, o chamador precisará decodificar o conteúdo em Base64 antes de chamar um método de importação.

[!code-csharp[RSA](./snippets/dotnet-core-3-0/csharp/RSA.cs#Rsa)]

Arquivos **PKCS nº 8** podem ser inspecionados com <xref:System.Security.Cryptography.Pkcs.Pkcs8PrivateKeyInfo?displayProperty=nameWithType> e **arquivos PFX/PKCS nº 12** podem ser inspecionados com <xref:System.Security.Cryptography.Pkcs.Pkcs12Info?displayProperty=nameWithType>. Arquivos **PFX/PKCS nº 12** podem ser manipulados com <xref:System.Security.Cryptography.Pkcs.Pkcs12Builder?displayProperty=nameWithType>.

## <a name="net-core-30-api-changes"></a>Alterações da API do .NET Core 3,0

### <a name="ranges-and-indices"></a>Intervalos e índices

O novo tipo <xref:System.Index?displayProperty=nameWithType> pode ser usado para indexação. É possível criar um a partir de um `int` que conta desde o início, ou com um operador `^` de prefixo (C#) que conta do final:

```csharp
Index i1 = 3;  // number 3 from beginning
Index i2 = ^4; // number 4 from end
int[] a = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
Console.WriteLine($"{a[i1]}, {a[i2]}"); // "3, 6"
```

Há também o tipo <xref:System.Range?displayProperty=nameWithType>, que consiste em dois valores `Index` (um para o início e outro para o final) e pode ser escrito com uma expressão de intervalo `x..y` (C#). Em seguida, você pode indexar com um `Range`, o que produz uma fatia:

```csharp
var slice = a[i1..i2]; // { 3, 4, 5 }
```

Para obter mais informações, consulte o [tutorial de intervalos e índices](../../csharp/tutorials/ranges-indexes.md).

### <a name="async-streams"></a>Fluxos assíncronos

O tipo <xref:System.Collections.Generic.IAsyncEnumerable%601> é uma nova versão assíncrona de <xref:System.Collections.Generic.IEnumerable%601>. A linguagem permite o uso de `await foreach` em `IAsyncEnumerable<T>` para consumir seus elementos e o uso de `yield return` neles para produzir elementos.

O exemplo a seguir demonstra a produção e o consumo de fluxos assíncronos. A instrução `foreach` é assíncrona e usa `yield return` para produzir um fluxo assíncrono para chamadores. Esse padrão (usar `yield return`) é o modelo recomendado para a produção de fluxos assíncronos.

```csharp
async IAsyncEnumerable<int> GetBigResultsAsync()
{
    await foreach (var result in GetResultsAsync())
    {
        if (result > 20) yield return result;
    }
}
```

Além de poder `await foreach`, você também pode criar iteradores assíncronos, por exemplo, um iterador que retorne um `IAsyncEnumerable/IAsyncEnumerator` em que é possível aplicar `await` e `yield`. Para objetos que precisam ser descartados, você pode usar `IAsyncDisposable`, que vários tipos BCL implementam, como `Stream` e `Timer`.

Para obter mais informações, consulte o [tutorial de fluxos assíncronos](../../csharp/tutorials/generate-consume-asynchronous-stream.md).

### <a name="ieee-floating-point"></a>Ponto flutuante de IEEE

APIs de ponto flutuante estão sendo atualizadas para entrar em conformidade com a [revisão IEEE 754-2008](https://en.wikipedia.org/wiki/IEEE_754-2008_revision). O objetivo dessas alterações é expor todas as operações **necessárias** e garantir que elas sejam compatíveis de forma comportamental com a especificação IEEE. Para obter mais informações sobre melhorias de ponto flutuante, consulte a postagem de [ponto flutuante e aprimoramentos de formatação no blog do .NET Core 3,0](https://devblogs.microsoft.com/dotnet/floating-point-parsing-and-formatting-improvements-in-net-core-3-0/) .

As correções de análise e formatação incluem:

- Analisar e arredondar corretamente entradas de qualquer tamanho.
- Analisar e formatar corretamente o zero negativo.
- Analisar corretamente `Infinity` e `NaN`, fazendo uma verificação sem diferenciação de maiúsculas e minúsculas e permitindo um `+` precedente opcional onde aplicável.

Novas APIs <xref:System.Math?displayProperty=nameWithType> incluem:

- <xref:System.Math.BitIncrement(System.Double)> e <xref:System.Math.BitDecrement(System.Double)>\
Correspondem às operações `nextUp` e `nextDown` do IEEE. Retornam o menor número de ponto flutuante que compara o valor maior ou menor que a entrada (respectivamente). Por exemplo, `Math.BitIncrement(0.0)` retorna `double.Epsilon`.

- <xref:System.Math.MaxMagnitude(System.Double,System.Double)> e <xref:System.Math.MinMagnitude(System.Double,System.Double)>\
Correspondem às operações `maxNumMag` e `minNumMag` do IEEE; retornam o valor maior ou menor em magnitude das duas entradas (respectivamente). Por exemplo, `Math.MaxMagnitude(2.0, -3.0)` retorna `-3.0`.

- <xref:System.Math.ILogB(System.Double)>\
Corresponde à operação `logB` IEEE que retorna um valor integral, ele retorna o log de base 2 integral do parâmetro de entrada. Esse método é praticamente o mesmo que `floor(log2(x))`, mas feito com o mínimo de erro de arredondamento.

- <xref:System.Math.ScaleB(System.Double,System.Int32)>\
Corresponde à operação IEEE `scaleB` que usa um valor integral, ele retorna efetivamente `x * pow(2, n)`, mas é feito com o mínimo de erro de arredondamento.

- <xref:System.Math.Log2(System.Double)>\
Corresponde à operação `log2` do IEEE; retorna o logaritmo de base 2. Minimiza o erro de arredondamento.

- <xref:System.Math.FusedMultiplyAdd(System.Double,System.Double,System.Double)>\
Corresponde à operação `fma` do IEEE; executa uma adição e multiplicação fundida. Em outras palavras, realiza `(x * y) + z` como uma única operação, minimizando o erro de arredondamento. Um exemplo é `FusedMultiplyAdd(1e308, 2.0, -1e308)` , que retorna `1e308` . O `(1e308 * 2.0) - 1e308` regular retorna `double.PositiveInfinity`.

- <xref:System.Math.CopySign(System.Double,System.Double)>\
Corresponde à operação `copySign` do IEEE; retorna o valor de `x`, mas com o sinal de `y`.

### <a name="net-platform-dependent-intrinsics"></a>Intrínsecos dependentes da plataforma .NET

Foram adicionadas APIs que permitem acesso a determinadas instruções da CPU orientadas a desempenho, como o **SIMD** ou conjuntos de **instruções de manipulação de bits**. Essas instruções podem ajudar a obter melhorias significativas de desempenho em determinados cenários, tais como processamento de dados eficiente em paralelo.

Quando apropriado, as bibliotecas .NET começaram usando estas instruções para melhorar o desempenho.

Para obter mais informações, consulte [intrínsecos dependentes da plataforma .net](https://github.com/dotnet/designs/blob/master/accepted/2018/platform-intrinsics.md).

### <a name="improved-net-core-version-apis"></a>APIs de versão aprimoradas do .NET Core

Começando com o .NET Core 3.0, as APIs de versão fornecidas com o .NET Core agora retornam as informações que você espera. Por exemplo:

```csharp
System.Console.WriteLine($"Environment.Version: {System.Environment.Version}");

// Old result
//   Environment.Version: 4.0.30319.42000
//
// New result
//   Environment.Version: 3.0.0
```

```csharp
System.Console.WriteLine($"RuntimeInformation.FrameworkDescription: {System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription}");

// Old result
//   RuntimeInformation.FrameworkDescription: .NET Core 4.6.27415.71
//
// New result (notice the value includes any preview release information)
//   RuntimeInformation.FrameworkDescription: .NET Core 3.0.0-preview4-27615-11
```

> [!WARNING]
> Alteração da falha. Isso é tecnicamente uma alteração da falha, porque o esquema de controle de versão foi alterado.

### <a name="fast-built-in-json-support"></a>Suporte interno rápido a JSON

Os usuários do .NET confiam amplamente em [Newtonsoft.Js](https://www.newtonsoft.com/json) e outras bibliotecas JSON populares, que continuam a ser boas escolhas. `Newtonsoft.Json` usa cadeias de caracteres .NET como seu tipo de dados base, que é UTF-16 nos bastidores.

O novo suporte interno a JSON é alto desempenho, baixa alocação e funciona com texto JSON codificado em UTF-8. Para obter mais informações sobre o <xref:System.Text.Json> namespace e os tipos, consulte os seguintes artigos:

* [Serialização JSON no .NET-visão geral](../../standard/serialization/system-text-json-overview.md)
* [Como serializar e desserializar JSON no .net](../../standard/serialization/system-text-json-how-to.md).
* [Como migrar do Newtonsoft.Jspara o System.Text.Jsem](../../standard/serialization/system-text-json-migrate-from-newtonsoft-how-to.md)

### <a name="http2-support"></a>Suporte do HTTP/2

O tipo <xref:System.Net.Http.HttpClient?displayProperty=nameWithType> dá suporte ao protocolo HTTP/2. Se o HTTP/2 estiver habilitado, a versão do protocolo HTTP é negociada via TLS/ALPN, e o HTTP/2 é usado apenas se o servidor selecionar seu uso.

O protocolo padrão permanece HTTP/1.1, mas o HTTP/2 pode ser ativado de duas maneiras diferentes. Primeiro, você pode definir a mensagem de solicitação HTTP para usar HTTP/2:

[!code-csharp[Http2Request](./snippets/dotnet-core-3-0/csharp/http.cs#Request)]

Segundo, você pode alterar <xref:System.Net.Http.HttpClient> para usar HTTP/2 por padrão:

[!code-csharp[Http2Client](./snippets/dotnet-core-3-0/csharp/http.cs#Client)]

Muitas vezes, quando você está desenvolvendo um aplicativo, quer usar uma conexão não criptografada. Se você souber que o ponto de extremidade estará usando HTTP/2, poderá ativar conexões não criptografadas para HTTP/2. Você pode ativá-lo definindo a variável de ambiente `DOTNET_SYSTEM_NET_HTTP_SOCKETSHTTPHANDLER_HTTP2UNENCRYPTEDSUPPORT` como `1` ou ativando-a no contexto do aplicativo:

[!code-csharp[Http2Context](./snippets/dotnet-core-3-0/csharp/http.cs#AppContext)]

## <a name="next-steps"></a>Próximas etapas

- [Examine as alterações significativas entre o .NET Core 2,2 e 3,0.](../compatibility/2.2-3.0.md)
- [Examine as alterações significativas no .NET Core 3,0 para aplicativos Windows Forms.](../compatibility/winforms.md#net-core-30)
