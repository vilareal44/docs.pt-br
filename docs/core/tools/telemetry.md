---
title: Telemetria do SDK do .NET Core
description: Descubra os recursos de telemetria do SDK do .NET Core que coletam informações de uso para análise, quais dados são coletados e como desabilitá-los.
author: KathleenDollard
ms.date: 08/27/2019
ms.openlocfilehash: 0917dae23588ccd1809252aaf484c397e84561c7
ms.sourcegitcommit: 67cf756b033c6173a1bbd1cbd5aef1fccac99e34
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226563"
---
# <a name="net-core-sdk-telemetry"></a>Telemetria do SDK do .NET Core

O [SDK do .NET Core](index.md) inclui um recurso de telemetria que coleta dados de uso e informações de exceção em caso de falha da CLI do .NET Core. A CLI do .NET Core é fornecida com o SDK do .NET Core e consiste no conjunto de verbos que permitem criar, testar e publicar os aplicativos .NET Core. É importante que a equipe do .NET entenda como as ferramentas são usadas para poder melhorá-las. As informações sobre falhas ajudam a equipe a resolver problemas e corrigir bugs.

Os dados coletados são anônimos e publicados em agregação sob a [Licença de Atribuição da Creative Commons](https://creativecommons.org/licenses/by/4.0/).

## <a name="scope"></a>Escopo

`dotnet` tem duas funções: executar aplicativos e executar comandos da CLI. A telemetria *não é coletada* durante o uso de `dotnet` para iniciar um aplicativo no seguinte formato:

- `dotnet [path-to-app].dll`

A telemetria *é coletada* durante o uso de um dos [comandos da CLI do .NET Core](index.md), como:

- `dotnet build`
- `dotnet pack`
- `dotnet run`

## <a name="how-to-opt-out"></a>Como recusar

O recurso de telemetria do SDK do .NET Core é habilitado por padrão. Para recusar o recurso de telemetria, defina a variável de ambiente `DOTNET_CLI_TELEMETRY_OPTOUT` como `1` ou `true`.

Uma única entrada de telemetria também é enviada pelo instalador do SDK do .NET Core quando ocorre uma instalação bem-sucedida. Para recusá-la, defina a variável de ambiente `DOTNET_CLI_TELEMETRY_OPTOUT` antes de instalar o SDK do .NET Core.

## <a name="disclosure"></a>Divulgação

O SDK do .NET Core exibe um texto semelhante ao mostrado a seguir quando você executa um dos [comandos da CLI do .NET Core](index.md) pela primeira vez (por exemplo, `dotnet build`). O texto pode variar um pouco dependendo da versão do SDK que você está executando. Essa experiência de “primeira execução” é como a Microsoft notifica você sobre a coleta de dados.

```console
Telemetry
---------
The .NET Core tools collect usage data in order to help us improve your experience. The data is anonymous. It is collected by Microsoft and shared with the community. You can opt-out of telemetry by setting the DOTNET_CLI_TELEMETRY_OPTOUT environment variable to '1' or 'true' using your favorite shell.

Read more about .NET Core CLI Tools telemetry: https://aka.ms/dotnet-cli-telemetry
```

Para desabilitar essa mensagem e a mensagem de boas-vindas do .NET Core, defina a `DOTNET_NOLOGO` variável de ambiente como `true` . Observe que essa variável não tem nenhum efeito na recusa de telemetria.

## <a name="data-points"></a>Pontos de dados

O recurso de telemetria não coleta dados pessoais, como nomes de usuário ou endereços de email. Ele não examina o código nem extrai dados no nível de projeto, como nome, repositório ou autor. Os dados são enviados com segurança para os servidores Microsoft usando a tecnologia [Azure Monitor](https://azure.microsoft.com/services/monitor/), mantidos em acesso restrito e publicados sob controles de segurança rigorosos por meio dos sistemas seguros do [Armazenamento do Azure](https://azure.microsoft.com/services/storage/).

A proteção de sua privacidade é importante para nós. Se você suspeitar que a telemetria está coletando dados confidenciais ou que os dados estão sendo inseguros ou indevidamente manipulados, execute um problema no repositório [dotnet/SDK](https://github.com/dotnet/sdk/issues) ou envie um email para [dotnet@microsoft.com](mailto:dotnet@microsoft.com) para investigação.

O recurso de telemetria coleta os seguintes dados:

| Versões do SDK | Dados |
|--------------|------|
| Tudo          | Carimbo de data/hora da invocação. |
| Tudo          | Comando invocado (por exemplo, "build"), com hash no 2.1 em diante. |
| Tudo          | Três endereços IP de octeto usados para determinar a localização geográfica. |
| Tudo          | Sistema operacional e versão. |
| Tudo          | RID (ID de runtime) em que o SDK está em execução. |
| Tudo          | Versão do SDK do .NET Core. |
| Tudo          | Perfil de telemetria: um valor opcional usado somente com o consentimento explícito do usuário e usado internamente na Microsoft. |
| >=2.0        | Opções e argumentos de comando: várias opções e vários argumentos são coletados (não cadeias de caracteres arbitrárias). Confira [Opções coletadas](#collected-options). Com hash após 2.1.300. |
| >=2.0         | Se o SDK está em execução em um contêiner. |
| >=2.0         | Estruturas de destino (do evento `TargetFramework`), com hash começando em 2.1. |
| >=2.0         | Endereço MAC (controle de acesso à mídia) com hash: uma ID exclusiva e criptograficamente anônima (SHA256) para um computador. |
| >=2.0         | Diretório de trabalho atual com hash. |
| >=2.0         | Instale o relatório de êxito com o nome de arquivo exe do instalador com hash. |
| >=2.1.300     | Versão do kernel. |
| >=2.1.300     | Versão/liberação da Libc. |
| >=3.0.100     | Indica se a saída foi redirecionada (verdadeiro ou falso). |
| >=3.0.100     | Em uma falha da CLI/do SDK, o tipo de exceção e seu rastreamento de pilha (somente o código da CLI/do SDK é incluído no rastreamento de pilha enviado). Para obter mais informações, confira [Telemetria de exceção de falha da CLI/do SDK do .NET Core coletada](#net-core-clisdk-crash-exception-telemetry-collected). |

### <a name="collected-options"></a>Opções coletadas

Alguns comandos enviam dados adicionais. Um subconjunto de comandos envia o primeiro argumento:

| Comando               | Dados do primeiro argumento enviados                |
|-----------------------|-----------------------------------------|
| `dotnet help <arg>`   | A ajuda do comando está sendo consultada.  |
| `dotnet new <arg>`    | O nome do modelo (com hash).             |
| `dotnet add <arg>`    | A palavra `package` ou `reference`.      |
| `dotnet remove <arg>` | A palavra `package` ou `reference`.      |
| `dotnet list <arg>`   | A palavra `package` ou `reference`.      |
| `dotnet sln <arg>`    | A palavra `add`, `list` ou `remove`.    |
| `dotnet nuget <arg>`  | A palavra `delete`, `locals` ou `push`. |

Um subconjunto de comandos envia as opções selecionadas se elas são usadas, juntamente com seus valores:

| Opção                  | Comandos                                                                                       |
|-------------------------|------------------------------------------------------------------------------------------------|
| `--verbosity`           | Todos os comandos                                                                                   |
| `--language`            | `dotnet new`                                                                                   |
| `--configuration`       | `dotnet build`, `dotnet clean`, `dotnet publish`, `dotnet run`, `dotnet test`                  |
| `--framework`           | `dotnet build`, `dotnet clean`, `dotnet publish`, `dotnet run`, `dotnet test`, `dotnet vstest` |
| `--runtime`             | `dotnet build`,  `dotnet publish`                                                              |
| `--platform`            | `dotnet vstest`                                                                                |
| `--logger`              | `dotnet vstest`                                                                                |
| `--sdk-package-version` | `dotnet migrate`                                                                               |

Exceto para `--verbosity` e `--sdk-package-version`, todos os outros valores são codificados no SDK 2.1.100 do .NET Core em diante.

## <a name="net-core-clisdk-crash-exception-telemetry-collected"></a>Telemetria de exceção de falha do SDK/da CLI do .NET Core coletada

Se a CLI/o SDK do .NET Core falhar, o nome da exceção e o rastreamento de pilha do código da CLI/do SDK serão coletados. Essas informações são coletadas para avaliar problemas e melhorar a qualidade do SDK e da CLI do .NET Core. Este artigo fornece informações sobre os dados que coletamos. Ele também fornece dicas sobre como os usuários que criam sua própria versão do SDK do .NET Core podem evitar a divulgação não intencional de informações pessoais ou confidenciais.

### <a name="types-of-collected-data"></a>Tipos de dados coletados

A CLI do .NET Core coleta informações apenas das exceções da CLI/do SDK, não das exceções no aplicativo. Os dados coletados contêm o nome da exceção e o rastreamento de pilha. Esse rastreamento de pilha é do código da CLI/do SDK.

O seguinte exemplo mostra os tipos de dados coletados:

```console
System.IO.IOException
at System.ConsolePal.WindowsConsoleStream.Write(Byte[] buffer, Int32 offset, Int32 count)
at System.IO.StreamWriter.Flush(Boolean flushStream, Boolean flushEncoder)
at System.IO.StreamWriter.Write(Char[] buffer)
at System.IO.TextWriter.WriteLine()
at System.IO.TextWriter.SyncTextWriter.WriteLine()
at Microsoft.DotNet.Cli.Utils.Reporter.WriteLine()
at Microsoft.DotNet.Tools.Run.RunCommand.EnsureProjectIsBuilt()
at Microsoft.DotNet.Tools.Run.RunCommand.Execute()
at Microsoft.DotNet.Tools.Run.RunCommand.Run(String[] args)
at Microsoft.DotNet.Cli.Program.ProcessArgs(String[] args, ITelemetry telemetryClient)
at Microsoft.DotNet.Cli.Program.Main(String[] args)
```

### <a name="avoid-inadvertent-disclosure-of-information"></a>Evite a divulgação inadvertida de informações

Os colaboradores do .NET Core e qualquer outra pessoa que estejam executando uma versão do SDK do .NET Core criada por conta própria devem considerar o caminho para o código-fonte do SDK. Se ocorrer uma falha durante o uso de um SDK do .NET Core que seja um build de depuração personalizado ou configurado com arquivos de símbolo de build personalizado, o caminho do arquivo de origem do SDK do computador de build será coletado como parte do rastreamento de pilha e não terá hash.

Por isso, os builds personalizados do SDK do .NET Core não devem estar localizados em diretórios cujos nomes de caminho exponham informações pessoais ou confidenciais.

## <a name="see-also"></a>Confira também

- [Telemetria da CLI do .NET Core – dados do T2 de 2019](https://dotnet.microsoft.com/platform/telemetry/dotnet-core-cli-2019q2)
- [Fonte de referência de telemetria (repositório dotnet/SDK)](https://github.com/dotnet/sdk/tree/master/src/Cli/dotnet/Telemetry)
