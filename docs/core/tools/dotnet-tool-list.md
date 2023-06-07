---
title: Comando dotnet tool list
description: O comando dotnet da lista de ferramentas lista as ferramentas do .NET Core que estão instaladas em seu computador.
ms.date: 02/14/2020
ms.openlocfilehash: 4035c5be233232e53c6d7150485f737108c1e18d
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86925456"
---
# <a name="dotnet-tool-list"></a>dotnet tool list

**Este artigo aplica-se a:** ✔️ SDK do .net Core 2,1 e versões posteriores

## <a name="name"></a>Nome

`dotnet tool list`-Lista todas as [Ferramentas do .NET Core](global-tools.md) do tipo especificado atualmente instalado no seu computador.

## <a name="synopsis"></a>Sinopse

```dotnetcli
dotnet tool list -g|--global

dotnet tool list --tool-path <PATH>

dotnet tool list --local

dotnet tool list

dotnet tool list -h|--help
```

## <a name="description"></a>Descrição

O `dotnet tool list` comando fornece uma maneira de listar todas as ferramentas globais, de caminho de ferramenta ou locais do .NET Core instaladas em seu computador. O comando lista o nome do pacote, a versão instalada e o comando da ferramenta.  Para usar o comando, especifique um dos seguintes:

* Para listar as ferramentas globais instaladas no local padrão, use a `--global` opção
* Para listar as ferramentas globais instaladas em um local personalizado, use a `--tool-path` opção.
* Para listar as ferramentas locais, use a `--local` opção ou omita as `--global` `--tool-path` Opções, e `--local` .

**As ferramentas locais estão disponíveis a partir do SDK do .NET Core 3,0.**

## <a name="options"></a>Opções

- **`-g|--global`**

  Lista as ferramentas globais em todo o usuário. Não pode ser combinada com a opção `--tool-path`. Omitir `--global` e `--tool-path` listar as ferramentas locais.

- **`-h|--help`**

  Imprime uma ajuda breve para o comando.

- **`--local`**

  Lista as ferramentas locais para o diretório atual. Não pode ser combinado com `--global` as `--tool-path` Opções ou. Omitir `--global` e `--tool-path` listar as ferramentas locais, mesmo se `--local` não for especificado.

- **`--tool-path <PATH>`**

  Especifica um local personalizado onde encontrar ferramentas globais. PATH pode ser absoluto ou relativo. Não pode ser combinada com a opção `--global`. Omitir `--global` e `--tool-path` listar as ferramentas locais.

## <a name="examples"></a>Exemplos

- **`dotnet tool list -g`**

  Lista todas as ferramentas globais instaladas por todo o usuário em seu computador (perfil de usuário atual).

- **`dotnet tool list --tool-path c:\global-tools`**

  Lista as ferramentas globais de um diretório específico do Windows.

- **`dotnet tool list --tool-path ~/bin`**

  Lista as ferramentas globais de um diretório específico do Linux/macOS.

- **`dotnet tool list`** or**`dotnet tool list --local`**

  Lista todas as ferramentas locais disponíveis no diretório atual.

## <a name="see-also"></a>Veja também

- [Ferramentas do .NET Core](global-tools.md)
- [Tutorial: instalar e usar uma ferramenta global do .NET Core usando o CLI do .NET Core](global-tools-how-to-use.md)
- [Tutorial: instalar e usar uma ferramenta local do .NET Core usando o CLI do .NET Core](local-tools-how-to-use.md)
