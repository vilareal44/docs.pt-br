---
title: Propriedades do MSBuild para Microsoft. NET. Sdk
description: Referência para as propriedades e os itens do MSBuild que são compreendidos pelo SDK do .NET Core.
ms.date: 02/14/2020
ms.topic: reference
ms.openlocfilehash: 7980369b87d606d3876fe043e929a65da1d0d92b
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87916245"
---
# <a name="msbuild-reference-for-net-core-sdk-projects"></a>Referência do MSBuild para projetos de SDK do .NET Core

Esta página é uma referência para as propriedades e os itens do MSBuild que você pode usar para configurar projetos do .NET Core.

> [!NOTE]
> Esta página é um trabalho em andamento e não lista todas as propriedades de MSBuild úteis para o SDK do .NET Core. Para obter uma lista de propriedades comuns do MSBuild, consulte [Propriedades comuns do MSBuild](/visualstudio/msbuild/common-msbuild-project-properties).

## <a name="framework-properties"></a>Propriedades da estrutura

- [TargetFramework](#targetframework)
- [TargetFrameworks](#targetframeworks)
- [NetStandardImplicitPackageVersion](#netstandardimplicitpackageversion)

### <a name="targetframework"></a>TargetFramework

A `TargetFramework` propriedade especifica a versão do Framework de destino para o aplicativo. Para obter uma lista de monikers de estrutura de destino válidos, consulte [estruturas de destino em projetos em estilo SDK](../../standard/frameworks.md#supported-target-framework-versions).

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.1</TargetFramework>
</PropertyGroup>
```

Para obter mais informações, consulte [estruturas de destino em projetos em estilo SDK](../../standard/frameworks.md).

### <a name="targetframeworks"></a>TargetFrameworks

Use a `TargetFrameworks` propriedade quando desejar que seu aplicativo direcione várias plataformas. Para obter uma lista de monikers de estrutura de destino válidos, consulte [estruturas de destino em projetos em estilo SDK](../../standard/frameworks.md#supported-target-framework-versions).

> [!NOTE]
> Essa propriedade será ignorada se `TargetFramework` (singular) for especificado.

```xml
<PropertyGroup>
  <TargetFrameworks>netcoreapp3.1;net462</TargetFrameworks>
</PropertyGroup>
```

Para obter mais informações, consulte [estruturas de destino em projetos em estilo SDK](../../standard/frameworks.md).

### <a name="netstandardimplicitpackageversion"></a>NetStandardImplicitPackageVersion

> [!NOTE]
> Essa propriedade só se aplica a projetos que usam o `netstandard1.x` . Ele não se aplica a projetos que usam o `netstandard2.x` .

Use a `NetStandardImplicitPackageVersion` propriedade quando desejar especificar uma versão de estrutura inferior à versão do metapacote. O arquivo de projeto no exemplo a seguir tem `netstandard1.3` como destino, mas usa a versão 1.6.0 do `NETStandard.Library` .

```xml
<PropertyGroup>
  <TargetFramework>netstandard1.3</TargetFramework>
  <NetStandardImplicitPackageVersion>1.6.0</NetStandardImplicitPackageVersion>
</PropertyGroup>
```

## <a name="package-properties"></a>Propriedades do pacote

Você pode especificar propriedades como `PackageId` ,, `PackageVersion` , `PackageIcon` `Title` e `Description` para descrever o pacote que é criado a partir do seu projeto. Para obter informações sobre essas e outras propriedades, consulte [pacote de destino](/nuget/reference/msbuild-targets#pack-target).

```xml
<PropertyGroup>
  ...
  <PackageId>ClassLibDotNetStandard</PackageId>
  <Version>1.0.0</Version>
  <Authors>John Doe</Authors>
  <Company>Contoso</Company>
</PropertyGroup>
```

## <a name="publish-properties-and-items"></a>Publicar Propriedades e itens

- [RuntimeIdentifier](#runtimeidentifier)
- [RuntimeIdentifiers](#runtimeidentifiers)
- [TrimmerRootAssembly](#trimmerrootassembly)
- [UseAppHost](#useapphost)

### <a name="runtimeidentifier"></a>RuntimeIdentifier

A `RuntimeIdentifier` propriedade permite que você especifique um único [identificador de tempo de execução (RID)](../rid-catalog.md) para o projeto. O RID permite a publicação de uma implantação autossuficiente.

```xml
<PropertyGroup>
  <RuntimeIdentifier>ubuntu.16.04-x64</RuntimeIdentifier>
</PropertyGroup>
```

### <a name="runtimeidentifiers"></a>RuntimeIdentifiers

A `RuntimeIdentifiers` propriedade permite que você especifique uma lista delimitada por ponto-e-vírgula de [RIDs (identificadores de tempo de execução)](../rid-catalog.md) para o projeto. Use essa propriedade se você precisar publicar para vários tempos de execução. `RuntimeIdentifiers`é usado no momento da restauração para garantir que os ativos corretos estejam no grafo.

> [!TIP]
> `RuntimeIdentifier`(singular) pode fornecer compilações mais rápidas quando apenas um único tempo de execução é necessário.

```xml
<PropertyGroup>
  <RuntimeIdentifiers>win10-x64;osx.10.11-x64;ubuntu.16.04-x64</RuntimeIdentifiers>
</PropertyGroup>
```

### <a name="trimmerrootassembly"></a>TrimmerRootAssembly

O `TrimmerRootAssembly` Item permite que você exclua um assembly de [*corte*](../deploying/trim-self-contained.md). O corte é o processo de remover partes não usadas do tempo de execução de um aplicativo empacotado. Em alguns casos, a remoção pode remover incorretamente as referências necessárias.

O XML a seguir exclui o `System.Security` assembly de corte.

```xml
<ItemGroup>
  <TrimmerRootAssembly Include="System.Security" />
</ItemGroup>
```

### <a name="useapphost"></a>UseAppHost

A `UseAppHost` propriedade foi introduzida na versão 2.1.400 do SDK do .NET Core. Ele controla se um executável nativo é criado para uma implantação ou não. Um executável nativo é necessário para implantações independentes.

No .NET Core 3,0 e versões posteriores, um executável dependente da estrutura é criado por padrão. Defina a `UseAppHost` propriedade como `false` para desabilitar a geração do executável.

```xml
<PropertyGroup>
  <UseAppHost>false</UseAppHost>
</PropertyGroup>
```

Para obter mais informações sobre a implantação, consulte [implantação de aplicativos do .NET Core](../deploying/index.md).

## <a name="compile-properties"></a>Compilar propriedades

- [EmbeddedResourceUseDependentUponConvention](#embeddedresourceusedependentuponconvention)
- [LangVersion](#langversion)

### <a name="embeddedresourceusedependentuponconvention"></a>EmbeddedResourceUseDependentUponConvention

A `EmbeddedResourceUseDependentUponConvention` propriedade define se os nomes de arquivo de manifesto de recurso são gerados a partir de informações de tipo em arquivos de origem que são colocados com arquivos de recursos. Por exemplo, se *Form1. resx* estiver na mesma pasta que *Form1.cs*e `EmbeddedResourceUseDependentUponConvention` for definido como `true` , o arquivo *. Resources* gerado usará seu nome do primeiro tipo definido em *Form1.cs*. Por exemplo, se `MyNamespace.Form1` for o primeiro tipo definido em *Form1.cs*, o nome de arquivo gerado *será MyNamespace. Form1. Resources*.

> [!NOTE]
> Se `LogicalName` `ManifestResourceName` os metadados,, ou `DependentUpon` forem especificados para um `EmbeddedResource` Item, o nome do arquivo de manifesto gerado para esse arquivo de recurso será baseado nesses metadados em vez disso.

Por padrão, em um novo projeto .NET Core, essa propriedade é definida como `true` . Se for definido como `false` , e não `LogicalName` , ou os `ManifestResourceName` `DependentUpon` metadados forem especificados para o `EmbeddedResource` item no arquivo de projeto, o nome do arquivo de manifesto de recurso será baseado no namespace raiz do projeto e no caminho de arquivo relativo para o arquivo *. resx* . Para obter mais informações, consulte [como os arquivos de manifesto de recurso são nomeados](../resources/manifest-file-names.md).

```xml
<PropertyGroup>
  <EmbeddedResourceUseDependentUponConvention>true</EmbeddedResourceUseDependentUponConvention>
</PropertyGroup>
```

### <a name="langversion"></a>LangVersion

A `LangVersion` propriedade permite especificar uma versão de linguagem de programação específica. Por exemplo, se você quiser acessar os recursos do C# Preview, defina `LangVersion` como `preview` .

```xml
<PropertyGroup>
  <LangVersion>preview</LangVersion>
</PropertyGroup>
```

Para obter mais informações, consulte [controle de versão da linguagem C#](../../csharp/language-reference/configure-language-version.md#override-a-default).

## <a name="run-time-configuration-properties"></a>Propriedades de configuração de tempo de execução

Você pode configurar alguns comportamentos de tempo de execução especificando as propriedades do MSBuild no arquivo de projeto do aplicativo. Para obter informações sobre outras maneiras de configurar o comportamento de tempo de execução, consulte [definições de configuração de tempo de execução do .NET Core](../run-time-config/index.md).

- [ConcurrentGarbageCollection](#concurrentgarbagecollection)
- [InvariantGlobalization](#invariantglobalization)
- [RetainVMGarbageCollection](#retainvmgarbagecollection)
- [ServerGarbageCollection](#servergarbagecollection)
- [ThreadPoolMaxThreads](#threadpoolmaxthreads)
- [ThreadPoolMinThreads](#threadpoolminthreads)
- [TieredCompilation](#tieredcompilation)
- [TieredCompilationQuickJit](#tieredcompilationquickjit)
- [TieredCompilationQuickJitForLoops](#tieredcompilationquickjitforloops)

### <a name="concurrentgarbagecollection"></a>ConcurrentGarbageCollection

A `ConcurrentGarbageCollection` propriedade define se a [coleta de lixo em segundo plano (simultânea)](../../standard/garbage-collection/background-gc.md) está habilitada. Defina o valor como `false` para desabilitar a coleta de lixo em segundo plano. Para obter mais informações, consulte [background GC](../run-time-config/garbage-collector.md#background-gc).

```xml
<PropertyGroup>
  <ConcurrentGarbageCollection>false</ConcurrentGarbageCollection>
</PropertyGroup>
```

### <a name="invariantglobalization"></a>InvariantGlobalization

A `InvariantGlobalization` propriedade define se o aplicativo é executado no modo *invariável-globalização* , o que significa que ele não tem acesso a dados específicos de cultura. Defina o valor a `true` ser executado no modo invariável-Globally. Para obter mais informações, consulte [modo invariável](../run-time-config/globalization.md#invariant-mode).

```xml
<PropertyGroup>
  <InvariantGlobalization>true</InvariantGlobalization>
</PropertyGroup>
```

### <a name="retainvmgarbagecollection"></a>RetainVMGarbageCollection

A `RetainVMGarbageCollection` Propriedade configura o coletor de lixo para colocar os segmentos de memória excluídos em uma lista em espera para uso futuro ou liberá-los. Definir o valor para `true` instruir o coletor de lixo a colocar os segmentos em uma lista de espera. Para obter mais informações, consulte [reter VM](../run-time-config/garbage-collector.md#retain-vm).

```xml
<PropertyGroup>
  <RetainVMGarbageCollection>true</RetainVMGarbageCollection>
</PropertyGroup>
```

### <a name="servergarbagecollection"></a>ServerGarbageCollection

A `ServerGarbageCollection` propriedade define se o aplicativo usa a coleta de lixo da [estação de trabalho ou a coleta de lixo do servidor](../../standard/garbage-collection/workstation-server-gc.md). Defina o valor como para `true` usar a coleta de lixo do servidor. Para obter mais informações, consulte [estação de trabalho versus servidor](../run-time-config/garbage-collector.md#workstation-vs-server).

```xml
<PropertyGroup>
  <ServerGarbageCollection>true</ServerGarbageCollection>
</PropertyGroup>
```

### <a name="threadpoolmaxthreads"></a>ThreadPoolMaxThreads

A `ThreadPoolMaxThreads` Propriedade configura o número máximo de threads para o pool de threads de trabalho. Para obter mais informações, consulte [Maximum threads](../run-time-config/threading.md#maximum-threads).

```xml
<PropertyGroup>
  <ThreadPoolMaxThreads>20</ThreadPoolMaxThreads>
</PropertyGroup>
```

### <a name="threadpoolminthreads"></a>ThreadPoolMinThreads

A `ThreadPoolMinThreads` Propriedade configura o número mínimo de threads para o pool de threads de trabalho. Para obter mais informações, consulte [mínimo de threads](../run-time-config/threading.md#minimum-threads).

```xml
<PropertyGroup>
  <ThreadPoolMinThreads>4</ThreadPoolMinThreads>
</PropertyGroup>
```

### <a name="tieredcompilation"></a>TieredCompilation

A `TieredCompilation` propriedade define se o compilador JIT (just-in-time) usa compilação em [camadas](../whats-new/dotnet-core-3-0.md#tiered-compilation). Defina o valor como `false` para desabilitar a compilação em camadas. Para obter mais informações, consulte [compilação em camadas](../run-time-config/compilation.md#tiered-compilation).

```xml
<PropertyGroup>
  <TieredCompilation>false</TieredCompilation>
</PropertyGroup>
```

### <a name="tieredcompilationquickjit"></a>TieredCompilationQuickJit

A `TieredCompilationQuickJit` propriedade define se o compilador JIT usa o JIT rápido. Defina o valor como `false` para desabilitar o JIT rápido. Para obter mais informações, consulte [Quick JIT](../run-time-config/compilation.md#quick-jit).

```xml
<PropertyGroup>
  <TieredCompilationQuickJit>false</TieredCompilationQuickJit>
</PropertyGroup>
```

### <a name="tieredcompilationquickjitforloops"></a>TieredCompilationQuickJitForLoops

A `TieredCompilationQuickJitForLoops` propriedade define se o compilador JIT usa o JIT rápido em métodos que contêm loops. Defina o valor como `true` para habilitar o JIT rápido em métodos que contêm loops. Para obter mais informações, consulte [Quick JIT for loops](../run-time-config/compilation.md#quick-jit-for-loops).

```xml
<PropertyGroup>
  <TieredCompilationQuickJitForLoops>true</TieredCompilationQuickJitForLoops>
</PropertyGroup>
```

## <a name="reference-properties-and-items"></a>Propriedades e itens de referência

- [AssetTargetFallback](#assettargetfallback)
- [PackageReference](#packagereference)
- [ProjectReference](#projectreference)
- [Referência](#reference)
- [Restaurar propriedades](#restore-properties)

### <a name="assettargetfallback"></a>AssetTargetFallback

A `AssetTargetFallback` propriedade permite que você especifique versões de estrutura compatíveis adicionais para referências de projeto e pacotes NuGet. Por exemplo, se você especificar uma dependência de pacote usando `PackageReference` , mas esse pacote não contiver ativos que são compatíveis com seus projetos `TargetFramework` , a `AssetTargetFallback` Propriedade entrará em cena. A compatibilidade do pacote referenciado é verificada novamente usando cada estrutura de destino especificada em `AssetTargetFallback` .

Você pode definir a `AssetTargetFallback` propriedade para uma ou mais [versões da estrutura de destino](../../standard/frameworks.md#supported-target-framework-versions).

```xml
<PropertyGroup>
  <AssetTargetFallback>net461</AssetTargetFallback>
</PropertyGroup>
```

### <a name="packagereference"></a>PackageReference

O `PackageReference` Item define uma referência a um pacote NuGet.

O atributo `Include` especifica a ID do pacote. O `Version` atributo especifica a versão ou o intervalo de versão. Para obter informações sobre como especificar uma versão mínima, a versão máxima, o intervalo ou a correspondência exata, consulte [intervalos de versão](/nuget/concepts/package-versioning#version-ranges). Você também pode adicionar os seguintes metadados a uma referência de projeto: `IncludeAssets` , `ExcludeAssets` e `PrivateAssets` .

O trecho do arquivo de projeto no exemplo a seguir faz referência ao pacote [System. Runtime](https://www.nuget.org/packages/System.Runtime/) .

```xml
<ItemGroup>
  <PackageReference Include="System.Runtime" Version="4.3.0" />
</ItemGroup>
```

Para obter mais informações, consulte [referências de pacote em arquivos de projeto](/nuget/consume-packages/package-references-in-project-files).

### <a name="projectreference"></a>ProjectReference

O `ProjectReference` Item define uma referência a outro projeto. O projeto referenciado é adicionado como uma dependência de pacote NuGet, ou seja, é tratado da mesma forma que um `PackageReference` .

O `Include` atributo especifica o caminho para o projeto. Você também pode adicionar os seguintes metadados a uma referência de projeto: `IncludeAssets` , `ExcludeAssets` e `PrivateAssets` .

O trecho do arquivo de projeto no exemplo a seguir faz referência a um projeto chamado `Project2` .

```xml
<ItemGroup>
  <ProjectReference Include="..\Project2.csproj" />
</ItemGroup>
```

### <a name="reference"></a>Referência

O `Reference` Item define uma referência a um arquivo de assembly.

O `Include` atributo especifica o nome do arquivo e os `HintPath` metadados especificam o caminho para o assembly.

```xml
<ItemGroup>
  <Reference Include="MyAssembly">
    <HintPath>..\..\Assemblies\MyAssembly.dll</HintPath>
  </Reference>
</ItemGroup>
```

### <a name="restore-properties"></a>Restaurar propriedades

A restauração de um pacote referenciado instala todas as suas dependências diretas e todas as dependências dessas dependências. Você pode personalizar a restauração do pacote especificando propriedades como `RestorePackagesPath` e `RestoreIgnoreFailedSources` . Para obter mais informações sobre essas e outras propriedades, consulte [Restore Target](/nuget/reference/msbuild-targets#restore-target).

```xml
<PropertyGroup>
  <RestoreIgnoreFailedSource>true</RestoreIgnoreFailedSource>
</PropertyGroup>
```

## <a name="see-also"></a>Consulte também

- [Referência de esquema do MSBuild](/visualstudio/msbuild/msbuild-project-file-schema-reference)
- [Propriedades comuns do MSBuild](/visualstudio/msbuild/common-msbuild-project-properties)
- [Propriedades do MSBuild para o pacote NuGet](/nuget/reference/msbuild-targets#pack-target)
- [Propriedades do MSBuild para restauração do NuGet](/nuget/reference/msbuild-targets#restore-properties)
- [Personalizar uma compilação](/visualstudio/msbuild/customize-your-build)
