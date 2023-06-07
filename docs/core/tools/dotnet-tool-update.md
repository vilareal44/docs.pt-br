---
title: Comando dotnet tool update
description: O comando dotnet ferramenta de atualização atualiza a ferramenta .NET Core especificada em seu computador.
ms.date: 07/08/2020
ms.openlocfilehash: a212fbb40af68019c1bc9a63963d960292be6b08
ms.sourcegitcommit: 0fa2b7b658bf137e813a7f4d09589d64c148ebf5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/14/2020
ms.locfileid: "86308865"
---
# <a name="dotnet-tool-update"></a>dotnet tool update

**Este artigo aplica-se a:** ✔️ SDK do .net Core 2,1 e versões posteriores

## <a name="name"></a>Nome

`dotnet tool update`-Atualiza a [ferramenta .NET Core](global-tools.md) especificada em seu computador.

## <a name="synopsis"></a>Sinopse

```dotnetcli
dotnet tool update <PACKAGE_ID> -g|--global
    [--add-source <SOURCE>] [--configfile <FILE>]
    [--disable-parallel] [--framework <FRAMEWORK>]
    [--ignore-failed-sources] [--interactive] [--no-cache]
    [-v|--verbosity <LEVEL>] [--version <VERSION>]

dotnet tool update <PACKAGE_ID> --tool-path <PATH>
    [--add-source <SOURCE>] [--configfile <FILE>]
    [--disable-parallel] [--framework <FRAMEWORK>]
    [--ignore-failed-sources] [--interactive] [--no-cache]
    [-v|--verbosity <LEVEL>] [--version <VERSION>]

dotnet tool update <PACKAGE_ID> --local
    [--add-source <SOURCE>] [--configfile <FILE>]
    [--disable-parallel] [--framework <FRAMEWORK>]
    [--ignore-failed-sources] [--interactive] [--no-cache]
    [--tool-manifest <PATH>]
    [-v|--verbosity <LEVEL>] [--version <VERSION>]

dotnet tool update -h|--help
```

## <a name="description"></a>Descrição

O `dotnet tool update` comando fornece uma maneira de atualizar as ferramentas do .NET Core em seu computador para a versão estável mais recente do pacote. O comando desinstala e reinstala uma ferramenta, atualizando-a efetivamente. Para usar o comando, especifique uma das seguintes opções:

* Para atualizar uma ferramenta global que foi instalada no local padrão, use a `--global` opção
* Para atualizar uma ferramenta global que foi instalada em um local personalizado, use a `--tool-path` opção.
* Para atualizar uma ferramenta local, use a `--local` opção.

**As ferramentas locais estão disponíveis a partir do SDK do .NET Core 3,0.**

## <a name="arguments"></a>Argumentos

- **`PACKAGE_ID`**

  Nome/ID do pacote NuGet que contém a ferramenta global .NET Core a ser atualizada. Encontre o nome do pacote usando o comando [dotnet tool list](dotnet-tool-list.md).

## <a name="options"></a>Opções

- **`--add-source <SOURCE>`**

  Adiciona outra origem do pacote NuGet a ser usada durante a instalação.

- **`--configfile <FILE>`**

  O arquivo de configuração do NuGet (*nuget.config*) a ser usado.

- **`--disable-parallel`**

  Impedir a restauração de vários projetos em paralelo.

- **`--framework <FRAMEWORK>`**

  Especifica a [estrutura de destino](../../standard/frameworks.md) para a qual atualizar a ferramenta.

- **`--ignore-failed-sources`**

  Tratar falhas de origem do pacote como avisos.

- **`--interactive`**

  Permite que o comando pare e aguarde a entrada ou uma ação do usuário (por exemplo, para concluir a autenticação).

- **`--local`**

  Atualize a ferramenta e o manifesto da ferramenta local. Não pode ser combinado com a `--global` opção ou a `--tool-path` opção.

- **`--no-cache`**

  Não armazene em cache pacotes e solicitações HTTP.

- **`--tool-manifest <PATH>`**

  Caminho para o arquivo de manifesto.

- **`--tool-path <PATH>`**

  Especifica o local onde a ferramenta global está instalada. PATH pode ser absoluto ou relativo. Não pode ser combinada com a opção `--global`. Omitir ambos `--global` e `--tool-path` especifica que a ferramenta a ser atualizada é uma ferramenta local.

- **`--version <VERSION>`**

  O intervalo de versão do pacote de ferramentas para o qual atualizar. Isso não pode ser usado para fazer downgrade de versões. primeiro, você deve obter `uninstall` versões mais recentes.

- **`-g|--global`**

  Especifica que a atualização destina-se a uma ferramenta de todos os usuários. Não pode ser combinada com a opção `--tool-path`. Omitir ambos `--global` e `--tool-path` especifica que a ferramenta a ser atualizada é uma ferramenta local.

- **`-h|--help`**

  Imprime uma ajuda breve para o comando.

- **`-v|--verbosity <LEVEL>`**

  Define o nível de detalhes do comando. Os valores permitidos são `q[uiet]`, `m[inimal]`, `n[ormal]`, `d[etailed]` e `diag[nostic]`.

## <a name="examples"></a>Exemplos

- **`dotnet tool update -g dotnetsay`**

  Atualiza a ferramenta global [dotnetsay](https://www.nuget.org/packages/dotnetsay/) .

- **`dotnet tool update dotnetsay --tool-path c:\global-tools`**

  Atualiza a ferramenta global [dotnetsay](https://www.nuget.org/packages/dotnetsay/) localizada em um diretório específico do Windows.

- **`dotnet tool update dotnetsay --tool-path ~/bin`**

  Atualiza a ferramenta global [dotnetsay](https://www.nuget.org/packages/dotnetsay/) localizada em um diretório específico do linux/MacOS.

- **`dotnet tool update dotnetsay`**

  Atualiza a ferramenta local [dotnetsay](https://www.nuget.org/packages/dotnetsay/) instalada para o diretório atual.

- **`dotnet tool update -g dotnetsay --version 2.0.*`**

  Atualiza a ferramenta global [dotnetsay](https://www.nuget.org/packages/dotnetsay/) para a versão mais recente do patch, com uma versão principal do `2` e uma versão secundária do `0` .

- **`dotnet tool update -g dotnetsay --version (2.0.*,2.1.4)`**

  Atualiza a ferramenta global [dotnetsay](https://www.nuget.org/packages/dotnetsay/) para a versão mais baixa dentro do intervalo especificado `(> 2.0.0 && < 2.1.4)` , a versão `2.1.0` deve ser instalada. Para obter mais informações sobre intervalos de controle de versão semântico, consulte [intervalos de versão de empacotamento NuGet](/nuget/concepts/package-versioning#version-ranges).

## <a name="see-also"></a>Confira também

- [Ferramentas do .NET Core](global-tools.md)
- [Controle de versão semântico](https://semver.org)
- [Tutorial: instalar e usar uma ferramenta global do .NET Core usando o CLI do .NET Core](global-tools-how-to-use.md)
- [Tutorial: instalar e usar uma ferramenta local do .NET Core usando o CLI do .NET Core](local-tools-how-to-use.md)
