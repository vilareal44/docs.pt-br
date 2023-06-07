---
title: Criar um modelo de projetos para o dotnet new
description: Saiba como criar um modelo de projetos para o comando dotnet new.
author: adegeo
ms.date: 06/25/2019
ms.topic: tutorial
ms.author: adegeo
ms.openlocfilehash: 75fedb2333a4ef9e16a27126055b6cacaf37c1c5
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85324328"
---
# <a name="tutorial-create-a-project-template"></a>Tutorial: criar um modelo de projeto

Com o .NET Core, você pode criar e implantar modelos que geram projetos, arquivos e até recursos. Este tutorial é a parte dois de uma série que ensina como criar, instalar e desinstalar modelos para usar com o comando `dotnet new`.

Nesta parte da série, você aprenderá a:

> [!div class="checklist"]
>
> * Criar os recursos de um modelo de projeto
> * Criar a pasta e o arquivo de configuração do modelo
> * Instalar um modelo a partir de um caminho de arquivos
> * Testar um modelo de item
> * Desinstalar um modelo de item

## <a name="prerequisites"></a>Pré-requisitos

* Conclua a [parte 1](cli-templates-create-item-template.md) desta série de tutoriais.
* Abra um terminal e navegue até a pasta _working\templates_.

## <a name="create-a-project-template"></a>Criar um modelo de projeto

Os modelos de projeto produzem projetos prontos para execução que ajudam os usuários a começar com um conjunto de códigos de trabalho. O .NET Core inclui alguns modelos de projeto, como um aplicativo de console ou uma biblioteca de classes. Neste exemplo, você criará um novo projeto de console que habilitará o C# 8.0 e produzirá um ponto de entrada `async main`.

No terminal, navegue até a pasta _working\templates_ e crie uma nova subpasta chamada _consoleasync_. Insira a subpasta e execute `dotnet new console` para gerar o aplicativo de console padrão. Você editará os arquivos produzidos por este modelo para criar um novo modelo.

```console
working
└───templates
    └───consoleasync
            consoleasync.csproj
            Program.cs
```

## <a name="modify-programcs"></a>Modificar o Program.cs

Abra o arquivo _program.cs_. O projeto do console não usa um ponto de entrada assíncrono, então vamos adicionar isso. Altere seu código para o seguinte e salve o arquivo.

```csharp
using System;
using System.Threading.Tasks;

namespace consoleasync
{
    class Program
    {
        static async Task Main(string[] args)
        {
            await Console.Out.WriteAsync("Hello World with C# 8.0!");
        }
    }
}
```

## <a name="modify-consoleasynccsproj"></a>Modificar consoleasync.csproj

Vamos atualizar a versão em linguagem C# que o projeto usa para a versão 8.0. Edite o arquivo _consoleasync.csproj_ e adicione a configuração `<LangVersion>` a um nó `<PropertyGroup>`.

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp2.2</TargetFramework>

    <LangVersion>8.0</LangVersion>

  </PropertyGroup>
  
</Project>
```

## <a name="build-the-project"></a>Compilar o projeto

Antes de concluir um modelo de projeto, você deve testá-lo para garantir que ele seja compilado e executado corretamente.

No seu terminal, execute o comando a seguir.

```dotnetcli
dotnet run
```

Você Obtém a saída a seguir.

```console
Hello World with C# 8.0!
```

Você pode excluir as pastas _obj_ e _bin_ criadas usando `dotnet run`. A exclusão desses arquivos garante que o modelo inclua apenas os arquivos relacionados ao modelo e não os arquivos resultantes de uma ação de compilação.

Agora que você tem o conteúdo do modelo criado, é necessário criar a configuração do modelo na pasta raiz do modelo.

## <a name="create-the-template-config"></a>Criar a configuração do modelo

Os modelos são reconhecidos no .NET Core por uma pasta especial e um arquivo de configuração que está na raiz do modelo. Neste tutorial, a pasta de modelos está localizada em _working\templates\consoleasync_.

Quando você cria um modelo, todos os arquivos e pastas na pasta de modelos são incluídos como parte do modelo, exceto a pasta de configuração especial. Esta pasta de configuração chama-se _.template.config_.

Primeiro, crie uma nova subpasta chamada _.template.config_, insira-a. Em seguida, crie um novo arquivo chamado _template.json_. A estrutura de pastas deve ser parecida com esta.

```console
working
└───templates
    └───consoleasync
        └───.template.config
                template.json
