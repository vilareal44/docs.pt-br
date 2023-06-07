---
title: Visão geral e introdução ao .NET Core
description: O .NET Core é uma implementação modular de alto desempenho do .NET para a criação de aplicativos para Windows, Linux e macOS. Saiba mais sobre o .NET Core para começar.
author: richlander
ms.date: 03/26/2020
ms.custom: updateeachrelease
ms.openlocfilehash: b28ad965e54680e2e1134c389266741ade28084f
ms.sourcegitcommit: 67cf756b033c6173a1bbd1cbd5aef1fccac99e34
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226576"
---
# <a name="introduction-to-net-core"></a>Introdução ao .NET Core

O [.NET Core](about.md) é uma plataforma de desenvolvimento de software livre [, de uso](https://github.com/dotnet/runtime/blob/master/LICENSE.TXT)geral. Você pode criar aplicativos .NET Core para Windows, macOS e Linux para processadores x64, x86, ARM32 e ARM64 usando várias linguagens de programação. Estruturas e APIs são fornecidas para a [nuvem](/aspnet/core/), a [IOT](/archive/msdn-magazine/2019/august/net-core-cross-platform-iot-programming-with-net-core-3-0), a [interface do usuário do cliente](../desktop-wpf/overview/index.md)e o [Machine Learning](/dotnet/machine-learning/).

[Baixe o SDK do .NET Core](https://dotnet.microsoft.com/download) para experimentar o .NET Core em seu computador. A versão mais recente é o [.NET Core 3,1](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-1/).

## <a name="download-net-core"></a>Baixe o .NET Core

Você pode obter o .NET Core das seguintes maneiras:

* [Instaladores para Windows e macOS](https://dotnet.microsoft.com/download)
* [Pacotes do Linux](https://docs.microsoft.com/dotnet/core/install/linux-package-managers)
* [Contêineres do Docker](https://hub.docker.com/_/microsoft-dotnet-core/)
* [Zips e tarballs](https://dotnet.microsoft.com/download/dotnet-core/3.1)
* [Scripts de instalação](https://dotnet.microsoft.com/download/dotnet-core/scripts)
* [Notas de versão](https://github.com/dotnet/core/tree/master/release-notes)

## <a name="create-your-first-application"></a>Criar seu primeiro aplicativo

Após instalar o SDK do .NET Core, abra um prompt de comando. Use os comandos a seguir para criar e executar um aplicativo:

```dotnetcli
dotnet new console
dotnet run
```

A seguinte saída deve ser exibida:

```output
Hello World!
```

## <a name="contribute"></a>Contribuir

O .NET Core é uma plataforma aberta. Todos são bem-vindos a participar.

* Arquivar questões e problemas do produto na [comunidade de desenvolvedores](https://developercommunity.visualstudio.com/spaces/61/index.html).
* As contribuições de produto devem ser feitas em um dos repositórios do projeto, como [dotnet/tempo de execução](https://github.com/dotnet/runtime), [dotnet/SDK](https://github.com/dotnet/sdk), [dotnet/Rosyln](https://github.com/dotnet/roslyn)ou [aspnetcore](https://github.com/dotnet/aspnetcore). Para obter mais informações, consulte [.NET Core repositórios](https://github.com/dotnet/core/blob/master/Documentation/core-repos.md).

## <a name="support"></a>Suporte

O .NET Core tem suporte da Microsoft no Windows, macOS e Linux e pelo Red Hat em Red Hat Enterprise Linux.

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Tutoriais do .NET Core](tutorials/index.md)

> [!div class="nextstepaction"]
> [Experimente o .NET Core em seu navegador](../csharp/tutorials/intro-to-csharp/numbers-in-csharp.yml)
