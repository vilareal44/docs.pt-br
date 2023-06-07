---
title: Comando dotnet list reference
description: O comando dotnet list reference fornece uma opção conveniente para listar referências projeto a projeto.
ms.date: 02/14/2020
ms.openlocfilehash: b9b34c17102c6218f3d99f6e2620e99f70140284
ms.sourcegitcommit: d223616e7e6fe2139079052e6fcbe25413fb9900
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83802759"
---
# <a name="dotnet-list-reference"></a>dotnet list reference

**Este artigo aplica-se a:** ✔️ SDK do .NET Core 2. x e versões posteriores

## <a name="name"></a>Nome

`dotnet list reference` – lista as referências projeto a projeto.

## <a name="synopsis"></a>Sinopse

```dotnetcli
dotnet list [<PROJECT>] reference

dotnet list -h|--help
```

## <a name="description"></a>Descrição

O comando `dotnet list reference` fornece uma opção conveniente para listar as referências de projeto de determinado projeto.

## <a name="arguments"></a>Argumentos

* **`PROJECT`**

  O arquivo de projeto no qual operar. Se um arquivo não for especificado, o comando pesquisará o diretório atual em busca de um.

## <a name="options"></a>Opções

* **`-h|--help`**

  Imprime uma ajuda breve para o comando.

## <a name="examples"></a>Exemplos

* Listar as referências de projeto do projeto especificado:

  ```dotnetcli
  dotnet list app/app.csproj reference
  ```

* Listar as referências de projeto do projeto no diretório atual:

  ```dotnetcli
  dotnet list reference
  ```
