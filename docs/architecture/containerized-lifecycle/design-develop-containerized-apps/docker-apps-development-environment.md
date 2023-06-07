---
title: Ambiente de desenvolvimento para aplicativos do Docker
description: Saiba mais sobre as opções de ferramentas de desenvolvimento mais importantes compatíveis com o ciclo de vida de desenvolvimento do Docker.
ms.date: 08/06/2020
ms.openlocfilehash: 07b42b2bd05ab16ba0fbf61863b050ee2c9e242b
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87916030"
---
# <a name="development-environment-for-docker-apps"></a>Ambiente de desenvolvimento para aplicativos do Docker

## <a name="development-tools-choices-ide-or-editor"></a>Opções de ferramentas de desenvolvimento: IDE ou editor

Seja qual for sua preferência, um IDE avançado e completo ou um editor leve e ágil, a Microsoft oferece tudo que você precisa para desenvolver aplicativos do Docker.

### <a name="visual-studio-code-and-docker-cli-cross-platform-tools-for-mac-linux-and-windows"></a>Visual Studio Code e CLI do Docker (ferramentas multiplataforma para Mac, Linux e Windows)

Se preferir um editor leve e multiplataforma compatível com qualquer linguagem de desenvolvimento, use o Visual Studio Code e a CLI do Docker. Esses produtos fornecem uma experiência simples e robusta fundamental para simplificar o fluxo de trabalho do desenvolvedor. Ao instalar o "Docker for Mac" ou "Docker for Windows" (ambiente de desenvolvimento), os desenvolvedores do Docker podem usar uma única CLI do Docker para criar aplicativos para Windows ou Linux (ambiente de runtime). Além disso, o Visual Studio Code é compatível com as extensões do Docker com IntelliSense para Dockerfiles e tarefas de atalho, para executar os comandos do Docker usando o editor.

> [!NOTE]
> Para baixar o Visual Studio Code, acesse <https://code.visualstudio.com/download>.
>
> Para baixar o Docker for Mac e Windows, acesse <https://www.docker.com/products/docker>.

### <a name="visual-studio-with-docker-tools-windows-development-machine"></a>Visual Studio com ferramentas do Docker (computador de desenvolvimento do Windows)

É recomendável que você use o Visual Studio 2019 com as ferramentas internas do Docker habilitadas. Com o Visual Studio, é possível desenvolver, executar e validar seus aplicativos diretamente no ambiente do Docker escolhido. Pressione F5 para depurar seu aplicativo (contêiner único ou vários contêineres) diretamente em um host do Docker ou pressione CTRL + F5 para editar e atualizar o aplicativo sem precisar recompilar o contêiner. Essa é a opção mais simples e mais eficiente para desenvolvedores do Windows para criação de contêineres do Docker para Windows ou Linux.

### <a name="visual-studio-for-mac-mac-development-machine"></a>Visual Studio para Mac (computador de desenvolvimento do Mac)

Você pode usar o [Visual Studio para Mac](https://visualstudio.microsoft.com/vs/mac/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link) ao desenvolver aplicativos baseados no Docker. O Visual Studio para Mac oferece um IDE mais avançado quando comparado ao Visual Studio Code para Mac.

## <a name="language-and-framework-choices"></a>Opções de estrutura e linguagem

Você pode desenvolver aplicativos do Docker usando as ferramentas da Microsoft com linguagens mais modernas. A seguir está uma lista inicial, mas você não está limitado a ela:

- .NET Core e ASP.NET Core
- Node.js
- Go
- Java
- Ruby
- Python

Basicamente, você pode usar qualquer linguagem moderna compatível com o Docker no Linux ou Windows.

>[!div class="step-by-step"]
>[Anterior](deploy-azure-kubernetes-service.md) 
> [Avançar](docker-apps-inner-loop-workflow.md)