```

Abra o _template.jsem_ com seu editor de texto favorito e cole o código JSON a seguir e salve-o.

```json
{
  "$schema": "http://json.schemastore.org/template",
  "author": "Me",
  "classifications": [ "Common", "Console", "C#8" ],
  "identity": "ExampleTemplate.AsyncProject",
  "name": "Example templates: async project",
  "shortName": "consoleasync",
  "tags": {
    "language": "C#",
    "type": "project"
  }
}
```

Esse arquivo de configuração contém todas as configurações do modelo. Você pode ver as configurações básicas, como `name` e `shortName`, mas também há um valor `tags/type` que é definido como `project`. Isso designa seu modelo como um modelo de projeto. Não há restrições quanto ao tipo do modelo criado. Os valores `item` e `project` são nomes comuns que o .NET Core recomenda para que os usuários possam filtrar facilmente o tipo de modelo que eles pesquisam.

O item `classifications` representa a coluna **marcações** que você vê quando executa `dotnet new` e obtém uma lista de modelos. Os usuários também podem pesquisar com base nas marcações de classificação. Não confunda a propriedade `tags` no arquivo json com a lista de marcações `classifications`. São duas coisas diferentes, mas, infelizmente, nomeadas da mesma forma. O esquema completo do arquivo *template.json* é encontrado no [Repositório de Esquema JSON](http://json.schemastore.org/template). Para saber mais sobre o arquivo *template.json*, veja o [wiki de modelagem dotnet](https://github.com/dotnet/templating/wiki).

Agora que você já tem um arquivo _.template.config/template.json_ válido, seu modelo está pronto para ser instalado. Antes de instalar o modelo, exclua todas as pastas e arquivos extras que você não deseja incluir no modelo, como as pastas _bin_ ou _obj_. No terminal, navegue até a pasta _consoleasync_ e execute `dotnet new -i .\` para instalar o modelo localizado na pasta atual. Se você estiver usando um sistema operacional Linux ou macOS, use uma barra invertida: `dotnet new -i ./` .

Esse comando gera a lista de modelos instalados que deve incluir o seu.

```dotnetcli
dotnet new -i .\
```

Você Obtém uma saída semelhante à seguinte.

```console
Usage: new [options]

Options:
  -h, --help          Displays help for this command.
  -l, --list          Lists templates containing the specified name. If no name is specified, lists all templates.

... cut to save space ...

Templates                                         Short Name            Language          Tags
-------------------------------------------------------------------------------------------------------------------------------
Console Application                               console               [C#], F#, VB      Common/Console
Example templates: async project                  consoleasync          [C#]              Common/Console/C#8
Class library                                     classlib              [C#], F#, VB      Common/Library
WPF Application                                   wpf                   [C#], VB          Common/WPF
Windows Forms (WinForms) Application              winforms              [C#], VB          Common/WinForms
Worker Service                                    worker                [C#]              Common/Worker/Web
```

### <a name="test-the-project-template"></a>Testar o modelo do projeto

Agora que você tem um modelo de item instalado, teste-o.

1. Navegue até a pasta de _teste_

1. Crie um novo aplicativo de console com o comando a seguir, que gera um projeto em funcionamento que você pode testar facilmente com o `dotnet run` comando.

    ```dotnetcli
    dotnet new consoleasync
    ```

    Você Obtém a saída a seguir.

    ```console
    The template "Example templates: async project" was created successfully.
    ```

1. Execute o projeto usando o comando a seguir.

    ```dotnetcli
    dotnet run
    ```

    Você Obtém a saída a seguir.

    ```console
    Hello World with C# 8.0!
    ```

Parabéns! Você criou e implantou um modelo de projeto com o .NET Core. Para se preparar para a próxima parte desta série de tutoriais, você deverá desinstalar o modelo criado. Lembre-se de também excluir todos os arquivos da pasta _test_. Isso levará você de volta a um estado limpo, pronto para a próxima seção principal deste tutorial.

### <a name="uninstall-the-template"></a>Desinstalar o modelo

Como você instalou o modelo usando um caminho de arquivo, você deve desinstalá-lo com o caminho de arquivo **absoluto**. Você pode ver uma lista de modelos instalados executando o comando `dotnet new -u`. Seu modelo deve ser listado por último. Use o caminho listado para desinstalar o modelo com o comando `dotnet new -u <ABSOLUTE PATH TO TEMPLATE DIRECTORY>`.

```dotnetcli
dotnet new -u
```

Você Obtém uma saída semelhante à seguinte.

```console
Template Instantiation Commands for .NET Core CLI

Currently installed items:
  Microsoft.DotNet.Common.ItemTemplates
    Templates:
      dotnet gitignore file (gitignore)
      global.json file (globaljson)
      NuGet Config (nugetconfig)
      Solution File (sln)
      Dotnet local tool manifest file (tool-manifest)
      Web Config (webconfig)

... cut to save space ...

  NUnit3.DotNetNew.Template
    Templates:
      NUnit 3 Test Project (nunit) C#
      NUnit 3 Test Item (nunit-test) C#
      NUnit 3 Test Project (nunit) F#
      NUnit 3 Test Item (nunit-test) F#
      NUnit 3 Test Project (nunit) VB
      NUnit 3 Test Item (nunit-test) VB
  C:\working\templates\consoleasync
    Templates:
      Example templates: async project (consoleasync) C#
```

Para desinstalar um modelo, execute o comando a seguir.

```dotnetcli
dotnet new -u C:\working\templates\consoleasync
```

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você criou um modelo de projeto. Para aprender como empacotar os modelos de item e projeto em um arquivo simples, continue a ver esta série de tutoriais.

> [!div class="nextstepaction"]
> [Criar um pacote de modelos](cli-templates-create-template-pack.md)
