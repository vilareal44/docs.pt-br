---
title: Comparação entre project.json e csproj
description: Veja um mapeamento entre os elementos project.json e csproj.
author: natemcmaster
ms.date: 03/13/2017
ms.openlocfilehash: c8638bc30ba09d8e8d464159aded60dcde4b8dc0
ms.sourcegitcommit: 32f0d6f4c01ddc6ca78767c3a30e3305f8cd032c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87427015"
---
# <a name="a-mapping-between-projectjson-and-csproj-properties"></a>Um mapeamento entre as propriedades de project.json e csproj

Por [Nate McMaster](https://github.com/natemcmaster)

Durante o desenvolvimento das ferramentas do .NET Core, foi feita uma importante alteração de design para que os arquivos *project.json* não tivessem mais suporte e, em vez disso, os projetos do .NET Core fossem movidos para o formato MSBuild/csproj.

Este artigo mostra como as configurações em *project.json* são representadas no formato MSBuild/csproj para que você saiba como usar o novo formato e entenda as alterações feitas pelas ferramentas de migração ao atualizar o projeto para a última versão da ferramenta.

## <a name="the-csproj-format"></a>O formato csproj

O novo formato, \*.csproj, é um formato baseado em XML. O exemplo a seguir mostra o nó raiz de um projeto do .NET Core que usa o `Microsoft.NET.Sdk`. Para projetos Web, o SDK usado é `Microsoft.NET.Sdk.Web`.

```xml
<Project Sdk="Microsoft.NET.Sdk">
...
</Project>
```

## <a name="common-top-level-properties"></a>Propriedades comuns de nível superior

### <a name="name"></a>name

```json
{
  "name": "MyProjectName"
}
```

Não tem mais suporte. Em csproj, isso é determinado pelo nome de arquivo do projeto, que normalmente corresponde ao nome do diretório. Por exemplo, `MyProjectName.csproj`.

Por padrão, o nome de arquivo do projeto também especifica o valor das propriedades `<AssemblyName>` e `<PackageId>`.

```xml
<PropertyGroup>
  <AssemblyName>MyProjectName</AssemblyName>
  <PackageId>MyProjectName</PackageId>
</PropertyGroup>
```

O `<AssemblyName>` terá um valor diferente de `<PackageId>` se a propriedade `buildOptions\outputName` tiver sido definida em project.json.
Para obter mais informações, consulte [Outras opções comuns de build](#other-common-build-options).

### <a name="version"></a>version

```json
{
  "version": "1.0.0-alpha-*"
}
```

Use as propriedades `VersionPrefix` e `VersionSuffix`:

```xml
<PropertyGroup>
  <VersionPrefix>1.0.0</VersionPrefix>
  <VersionSuffix>alpha</VersionSuffix>
</PropertyGroup>
```

Também é possível usar a propriedade `Version`, mas isso poderá substituir as configurações de versão durante o empacotamento:

```xml
<PropertyGroup>
  <Version>1.0.0-alpha</Version>
</PropertyGroup>
```

### <a name="other-common-root-level-options"></a>Outras opções comuns de nível raiz

```json
{
  "authors": [ "Anne", "Bob" ],
  "company": "Contoso",
  "language": "en-US",
  "title": "My library",
  "description": "This is my library.\r\nAnd it's really great!",
  "copyright": "Nugetizer 3000",
  "userSecretsId": "xyz123"
}
```

```xml
<PropertyGroup>
  <Authors>Anne;Bob</Authors>
  <Company>Contoso</Company>
  <NeutralLanguage>en-US</NeutralLanguage>
  <AssemblyTitle>My library</AssemblyTitle>
  <Description>This is my library.
And it's really great!</Description>
  <Copyright>Nugetizer 3000</Copyright>
  <UserSecretsId>xyz123</UserSecretsId>
</PropertyGroup>
```

## <a name="frameworks"></a>estruturas

### <a name="one-target-framework"></a>Uma estrutura de destino

```json
{
  "frameworks": {
    "netcoreapp1.0": {}
  }
}
```

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp1.0</TargetFramework>
</PropertyGroup>
```

### <a name="multiple-target-frameworks"></a>Várias estruturas de destino

```json
{
  "frameworks": {
    "netcoreapp1.0": {},
    "net451": {}
  }
}
```

Use a propriedade `TargetFrameworks` para definir a lista de estruturas de destino. Use ponto e vírgula para separar vários valores de estrutura.

```xml
<PropertyGroup>
  <TargetFrameworks>netcoreapp1.0;net451</TargetFrameworks>
</PropertyGroup>
```

## <a name="dependencies"></a>dependencies

> [!IMPORTANT]
> Se a dependência for um **projeto** e não um pacote, o formato será diferente.
> Para obter mais informações, consulte a seção [Tipo de dependência](#dependency-type).

### <a name="netstandardlibrary-metapackage"></a>Metapacote .NETStandard.Library

```json
{
  "dependencies": {
    "NETStandard.Library": "1.6.0"
  }
}
```

```xml
<PropertyGroup>
  <NetStandardImplicitPackageVersion>1.6.0</NetStandardImplicitPackageVersion>
</PropertyGroup>
```

### <a name="microsoftnetcoreapp-metapackage"></a>Metapacote Microsoft.NETCore.App

```json
{
  "dependencies": {
    "Microsoft.NETCore.App": "1.0.0"
  }
}
```

```xml
<PropertyGroup>
  <RuntimeFrameworkVersion>1.0.3</RuntimeFrameworkVersion>
</PropertyGroup>
```

O `<RuntimeFrameworkVersion>` valor no projeto migrado é determinado pela versão do SDK que está instalado.

### <a name="top-level-dependencies"></a>Dependências de nível superior

```json
{
  "dependencies": {
    "Microsoft.AspNetCore": "1.1.0"
  }
}
```

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore" Version="1.1.0" />
</ItemGroup>
```

### <a name="per-framework-dependencies"></a>Dependências por estrutura

```json
{
  "framework": {
    "net451": {
      "dependencies": {
        "System.Collections.Immutable": "1.3.1"
      }
    },
    "netstandard1.5": {
      "dependencies": {
        "Newtonsoft.Json": "9.0.1"
      }
    }
  }
}
```

```xml
<ItemGroup Condition="'$(TargetFramework)'=='net451'">
  <PackageReference Include="System.Collections.Immutable" Version="1.3.1" />
</ItemGroup>

<ItemGroup Condition="'$(TargetFramework)'=='netstandard1.5'">
  <PackageReference Include="Newtonsoft.Json" Version="9.0.1" />
</ItemGroup>
```

### <a name="imports"></a>importações

```json
{
  "dependencies": {
    "YamlDotNet": "4.0.1-pre309"
  },
  "frameworks": {
    "netcoreapp1.0": {
      "imports": [
        "dnxcore50",
        "dotnet"
      ]
    }
  }
}
```

```xml
<PropertyGroup>
  <PackageTargetFallback>dnxcore50;dotnet</PackageTargetFallback>
</PropertyGroup>
<ItemGroup>
  <PackageReference Include="YamlDotNet" Version="4.0.1-pre309" />
</ItemGroup>
```

### <a name="dependency-type"></a>tipo de dependência

#### <a name="type-project"></a>tipo: projeto

```json
{
  "dependencies": {
    "MyOtherProject": "1.0.0-*",
    "AnotherProject": {
      "type": "project"
    }
  }
}
```

```xml
<ItemGroup>
  <ProjectReference Include="..\MyOtherProject\MyOtherProject.csproj" />
  <ProjectReference Include="..\AnotherProject\AnotherProject.csproj" />
</ItemGroup>
```

> [!NOTE]
> Isso detalhará a maneira pela qual o `dotnet pack --version-suffix $suffix` determina a versão de dependência de uma referência de projeto.

#### <a name="type-build"></a>tipo: build

```json
{
  "dependencies": {
    "Microsoft.EntityFrameworkCore.Design": {
      "version": "1.1.0",
      "type": "build"
    }
  }
}
```

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="1.1.0" PrivateAssets="All" />
</ItemGroup>
```

#### <a name="type-platform"></a>tipo: plataforma

```json
{
  "dependencies": {
    "Microsoft.NETCore.App": {
      "version": "1.1.0",
      "type": "platform"
    }
  }
}
```

Não há nenhum equivalente em csproj.

## <a name="runtimes"></a>runtimes

```json
{
  "runtimes": {
    "win7-x64": {},
    "osx.10.11-x64": {},
    "ubuntu.16.04-x64": {}
  }
}
```

```xml
<PropertyGroup>
  <RuntimeIdentifiers>win7-x64;osx.10.11-x64;ubuntu.16.04-x64</RuntimeIdentifiers>
</PropertyGroup>
```

### <a name="standalone-apps-self-contained-deployment"></a>Aplicativos independentes (implantação independente)

Em project.json, a definição de uma seção `runtimes` significa que o aplicativo era independente durante o build e a publicação.
No MSBuild, todos os projetos são *portáteis* durante o build, mas podem ser publicados como independentes.

`dotnet publish --framework netcoreapp1.0 --runtime osx.10.11-x64`

Para obter mais informações, consulte [SCD (implantações independentes)](../deploying/index.md#publish-self-contained).

## <a name="tools"></a>tools

```json
{
  "tools": {
    "Microsoft.EntityFrameworkCore.Tools.DotNet": "1.0.0-*"
  }
}
```

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="1.0.0" />
</ItemGroup>
```

> [!NOTE]
> Não há suporte para `imports` em ferramentas em csproj. As ferramentas que precisam de importações não funcionarão com o novo `Microsoft.NET.Sdk`.

## <a name="buildoptions"></a>buildOptions

Consulte também [Arquivos](#files).

### <a name="emitentrypoint"></a>emitEntryPoint

```json
{
  "buildOptions": {
    "emitEntryPoint": true
  }
}
```

```xml
<PropertyGroup>
  <OutputType>Exe</OutputType>
</PropertyGroup>
```

Se `emitEntryPoint` era `false`, o valor de `OutputType` é convertido em `Library`, que é o valor padrão:

```json
{
  "buildOptions": {
    "emitEntryPoint": false
  }
}
```

```xml
<PropertyGroup>
  <OutputType>Library</OutputType>
  <!-- or, omit altogether. It defaults to 'Library' -->
</PropertyGroup>
```

### <a name="keyfile"></a>keyFile

```json
{
  "buildOptions": {
    "keyFile": "MyKey.snk"
  }
}
```

O elemento `keyFile` se expande para três propriedades no MSBuild:

```xml
<PropertyGroup>
  <AssemblyOriginatorKeyFile>MyKey.snk</AssemblyOriginatorKeyFile>
  <SignAssembly>true</SignAssembly>
  <PublicSign Condition="'$(OS)' != 'Windows_NT'">true</PublicSign>
</PropertyGroup>
```

### <a name="other-common-build-options"></a>Outras opções comuns de build

```json
{
  "buildOptions": {
    "warningsAsErrors": true,
    "nowarn": ["CS0168", "CS0219"],
    "xmlDoc": true,
    "preserveCompilationContext": true,
    "outputName": "Different.AssemblyName",
    "debugType": "portable",
    "allowUnsafe": true,
    "define": ["TEST", "OTHERCONDITION"]
  }
}
```

```xml
<PropertyGroup>
  <TreatWarningsAsErrors>true</TreatWarningsAsErrors>
  <NoWarn>$(NoWarn);CS0168;CS0219</NoWarn>
  <GenerateDocumentationFile>true</GenerateDocumentationFile>
  <PreserveCompilationContext>true</PreserveCompilationContext>
  <AssemblyName>Different.AssemblyName</AssemblyName>
  <DebugType>portable</DebugType>
  <AllowUnsafeBlocks>true</AllowUnsafeBlocks>
  <DefineConstants>$(DefineConstants);TEST;OTHERCONDITION</DefineConstants>
</PropertyGroup>
```

## <a name="packoptions"></a>packOptions

Consulte também [Arquivos](#files).

### <a name="common-pack-options"></a>Opções comuns de pacote

```json
{
  "packOptions": {
    "summary": "numl is a machine learning library intended to ease the use of using standard modeling techniques for both prediction and clustering.",
    "tags": ["machine learning", "framework"],
    "releaseNotes": "Version 0.9.12-beta",
    "iconUrl": "http://numl.net/images/ico.png",
    "projectUrl": "http://numl.net",
    "licenseUrl": "https://raw.githubusercontent.com/sethjuarez/numl/master/LICENSE.md",
    "requireLicenseAcceptance": false,
    "repository": {
      "type": "git",
      "url": "https://raw.githubusercontent.com/sethjuarez/numl"
    },
    "owners": ["Seth Juarez"]
  }
}
```

```xml
<PropertyGroup>
  <!-- summary is not migrated from project.json, but you can use the <Description> property for that if needed. -->
  <PackageTags>machine learning;framework</PackageTags>
  <PackageReleaseNotes>Version 0.9.12-beta</PackageReleaseNotes>
  <PackageIcon>ico.png</PackageIcon>
  <PackageProjectUrl>http://numl.net</PackageProjectUrl>
  <PackageLicenseUrl>https://raw.githubusercontent.com/sethjuarez/numl/master/LICENSE.md</PackageLicenseUrl>
  <PackageRequireLicenseAcceptance>false</PackageRequireLicenseAcceptance>
  <RepositoryType>git</RepositoryType>
  <RepositoryUrl>https://raw.githubusercontent.com/sethjuarez/numl</RepositoryUrl>
  <!-- owners is not supported in MSBuild -->
</PropertyGroup>
```

Não há nenhum equivalente do elemento `owners` no MSBuild. Para `summary` o, você pode usar a `<Description>` Propriedade MSBuild. O valor de `summary` não é migrado automaticamente para essa propriedade, já que essa propriedade é mapeada para o [`description`](#other-common-root-level-options) elemento.  [PackageIconUrl foi preterido](/nuget/reference/msbuild-targets#packageiconurl) em favor de PackageIcon.

## <a name="scripts"></a>scripts

```json
{
  "scripts": {
    "precompile": "generateCode.cmd",
    "postpublish": [ "obfuscate.cmd", "removeTempFiles.cmd" ]
  }
}
```

Seus equivalentes no MSBuild são [destinos](/visualstudio/msbuild/msbuild-targets):

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="generateCode.cmd" />
</Target>

<Target Name="MyPostCompileTarget" AfterTargets="Publish">
  <Exec Command="obfuscate.cmd" />
  <Exec Command="removeTempFiles.cmd" />
</Target>
```

## <a name="runtimeoptions"></a>runtimeOptions

```json
{
  "runtimeOptions": {
    "configProperties": {
      "System.GC.Server": true,
      "System.GC.Concurrent": true,
      "System.GC.RetainVM": true,
      "System.Threading.ThreadPool.MinThreads": 4,
      "System.Threading.ThreadPool.MaxThreads": 25
    }
  }
}
```

Todas as configurações nesse grupo, exceto para a `System.GC.Server` propriedade, são colocadas em um arquivo chamado *runtimeconfig.template.js* na pasta do projeto, com opções levantadas para o objeto raiz durante o processo de migração:

```json
{
  "configProperties": {
    "System.GC.Concurrent": true,
    "System.GC.RetainVM": true,
    "System.Threading.ThreadPool.MinThreads": 4,
    "System.Threading.ThreadPool.MaxThreads": 25
  }
}
```

A `System.GC.Server` propriedade é migrada para o arquivo csproj:

```xml
<PropertyGroup>
  <ServerGarbageCollection>true</ServerGarbageCollection>
</PropertyGroup>
```

No entanto, é possível definir todos esses valores em csproj, bem como propriedades do MSBuild:

```xml
<PropertyGroup>
  <ServerGarbageCollection>true</ServerGarbageCollection>
  <ConcurrentGarbageCollection>true</ConcurrentGarbageCollection>
  <RetainVMGarbageCollection>true</RetainVMGarbageCollection>
  <ThreadPoolMinThreads>4</ThreadPoolMinThreads>
  <ThreadPoolMaxThreads>25</ThreadPoolMaxThreads>
</PropertyGroup>
```

## <a name="shared"></a>shared

```json
{
  "shared": "shared/**/*.cs"
}
```

Sem suporte em csproj. Em vez disso, Crie arquivos de conteúdo de inclusão em seu arquivo *. nuspec* .
Para obter mais informações, consulte [Incluindo arquivos de conteúdo](/nuget/schema/nuspec#including-content-files).

## <a name="files"></a>files

Em *project.json*, o build e o pacote poderão ser estendidos para serem compilados e inseridos de pastas diferentes.
No MSBuild, isso é feito com [itens](/visualstudio/msbuild/common-msbuild-project-items). O seguinte exemplo é uma conversão comum:

```json
{
  "buildOptions": {
    "compile": {
      "copyToOutput": "notes.txt",
      "include": "../Shared/*.cs",
      "exclude": "../Shared/Not/*.cs"
    },
    "embed": {
      "include": "../Shared/*.resx"
    }
  },
  "packOptions": {
    "include": "Views/",
    "mappings": {
      "some/path/in/project.txt": "in/package.txt"
    }
  },
  "publishOptions": {
    "include": [
      "files/",
      "publishnotes.txt"
    ]
  }
}
```

```xml
<ItemGroup>
  <Compile Include="..\Shared\*.cs" Exclude="..\Shared\Not\*.cs" />
  <EmbeddedResource Include="..\Shared\*.resx" />
  <Content Include="Views\**\*" PackagePath="%(Identity)" />
  <None Include="some/path/in/project.txt" Pack="true" PackagePath="in/package.txt" />
  
  <None Include="notes.txt" CopyToOutputDirectory="Always" />
  <!-- CopyToOutputDirectory = { Always, PreserveNewest, Never } -->

  <Content Include="files\**\*" CopyToPublishDirectory="PreserveNewest" />
  <None Include="publishnotes.txt" CopyToPublishDirectory="Always" />
  <!-- CopyToPublishDirectory = { Always, PreserveNewest, Never } -->
</ItemGroup>
```

> [!NOTE]
> Muitos dos [padrões de recurso de curinga](https://en.wikipedia.org/wiki/Glob_(programming)) são adicionados automaticamente pelo SDK do .NET Core. Para obter mais informações, consulte a [compilação padrão inclui](../project-sdk/overview.md#default-compilation-includes).

Todos os elementos `ItemGroup` do MSBuild dão suporte a `Include`, `Exclude` e `Remove`.

O layout do pacote dentro do .nupkg pode ser modificado com `PackagePath="path"`.

Com exceção de `Content`, a maioria dos grupos de itens exige a adição explícita de `Pack="true"` a ser incluída no pacote. `Content` será colocado na pasta *conteúdo* de um pacote, pois a propriedade `<IncludeContentInPack>` do MSBuild está definida como `true` por padrão.
Para obter mais informações, consulte [Incluindo conteúdo em um pacote](/nuget/schema/msbuild-targets#including-content-in-a-package).

`PackagePath="%(Identity)"` é uma maneira curta de configurar o caminho do pacote para o caminho do arquivo relativo do projeto.

## <a name="testrunner"></a>testRunner

### <a name="xunit"></a>xUnit

```json
{
  "testRunner": "xunit",
  "dependencies": {
    "dotnet-test-xunit": "<any>"
  }
}
```

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Test.Sdk" Version="15.0.0-*" />
  <PackageReference Include="xunit" Version="2.2.0-*" />
  <PackageReference Include="xunit.runner.visualstudio" Version="2.2.0-*" />
</ItemGroup>
```

### <a name="mstest"></a>MSTest

```json
{
  "testRunner": "mstest",
  "dependencies": {
    "dotnet-test-mstest": "<any>"
  }
}
```

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Test.Sdk" Version="15.0.0-*" />
  <PackageReference Include="MSTest.TestAdapter" Version="1.1.12-*" />
  <PackageReference Include="MSTest.TestFramework" Version="1.1.11-*" />
</ItemGroup>
```

## <a name="see-also"></a>Veja também

- [Visão geral de alto nível das alterações na CLI](cli-msbuild-architecture.md)
