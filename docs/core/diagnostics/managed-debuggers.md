---
title: Depuradores gerenciados-.NET Core
description: Uma visão geral do Visual Studio e Visual Studio Code depuradores gerenciados.
ms.date: 08/05/2019
ms.openlocfilehash: 28cc21980bc78234f7af3b4668e2fa77fbf8ce5b
ms.sourcegitcommit: 71b8f5a2108a0f1a4ef1d8d75c5b3e129ec5ca1e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84200854"
---
# <a name="net-core-managed-debuggers"></a>Depurador gerenciados do .NET Core

Os depuradores permitem que os programas sejam pausados ou executados passo a passo. Quando em pausa, o estado atual do processo pode ser exibido. Percorrendo as seções principais, você tem noções básicas sobre seu código e por que ele produz o resultado que ele faz.

A Microsoft fornece depuradores para código gerenciado no **Visual Studio** e **Visual Studio Code**.

## <a name="visual-studio-managed-debugger"></a>Depurador gerenciado do Visual Studio

O **Visual Studio** é um ambiente de desenvolvimento integrado com o depurador mais abrangente disponível. O Visual Studio é uma excelente opção para os desenvolvedores que trabalham no Windows.

- [Tutorial-depuração de um aplicativo .NET Core no Windows com o Visual Studio](../tutorials/debugging-with-visual-studio.md)

Embora o Visual Studio seja um aplicativo do Windows, ele ainda pode ser usado para depurar aplicativos Linux e macOS remotamente.

- [Depurando um aplicativo .NET Core no Linux/OSX com o Visual Studio](https://github.com/Microsoft/MIEngine/wiki/Offroad-Debugging-of-.NET-Core-on-Linux---OSX-from-Visual-Studio)

 A depuração ASP.NET Core aplicativos requer instruções um pouco diferentes.

- [Depurar ASP.NET Core aplicativos no Visual Studio](/visualstudio/debugger/how-to-enable-debugging-for-aspnet-applications#debug-aspnet-core-apps)

## <a name="visual-studio-code-managed-debugger"></a>Visual Studio Code depurador gerenciado

**Visual Studio Code** é um editor de código de plataforma cruzada leve. Ele usa a mesma implementação do depurador do .NET Core que o Visual Studio, mas com uma interface do usuário simplificada.

- [Tutorial-depuração de um aplicativo .NET Core com Visual Studio Code](../tutorials/debugging-with-visual-studio-code.md)
- [Depurar no Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging)
