---
title: Ferramentas para desenvolvedores do .NET do Azure
description: Obtenha as ferramentas para começar a usar as bibliotecas .NET do Azure em um ambiente Windows, Linux ou Mac.
ms.date: 06/19/2020
ms.custom: azure-sdk-dotnet
ms.openlocfilehash: fa645b152f15550b25a3542ba0cdbb43799536b9
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174891"
---
# <a name="azure-tools-for-developing-with-net"></a>Ferramentas do Azure para desenvolvimento com o .NET

## <a name="sdk-downloads"></a>Downloads do SDK

As bibliotecas do Azure para .NET são implementadas como [pacotes NuGet](https://www.nuget.org/packages?q=windowsazureofficial). Consulte a [referência de API](/dotnet/api/overview/azure/?view=azure-dotnet) para obter a documentação de referência organizada pelo serviço do Azure.

## <a name="development-tools"></a>Ferramentas de desenvolvimento

O Visual Studio tem ferramentas para muitos serviços do Azure internos. Alguns serviços do Azure têm ferramentas ou emuladores adicionais disponíveis, como [Gerenciador de armazenamento do Azure](https://azure.microsoft.com/features/storage-explorer/). Consulte a documentação do serviço do Azure para obter outras ferramentas, se necessário.

Selecione seu ambiente de desenvolvimento preferido abaixo.

## <a name="visual-studio-on-windows"></a>[Visual Studio no Windows](#tab/vs)

O Visual Studio versão 2017 e versões posteriores têm suporte interno para o desenvolvimento do Azure. As etapas a seguir descrevem a habilitação do suporte de desenvolvimento do Azure no Visual Studio.

Para o Visual Studio 2015 e anterior, <a href="vs2015-install.md">siga estas instruções</a>.

### <a name="step-1-download-visual-studio-2019"></a>Etapa 1: baixar o Visual Studio 2019

Você pode ignorar esta etapa se já tiver o Visual Studio 2019 instalado.

> [!div class="nextstepaction"]
> [Baixar o Visual Studio 2019](https://www.visualstudio.com/downloads/)

### <a name="step-2-install-the-two-azure-workloads"></a>Etapa 2: instalar as duas cargas de trabalho do Azure

No instalador do Visual Studio, instale o Visual Studio (ou modifique uma instalação existente). Verifique se as cargas de trabalho *desenvolvimento do Azure* e *ASP.net e desenvolvimento Web* estão selecionadas.

### <a name="step-3-develop-with-net-on-azure"></a>Etapa 3: desenvolver com o .NET no Azure

Comece [implantando seu primeiro aplicativo Web ASP.NET Core no Serviço de Aplicativo do Azure](/azure/app-service-web/app-service-web-get-started-dotnet).

## <a name="visual-studio-code-cross-platform"></a>[Visual Studio Code (plataforma cruzada)](#tab/vscode)

Visual Studio Code tem tudo o que você precisa para o desenvolvimento do Azure entre plataformas.

### <a name="step-1-download-the-net-core-sdk"></a>Etapa 1: Baixar o SDK do .NET Core

O SDK e as ferramentas de linha de comando para aplicativos .NET Core.

> [!div class="nextstepaction"]
> [Baixar o SDK do .NET Core](https://dotnet.microsoft.com/download)

### <a name="step-2-visual-studio-code"></a>Etapa 2: Visual Studio Code

Edite e depure aplicativos .NET Core em qualquer sistema operacional: Windows, Mac e Linux.

> [!div class="nextstepaction"]
> [Baixar o Visual Studio Code](https://code.visualstudio.com)

### <a name="step-3-azure-tools-extension"></a>Etapa 3: extensão das ferramentas do Azure

Instale a extensão de ferramentas do Azure no Visual Studio Code.

> [!div class="nextstepaction"]
> [Instalar extensão de ferramentas do Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack)

### <a name="step-4-develop-with-net-on-azure"></a>Etapa 4: desenvolver com o .NET no Azure

Comece [implantando seu primeiro aplicativo web ASP.NET Core no serviço Azure app no Linux](/azure/app-service/containers/quickstart-dotnetcore).

---
