---
title: .NET Core e software livre
description: Leia uma visão geral sobre o .NET Core, que é uma implementação de uso geral, modular, de plataforma cruzada e de código aberto do .NET Standard.
ms.date: 03/30/2017
ms.assetid: e6bd4655-ce37-4003-8462-468a6fe2c40f
ms.openlocfilehash: 08d30d047c25b3b6d681d72b46b81a0cb21f8e0a
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85622751"
---
# <a name="net-core-and-open-source"></a>.NET Core e software livre

Este artigo fornece uma breve visão geral do que é o .NET Core e mostra como você pode encontrar mais informações. Para encontrar a lista completa de documentação do .NET Core, visite o [Guia do .NET Core](../../core/index.yml).

## <a name="what-is-net-core"></a>O que é o .NET Core?  

O .NET Core é uma implementação de uso geral, modular, de plataforma cruzada e de software livre do .NET Standard. Ele contém muitas das mesmas APIs que o .NET Framework (mas o .NET Core é um conjunto menor) e inclui componentes de tempo de execução, estrutura, compilador e ferramentas que dão suporte a uma variedade de sistemas operacionais e destinos de chips. A implementação do .NET Core foi conduzida principalmente por cargas de trabalho ASP.NET Core, mas também pela necessidade e desejo de ter uma implementação mais moderna. Ele pode ser usado em cenários de dispositivo, nuvem e incorporado/IoT.  
  
Para começar a usar o .NET Core, visite o tutorial do .NET [Olá, mundo em 10 minutos](https://dotnet.microsoft.com/learn/dotnet/hello-world-tutorial/intro).  
  
As principais características do .NET Core são:
  
- **Plataforma cruzada:** o .NET Core fornece uma funcionalidade essencial para implementação de recursos de aplicativo necessários, e reutilização desse código independentemente de seu destino de plataforma. Atualmente, ele dá suporte a três sistemas operacionais principais (SO): Windows, Linux e macOS. Você pode escrever aplicativos e bibliotecas que são executados sem modificações em sistemas operacionais com suporte. Para ver a lista de sistemas operacionais com suporte, visite [Roteiro do .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md).
  
- **Software livre:** o .NET Core é um dos muitos projetos sob a administração do [.NET Foundation](https://www.dotnetfoundation.org/) e está disponível no [GitHub](https://github.com/). Como um projeto de código-fonte aberto, o .NET Core promove um processo de desenvolvimento mais transparente e uma comunidade ativa e envolvida.  
  
- **Implantação flexível:** há duas maneiras de implantar seu aplicativo: implantação dependente da estrutura ou implantação independente. Com a implantação dependente da estrutura, apenas as dependências de seu aplicativo e de terceiros serão instaladas, e seu aplicativo dependerá da presença de uma versão de todo o sistema do .NET Core. Com a implantação independente, a versão do .NET Core usada para criar seu aplicativo também é implantada junto com seu aplicativo e dependências de terceiros e pode ser executada lado a lado com outras versões. Para saber mais, confira [Implantação de aplicativos .NET Core](../../core/deploying/index.md).

- **Modular:** o .NET Core é modular, pois é liberado por meio do NuGet em pacotes de assembly menores. Em vez de um grande assembly que contém a maior parte da funcionalidade principal, o .NET Core é disponibilizado como pacotes menores e centrados em recursos. Essa modularidade permite um modelo de desenvolvimento mais ágil para nós e permite que você Otimize seu aplicativo para incluir apenas os pacotes NuGet de que você precisa. Os benefícios de uma área de superfície menor do aplicativo incluem segurança mais rigorosa, manutenção reduzida, desempenho aprimorado e redução dos custos em um modelo de pagamento “pague pelo que usar”.  
  
## <a name="the-net-core-platform"></a>A plataforma .NET Core
  
A plataforma .NET Core é composta por vários componentes, incluindo os compiladores gerenciados, o tempo de execução, as bibliotecas de classes base e vários modelos de aplicativos, como ASP.NET Core. Você pode saber mais sobre os diferentes componentes e estar envolvido visitando o seguinte repositórios do [GitHub](https://github.com/) :  
  
- [Página inicial do .NET Core](https://github.com/dotnet/core)  
  
- [Tempo de execução-plataforma e tempo de execução do .NET Core](https://github.com/dotnet/runtime)  
  
- [CLI - Ferramentas da interface de linha de comando do .NET Core](https://github.com/dotnet/cli)  
  
- [Roslyn - Plataforma do Compilador .NET](https://github.com/dotnet/roslyn)  
  
- [ASP.NET Core](https://github.com/dotnet/aspnetcore)  
  
## <a name="see-also"></a>Consulte também

- [Tutorial .NET – Olá, Mundo em 10 minutos](https://dotnet.microsoft.com/learn/dotnet/hello-world-tutorial/intro)
- [Guia do .NET Core](../../core/index.yml)
- [ASP.NET Core docs](/aspnet/core/)
