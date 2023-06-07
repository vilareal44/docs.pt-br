---
title: Visão geral do SDK do projeto .NET Core
titleSuffix: ''
description: Saiba mais sobre os SDKs do projeto .NET Core.
ms.date: 02/02/2020
ms.topic: conceptual
ms.openlocfilehash: 873c06007307c5892c4828f987486b4dd98dc9ae
ms.sourcegitcommit: d337df55f83325918cbbd095eb573400bea49064
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88187920"
---
# <a name="net-core-project-sdks"></a>SDKs do projeto do .NET Core

Os projetos do .NET Core são associados a um Software Development Kit (SDK). Cada *SDK do projeto* é um conjunto de [destinos](/visualstudio/msbuild/msbuild-targets) do MSBuild e [tarefas](/visualstudio/msbuild/msbuild-tasks) associadas que são responsáveis por compilar, empacotar e publicar código. Um projeto que faz referência a um SDK de projeto é, às vezes, chamado de *projeto de estilo SDK*.

## <a name="available-sdks"></a>SDKs disponíveis

Os seguintes SDKs estão disponíveis para o .NET Core:

| ID | Descrição | Repositório|
| - | - | - |
| `Microsoft.NET.Sdk` | O SDK do .NET Core | <https://github.com/dotnet/sdk> |
| `Microsoft.NET.Sdk.Web` | O [SDK Web](/aspnet/core/razor-pages/web-sdk) do .NET Core | <https://github.com/dotnet/sdk> |
| `Microsoft.NET.Sdk.Razor` | O SDK do .NET Core do [Razor](/aspnet/core/razor-pages/sdk) |
| `Microsoft.NET.Sdk.Worker` | O SDK do serviço de trabalho do .NET Core |
| `Microsoft.NET.Sdk.WindowsDesktop` | O SDK do .NET Core WinForms e WPF |

O SDK do .NET Core é o SDK base para .NET Core. Os outros SDKs fazem referência à SDK do .NET Core e os projetos associados a outros SDKs têm todas as propriedades de SDK do .NET Core disponíveis para eles. O SDK da Web, por exemplo, depende do SDK do .NET Core e do SDK do Razor.

Você também pode criar seu próprio SDK que pode ser distribuído via NuGet.

## <a name="project-files"></a>Arquivos de projeto

Os projetos do .NET Core são baseados no formato [MSBuild](/visualstudio/msbuild/msbuild) . Arquivos de projeto, que têm extensões como *. csproj* para projetos C# e *. Fsproj* para projetos F #, estão no formato XML. O elemento raiz de um arquivo de projeto do MSBuild é o elemento [Project](/visualstudio/msbuild/project-element-msbuild) . O `Project` elemento tem um `Sdk` atributo opcional que especifica qual SDK (e versão) usar. Para usar as ferramentas do .NET Core e criar seu código, defina o `Sdk` atributo como uma das IDs na tabela de [SDKs disponíveis](#available-sdks) .

```xml
<Project Sdk="Microsoft.NET.Sdk">
  ...
</Project>
```

Para especificar um SDK proveniente do NuGet, inclua a versão no final do nome ou especifique o nome e a versão na *global.jsno* arquivo.

```xml
<Project Sdk="MSBuild.Sdk.Extras/2.0.54">
  ...
</Project>
```

Outra maneira de especificar o SDK é com o elemento do [SDK](/visualstudio/msbuild/sdk-element-msbuild) de nível superior:

```xml
<Project>
  <Sdk Name="Microsoft.NET.Sdk" />
  ...
</Project>
```

Fazer referência a um SDK de uma dessas maneiras simplifica bastante os arquivos de projeto para o .NET Core. Ao avaliar o projeto, o MSBuild adiciona importações implícitas para `Sdk.props` na parte superior do arquivo de projeto e `Sdk.targets` na parte inferior.

```xml
<Project>
  <!-- Implicit top import -->
  <Import Project="Sdk.props" Sdk="Microsoft.NET.Sdk" />
  ...
  <!-- Implicit bottom import -->
  <Import Project="Sdk.targets" Sdk="Microsoft.NET.Sdk" />
</Project>
```

> [!TIP]
> Em um computador Windows, os arquivos *SDK. props* e *SDK. targets* podem ser encontrados na pasta *%ProgramFiles%\dotnet\sdk \\ [Version] \Sdks\Microsoft.net.Sdk\Sdk*

### <a name="preprocess-the-project-file"></a>Pré-processar o arquivo de projeto

