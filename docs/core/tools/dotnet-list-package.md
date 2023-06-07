---
title: Comando dotnet list package
description: O comando 'dotnet list package' fornece uma opção conveniente para listar as referências de pacote de um projeto ou solução.
ms.date: 02/14/2020
ms.openlocfilehash: 7157e56860936d10aa322854a589ae89e2bc0826
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87164759"
---
# <a name="dotnet-list-package"></a>dotnet list package

**Este artigo aplica-se a:** ✔️ SDK do .net Core 2,2 e versões posteriores

## <a name="name"></a>Nome

`dotnet list package` – Lista as referências de pacote para um projeto ou solução.

## <a name="synopsis"></a>Sinopse

```dotnetcli
dotnet list [<PROJECT>|<SOLUTION>] package [--config <SOURCE>]
    [--deprecated]
    [--framework <FRAMEWORK>] [--highest-minor] [--highest-patch]
    [--include-prerelease] [--include-transitive] [--interactive]
    [--outdated] [--source <SOURCE>]

dotnet list package -h|--help
```

## <a name="description"></a>DESCRIÇÃO

O comando `dotnet list package` fornece uma opção conveniente para listar todas as referências de pacotes do NuGet para um projeto específico ou uma solução. Primeiro, você precisa criar o projeto para ter os recursos necessários para que esse comando seja processado. O exemplo a seguir mostra a saída do comando `dotnet list package` para o projeto [SentimentAnalysis](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/SentimentAnalysis):

```output
Project 'SentimentAnalysis' has the following package references
   [netcoreapp2.1]:
   Top-level Package               Requested   Resolved
   > Microsoft.ML                  1.4.0       1.4.0
   > Microsoft.NETCore.App   (A)   [2.1.0, )   2.1.0

(A) : Auto-referenced package.
```

A coluna **Requested** refere-se à versão do pacote especificada no arquivo de projeto e pode ser um intervalo. A coluna **Resolved** lista a versão que o projeto está usando atualmente e é sempre um valor único. Os pacotes que exibem um `(A)` ao lado de seus nomes representam [referências de pacotes implícitas](csproj.md#implicit-package-references) que são inferidas das configurações do seu projeto (tipo `Sdk`, propriedade `<TargetFramework>` ou `<TargetFrameworks>`, etc.)

Use a opção `--outdated` para descobrir se existem versões mais recentes dos pacotes que você está usando em seus projetos. Por padrão, `--outdated` lista os pacotes estáveis mais recentes, a menos que a versão resolvida também seja uma versão de pré-lançamento. Para incluir versões de pré-lançamento ao listar versões mais recentes, especifique também a opção `--include-prerelease`. Os exemplos a seguir mostram a saída do comando `dotnet list package --outdated --include-prerelease` para o mesmo projeto do exemplo anterior:

```output
The following sources were used:
   https://api.nuget.org/v3/index.json
   C:\Program Files (x86)\Microsoft SDKs\NuGetPackages\

Project `SentimentAnalysis` has the following updates to its packages
   [netcoreapp2.1]:
   Top-level Package      Requested   Resolved   Latest
   > Microsoft.ML         1.4.0       1.4.0      1.5.0-preview
```

Se você precisa descobrir se seu projeto tem dependências transitivas, use a opção `--include-transitive`. Dependências transitivas ocorrem quando você adiciona um pacote ao seu projeto que, por sua vez, depende de outro pacote. O exemplo a seguir mostra a saída da execução do comando `dotnet list package --include-transitive` para o projeto [HelloPlugin](https://github.com/dotnet/samples/tree/master/core/extensions/AppWithPlugin/HelloPlugin), que exibe os pacotes de nível superior e os pacotes dos quais eles dependem:

```output
Project 'HelloPlugin' has the following package references
   [netcoreapp3.0]:
   Transitive Package      Resolved
   > PluginBase            1.0.0
```

## <a name="arguments"></a>Argumentos

`PROJECT | SOLUTION`

O arquivo de projeto ou solução para operar. Se não for especificado, o comando pesquisará um no diretório atual. Se mais de uma solução ou projeto for encontrado, um erro será lançado.

## <a name="options"></a>Opções

- **`--config <SOURCE>`**

  A fontes do NuGet a serem usadas ao procurar pacotes mais novos. Requer a opção `--outdated`.

- **`--deprecated`**

  Exibe os pacotes que foram preteridos.

- **`--framework <FRAMEWORK>`**

  Exibe apenas os pacotes aplicáveis para a [estrutura de destino](../../standard/frameworks.md) especificada. Para especificar várias estruturas, repita a opção várias vezes. Por exemplo: `--framework netcoreapp2.2 --framework netstandard2.0`.

- **`-h|--help`**

  Imprime uma ajuda breve para o comando.

- **`--highest-minor`**

  Ao procurar pacotes mais novos, considerar apenas os pacotes com um número de versão principal correspondente. Requer a `--outdated` `--deprecated` opção ou.

- **`--highest-patch`**

  Ao procurar pacotes mais novos, considerar apenas os pacotes com números de versão principais e secundários correspondentes. Requer a `--outdated` `--deprecated` opção ou.

- **`--include-prerelease`**

  Ao procurar pacotes mais novos, considere pacotes com versões de pré-lançamento. Requer a `--outdated` `--deprecated` opção ou.

- **`--include-transitive`**

  Lista pacotes transitivos, além dos pacotes de nível superior. Ao especificar essa opção, você obtém uma lista de pacotes dos quais os pacotes de nível superior dependem.

- **`--interactive`**

  Permite que o comando pare e aguarde entrada ou ação do usuário. Por exemplo, para concluir a autenticação. Disponível desde o SDK do .NET Core 3.0.

- **`--outdated`**

  Lista os pacotes que têm as versões mais recentes disponíveis.

- **`-s|--source <SOURCE>`**

  A fontes do NuGet a serem usadas ao procurar pacotes mais novos. Requer a `--outdated` `--deprecated` opção ou.

## <a name="examples"></a>Exemplos

- Listar referências de pacote de um projeto específico:

  ```dotnetcli
  dotnet list SentimentAnalysis.csproj package
  ```

- Listar referências de pacote com versões mais recentes disponíveis, incluindo versões de pré-lançamento:

  ```dotnetcli
  dotnet list package --outdated --include-prerelease
  ```

- Listar referências de pacotes para uma estrutura de destino específica:

  ```dotnetcli
  dotnet list package --framework netcoreapp3.0
  ```
