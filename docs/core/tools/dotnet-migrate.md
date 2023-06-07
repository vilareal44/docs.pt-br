---
title: Comando dotnet migrate
description: O comando dotnet migrate migra um projeto e todas as suas dependências.
ms.date: 02/14/2020
ms.openlocfilehash: 2e7f9ae5a1d11c54280d914b04df761f0d5aff99
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84284086"
---
# <a name="dotnet-migrate"></a>dotnet migrate

**Este artigo aplica-se a:** ✔️ SDK do .NET Core 2. x

## <a name="name"></a>Nome

`dotnet migrate` – Migrar um projeto do .NET Core Versão Prévia 2 para um projeto no estilo do SDK do .NET Core.

## <a name="synopsis"></a>Sinopse

```dotnetcli
dotnet migrate [<SOLUTION_FILE|PROJECT_DIR>] [--format-report-file-json <REPORT_FILE>]
    [-r|--report-file <REPORT_FILE>] [-s|--skip-project-references [Debug|Release]]
    [--skip-backup] [-t|--template-file <TEMPLATE_FILE>] [-v|--sdk-package-version]
    [-x|--xproj-file]

dotnet migrate -h|--help
```

## <a name="description"></a>Descrição

Este comando foi preterido. O `dotnet migrate` comando não está mais disponível a partir do SDK do .NET Core 3,0. Ele só pode migrar um projeto do .NET Core da versão prévia 2 para um projeto do .NET Core 1. x, que está sem suporte.

Por padrão, o comando migra o projeto raiz e as referências de projeto que o projeto raiz contém. Esse comportamento é desabilitado usando a `--skip-project-references` opção em tempo de execução.

A migração pode ser realizada nos seguintes ativos:

* Um único projeto, especificando o arquivo *project.json* a ser migrado.
* Todos os diretórios especificados no arquivo *global.json*, passando um caminho para o arquivo *global.json*.
* O arquivo *solution.sln*, onde ele migra os projetos referenciados na solução.
* Em todos os subdiretórios do diretório especificado, recursivamente.

O comando `dotnet migrate` mantém o arquivo *project.json* migrado em um diretório `backup`, que será criado se ainda não existir. Esse comportamento é substituído com a opção `--skip-backup`.

Por padrão, a operação de migração produzirá o estado do processo de migração para a saída padrão (STDOUT). Se você usar a opção `--report-file <REPORT_FILE>`, a saída será salva no arquivo especificado.

O comando `dotnet migrate` dá suporte apenas a projetos com base em *project.json* da Visualização 2. Isso significa que você não pode usá-lo para migrar projetos com base em *project.json* de DNX ou Visualização 1 diretamente para projetos de MSBuild/csproj. Primeiro, você precisa migrar manualmente o projeto para um projeto com base em *project.json* de Visualização 2 e, em seguida, usar o comando `dotnet migrate` para migrar o projeto.

## <a name="arguments"></a>Argumentos

`PROJECT_JSON/GLOBAL_JSON/SOLUTION_FILE/PROJECT_DIR`

O caminho até um dos seguintes:

* um arquivo *project.json* a ser migrado.
* um arquivo *global.json*: as pastas especificadas em *global.json* são migradas.
* um arquivo *solution.sln*: os projetos referenciados na solução são migrados.
* um diretório a ser migrado: pesquisa recursivamente arquivos *project.json* para fazer a migração dentro do diretório especificado.

Se nada for especificado, o padrão será o diretório atual.

## <a name="options"></a>Opções

`--format-report-file-json <REPORT_FILE>`

Arquivo de relatório de migração de saída como JSON em vez de mensagens de usuário.

`-h|--help`

Imprime uma ajuda breve para o comando.

`-r|--report-file <REPORT_FILE>`

Relatório de migração de saída em um arquivo além do console.

`-s|--skip-project-references [Debug|Release]`

Ignorar referências de projeto migradas. Por padrão, as referências de projeto são migradas recursivamente.

`--skip-backup`

Ignorar a movimentação de *project.json*, *global.json* e *\*.xproj* para um diretório `backup` após a migração bem-sucedida.

`-t|--template-file <TEMPLATE_FILE>`

Arquivo csproj de modelo a ser usado para migração. Por padrão, o mesmo modelo removido por `dotnet new console` será usado.

`-v|--sdk-package-version <VERSION>`

A versão do pacote do sdk referenciada no aplicativo migrado. O padrão é a versão do SDK no `dotnet new`.

`-x|--xproj-file <FILE>`

O caminho para o arquivo xproj a ser usado. Necessário quando há mais de um xproj em um diretório do projeto.

## <a name="examples"></a>Exemplos

Migre um projeto no diretório atual e todas as dependências projeto a projeto:

`dotnet migrate`

Migre todos os projetos incluídos pelo arquivo *global.json*:

`dotnet migrate path/to/global.json`

Migre apenas o projeto atual e não as dependências P2P (projeto a projeto). Além disso, use uma versão específica do SDK:

`dotnet migrate -s -v 1.0.0-preview4`
