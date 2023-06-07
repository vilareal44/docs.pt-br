---
title: Comando dotnet msbuild
description: O comando dotnet msbuild fornece acesso à linha de comando MSBuild.
ms.date: 02/14/2020
ms.openlocfilehash: 9739fe782e17db3955db087ca1781ad4280cd491
ms.sourcegitcommit: 1eae045421d9ea2bfc82aaccfa5b1ff1b8c9e0e4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84803171"
---
# <a name="dotnet-msbuild"></a>dotnet msbuild

**Este artigo aplica-se a:** ✔️ SDK do .NET Core 2. x e versões posteriores

## <a name="name"></a>Nome

`dotnet msbuild` – Compila um projeto e todas as suas dependências. Observação: um arquivo de solução ou de projeto pode precisar ser especificado se houver vários.

## <a name="synopsis"></a>Sinopse

```dotnetcli
dotnet msbuild <MSBUILD_ARGUMENTS>

dotnet msbuild -h
```

## <a name="description"></a>Descrição

O comando `dotnet msbuild` permite o acesso a um MSBuild totalmente funcional.

O comando tem exatamente os mesmos recursos que o cliente de linha de comando do MSBuild existente apenas para projetos no estilo SDK. As opções são todas iguais. Para obter mais informações sobre as opções disponíveis, consulte a [referência de linha de comando do MSBuild](/visualstudio/msbuild/msbuild-command-line-reference).

O comando [dotnet build](dotnet-build.md) é equivalente ao comando `dotnet msbuild -restore`. Quando você não quiser compilar o projeto e tiver um destino específico que deseja executar, use `dotnet build` ou `dotnet msbuild` e especifique o destino.

## <a name="examples"></a>Exemplos

- Compile um projeto e suas dependências:

  ```dotnetcli
  dotnet msbuild
  ```

- Compile um projeto e suas dependências usando a configuração da Versão:

  ```dotnetcli
  dotnet msbuild -property:Configuration=Release
  ```

- Execute o destino de publicação e publique para o RID `osx.10.11-x64`:

  ```dotnetcli
  dotnet msbuild -target:Publish -property:RuntimeIdentifiers=osx.10.11-x64
  ```

- Confira o projeto inteiro com todos os destinos incluídos pelo SDK:

  ```dotnetcli
  dotnet msbuild -preprocess
  dotnet msbuild -preprocess:<fileName>.xml
  ```
