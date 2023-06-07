---
title: Visão geral do .NET Core
description: Saiba mais sobre as características e a composição do .NET Core e compare-o com outras implementações do .NET.
ms.date: 03/26/2020
ms.openlocfilehash: d5ef79fe5a8fbb56beae77edd01830fe6561fa51
ms.sourcegitcommit: 4ad2f8920251f3744240c3b42a443ffbe0a46577
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86100724"
---
# <a name="net-core-overview"></a>Visão geral do .NET Core

> [!div class="button"]
> [Baixe o .NET Core](https://dotnet.microsoft.com/download)

O .NET Core tem as seguintes características:

- **Plataforma cruzada:** É executado em [sistemas operacionais](https://github.com/dotnet/core/blob/master/os-lifecycle-policy.md)Windows, MacOS e Linux.
- **Código-fonte aberto:** O .NET Core [Framework é um](https://github.com/dotnet/core)software livre, usando as licenças MIT e Apache 2. O .NET Core é um projeto do [.NET Foundation](https://dotnetfoundation.org/).
- **Moderno:** Ele implementa paradigmas modernos como programação assíncrona, padrões sem cópia usando structs e governança de recursos para contêineres.
- **Desempenho:**  Fornece [alto desempenho](https://devblogs.microsoft.com/dotnet/performance-improvements-in-net-core-3-0/) com recursos como [intrínsecos de hardware](https://devblogs.microsoft.com/dotnet/hardware-intrinsics-in-net-core/), [compilação em camadas](https://github.com/dotnet/coreclr/blob/master/Documentation/design-docs/tiered-compilation.md)e [span \<T> ](../standard/memory-and-spans/index.md).
- **Consistente entre ambientes:** Executa o código com o mesmo comportamento em vários sistemas operacionais e arquiteturas, incluindo x64, x86 e ARM.
- **Ferramentas de linha de comando:**  Inclui ferramentas de linha de comando fáceis de usar que podem ser usadas para o desenvolvimento local e para a integração contínua.
- **Implantação flexível:** Você pode incluir o .NET Core em seu aplicativo ou instalá-lo lado a lado (instalações em todo o usuário ou todo o sistema). Pode ser usado com os [contêineres do Docker](docker/introduction.md).

## <a name="languages"></a>Idiomas

As linguagens [C#](../csharp/index.yml), [Visual Basic](../visual-basic/index.yml)e [F #](../fsharp/index.yml) podem ser usadas para escrever aplicativos e bibliotecas para o .NET Core. Essas linguagens podem ser usadas em seu editor de texto favorito ou IDE (ambiente de desenvolvimento integrado), incluindo:

- [Visual Studio](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link)
- [Visual Studio Code](https://code.visualstudio.com/download)

A integração do editor é fornecida, em parte, pelos colaboradores dos projetos [OmniSharp](https://www.omnisharp.net/) e [Ionide](https://ionide.io) .

## <a name="apis"></a>APIs

O .NET Core expõe estruturas para a criação de qualquer tipo de aplicativo:

* Aplicativos de nuvem com [ASP.NET Core](/aspnet/core/)
* Aplicativos móveis com o [Xamarin](/xamarin)
* Aplicativos de IoT com [System. Device. GPIO](https://docs.microsoft.com/archive/msdn-magazine/2019/august/net-core-cross-platform-iot-programming-with-net-core-3-0)
* Aplicativos cliente do Windows com [WPF](../desktop-wpf/overview/index.md) e Windows Forms
* [Ml.net](../machine-learning/index.yml) de Machine Learning

Há muitas APIs incluídas que atendem às necessidades comuns:

- Tipos primitivos, como <xref:System.Boolean?displayProperty=nameWithType> e <xref:System.Int32?displayProperty=nameWithType> .
- Coleções, como <xref:System.Collections.Generic.List%601?displayProperty=nameWithType> e <xref:System.Collections.Generic.Dictionary%602?displayProperty=nameWithType>.
- Tipos de utilitário, como <xref:System.Net.Http.HttpClient?displayProperty=nameWithType> e <xref:System.IO.FileStream?displayProperty=nameWithType>.
- Tipos de dados, como <xref:System.Data.DataSet?displayProperty=nameWithType> e <xref:System.Data.Entity.DbSet?displayProperty=nameWithType> .
- Tipos de alto desempenho, como <xref:System.Span%601?displayProperty=nameWithType> <xref:System.Numerics.Vector?displayProperty=nameWithType> [pipelines](../standard/io/pipelines.md), e.

O .NET Core oferece compatibilidade com as APIs do .NET Framework e do Mono por meio da implementação da especificação [.NET Standard](../standard/net-standard.md).

## <a name="composition"></a>Composição

O .NET Core é composto pelas seguintes partes:

- O [tempo de execução do .NET Core](https://github.com/dotnet/runtime/tree/master/src/coreclr), que fornece um sistema de tipos, carregamento de assembly, coletor de lixo, interoperabilidade nativa e outros serviços básicos. As [bibliotecas do .NET Core Framework](https://github.com/dotnet/runtime/tree/master/src/libraries) fornecem tipos de dados primitivos, tipos de composição de aplicativo e utilitários fundamentais.
- O [tempo de execução de ASP.NET Core](https://github.com/dotnet/aspnetcore), que fornece uma estrutura para a criação de aplicativos modernos conectados à Internet, como aplicativos Web, aplicativos de IOT e back-ends móveis.
- Os compiladores de [SDK do .NET Core](https://github.com/dotnet/sdk) e linguagem ([Roslyn](https://github.com/dotnet/roslyn) e [F #](https://github.com/microsoft/visualfsharp)) que habilitam a experiência do desenvolvedor do .NET Core.
- O [comando dotnet](./tools/dotnet.md), que é usado para iniciar aplicativos .NET Core e comandos da CLI. Ele seleciona e hospeda o tempo de execução, fornece uma política de carregamento de assembly e inicia aplicativos e ferramentas.

### <a name="open-source"></a>Código-fonte aberto

O [.NET Core](about.md) é uma plataforma de desenvolvimento de software livre [, de uso](https://github.com/dotnet/runtime/blob/master/LICENSE.TXT)geral. Você pode criar aplicativos .NET Core para Windows, macOS e Linux para processadores x64, x86, ARM32 e ARM64. Estruturas e APIs são fornecidas para a [nuvem](/aspnet/core/), a [IOT](https://docs.microsoft.com/archive/msdn-magazine/2019/august/net-core-cross-platform-iot-programming-with-net-core-3-0), a [interface do usuário do cliente](../desktop-wpf/overview/index.md)e o [Machine Learning](../machine-learning/index.yml).

## <a name="support"></a>Suporte

O .NET Core tem [suporte da Microsoft](https://dotnet.microsoft.com/platform/support/policy) no Windows, MacOS e Linux. Ele é atualizado com segurança e qualidade regularmente (a segunda terça-feira de cada mês).

As distribuições binárias do .NET Core da Microsoft são criadas e testadas em servidores mantidos pela Microsoft no Azure e seguem práticas de engenharia e segurança da Microsoft.

A [Red Hat oferece suporte ao .NET Core](https://developers.redhat.com/topics/dotnet/) no Red Hat Enterprise Linux (RHEL). A Red Hat compila o .NET Core da origem e o disponibiliza nas [Coleções de Software do Red Hat](https://developers.redhat.com/products/softwarecollections/overview/). Red Hat e Microsoft colaboram para garantir que o .NET Core funcione bem no RHEL.

O [tizen dá suporte ao .NET Core](https://developer.tizen.org/development/training/.net-application) em plataformas tizen.
