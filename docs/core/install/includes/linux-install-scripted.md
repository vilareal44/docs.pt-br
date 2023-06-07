---
ms.openlocfilehash: fb78e1439a680a8dbb9fc0eb8afdeee3efef7ead
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84768385"
---

Os [scripts dotnet-install](../../tools/dotnet-install-script.md) são usados para instalações de automação e não administrativas do **SDK** e do **tempo de execução**. É possível baixar o script em <https://dot.net/v1/dotnet-install.sh>.

O script assume como padrão a instalação da versão mais recente do [LTS (suporte a longo prazo)](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) do SDK, que é o .net Core 3,1. Para instalar a versão atual, que pode não ser uma versão (LTS), use o `-c Current` parâmetro.

```bash
./dotnet-install.sh -c Current
```

Para instalar o tempo de execução do .NET Core em vez do SDK, use o `--runtime` parâmetro.

```bash
./dotnet-install.sh -c Current --runtime aspnetcore
```

Você pode instalar uma versão específica alterando o `-c` parâmetro para indicar a versão específica. O comando a seguir instala SDK do .NET Core 3,1.

```bash
./dotnet-install.sh -c 3.1
```

Para obter mais informações, consulte [dotnet – referência de scripts de instalação](../../tools/dotnet-install-script.md).
