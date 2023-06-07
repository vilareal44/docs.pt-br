---
title: Criar um modelo de item para o comando dotnet new - CLI do .NET Core
description: Saiba como criar um modelo de item para o comando dotnet new. Os modelos de item podem conter qualquer quantidade de arquivos.
author: adegeo
ms.date: 06/25/2019
ms.topic: tutorial
ms.author: adegeo
ms.openlocfilehash: 0b804d26b2f33d4d600c17de2f7f71101a0f9c98
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85324373"
---
# <a name="tutorial-create-an-item-template"></a>Tutorial: criar um modelo de item

Com o .NET Core, você pode criar e implantar modelos que geram projetos, arquivos e até recursos. Este tutorial é a parte um de uma série que ensina como criar, instalar e desinstalar modelos para usar com o comando `dotnet new`.

Nesta parte da série, você aprenderá a:

> [!div class="checklist"]
>
> * Criar uma classe para um modelo de item
> * Criar a pasta e o arquivo de configuração do modelo
> * Instalar um modelo a partir de um caminho de arquivos
> * Testar um modelo de item
> * Desinstalar um modelo de item

## <a name="prerequisites"></a>Pré-requisitos

* [SDK do .NET Core 2.2](https://dotnet.microsoft.com/download) ou versões posteriores.
* Leia o artigo de referência [Modelos personalizados para dotnet new](../tools/custom-templates.md).

  O artigo de referência explica os conceitos básicos sobre modelos e como eles são agrupados. Algumas dessas informações serão reiteradas aqui.

* Abra um terminal e navegue até a pasta _working\templates_.

## <a name="create-the-required-folders"></a>Criar as pastas obrigatórias

Esta série usa uma "pasta de trabalho" na qual sua fonte de modelo está contida e uma "pasta de teste" usada para testar seus modelos. A pasta de trabalho e a pasta de teste devem estar na mesma pasta pai.

Primeiro, crie a pasta pai. Use o nome que desejar para a pasta. Em seguida, crie uma subpasta chamada _working_. Na pasta _working_, crie uma subpasta chamada _templates_.

Em seguida, crie uma pasta na pasta pai chamada _test_. A estrutura de pastas deve ser parecida com a seguinte.

```console
parent_folder
├───test
└───working
    └───templates
```

## <a name="create-an-item-template"></a>Criar um modelo de item

Um modelo de item é um tipo específico de modelo que contém um ou mais arquivos. Esses tipos de modelos são úteis quando você deseja gerar algo como um arquivo de configuração, código ou solução. Neste exemplo, você criará uma classe que adiciona um método de extensão ao tipo de cadeia de caracteres.

No terminal, navegue até a pasta _working\templates_ e crie uma nova subpasta chamada _extensions_. Insira a pasta.

```console
working
└───templates
    └───extensions
```

Crie um novo arquivo chamado _CommonExtensions.cs_ e abra-o com seu editor de texto favorito. Essa classe fornecerá um método de extensão chamado `Reverse` que reverterá o conteúdo de uma cadeia de caracteres. Cole no seguinte código e salve o arquivo:

```csharp
using System;

namespace System
{
    public static class StringExtensions
    {
        public static string Reverse(this string value)
        {
            var tempArray = value.ToCharArray();
            Array.Reverse(tempArray);
            return new string(tempArray);
        }
    }
}
```

Agora que você tem o conteúdo do modelo criado, é necessário criar a configuração do modelo na pasta raiz do modelo.

## <a name="create-the-template-config"></a>Criar a configuração do modelo

Os modelos são reconhecidos no .NET Core por uma pasta especial e um arquivo de configuração que está na raiz do modelo. Neste tutorial, a pasta de modelos está localizada em _working\templates\extensions_.

Quando você cria um modelo, todos os arquivos e pastas na pasta de modelos são incluídos como parte do modelo, exceto a pasta de configuração especial. Esta pasta de configuração chama-se _.template.config_.

Primeiro, crie uma nova subpasta chamada _.template.config_, insira-a. Em seguida, crie um novo arquivo chamado _template.json_. A estrutura de pastas devem ter a seguinte aparência:

```console
working
└───templates
    └───extensions
        └───.template.config
                template.json
```

Abra o _template.jsem_ com seu editor de texto favorito e cole o código JSON a seguir e salve-o.

```json
{
  "$schema": "http://json.schemastore.org/template",
  "author": "Me",
  "classifications": [ "Common", "Code" ],
  "identity": "ExampleTemplate.StringExtensions",
  "name": "Example templates: string extensions",
  "shortName": "stringext",
  "tags": {
    "language": "C#",
    "type": "item"
  }
}
```

Esse arquivo de configuração contém todas as configurações do modelo. Você pode ver as configurações básicas, como `name` e `shortName`, mas também há um valor `tags/type` que está definido como `item`. Isso categoriza seu modelo como um modelo de item. Não há restrições quanto ao tipo do modelo criado. Os valores `item` e `project` são nomes comuns que o .NET Core recomenda para que os usuários possam filtrar facilmente o tipo de modelo que eles pesquisam.

O item `classifications` representa a coluna **marcações** que você vê quando executa `dotnet new` e obtém uma lista de modelos. Os usuários também podem pesquisar com base nas marcações de classificação. Não confunda a propriedade `tags` no arquivo \*.json com a lista de marcações `classifications`. São duas coisas diferentes, mas, infelizmente, nomeadas da mesma forma. O esquema completo do arquivo *template.json* é encontrado no [Repositório de Esquema JSON](http://json.schemastore.org/template). Para saber mais sobre o arquivo *template.json*, veja o [wiki de modelagem dotnet](https://github.com/dotnet/templating/wiki).

Agora que você já tem um arquivo _.template.config/template.json_ válido, seu modelo está pronto para ser instalado. No terminal, navegue até a pasta _extensions_ e execute o seguinte comando para instalar o modelo localizado na pasta atual:

* **No Windows**:`dotnet new -i .\`
* **No Linux ou MacOS**:`dotnet new -i ./`

Esse comando gera a lista de modelos instalados que deve incluir o seu.

```console
C:\working\templates\extensions> dotnet new -i .\
Usage: new [options]

Options:
  -h, --help          Displays help for this command.
  -l, --list          Lists templates containing the specified name. If no name is specified, lists all templates.

... cut to save space ...

Templates                                         Short Name            Language          Tags
-------------------------------------------------------------------------------------------------------------------------------
Example templates: string extensions              stringext             [C#]              Common/Code
Console Application                               console               [C#], F#, VB      Common/Console
Class library                                     classlib              [C#], F#, VB      Common/Library
WPF Application                                   wpf                   [C#], VB          Common/WPF
Windows Forms (WinForms) Application              winforms              [C#], VB          Common/WinForms
Worker Service                                    worker                [C#]              Common/Worker/Web
```

## <a name="test-the-item-template"></a>Testar o modelo de item

Agora que você tem um modelo de item instalado, teste-o. Navegue até a pasta _test/_ e crie um novo aplicativo de console com `dotnet new console`. Isso gera um projeto funcional que você pode testar facilmente com o comando `dotnet run`.

```dotnetcli
dotnet new console
```

Você Obtém uma saída semelhante à seguinte.

```console
The template "Console Application" was created successfully.

Processing post-creation actions...
Running 'dotnet restore' on C:\test\test.csproj...
  Restore completed in 54.82 ms for C:\test\test.csproj.

Restore succeeded.
```

Execute o projeto com.

```dotnetcli
dotnet run
```

Você Obtém a saída a seguir.

```console
Hello World!
```

Em seguida, execute `dotnet new stringext` para gerar o _CommonExtensions.cs_ a partir do modelo.

```dotnetcli
dotnet new stringext
```

Você Obtém a saída a seguir.

```console
The template "Example templates: string extensions" was created successfully.
```

Altere o código em _Program.cs_ para inverter a cadeia de caracteres `"Hello World"` com o método de extensão fornecido pelo modelo.

```csharp
Console.WriteLine("Hello World!".Reverse());
```

Execute o programa novamente e você verá que o resultado foi invertido.

```dotnetcli
dotnet run
```

Você Obtém a saída a seguir.

```console
!dlroW olleH
```

Parabéns! Você criou e implantou um modelo de item com o .NET Core. Para se preparar para a próxima parte desta série de tutoriais, você deverá desinstalar o modelo criado. Lembre-se de também excluir todos os arquivos da pasta _test_. Isso levará você de volta a um estado limpo, pronto para a próxima seção principal deste tutorial.

## <a name="uninstall-the-template"></a>Desinstalar o modelo

Como você instalou o modelo com o caminho de arquivo, você deve desinstalá-lo com o caminho de arquivo **absoluto**. Você pode ver uma lista de modelos instalados executando o comando `dotnet new -u`. Seu modelo deve ser listado por último. Use o caminho listado para desinstalar o modelo com o comando `dotnet new -u <ABSOLUTE PATH TO TEMPLATE DIRECTORY>`.

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
  C:\working\templates\extensions
    Templates:
      Example templates: string extensions (stringext) C#
```

Para desinstalar um modelo, execute o comando a seguir.

```dotnetcli
dotnet new -u C:\working\templates\extensions
```

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você criou um modelo de item. Para saber como criar um modelo de projeto, continue a ver essa série de tutoriais.

> [!div class="nextstepaction"]
> [Criar um modelo de projeto](cli-templates-create-project-template.md)
