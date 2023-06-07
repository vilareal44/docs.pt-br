---
title: Modelos personalizados para dotnet new
description: Saiba mais sobre modelos personalizados para qualquer tipo de projeto ou de arquivos do .NET.
author: adegeo
ms.date: 05/20/2020
ms.openlocfilehash: cabe220917e7ff688a2c2d2df56d9bc7f8afdf56
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85324502"
---
# <a name="custom-templates-for-dotnet-new"></a>Modelos personalizados para dotnet new

O [SDK do .NET Core](https://dotnet.microsoft.com/download) apresenta muitos modelos já instalados e prontos para uso. O [ `dotnet new` comando](dotnet-new.md) não é apenas a maneira de usar um modelo, mas também como instalar e desinstalar modelos. A partir do .NET Core 2.0, é possível criar seus próprios modelos personalizados para qualquer tipo de projeto, como um aplicativo, serviço, ferramenta ou biblioteca de classes. É possível, inclusive, criar um modelo que gere um ou mais arquivos independentes, como um arquivo de configuração.

Você pode instalar modelos personalizados de um pacote NuGet em qualquer feed do NuGet, referenciando um arquivo NuGet *. nupkg* diretamente ou especificando um diretório do sistema de arquivos que contém o modelo. O mecanismo de modelo oferece recursos que permitem substituir valores, incluir e excluir arquivos e executar operações de processamento personalizadas quando seu modelo é usado.

O mecanismo de modelo é um software livre e o repositório de código online fica em [dotnet/modelagem](https://github.com/dotnet/templating/) no GitHub. Mais modelos, incluindo modelos de terceiros, são encontrados em [Available templates for dotnet new](https://github.com/dotnet/templating/wiki/Available-templates-for-dotnet-new) (Modelos disponíveis para dotnet new) no GitHub. Para obter mais informações sobre a criação e o uso de modelos personalizados, consulte [How to create your own templates for dotnet new](https://devblogs.microsoft.com/dotnet/how-to-create-your-own-templates-for-dotnet-new/) (Como criar seus próprios modelos para dotnet new) e o [Wiki do repositório GitHub dotnet/modelagem](https://github.com/dotnet/templating/wiki).

> [!NOTE]
> Os exemplos de modelo estão disponíveis no repositório [dotnet/dotnet-template-Samples](https://github.com/dotnet/dotnet-template-samples) github. No entanto, embora esses exemplos sejam um bom recurso para aprender como os modelos funcionam, o repositório é arquivado e não é mais mantido. Os exemplos podem estar desatualizados e não funcionar mais.

Para seguir um passo a passo e criar um modelo, consulte o tutorial [Create a custom template for dotnet new](../tutorials/cli-templates-create-item-template.md) (Criar um modelo personalizado para dotnet new).

### <a name="net-default-templates"></a>Modelos padrão do .NET

Quando você instala o [SDK do .NET Core](https://dotnet.microsoft.com/download), você recebe dezenas de modelos internos para criar projetos e arquivos, incluindo aplicativos de console, bibliotecas de classes, projetos de teste de unidade, aplicativos ASP.NET Core (incluindo projetos [Angulares](https://angular.io/) e de [Reação](https://facebook.github.io/react/)) e arquivos de configuração. Para listar os modelos internos, execute o comando `dotnet new` com a opção `-l|--list`:

```dotnetcli
dotnet new --list
```

## <a name="configuration"></a>Configuração

O modelo é composto pelas seguintes partes:

- Arquivos e pastas de origem.
- Um arquivo de configuração (*template.jsem*).

### <a name="source-files-and-folders"></a>Arquivos e pastas de origem

Os arquivos e pastas de origem incluem os arquivos e pastas que você desejar que o mecanismo de modelo use quando o comando `dotnet new <TEMPLATE>` for executado. O mecanismo de modelo foi desenvolvido para usar *projetos executáveis* como código-fonte para produzir os projetos. Isso tem várias vantagens:

- O mecanismo de modelo não exige que você insira tokens especiais no código-fonte do seu projeto.
- Os arquivos de código não são arquivos especiais nem modificados de nenhuma maneira para trabalhar com o mecanismo de modelo. Portanto, as ferramentas que você normalmente usa ao trabalhar com projetos também funcionam com o conteúdo do modelo.
- Compile, execute e depurar seus projetos de modelo assim como você faz com quaisquer outros projetos.
- É possível criar rapidamente um modelo com base em um projeto existente simplesmente adicionando um arquivo de configuração *./.template.config/template.json* ao projeto.

Os arquivos e pastas armazenados no modelo não são limitados aos tipos de projeto .NET formais. Os arquivos e pastas de origem podem ser compostos por qualquer conteúdo que você queira criar quando o modelo é usado, mesmo se o mecanismo do modelo produzir apenas um arquivo para sua saída.

Os arquivos gerados pelo modelo podem ser modificados com base na lógica e nas configurações que você forneceu no arquivo de configuração *template.json*. O usuário pode substituir essas configurações transmitindo opções para o comando `dotnet new <TEMPLATE>`. Um exemplo comum de lógica personalizada é fornecer o nome de uma classe ou variável no arquivo de código que é implantado por um modelo.

### <a name="templatejson"></a>template.json

O arquivo *template.json* é colocado em uma pasta *.template.config* no diretório raiz do modelo. O arquivo fornece informações de configuração para o mecanismo de modelo. A configuração mínima requer os membros mostrados na tabela a seguir, suficiente para criar um modelo funcional.

| Membro            | Tipo          | Descrição |
| ----------------- | ------------- | ----------- |
| `$schema`         | URI           | O esquema JSON do arquivo *template.json*. Os editores que dão suporte a esquemas JSON habilitam recursos de edição de JSON quando o esquema é especificado. Por exemplo, o [Visual Studio Code](https://code.visualstudio.com/) requer que esse membro habilite o IntelliSense. Use um valor de `http://json.schemastore.org/template`. |
| `author`          | string        | O autor do modelo. |
| `classifications` | array(string) | Zero ou mais características do modelo que um usuário pode usar para localizá-lo ao procurá-lo. As classificações também são exibidas na coluna *Marcas* quando ela é exibida em uma lista de modelos produzida usando o comando `dotnet new -l|--list`. |
| `identity`        | string        | Um nome exclusivo para este modelo. |
| `name`            | string        | O nome do modelo que os usuários devem ver. |
| `shortName`       | string        | Um nome abreviado padrão para seleção do modelo aplicável aos ambientes em que o nome do modelo é especificado pelo usuário, não selecionado por uma GUI. Por exemplo, o nome curto é útil ao usar os modelos em um prompt de comando com comandos CLI. |

O esquema completo do arquivo *template.json* é encontrado no [Repositório de Esquema JSON](http://json.schemastore.org/template). Para saber mais sobre o arquivo *template.json*, veja o [wiki de modelagem dotnet](https://github.com/dotnet/templating/wiki).

#### <a name="example"></a>Exemplo

Veja a seguir um exemplo de pasta de modelo que contém dois arquivos de conteúdo: *console.cs* e *readme.txt*. Observe que há uma pasta obrigatória chamada *.template.config* que contém o arquivo *template.json*.

```text
└───mytemplate
    │   console.cs
    │   readme.txt
    │
    └───.template.config
            template.json
```

O arquivo *template.json* será parecido com este:

```json
{
  "$schema": "http://json.schemastore.org/template",
  "author": "Travis Chau",
  "classifications": [ "Common", "Console" ],
  "identity": "AdatumCorporation.ConsoleTemplate.CSharp",
  "name": "Adatum Corporation Console Application",
  "shortName": "adatumconsole"
}
```

A pasta *mytemplate* é um pacote de modelo que pode ser instalado. Depois que o pacote estiver instalado, o `shortName` poderá ser usado com o comando `dotnet new`. Por exemplo, `dotnet new adatumconsole` resultaria nos arquivos `console.cs` e `readme.txt` para a pasta atual.

## <a name="packing-a-template-into-a-nuget-package-nupkg-file"></a>Empacotamento de um modelo em um pacote NuGet (arquivo nupkg)

Um modelo personalizado é fornecido com o comando [dotnet pack](dotnet-pack.md) e um arquivo *csproj*. Como alternativa, [NuGet](https://docs.microsoft.com/nuget/tools/nuget-exe-cli-reference) pode ser usado com o comando [nuget pack](https://docs.microsoft.com/nuget/tools/cli-ref-pack) com um arquivo *.nuspec*. No entanto, o NuGet requer o .NET Framework no Windows e no [mono](https://www.mono-project.com/) no Linux e no MacOS.

O arquivo *.csproj* é ligeiramente diferente de um arquivo *.csproj* de projeto de código tradicional. Observe as seguintes configurações:

01. A configuração `<PackageType>` é adicionada e definida como `Template`.
01. A configuração `<PackageVersion>` é adicionada e definida como um [número de versão do NuGet](/nuget/reference/package-versioning) válido.
01. A configuração `<PackageId>` é adicionada e definida como um identificador exclusivo. Esse identificador é usado para desinstalar o pacote de modelo e também pelos feeds do NuGet para registrar seu pacote de modelo.
01. As configurações genéricas de metadados devem ser definidas: `<Title>`, `<Authors>`, `<Description>` e `<PackageTags>`.
01. A configuração `<TargetFramework>` deve ser definida, mesmo que o binário produzido pelo processo de modelo não seja usado. No exemplo abaixo, ela é definida como `netstandard2.0`.

Um pacote de modelo, na forma de um pacote NuGet *.nupkg*, requer que todos os modelos sejam armazenados na pasta *content* dentro do pacote. Há mais algumas configurações a serem adicionadas a um arquivo *.csproj* para garantir que o *.nupkg* gerado possa ser instalado como um pacote de modelo:

01. A configuração `<IncludeContentInPack>` é definida como `true` para incluir qualquer arquivo de projeto defina como **conteúdo** no pacote NuGet.
01. A configuração `<IncludeBuildOutput>` é definida como `false` para excluir todos os binários gerados pelo compilador do pacote NuGet.
01. A configuração `<ContentTargetFolders>` é definida como `content`. Isso garante que os arquivos definidos como **conteúdo** sejam armazenadas na pasta *content* do pacote NuGet. Essa pasta no pacote NuGet é analisada pelo sistema de modelo do dotnet.

Uma maneira fácil de impedir que todos os arquivos de código sejam compilados pelo seu projeto de modelo é usar o item `<Compile Remove="**\*" />` em seu arquivo de projeto, dentro de um elemento `<ItemGroup>`.

Uma maneira fácil de estruturar seu pacote de modelo é colocar todos os modelos em pastas individuais e, em seguida, cada pasta de modelo dentro de uma pasta *templates* que está localizada no mesmo diretório que seu arquivo *.csproj*. Dessa forma, você pode usar um único item de projeto para incluir todos os arquivos e pastas nos *modelos* como **conteúdo**. Dentro de um elemento `<ItemGroup>`, crie um item `<Content Include="templates\**\*" Exclude="templates\**\bin\**;templates\**\obj\**" />`.

Veja a seguir um exemplo de arquivo *.csproj* que segue todas as diretrizes acima. Ele empacota a pasta filho *templates* na pasta do pacote *content* e impede que qualquer arquivo de código seja compilado.

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <PackageType>Template</PackageType>
    <PackageVersion>1.0</PackageVersion>
    <PackageId>AdatumCorporation.Utility.Templates</PackageId>
    <Title>AdatumCorporation Templates</Title>
    <Authors>Me</Authors>
    <Description>Templates to use when creating an application for Adatum Corporation.</Description>
    <PackageTags>dotnet-new;templates;contoso</PackageTags>
    <TargetFramework>netstandard2.0</TargetFramework>

    <IncludeContentInPack>true</IncludeContentInPack>
    <IncludeBuildOutput>false</IncludeBuildOutput>
    <ContentTargetFolders>content</ContentTargetFolders>
  </PropertyGroup>

  <ItemGroup>
    <Content Include="templates\**\*" Exclude="templates\**\bin\**;templates\**\obj\**" />
    <Compile Remove="**\*" />
  </ItemGroup>

</Project>
```

O exemplo abaixo demonstra a estrutura de arquivos e pastas ao usar um *.csproj* para criar um pacote de modelo. O arquivo *MyDotnetTemplates.csproj* e a pasta *templates* estão localizados na raiz de um diretório chamado *project_folder*. A pasta *templates* contém dois modelos, *mytemplate1* e *mytemplate2*. Cada modelo tem arquivos de conteúdo e uma pasta *.template.config* com um arquivo de configuração *template.json*.

```text
project_folder
│   MyDotnetTemplates.csproj
│
└───templates
    ├───mytemplate1
    │   │   console.cs
    │   │   readme.txt
    │   │
    │   └───.template.config
    │           template.json
    │
    └───mytemplate2
        │   otherfile.cs
        │
        └───.template.config
                template.json
```

## <a name="installing-a-template"></a>Instalação de um modelo

Use o comando [dotnet new -i|--install](dotnet-new.md) para instalar um pacote.

### <a name="to-install-a-template-from-a-nuget-package-stored-at-nugetorg"></a>Para instalar um modelo de um pacote NuGet armazenado em nuget.org

Use o identificador do pacote NuGet para instalar um pacote de modelo.

```dotnetcli
dotnet new -i <NUGET_PACKAGE_ID>
```

### <a name="to-install-a-template-from-a-local-nupkg-file"></a>Para instalar um modelo de um arquivo nupkg local

Forneça o caminho para um arquivo de pacote NuGet *.nupkg*.

```dotnetcli
dotnet new -i <PATH_TO_NUPKG_FILE>
```

### <a name="to-install-a-template-from-a-file-system-directory"></a>Para instalar um modelo de um diretório de sistema de arquivos

Os modelos podem ser instalados de uma pasta de modelo, como a pasta *mytemplate1* do exemplo acima. Especifique o caminho da pasta *.template.config*. O caminho para o diretório de modelo não precisa ser absoluto. No entanto, um caminho absoluto é necessário para desinstalar um modelo que foi instalado de uma pasta.

```dotnetcli
dotnet new -i <FILE_SYSTEM_DIRECTORY>
```

## <a name="get-a-list-of-installed-templates"></a>Obter uma lista de modelos instalados

O comando de desinstalação, sem quaisquer outros parâmetros, listará todos os modelos.

```dotnetcli
dotnet new -u
```

Esse comando retorna algo semelhante à seguinte saída:

```console
Template Instantiation Commands for .NET Core CLI

Currently installed items:
  Microsoft.DotNet.Common.ItemTemplates
    Templates:
      global.json file (globaljson)
      NuGet Config (nugetconfig)
      Solution File (sln)
      Dotnet local tool manifest file (tool-manifest)
      Web Config (webconfig)
  Microsoft.DotNet.Common.ProjectTemplates.3.0
    Templates:
      Class library (classlib) C#
      Class library (classlib) F#
      Class library (classlib) VB
      Console Application (console) C#
      Console Application (console) F#
      Console Application (console) VB
...
```

O primeiro nível de itens após `Currently installed items:` são os identificadores usados na desinstalação de um modelo. E, no exemplo acima, são listados `Microsoft.DotNet.Common.ItemTemplates` e `Microsoft.DotNet.Common.ProjectTemplates.3.0`. Se o modelo foi instalado usando um caminho do sistema de arquivos, esse identificador será o caminho da pasta *.template.config*.

## <a name="uninstalling-a-template"></a>Desinstalação de um modelo

Use o comando [dotnet new -u|--uninstall](dotnet-new.md) para desinstalar um pacote.

Se o pacote foi instalado por um feed do NuGet ou por um arquivo *.nupkg* diretamente, forneça o identificador.

```dotnetcli
dotnet new -u <NUGET_PACKAGE_ID>
```

Se o pacote foi instalado com a especificação de um caminho para a pasta *.template.config*, use esse caminho **absoluto** para desinstalar o pacote. Você pode ver o caminho absoluto do modelo na saída fornecida pelo comando `dotnet new -u`. Para saber mais, confira a seção [Obter uma lista de modelos instalados](#get-a-list-of-installed-templates) acima.

```dotnetcli
dotnet new -u <ABSOLUTE_FILE_SYSTEM_DIRECTORY>
```

## <a name="create-a-project-using-a-custom-template"></a>Criar um projeto usando um modelo personalizado

Depois que um modelo é instalado, use o modelo executando o comando `dotnet new <TEMPLATE>` como você faria com qualquer outro modelo pré-instalado. Também é possível especificar [opções](dotnet-new.md#options) para o comando `dotnet new`, incluindo opções específicas do modelo definidas nas configurações de modelo. Forneça o nome curto do modelo diretamente no comando:

```dotnetcli
dotnet new <TEMPLATE>
```

## <a name="see-also"></a>Veja também

- [Criar um modelo personalizado para dotnet new (tutorial)](../tutorials/cli-templates-create-item-template.md)
- [Wiki do repositório GitHub dotnet/modelagem](https://github.com/dotnet/templating/wiki)
- [Repositório do GitHub de dotnet/dotnet-template-samples](https://github.com/dotnet/dotnet-template-samples)
- [Como criar seus próprios modelos para dotnet new](https://devblogs.microsoft.com/dotnet/how-to-create-your-own-templates-for-dotnet-new/)
- [Esquema *template.json* no Repositório de Esquema JSON](http://json.schemastore.org/template)