Você pode ver o projeto totalmente expandido, pois o MSBuild o vê depois que o SDK e seus destinos são incluídos usando o `dotnet msbuild -preprocess` comando. A opção de [pré-processamento](/visualstudio/msbuild/msbuild-command-line-reference#preprocess) do [`dotnet msbuild`](../tools/dotnet-msbuild.md) comando mostra quais arquivos são importados, suas origens e suas contribuições para a compilação sem realmente criar o projeto.

Se o projeto tiver várias estruturas de destino, concentre os resultados do comando em apenas uma estrutura especificando-o como uma propriedade do MSBuild. Por exemplo: 

`dotnet msbuild -property:TargetFramework=netcoreapp2.0 -preprocess:output.xml`

### <a name="default-compilation-includes"></a>A compilação padrão inclui

O padrão inclui e exclui itens de compilação, recursos incorporados e `None` itens são definidos no SDK. Ao contrário dos projetos .NET Framework não SDK, você não precisa especificar esses itens no arquivo de projeto, pois os padrões abrangem os casos de uso mais comuns. Isso torna o arquivo de projeto menor e mais fácil de entender e editar manualmente, se necessário.

A tabela a seguir mostra quais elementos e quais [globs](https://en.wikipedia.org/wiki/Glob_(programming)) são incluídos e excluídos no SDK do .NET Core:

| Elemento           | Incluir glob                              | Excluir glob                                                  | Remover glob              |
|-------------------|-------------------------------------------|---------------------------------------------------------------|--------------------------|
| Compilar           | \*\*/\*.cs (ou outras extensões de linguagem) | \*\*/\*.user; \*\*/\*.\*proj; \*\*/\*.sln; \*\*/\*.vssscc  | N/D                      |
| EmbeddedResource  | \*\*/\*.resx                              | \*\*/\*.user; \*\*/\*.\*proj; \*\*/\*.sln; \*\*/\*.vssscc     | N/D                      |
| Nenhum              | \*\*/\*                                   | \*\*/\*.user; \*\*/\*.\*proj; \*\*/\*.sln; \*\*/\*.vssscc     | \*\*/\*.cs; \*\*/\*.resx |

> [!NOTE]
> As `./bin` `./obj` pastas e, que são representadas pelas `$(BaseOutputPath)` Propriedades do e do `$(BaseIntermediateOutputPath)` MSBuild, são excluídas do globs por padrão. As exclusões são representadas pela propriedade `$(DefaultItemExcludes)` .

#### <a name="build-errors"></a>Erros de compilação

Se você definir explicitamente qualquer um desses itens em seu arquivo de projeto, provavelmente receberá um erro de compilação "NETSDK1022" semelhante ao seguinte:

  > Itens ' compile ' duplicados foram incluídos. O SDK do .NET inclui, por padrão, itens de "Compilar" do diretório do projeto. É possível remover esses itens do arquivo de projeto ou definir a propriedade “EnableDefaultCompileItems” como “false” se desejar incluí-los explicitamente no arquivo de projeto.

  > Itens ' EmbeddedResource ' duplicados foram incluídos. O SDK do .NET inclui os itens ' EmbeddedResource ' do diretório do projeto por padrão. Você pode remover esses itens do arquivo de projeto ou definir a propriedade ' EnableDefaultEmbeddedResourceItems ' como ' false ' se quiser incluí-los explicitamente em seu arquivo de projeto.

Para resolver os erros, siga um destes procedimentos:

- Remova os itens explícitos `Compile` , `EmbeddedResource` ou `None` que correspondem aos implícitos listados na tabela anterior.

- Defina a `EnableDefaultItems` propriedade como `false` para desabilitar toda a inclusão de arquivo implícito:

  ```xml
  <PropertyGroup>
    <EnableDefaultItems>false</EnableDefaultItems>
  </PropertyGroup>
  ```

  Se você quiser especificar arquivos a serem publicados com seu aplicativo, ainda poderá usar os mecanismos do MSBuild conhecidos para isso, por exemplo, o `Content` elemento.

- Desabilite seletivamente apenas `Compile` , `EmbeddedResource` , ou `None` globs definindo a `EnableDefaultCompileItems` propriedade, `EnableDefaultEmbeddedResourceItems` ou `EnableDefaultNoneItems` como `false` :

  ```xml
  <PropertyGroup>
    <EnableDefaultCompileItems>false</EnableDefaultCompileItems>
    <EnableDefaultEmbeddedResourceItems>false</EnableDefaultEmbeddedResourceItems>
    <EnableDefaultNoneItems>false</EnableDefaultNoneItems>
  </PropertyGroup>
  ```

  Se você desabilitar apenas `Compile` globs, Gerenciador de soluções no Visual Studio ainda mostrará \* itens. cs como parte do projeto, incluídos como `None` itens. Para desabilitar o `None` glob implícito, defina `EnableDefaultNoneItems` como `false` também.

## <a name="customize-the-build"></a>Personalizar a compilação

Há várias maneiras de [personalizar uma compilação](/visualstudio/msbuild/customize-your-build). Talvez você queira substituir uma propriedade passando-a como um argumento para um comando [MSBuild](/visualstudio/msbuild/msbuild-command-line-reference) ou [dotnet](../tools/index.md) . Você também pode adicionar a propriedade ao arquivo de projeto ou a um arquivo *Directory. Build. props* . Para obter uma lista de propriedades úteis para projetos do .NET Core, consulte [referência do MSBuild para projetos de SDK do .NET Core](msbuild-props.md).

### <a name="custom-targets"></a>Destinos personalizados

Os projetos do .NET Core podem empacotar destinos e propriedades do MSBuild personalizados para uso por projetos que consomem o pacote. Use esse tipo de extensibilidade quando desejar:

- Estenda o processo de compilação.
- Acessar artefatos do processo de compilação, como arquivos gerados.
- Inspecione a configuração sob a qual a compilação é invocada.

Você adiciona propriedades ou destinos de compilação personalizados colocando arquivos no formulário `<package_id>.targets` ou `<package_id>.props` (por exemplo, `Contoso.Utility.UsefulStuff.targets` ) na pasta *Build* do projeto.

O XML a seguir é um trecho de código de um arquivo *. csproj* que instrui o [`dotnet pack`](../tools/dotnet-pack.md) comando o que deve ser empacotado. O `<ItemGroup Label="dotnet pack instructions">` elemento coloca os arquivos de destino na pasta de *compilação* dentro do pacote. O `<Target Name="CollectRuntimeOutputs" BeforeTargets="_GetPackageFiles">` elemento coloca os assemblies e os arquivos *. JSON* na pasta *Build* .

```xml
<Project Sdk="Microsoft.NET.Sdk">

  ...
  <ItemGroup Label="dotnet pack instructions">
    <Content Include="build\*.targets">
      <Pack>true</Pack>
      <PackagePath>build\</PackagePath>
    </Content>
  </ItemGroup>
  <Target Name="CollectRuntimeOutputs" BeforeTargets="_GetPackageFiles">
    <!-- Collect these items inside a target that runs after build but before packaging. -->
    <ItemGroup>
      <Content Include="$(OutputPath)\*.dll;$(OutputPath)\*.json">
        <Pack>true</Pack>
        <PackagePath>build\</PackagePath>
      </Content>
    </ItemGroup>
  </Target>
  ...
  
</Project>
```

Para consumir um destino personalizado em seu projeto, adicione um `PackageReference` elemento que aponte para o pacote e sua versão. Ao contrário das ferramentas, o pacote de destinos personalizados é incluído no fechamento de dependência do projeto de consumo.

Você pode configurar como usar o destino personalizado. Como é um destino do MSBuild, ele pode depender de um determinado destino, executado após outro destino ou ser invocado manualmente usando o `dotnet msbuild -t:<target-name>` comando. No entanto, para proporcionar uma melhor experiência do usuário, você pode combinar ferramentas por projeto e destinos personalizados. Nesse cenário, a ferramenta por projeto aceita todos os parâmetros necessários e converte-os na [`dotnet msbuild`](../tools/dotnet-msbuild.md) invocação necessária que executa o destino. Veja um exemplo desse tipo de sinergia no repositório [Exemplos do MVP Summit 2016 Hackathon](https://github.com/dotnet/MVPSummitHackathon2016) do projeto [`dotnet-packer`](https://github.com/dotnet/MVPSummitHackathon2016/tree/master/dotnet-packer).

## <a name="see-also"></a>Confira também

- [Instalar o .NET Core](../install/index.yml)
- [Como usar SDKs de projeto do MSBuild](/visualstudio/msbuild/how-to-use-project-sdk)
- [Empacotar destinos e props personalizados do MSBuild com o NuGet](/nuget/create-packages/creating-a-package#include-msbuild-props-and-targets-in-a-package)
