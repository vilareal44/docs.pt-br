---
title: Introdução ao .NET Core
description: Encontre recursos para aprender a criar aplicativos .NET Core no Windows, Linux e macOS.
author: adegeo
ms.author: adegeo
ms.date: 12/03/2019
ms.custom: vs-dotnet
ms.openlocfilehash: 56eebc0fc5bad6f57d93358cbbef389d6355d66b
ms.sourcegitcommit: c4a15c6c4ecbb8a46ad4e67d9b3ab9b8b031d849
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88656684"
---
# <a name="get-started-with-net-core"></a>Introdução ao .NET Core

Este artigo fornece informações de como começar a usar o .NET Core. O .NET Core pode ser instalado no Windows, no Linux e no macOS. Você pode codificar em seu editor de texto favorito e produzir aplicativos e bibliotecas multiplataforma.

Se você não tiver certeza de qual é o .NET Core ou de como ele se relaciona com outras tecnologias .NET, comece com o [que é](https://dotnet.microsoft.com/learn/dotnet/what-is-dotnet) a visão geral do .net. Resumindo, o .NET Core é uma implementação open-source e multiplataforma do .NET.

## <a name="create-an-application"></a>Criar um aplicativo

Primeiro, baixe e instale o [SDK do .NET Core](https://dotnet.microsoft.com/download) em seu computador.

Em seguida, abra um terminal, como o **PowerShell**, um **prompt de comando** ou o **Bash**. Insira os seguintes `dotnet` comandos para criar e executar um aplicativo C#:

```dotnetcli
dotnet new console --output sample1
dotnet run --project sample1
```

A seguinte saída deve ser exibida:

```console
Hello World!
```

Parabéns! Você criou um aplicativo simples do .NET Core. Também é possível usar o [Visual Studio Code](./tutorials/with-visual-studio-code.md), o [Visual Studio](./tutorials/with-visual-studio.md) (somente Windows) ou o [Visual Studio para Mac](tutorials/with-visual-studio-mac.md) (somente macOS), para criar um aplicativo .NET Core.

## <a name="tutorials"></a>Tutoriais

Comece a desenvolver aplicativos .NET Core seguindo estes tutoriais passo a passo:

<!-- markdownlint-disable MD025 -->

# <a name="windows"></a>[Windows](#tab/windows)

- [Criar seu primeiro aplicativo de console do .NET Core no Visual Studio 2019](./tutorials/with-visual-studio.md)
- [Criar uma biblioteca de classes com .NET Standard no Visual Studio](./tutorials/library-with-visual-studio.md)
- [Tutorial: criar um aplicativo de console do .NET Core usando Visual Studio Code](tutorials/with-visual-studio-code.md)

|   |   |
|---|---|
| ![ícone de câmera para vídeo](./media/video-icon.png "Assistir a um vídeo") | Assista ao vídeo [como instalar e usar o Visual Studio Code e o .NET Core](https://channel9.msdn.com/Blogs/dotnet/Get-started-with-VS-Code-using-CSharp-and-NET-Core/) no Channel 9. |
| ![ícone de câmera para vídeo](./media/video-icon.png "Assistir a um vídeo") | Assista aos vídeos do [.NET Core 101](https://www.youtube.com/playlist?list=PLdo4fOcmZ0oWoazjhXQzBKMrFuArxpW80) no YouTube. |

Consulte o artigo [dependências e requisitos do .NET Core](install/dependencies.md?pivots=os-windows) para obter uma lista das versões do Windows com suporte.

# <a name="linux"></a>[Linux](#tab/linux)

Comece a desenvolver aplicativos .NET Core seguindo estes tutoriais passo a passo:

- [Tutorial: criar um aplicativo de console do .NET Core usando Visual Studio Code](tutorials/with-visual-studio-code.md)

|   |   |
|---|---|
| ![ícone de câmera para vídeo](./media/video-icon.png "Assistir a um vídeo") | Assista a um vídeo sobre uma [introdução ao Visual Studio Code usando C# e o .NET Core no Ubuntu](https://channel9.msdn.com/Blogs/dotnet/Get-started-with-VS-Code-Csharp-dotnet-Core-Ubuntu). |

Consulte o artigo [dependências e requisitos do .NET Core](install/dependencies.md?pivots=os-linux) para obter uma lista das versões e distribuições do Linux com suporte.

# <a name="macos"></a>[macOS](#tab/macos)

Comece a desenvolver aplicativos .NET Core seguindo estes tutoriais passo a passo:

- [Tutorial: criar um aplicativo de console do .NET Core usando Visual Studio Code](tutorials/with-visual-studio-code.md)
- [Tutorial: criar um aplicativo de console do .NET Core usando Visual Studio para Mac](tutorials/with-visual-studio-mac.md)
- [Criar uma biblioteca de .NET Standard no macOS usando Visual Studio para Mac](tutorials/library-with-visual-studio-mac.md)

|   |   |
|---|---|
| ![ícone de câmera para vídeo](media/video-icon.png "Assistir a um vídeo") | Assista a um vídeo sobre como [começar a usar o Visual Studio Code usando C# e .NET Core no MacOS](https://channel9.msdn.com/Blogs/dotnet/Get-started-VSCode-NET-Core-Mac). |

Consulte o artigo [dependências e requisitos do .NET Core](install/dependencies.md?pivots=os-macos) para obter uma lista das versões do os X/MacOS com suporte.

---
