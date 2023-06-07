---
title: CLI do .NET Core
titleSuffix: ''
description: Uma visão geral do CLI do .NET Core e de seus recursos.
ms.topic: overview
ms.date: 02/13/2020
ms.openlocfilehash: 18dde384058206f437b53572b2f8331d65324482
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88062685"
---
# <a name="net-core-cli-overview"></a>Visão geral da CLI do .NET Core

**Este artigo aplica-se a:** ✔️ SDK do .net Core 2,1 e versões posteriores

A CLI (interface de linha de comando) do .NET Core é uma ferramentas de plataforma cruzada para o desenvolvimento, a criação, a execução e a publicação de aplicativos .NET Core.

O CLI do .NET Core está incluído no [SDK do .NET Core](../sdk.md). Para saber como instalar o SDK do .NET Core, consulte [instalar o .NET Core](../install/windows.md).

## <a name="cli-commands"></a>Comandos de CLI

Os comandos a seguir são instalados por padrão:

### <a name="basic-commands"></a>Comandos básicos

- [`new`](dotnet-new.md)
- [`restore`](dotnet-restore.md)
- [`build`](dotnet-build.md)
- [`publish`](dotnet-publish.md)
- [`run`](dotnet-run.md)
- [`test`](dotnet-test.md)
- [`vstest`](dotnet-vstest.md)
- [`pack`](dotnet-pack.md)
- [`migrate`](dotnet-migrate.md)
- [`clean`](dotnet-clean.md)
- [`sln`](dotnet-sln.md)
- [`help`](dotnet-help.md)
- [`store`](dotnet-store.md)

### <a name="project-modification-commands"></a>Comandos de modificação de projeto

- [`add package`](dotnet-add-package.md)
- [`add reference`](dotnet-add-reference.md)
- [`remove package`](dotnet-remove-package.md)
- [`remove reference`](dotnet-remove-reference.md)
- [`list reference`](dotnet-list-reference.md)

### <a name="advanced-commands"></a>Comandos avançados

- [`nuget delete`](dotnet-nuget-delete.md)
- [`nuget locals`](dotnet-nuget-locals.md)
- [`nuget push`](dotnet-nuget-push.md)
- [`msbuild`](dotnet-msbuild.md)
- [`dotnet install script`](dotnet-install-script.md)

### <a name="tool-management-commands"></a>Comandos de gerenciamento de ferramentas

- [`tool install`](dotnet-tool-install.md)
- [`tool list`](dotnet-tool-list.md)
- [`tool update`](dotnet-tool-update.md)
- [`tool restore`](global-tools.md#install-a-local-tool)Disponível desde SDK do .NET Core 3,0.
- [`tool run`](global-tools.md#invoke-a-local-tool)Disponível desde SDK do .NET Core 3,0.
- [`tool uninstall`](dotnet-tool-uninstall.md)

Ferramentas são aplicativos de console que são instalados a partir de pacotes NuGet e são invocados no prompt de comando. Você pode escrever ferramentas por conta própria ou instalar ferramentas escritas por terceiros. As ferramentas também são conhecidas como ferramentas globais, ferramentas de caminho de ferramenta e ferramentas locais. Para obter mais informações, consulte [visão geral das ferramentas do .NET Core](global-tools.md).

## <a name="command-structure"></a>Estrutura de comando

A estrutura de comando CLI consiste no [driver ("dotnet")](#driver), [do comando](#command) e possivelmente de [opções](#options) e [argumentos](#arguments) de comando. É possível ver esse padrão na maioria das operações de CLI, como ao criar um novo aplicativo de console e executá-lo a partir da linha de comando, como mostram os comandos a seguir quando executados a partir de um diretório chamado *my_app*:

```dotnetcli
dotnet new console
dotnet build --output /build_output
dotnet /build_output/my_app.dll
```

### <a name="driver"></a>Driver

O driver é chamado [dotnet](dotnet.md) e tem duas responsabilidades, executar um [aplicativo dependente da estrutura](../deploying/index.md) ou executar um comando.

Para executar um aplicativo dependente da estrutura, especifique o aplicativo após o driver, por exemplo, `dotnet /path/to/my_app.dll`. Ao executar o comando na pasta onde está a DLL do aplicativo, basta executar `dotnet my_app.dll`. Se você quiser usar uma versão específica do .NET Core Runtime, use a opção `--fx-version <VERSION>` (consulte a referência [do comando dotnet](dotnet.md)).

Quando você fornece um comando para o driver, `dotnet.exe` inicia o processo de execução do comando da CLI. Por exemplo:

```dotnetcli
dotnet build
```

Primeiro, o driver determina a versão do SDK a ser usada. Se não houver nenhum [global.jsno](global-json.md) arquivo, a versão mais recente do SDK disponível será usada. Isso pode ser uma versão prévia ou estável, dependendo do que há de mais recente no computador.  Depois que a versão do SDK é determinada, ela executa o comando.

### <a name="command"></a>Comando

O comando executa uma ação. Por exemplo, `dotnet build` compila código. `dotnet publish` publica o código. Os comandos são implementados como um aplicativo de console usando uma convenção `dotnet {command}`.

### <a name="arguments"></a>Argumentos

Os argumentos que você passa na linha de comando são aqueles do comando invocado. Por exemplo, quando você executa `dotnet publish my_app.csproj` o, o `my_app.csproj` argumento indica o projeto a ser publicado e é passado para o `publish` comando.

### <a name="options"></a>Opções

As opções que você passa na linha de comando são aquelas do comando invocado. Por exemplo, quando você executa `dotnet publish --output /build_output` o, a `--output` opção e seu valor são passados para o `publish` comando.

## <a name="see-also"></a>Consulte também

- [repositório do GitHub do dotnet/SDK](https://github.com/dotnet/sdk/)
- [Guia de instalação do .NET Core](../install/windows.md)
