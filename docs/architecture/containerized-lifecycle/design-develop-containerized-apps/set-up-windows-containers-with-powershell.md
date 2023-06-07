---
title: Como usar comandos do Windows PowerShell em um DockerFile para configurar contêineres do Windows (baseado em Docker padrão)
description: Saiba como usar o PowerShell ao trabalhar com o Docker em contêineres do Windows
ms.date: 08/06/2020
ms.openlocfilehash: 4e7b9e7fedf11b97b3f468aef541bf72a4e88ebc
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87915388"
---
# <a name="using-windows-powershell-commands-in-a-dockerfile-to-set-up-windows-containers-docker-standard-based"></a>Como usar comandos do Windows PowerShell em um DockerFile para configurar contêineres do Windows (baseado em Docker padrão)

Com os [contêineres do Windows](/virtualization/windowscontainers/about/index), é possível converter seus aplicativos existentes do Windows em imagens do Docker e implantá-los com as mesmas ferramentas que o resto do ecossistema do Docker.

Para usar os contêineres do Windows, você apenas precisa gravar comandos do Windows PowerShell no DockerFile, conforme mostrado no exemplo a seguir:

```Dockerfile
FROM microsoft/windowsservercore
LABEL Description="IIS" Vendor="Microsoft" Version="10"
RUN powershell -Command Add-WindowsFeature Web-Server
CMD [ "ping", "localhost", "-t" ]
```

Neste caso, estamos usando o Windows PowerShell para instalar uma imagem base do Windows Server Core, bem como o IIS.

Da mesma forma, você também pode usar comandos do Windows PowerShell para configurar outros componentes, como o ASP.NET 4.x e o .NET 4.6 tradicionais ou qualquer outro software do Windows, como mostrado aqui:

```Dockerfile
RUN powershell add-windowsfeature web-asp-net45
```

>[!div class="step-by-step"]
>[Anterior](visual-studio-tools-for-docker.md) 
> [Avançar](build-aspnet-core-applications-linux-containers-aks-kubernetes.md)
