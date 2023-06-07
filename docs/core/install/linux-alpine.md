---
title: Instalar o .NET Core no Alpine-.NET Core
description: Demonstra as várias maneiras de instalar SDK do .NET Core e o tempo de execução do .NET Core em Alpine Ski.
author: adegeo
ms.author: adegeo
ms.date: 06/04/2020
ms.openlocfilehash: 0efe3bbacbe573b77eae8818ea29b5a3867e4570
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619514"
---
# <a name="install-net-core-sdk-or-net-core-runtime-on-alpine"></a>Instalar o SDK do .NET Core ou o tempo de execução do .NET Core no Alpine Ski

Este artigo descreve como instalar o .NET Core em Alpine Ski. Quando uma versão do Alpineio ficar sem suporte, o .NET Core não terá mais suporte nessa versão. No entanto, essas instruções podem ajudá-lo a obter o .NET Core em execução nessas versões, mesmo que não haja suporte.

[!INCLUDE [linux-intro-sdk-vs-runtime](includes/linux-intro-sdk-vs-runtime.md)]

Não há instaladores para o Alpine. Você deve usar o [script de instalação](#scripted-install) ou seguir as instruções de [instalação manual](#manual-install) .

## <a name="supported-distributions"></a>Distribuições com suporte

A tabela a seguir é uma lista de versões do .NET Core com suporte no momento e as versões do Alpine para as quais têm suporte. Essas versões permanecem com suporte até que a versão do [.NET Core atinja o fim do suporte](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) ou a versão do [Alpine alcance o fim da vida útil](https://wiki.alpinelinux.org/wiki/Alpine_Linux:Releases).

- Um ✔️ indica que a versão do Alpine ou .NET Core ainda tem suporte.
- Um ❌ indica que a versão do Alpine ou .NET Core não tem suporte nessa versão do Alpine.
- Quando uma versão do Alpine e uma versão do .NET Core têm ✔️, há suporte para essa combinação de so e .NET.

| Alpine                   | .NET Core 2.1 | .NET Core 3.1 | Versão prévia do .NET 5 |
|--------------------------|---------------|---------------|----------------|
| ✔️ 3,12  | ✔️ 2,1        | ✔️ 3,1        | versão prévia do ✔️ 5,0 |
| ✔️ 3,11  | ✔️ 2,1        | ✔️ 3,1        | versão prévia do ✔️ 5,0 |
| ✔️ 3,10  | ✔️ 2,1        | ✔️ 3,1        | versão prévia do ✔️ 5,0 |
| ✔️ 3,9   | ✔️ 2,1        | ✔️ 3,1        | versão prévia do ✔️ 5,0 |
| ❌3,8   | ✔️ 2,1        | ❌3,1        | ❌visualização de 5,0 |

Não há mais suporte para as seguintes versões do .NET Core. Os downloads para eles ainda permanecem publicados:

- 3.0
- 2.2
- 2,0

## <a name="dependencies"></a>Dependências

O .NET Core no Alpineum Linux requer as seguintes dependências instaladas:

- ICU-bibliotecas
- krb5-libs
- libgcc
- libintl
- libssl 1.1 (Alpine v 3.9 ou superior)
- libssl 1.0 (Alpine v 3.8 ou inferior)
- libstdc++
- zlib

## <a name="scripted-install"></a>Instalação com script

[!INCLUDE [linux-install-scripted](includes/linux-install-scripted.md)]

## <a name="manual-install"></a>Instalação manual

[!INCLUDE [linux-install-manual](includes/linux-install-manual.md)]

## <a name="next-steps"></a>Próximas etapas

- [Tutorial: criar um aplicativo de console com SDK do .NET Core usando Visual Studio Code](../tutorials/with-visual-studio-code.md)
