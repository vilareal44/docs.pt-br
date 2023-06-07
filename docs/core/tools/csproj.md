---
title: Adições ao formato csproj para .NET Core
description: Saiba mais sobre as diferenças entre arquivos existentes e de csproj do .NET Core
ms.topic: reference
ms.date: 04/08/2019
ms.openlocfilehash: 82174b2976abda2337a4a9b5a5a5e1f60a1094fb
ms.sourcegitcommit: cbb19e56d48cf88375d35d0c27554d4722761e0d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88608333"
---
# <a name="additions-to-the-csproj-format-for-net-core"></a>Adições ao formato csproj para .NET Core

Este documento descreve as alterações adicionadas aos arquivos de projeto como parte da mudança de *project.json* para *csproj* e o [MSBuild](https://github.com/Microsoft/MSBuild). Para mais informações sobre a sintaxe e a referência do arquivo de projeto geral, consulte a documentação do [arquivo de projeto MSBuild](/visualstudio/msbuild/msbuild-project-file-schema-reference).

## <a name="implicit-package-references"></a>Referências de pacote implícitas

Os metapacotes são referenciados implicitamente de acordo com as estruturas de destino especificadas na propriedade `<TargetFramework>` ou `<TargetFrameworks>` do arquivo de projeto. `<TargetFrameworks>` será ignorado se `<TargetFramework>` for especificado, independente da ordem.

```xml
 <PropertyGroup>
   <TargetFramework>netcoreapp2.1</TargetFramework>
 </PropertyGroup>
 ```

 ```xml
 <PropertyGroup>
   <TargetFrameworks>netcoreapp2.1;net462</TargetFrameworks>
 </PropertyGroup>
 ```

### <a name="recommendations"></a>Recomendações

Como os metapacotes `Microsoft.NETCore.App` ou `NETStandard.Library` são referenciados implicitamente, estas são nossas melhores práticas:

- Durante o direcionamento do .NET Core ou do .NET Standard, nunca tenha uma referência explícita aos metapacotes `Microsoft.NETCore.App` ou `NETStandard.Library` por meio de um item `<PackageReference>` no arquivo de projeto.
- Se precisar de uma versão específica do runtime ao direcionar o .NET Core, use a propriedade `<RuntimeFrameworkVersion>` no projeto (por exemplo, `1.0.4`) em vez de referenciar o metapacote.
  - Isso poderá ocorrer se você estiver usando [implantações independentes](../deploying/index.md#publish-self-contained) e precisar de uma versão de patch específica do runtime do 1.0.0 LTS, por exemplo.
- Se precisar de uma versão específica do metapacote `NETStandard.Library` ao direcionar o .NET Core, use a propriedade `<NetStandardImplicitPackageVersion>` e defina a versão necessária.
- Não adicione explicitamente nem atualize referências a nenhum dos metapacotes `Microsoft.NETCore.App` ou `NETStandard.Library` em projetos do .NET Framework. Se uma versão do `NETStandard.Library` for necessária ao usar um pacote NuGet baseado no .NET Standard, o NuGet automaticamente instalará essa versão.

## <a name="implicit-version-for-some-package-references"></a>Versão implícita para algumas referências de pacote

A maioria dos usos de [`<PackageReference>`](#packagereference) requer a configuração do `Version` atributo para especificar a versão do pacote NuGet a ser usada. Ao usar o .NET Core 2.1 ou 2.2 e referenciar [Microsoft.AspNetCore.App](/aspnet/core/fundamentals/metapackage-app) ou [Microsoft.AspNetCore.All](/aspnet/core/fundamentals/metapackage), no entanto, o atributo é desnecessário. O SDK do .NET Core pode selecionar automaticamente a versão desses pacotes que deve ser usada.

### <a name="recommendation"></a>Recomendação

Ao fazer referência aos pacotes `Microsoft.AspNetCore.App` ou `Microsoft.AspNetCore.All`, não especifique a versão. Se uma versão for especificada, o SDK poderá apresentar um aviso NETSDK1071. Para corrigir esse aviso, remova a versão do pacote, como no exemplo a seguir:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.App" />
</ItemGroup>
```

> Problema conhecido: o SDK do .NET Core 2.1 tem suporte apenas para esta sintaxe, mas o projeto também usa o Microsoft.NET.Sdk.Web. Esse problema foi solucionado no SDK do .NET Core 2.2.

Essas referências aos metapacotes do ASP.NET Core têm um comportamento ligeiramente diferente da maioria dos pacotes normais do NuGet. [Implantações dependentes da estrutura](../deploying/index.md#publish-framework-dependent) de aplicativos que usam metapacotes aproveitam automaticamente a estrutura compartilhada do ASP.NET Core. Quando você usa os metapacotes, **nenhum** ativo dos pacotes NuGet do ASP.NET Core referenciados é implantado com o aplicativo porque a estrutura compartilhada do ASP.NET Core contém esses ativos. Os ativos na estrutura compartilhada são otimizados para a plataforma de destino para melhorar o tempo de inicialização do aplicativo. Para obter mais informações sobre a estrutura compartilhada, confira [Empacotamento de distribuição do .NET Core](../distribution-packaging.md).

Se uma versão *for* especificada, ela será tratada como a *versão mínima* da estrutura compartilhada do ASP.NET Core para implantações dependentes de estrutura e como uma versão *exata* para implementações autossuficientes. Isso pode ter as seguintes consequências:

- Se a versão do ASP.NET Core instalada no servidor for menor que a versão especificada no PackageReference, o processo do .NET Core falhará ao iniciar. Atualizações para o metapacote estão frequentemente disponíveis no NuGet.org antes de as atualizações serem disponibilizadas em ambientes de hospedagem como o Azure. A atualização da versão no PackageReference para o ASP.NET Core pode fazer com que um aplicativo implantado falhe.
- Se o aplicativo for implantado como uma [implantação autossuficiente](../deploying/index.md#publish-self-contained), ele poderá não conter as atualizações de segurança mais recentes para o .NET Core. Quando uma versão não é especificada, o SDK pode incluir automaticamente a versão mais recente do ASP.NET Core na implantação autossuficiente.

## <a name="default-compilation-includes-in-net-core-projects"></a>Inclusões de compilação padrão em projetos do .NET Core

Com a mudança para o formato *csproj* nas últimas versões do SDK, mudamos as inclusões e exclusões padrão de itens de compilação e recursos inseridos dos arquivos de propriedades do SDK. Isso significa que você não precisa mais especificar esses itens no arquivo de projeto.

O principal motivo disso é reduzir a desordem no arquivo de projeto. Os padrões presentes no SDK devem abranger os casos de uso mais comuns e, portanto, não há necessidade de repeti-los em cada projeto criado. Isso resulta em arquivos de projeto menores que são mais fáceis de serem entendidos, bem como editados manualmente, se necessário.

A seguinte tabela mostra qual elemento e quais [globs](https://en.wikipedia.org/wiki/Glob_(programming)) são incluídos e excluídos do SDK:

| Elemento           | Incluir glob                              | Excluir glob                                                  | Remover glob              |
|-------------------|-------------------------------------------|---------------------------------------------------------------|----------------------------|
| Compilar           | \*\*/\*.cs (ou outras extensões de linguagem) | \*\*/\*.user; \*\*/\*.\*proj; \*\*/\*.sln; \*\*/\*.vssscc  | N/D                      |
| EmbeddedResource  | \*\*/\*.resx                              | \*\*/\*.user; \*\*/\*.\*proj; \*\*/\*.sln; \*\*/\*.vssscc     | N/D                      |
| Nenhum              | \*\*/\*                                   | \*\*/\*.user; \*\*/\*.\*proj; \*\*/\*.sln; \*\*/\*.vssscc     | \*\*/\*.cs; \*\*/\*.resx   |

> [!NOTE]
> **Excluir glob** sempre exclui as pastas `./bin` e `./obj`, representadas pelas propriedades `$(BaseOutputPath)` e `$(BaseIntermediateOutputPath)` do MSBuild, respectivamente. Como um todo, todas as exclusões são representadas por `$(DefaultItemExcludes)`.

Se você tiver globs no projeto e tentar compilá-lo usando o SDK mais novo, receberá o seguinte erro:

> Itens de compilação duplicados foram incluídos. Por padrão, o SDK do .NET inclui itens de Compilação do diretório de projeto. É possível remover esses itens do arquivo de projeto ou definir a propriedade “EnableDefaultCompileItems” como “false” se desejar incluí-los explicitamente no arquivo de projeto.

Para corrigir esse erro, é possível remover os itens `Compile` explícitos que correspondem aos listados na tabela anterior ou definir a propriedade `<EnableDefaultCompileItems>` como `false`, desta maneira:

```xml
<PropertyGroup>
    <EnableDefaultCompileItems>false</EnableDefaultCompileItems>
</PropertyGroup>
```

A configuração dessa propriedade como `false` desabilitará a inclusão implícita, revertendo o comportamento para os SDKs anteriores nos quais era necessário especificar os globs padrão no projeto.

Essa alteração não modifica a mecânica principal de outras inclusões. No entanto, se desejar especificar, por exemplo, alguns arquivos para serem publicados com o aplicativo, ainda poderá usar os mecanismos conhecidos em *csproj* para fazer isso (por exemplo, o elemento `<Content>`).

`<EnableDefaultCompileItems>` somente desabilita globs `Compile`, mas não afeta outros globs, como o glob `None` implícito, que também se aplica a itens \*.cs. Por isso, o **Gerenciador de Soluções** continuará mostrando itens \*.cs como parte do projeto, incluídos como itens `None`. De maneira semelhante, você pode definir `<EnableDefaultNoneItems>` como falso para desabilitar o glob `None` implícito, da seguinte maneira:

```xml
<PropertyGroup>
    <EnableDefaultNoneItems>false</EnableDefaultNoneItems>
</PropertyGroup>
```

Para desabilitar **todos os globs implícitos**, defina a propriedade `<EnableDefaultItems>` como `false` como no seguinte exemplo:

```xml
<PropertyGroup>
    <EnableDefaultItems>false</EnableDefaultItems>
</PropertyGroup>
```

## <a name="how-to-see-the-whole-project-as-msbuild-sees-it"></a>Como ver o projeto inteiro conforme exibido pelo MSBuild

Uma vez que essas alterações do csproj simplificam bastante os arquivos de projeto, talvez você queira conferir o projeto totalmente expandido conforme exibido pelo MSBuild, quando o SDK e os respectivos destinos forem incluídos. Pré-processe o projeto com [a opção `/pp`](/visualstudio/msbuild/msbuild-command-line-reference#preprocess) do comando [`dotnet msbuild`](dotnet-msbuild.md), que mostra quais arquivos foram importados, as respectivas origens e contribuições para o build, sem realmente criar o projeto:

`dotnet msbuild -pp:fullproject.xml`

Quando o projeto tem várias estruturas de destino, os resultados do comando devem se concentrar apenas em uma delas, especificando-a como uma propriedade do MSBuild:

`dotnet msbuild -p:TargetFramework=netcoreapp2.0 -pp:fullproject.xml`

## <a name="additions"></a>Adições

### <a name="sdk-attribute"></a>Atributo do SDK

O elemento `<Project>` raiz do arquivo *.csproj* tem um novo atributo chamado `Sdk`. `Sdk` especifica qual SDK será usado pelo projeto. O SDK, como o [documento de camadas](cli-msbuild-architecture.md) descreve, é um conjunto de [tarefas](/visualstudio/msbuild/msbuild-tasks) e [destinos](/visualstudio/msbuild/msbuild-targets) do MSBuild que podem compilar o código do .NET Core. Os seguintes SDKs estão disponíveis para o .NET Core:

1. O SDK do .NET Core com a ID `Microsoft.NET.Sdk`
2. O SDK da Web do .NET Core com a ID `Microsoft.NET.Sdk.Web`
3. O SDK da Biblioteca de Classes Razor do .NET Core com a ID `Microsoft.NET.Sdk.Razor`
4. O serviço de trabalho do .NET Core com a ID `Microsoft.NET.Sdk.Worker` (desde o .net core 3,0)
5. O .NET Core WinForms e o WPF com a ID `Microsoft.NET.Sdk.WindowsDesktop` (desde o .NET Core 3,0)

É necessário ter o atributo `Sdk` definido com uma dessas IDs no elemento `<Project>` para usar as ferramentas do .NET Core e compilar o código.

### <a name="packagereference"></a>PackageReference

Um elemento de item `<PackageReference>` especifica uma [dependência do NuGet no projeto](/nuget/consume-packages/package-references-in-project-files). O atributo `Include` especifica a ID do pacote.

```xml
<PackageReference Include="package-id" Version="" PrivateAssets="" IncludeAssets="" ExcludeAssets="" />
```

#### <a name="version"></a>Versão

O atributo `Version` obrigatório especifica a versão do pacote para restauração. O atributo respeita as regras do esquema de [intervalo de versão do NuGet](/nuget/concepts/package-versioning#version-ranges) . O comportamento padrão é uma versão mínima, uma correspondência inclusiva. Por exemplo, especificar `Version="1.2.3"` é equivalente à notação `[1.2.3, )` do NuGet e significa que o pacote resolvido terá a versão 1.2.3, se disponível ou maior caso contrário.

#### <a name="includeassets-excludeassets-and-privateassets"></a>IncludeAssets, ExcludeAssets e PrivateAssets

O atributo `IncludeAssets` especifica quais ativos que pertencem ao pacote especificado pelo `<PackageReference>` devem ser consumidos. Por padrão, todos os ativos de pacote estão incluídos.

O atributo `ExcludeAssets` especifica quais ativos que pertencem ao pacote especificado pelo `<PackageReference>` não devem ser consumidos.

O atributo `PrivateAssets` especifica quais ativos que pertencem ao pacote especificado pelo `<PackageReference>` devem ser consumidos, mas não fluir para o próximo projeto. Quando esse atributo não estiver presente, os ativos `Analyzers`, `Build` e `ContentFiles` serão privados por padrão.

> [!NOTE]
> `PrivateAssets`é equivalente ao *project.jsno* / elemento*xproj* `SuppressParent` .

Esses atributos podem conter um ou mais dos itens a seguir, separados pelo caractere de ponto e vírgula `;` se houver mais de um:

- `Compile` – o conteúdo da pasta *lib* está disponível para a compilação.
- `Runtime` – o conteúdo da pasta de *tempo de execução* é distribuído.
- `ContentFiles` – o conteúdo da pasta *contentfiles* é usado.
- `Build` – as props/destinos na pasta *Build* são usados.
- `Native` – o conteúdo de ativos nativos é copiado para a pasta de *saída* para tempo de execução.
- `Analyzers` – os analisadores são usados.

Como alternativa, o atributo pode conter:

- `None` – nenhum dos ativos é usado.
- `All` – todos os ativos são usados.

### <a name="dotnetclitoolreference"></a>DotNetCliToolReference

Um elemento de item `<DotNetCliToolReference>` especifica a ferramenta da CLI que o usuário deseja restaurar no contexto do projeto. É uma substituição do nó `tools` em *project.json*.

```xml
<DotNetCliToolReference Include="<package-id>" Version="" />
```

Observe que `DotNetCliToolReference` [agora é preterido](https://github.com/dotnet/announcements/issues/107) em favor das [ferramentas locais do .NET Core](./global-tools.md#install-a-local-tool).

#### <a name="version"></a>Versão

`Version` especifica a versão do pacote para restauração. O atributo respeita as regras do esquema de [controle de versão do NuGet](/nuget/create-packages/dependency-versions#version-ranges). O comportamento padrão é uma versão mínima, uma correspondência inclusiva. Por exemplo, especificar `Version="1.2.3"` é equivalente à notação `[1.2.3, )` do NuGet e significa que o pacote resolvido terá a versão 1.2.3, se disponível ou maior caso contrário.

### <a name="runtimeidentifiers"></a>RuntimeIdentifiers

O elemento de propriedade `<RuntimeIdentifiers>` permite especificar uma lista delimitada por ponto e vírgula de [RIDs (Identificadores de Runtime)](../rid-catalog.md) para o projeto.
Os RIDs permitem publicar implantações autocontidas.

```xml
<RuntimeIdentifiers>win10-x64;osx.10.11-x64;ubuntu.16.04-x64</RuntimeIdentifiers>
```

### <a name="runtimeidentifier"></a>RuntimeIdentifier

O elemento de propriedade `<RuntimeIdentifier>` permite especificar somente um [RID (Identificador de Runtime)](../rid-catalog.md) para o projeto. O RID permite a publicação de uma implantação autossuficiente.

```xml
<RuntimeIdentifier>ubuntu.16.04-x64</RuntimeIdentifier>
```

Use `<RuntimeIdentifiers>` (plural) nesse caso, se você precisar publicar para vários runtimes. `<RuntimeIdentifier>` pode fornecer builds mais rápidos quando apenas um único runtime é necessário.

### <a name="packagetargetfallback"></a>PackageTargetFallback

O elemento de propriedade `<PackageTargetFallback>` permite especificar um conjunto de destinos compatíveis a serem usados ao restaurar pacotes. Ele foi criado para permitir que os pacotes que usam o dotnet [TxM (Destino x Moniker)](/nuget/schema/target-frameworks) operem com pacotes que não declaram um dotnet TxM. Se o projeto usar o dotnet TxM, todos os pacotes dos quais ele depende também deverão ter um dotnet TxM, a menos que você adicione o `<PackageTargetFallback>` ao projeto para permitir que plataformas não dotnet sejam compatíveis com o dotnet.

O exemplo a seguir fornece os fallbacks para todos os destinos em seu projeto:

```xml
<PackageTargetFallback>
    $(PackageTargetFallback);portable-net45+win8+wpa81+wp8
</PackageTargetFallback >
```

O exemplo a seguir especifica os fallbacks apenas para o destino `netcoreapp2.1`:

```xml
<PackageTargetFallback Condition="'$(TargetFramework)'=='netcoreapp2.1'">
    $(PackageTargetFallback);portable-net45+win8+wpa81+wp8
</PackageTargetFallback >
```

## <a name="build-events"></a>Eventos de build

A maneira como os eventos de pré e pós-compilação são especificados no arquivo de projeto foi alterado. As propriedades PreBuildEvent e PostBuildEvent não são recomendadas no formato de projeto no estilo SDK, pois as macros como $ (ProjectDir) não são resolvidas. Por exemplo, o código a seguir não tem mais suporte:

```xml
<PropertyGroup>
    <PreBuildEvent>"$(ProjectDir)PreBuildEvent.bat" "$(ProjectDir)..\" "$(ProjectDir)" "$(TargetDir)"</PreBuildEvent>
</PropertyGroup>
```

Em projetos em estilo SDK, use um destino do MSBuild chamado `PreBuild` ou `PostBuild` e defina a `BeforeTargets` propriedade para `PreBuild` ou a `AfterTargets` propriedade para `PostBuild` . Para o exemplo anterior, use o seguinte código:

```xml
<Target Name="PreBuild" BeforeTargets="PreBuildEvent">
    <Exec Command="&quot;$(ProjectDir)PreBuildEvent.bat&quot; &quot;$(ProjectDir)..\&quot; &quot;$(ProjectDir)&quot; &quot;$(TargetDir)&quot;" />
</Target>

<Target Name="PostBuild" AfterTargets="PostBuildEvent">
   <Exec Command="echo Output written to $(TargetDir)" />
</Target>
```

> [!NOTE]
>Você pode usar qualquer nome para os destinos do MSBuild, mas o IDE do Visual Studio reconhece `PreBuild` e se `PostBuild` destina, portanto, é recomendável usar esses nomes para que você possa editar os comandos no IDE do Visual Studio.

## <a name="nuget-metadata-properties"></a>Propriedades de metadados do NuGet

Com a mudança para o MSBuild, movemos os metadados de entrada que são usados ao empacotar um pacote NuGet de *project.jsem* arquivos *. csproj* . As entradas são propriedades do MSBuild e, portanto, precisam estar em um grupo `<PropertyGroup>`. A seguir está a lista de propriedades que são usadas como entradas para o processo de empacotamento ao usar o `dotnet pack` comando ou o `Pack` destino do MSBuild que faz parte do SDK:

### <a name="ispackable"></a>IsPackable

Um valor booliano que especifica se o projeto pode ser empacotado. O valor padrão é `true`.

### <a name="packageversion"></a>PackageVersion

Especifica a versão que o pacote resultante terá. Aceita todos os formatos de cadeia de caracteres de versão do NuGet. O padrão é o valor `$(Version)`, ou seja, da propriedade `Version` no projeto.

### <a name="packageid"></a>PackageId

Especifica o nome para o pacote resultante. Se não for especificado, a operação `pack` usará como padrão o `AssemblyName` ou o nome do diretório como o nome do pacote.

### <a name="title"></a>Title

Um título amigável do pacote, geralmente usado em exibições de interface do usuário como em nuget.org e no Gerenciador de Pacotes do Visual Studio. Se não for especificado, a ID do pacote será usada em seu lugar.

### <a name="authors"></a>Autores

Uma lista separada por ponto-e-vírgula de autores de pacotes, que correspondem aos nomes de perfil em nuget.org. Eles são exibidos na galeria do NuGet em nuget.org e são usados para fazer referência cruzada de pacotes pelos mesmos autores.

### <a name="packagedescription"></a>PackageDescription

Uma descrição longa do pacote para exibição de interface do usuário.

### <a name="description"></a>Descrição

Uma descrição longa para o manifesto do assembly. Se `PackageDescription` não for especificado, essa propriedade também será usada como a descrição do pacote.

### <a name="copyright"></a>Direitos autorais

Detalhes sobre direitos autorais do pacote.

### <a name="packagerequirelicenseacceptance"></a>PackageRequireLicenseAcceptance

Um valor booliano que especifica se o cliente deve solicitar que o consumidor aceite a licença do pacote antes de instalá-lo. O padrão é `false`.

### <a name="developmentdependency"></a>DevelopmentDependency

Um valor booliano que especifica se o pacote está marcado como uma dependência somente de desenvolvimento, o que impede que o pacote seja incluído como uma dependência em outros pacotes. Com PackageReference (NuGet 4.8 +), esse sinalizador também significa que os ativos de tempo de compilação são excluídos da compilação. Para obter mais informações, confira [Suporte do DevelopmentDependency para PackageReference](https://github.com/NuGet/Home/wiki/DevelopmentDependency-support-for-PackageReference).

### <a name="packagelicenseexpression"></a>PackageLicenseExpression

Um [identificador de licença SPDX](https://spdx.org/licenses/) ou uma expressão. Por exemplo, `Apache-2.0`.

Aqui está a lista completa dos [identificadores de licença SPDX](https://spdx.org/licenses/). O NuGet.org aceita apenas licenças aprovadas por OSI ou FSF ao usar expressão de tipo de licença.

A sintaxe exata das expressões de licença será descrita abaixo em [ABNF](https://tools.ietf.org/html/rfc5234).

```abnf
license-id            = <short form license identifier from https://spdx.org/spdx-specification-21-web-version#h.luq9dgcle9mo>

license-exception-id  = <short form license exception identifier from https://spdx.org/spdx-specification-21-web-version#h.ruv3yl8g6czd>

simple-expression = license-id / license-id”+”

compound-expression =  1*1(simple-expression /
                simple-expression "WITH" license-exception-id /
                compound-expression "AND" compound-expression /
                compound-expression "OR" compound-expression ) /
                "(" compound-expression ")" )

license-expression =  1*1(simple-expression / compound-expression / UNLICENSED)
```

> [!NOTE]
> Somente um dentre `PackageLicenseExpression`, `PackageLicenseFile` e `PackageLicenseUrl` pode ser especificado por vez.

### <a name="packagelicensefile"></a>PackageLicenseFile

Caminho até um arquivo de licença dentro do pacote, se você estiver usando uma licença que não tenha recebido um identificador SPDX ou for uma licença personalizada (caso contrário, `PackageLicenseExpression` tem preferência)

Substitui `PackageLicenseUrl` , não pode ser combinado com `PackageLicenseExpression` e requer o Visual Studio versão 15.9.4 e o SDK do .NET 2.1.502 ou 2.2.101 ou mais recente.

Você precisará garantir o fornecimento do arquivo de licença adicionando-o explicitamente ao projeto. Exemplo de uso:

```xml
<PropertyGroup>
  <PackageLicenseFile>LICENSE.txt</PackageLicenseFile>
</PropertyGroup>
<ItemGroup>
  <None Include="licenses\LICENSE.txt" Pack="true" PackagePath="$(PackageLicenseFile)"/>
</ItemGroup>
```

### <a name="packagelicenseurl"></a>PackageLicenseUrl

Uma URL para a licença aplicável ao pacote. (_preterida desde o Visual Studio 15.9.4, SDK 2.1.502 e 2.2.101 do .NET_)

### <a name="packageicon"></a>PackageIcon

Um caminho para uma imagem no pacote a ser usado como um ícone de pacote. Leia mais sobre [ `icon` metadados](/nuget/reference/nuspec#icon). [PackageIconUrl foi preterido](/nuget/reference/msbuild-targets#packageiconurl) em favor de PackageIcon.

### <a name="packagereleasenotes"></a>PackageReleaseNotes

Notas de versão do projeto.

### <a name="packagetags"></a>PackageTags

Uma lista delimitada por ponto e vírgula de marcas que designam o pacote.

### <a name="packageoutputpath"></a>PackageOutputPath

Determina o caminho de saída no qual o pacote empacotado será solto. O padrão é `$(OutputPath)`.

### <a name="includesymbols"></a>IncludeSymbols
Esse valor booliano indica se o pacote deve criar um pacote de símbolos adicionais quando o projeto é empacotado. O formato do pacote de símbolos é controlado pela propriedade `SymbolPackageFormat`.

### <a name="symbolpackageformat"></a>SymbolPackageFormat
Especifica o formato do pacote de símbolos. Se "symbols.nupkg", um pacote legado de símbolos será criado com uma extensão *.symbols.nupkg* contendo PDBs, DLLs e outros arquivos de saída. Se "snupkg", um pacote de símbolos do snupkg será criado contendo os PDBs portáveis. O padrão é "symbols.nupkg".

### <a name="includesource"></a>IncludeSource

Esse valor booliano indica se o processo do pacote deve criar um pacote de origem. O pacote de origem contém o código-fonte da biblioteca, bem como arquivos PDB. Os arquivos de origem são colocados no diretório `src/ProjectName`, no arquivo de pacote resultante.

### <a name="istool"></a>IsTool

Especifica se todos os arquivos de saída são copiados para a pasta *tools* em vez da pasta *lib*. Isso é diferente de a `DotNetCliTool` , que é especificado definindo o `PackageType` no arquivo *. csproj* .

### <a name="repositoryurl"></a>RepositoryUrl

Especifica a URL para o repositório em que reside o código-fonte do pacote e/ou do qual ele está sendo compilado.

### <a name="repositorytype"></a>RepositoryType

Especifica o tipo do repositório. O padrão é “git”.

### <a name="repositorybranch"></a>RepositoryBranch
Especifica o nome da ramificação de origem no repositório. Quando o projeto é empacotado em um pacote NuGet, ele é adicionado aos metadados do pacote.

### <a name="repositorycommit"></a>RepositoryCommit
Confirmação opcional do repositório ou conjunto de alterações para indicar de qual fonte o pacote foi criado. `RepositoryUrl` também deve ser especificado para que essa propriedade seja incluída. Quando o projeto é empacotado em um pacote NuGet, esse Commit ou conjunto de alterações é adicionado aos metadados do pacote.

### <a name="nopackageanalysis"></a>NoPackageAnalysis

Especifica que o pacote não deve executar a análise de pacote após a compilação do pacote.

### <a name="minclientversion"></a>MinClientVersion

Especifica a versão mínima do cliente NuGet que pode instalar esse pacote, imposta pelo nuget.exe e pelo Gerenciador de Pacotes do Visual Studio.

### <a name="includebuildoutput"></a>IncludeBuildOutput

Esse valor booliano especifica se os assemblies de saída da compilação devem ser empacotados no arquivo *. nupkg* ou não.

### <a name="includecontentinpack"></a>IncludeContentInPack

Esse valor booliano especifica se todos os itens que têm um tipo `Content` serão incluídos automaticamente no pacote resultante. O padrão é `true`.

### <a name="buildoutputtargetfolder"></a>BuildOutputTargetFolder

Especifica a pasta para colocar os assemblies de saída. Os assemblies de saída (e outros arquivos de saída) são copiados para suas respectivas pastas de estrutura.

### <a name="contenttargetfolders"></a>ContentTargetFolders

Essa propriedade especifica o local padrão para o qual todos os arquivos de conteúdo deverão ir se `PackagePath` não for especificado para eles. O valor padrão é “content;contentFiles”.

### <a name="nuspecfile"></a>NuspecFile

Caminho relativo ou absoluto para o arquivo *.nuspec* que está sendo usado para o empacotamento.

> [!NOTE]
> Se o arquivo *.nuspec* for especificado, ele será usado **exclusivamente** para as informações de empacotamento e as informações nos projetos não serão usadas.

### <a name="nuspecbasepath"></a>NuspecBasePath

O caminho base para o arquivo *.nuspec*.

### <a name="nuspecproperties"></a>NuspecProperties

Lista separada por ponto e vírgula de pares chave-valor.

## <a name="assemblyinfo-properties"></a>Propriedades de AssemblyInfo

[Atributos de assembly](../../standard/assembly/set-attributes.md) que estavam presentes em um arquivo *AssemblyInfo* agora são gerados automaticamente de propriedades.

### <a name="properties-per-attribute"></a>Propriedades por atributo

Conforme mostrado na tabela a seguir, cada atributo tem uma propriedade que controla seu conteúdo e outro que desabilita sua geração:

| Atributo                                                      | Propriedade               | Propriedade para desabilitar                             |
|----------------------------------------------------------------|------------------------|-------------------------------------------------|
| <xref:System.Reflection.AssemblyCompanyAttribute>              | `Company`              | `GenerateAssemblyCompanyAttribute`              |
| <xref:System.Reflection.AssemblyConfigurationAttribute>        | `Configuration`        | `GenerateAssemblyConfigurationAttribute`        |
| <xref:System.Reflection.AssemblyCopyrightAttribute>            | `Copyright`            | `GenerateAssemblyCopyrightAttribute`            |
| <xref:System.Reflection.AssemblyDescriptionAttribute>          | `Description`          | `GenerateAssemblyDescriptionAttribute`          |
| <xref:System.Reflection.AssemblyFileVersionAttribute>          | `FileVersion`          | `GenerateAssemblyFileVersionAttribute`          |
| <xref:System.Reflection.AssemblyInformationalVersionAttribute> | `InformationalVersion` | `GenerateAssemblyInformationalVersionAttribute` |
| <xref:System.Reflection.AssemblyProductAttribute>              | `Product`              | `GenerateAssemblyProductAttribute`              |
| <xref:System.Reflection.AssemblyTitleAttribute>                | `AssemblyTitle`        | `GenerateAssemblyTitleAttribute`                |
| <xref:System.Reflection.AssemblyVersionAttribute>              | `AssemblyVersion`      | `GenerateAssemblyVersionAttribute`              |
| <xref:System.Resources.NeutralResourcesLanguageAttribute>      | `NeutralLanguage`      | `GenerateNeutralResourcesLanguageAttribute`     |

Observações:

- O padrão de `AssemblyVersion` e `FileVersion` é usar o valor de `$(Version)` sem sufixo. Por exemplo, se `$(Version)` fosse `1.2.3-beta.4`, então o valor seria `1.2.3`.
- `InformationalVersion` usa por padrão o valor de `$(Version)`.
- `$(SourceRevisionId)` é acrescentado a `InformationalVersion` se a propriedade está presente. Ele pode ser desabilitado usando `IncludeSourceRevisionInInformationalVersion`.
- As propriedades `Copyright` e `Description` também são usadas para metadados do NuGet.
- `Configuration` é compartilhado com o processo de build e definido por meio do parâmetro `--configuration` de comandos `dotnet`.

### <a name="generateassemblyinfo"></a>GenerateAssemblyInfo

Um booliano que habilita ou desabilita a geração de AssemblyInfo. O valor padrão é `true`.

### <a name="generatedassemblyinfofile"></a>GeneratedAssemblyInfoFile

O caminho do arquivo de informações do assembly gerado. Um arquivo no diretório `$(IntermediateOutputPath)` (obj) é usado por padrão.
