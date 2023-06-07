---
ms.openlocfilehash: f9ea0ee6402187365cec5cdced1617ee2ae66bed
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84603065"
---

### <a name="install-the-sdk"></a>Instalar o SDK

SDK do .NET Core permite que você desenvolva aplicativos com o .NET Core. Se você instalar SDK do .NET Core, não será necessário instalar o tempo de execução correspondente. Para instalar o SDK do .NET Core, execute os seguintes comandos:

```bash
sudo dnf install dotnet-sdk-3.0
```

### <a name="install-the-runtime"></a>Instalar o runtime

O tempo de execução do .NET Core permite executar aplicativos que foram feitos com o .NET Core que não incluiu o tempo de execução. Os comandos a seguir instalam o tempo de execução de ASP.NET Core, que é o tempo de execução mais compatível para o .NET Core. Em seu terminal, execute os comandos a seguir.

```bash
sudo dnf install aspnetcore-runtime-3.0
```

Como alternativa ao tempo de execução de ASP.NET Core, você pode instalar o tempo de execução do .NET Core que não inclui suporte a ASP.NET Core: substitua `aspnetcore-runtime-3.0` no comando acima por `dotnet-runtime-3.0` .

```bash
sudo dnf install dotnet-runtime-3.0
```
