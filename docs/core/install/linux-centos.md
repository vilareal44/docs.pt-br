---
title: Instalar o .NET Core no CentOS-.NET Core
description: Demonstra as várias maneiras de instalar o SDK do .NET Core e o tempo de execução do .NET Core no CentOS.
author: adegeo
ms.author: adegeo
ms.date: 06/04/2020
ms.openlocfilehash: 6b1bad3a6c967483bb683866de84c9e5077a336f
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619501"
---
# <a name="install-net-core-sdk-or-net-core-runtime-on-centos"></a>Instalar o SDK do .NET Core ou o tempo de execução do .NET Core no CentOS

O .NET Core tem suporte no CentOS. Este artigo descreve como instalar o .NET Core no CentOS.

[!INCLUDE [linux-intro-sdk-vs-runtime](includes/linux-intro-sdk-vs-runtime.md)]

[!INCLUDE [linux-install-package-manager-x64-vs-arm](includes/linux-install-package-manager-x64-vs-arm.md)]

## <a name="supported-distributions"></a>Distribuições com suporte

A tabela a seguir é uma lista de versões do .NET Core com suporte atualmente no CentOS 7 e no CentOS 8. Essas versões permanecem com suporte até que a versão do [.NET Core atinja o fim do suporte](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) ou a versão do CentOS não seja mais suportada.

- Um ✔️ indica que a versão do CentOS ou do .NET Core ainda tem suporte.
- Um ❌ indica que a versão do CentOS ou do .NET Core não tem suporte nessa versão do CentOS.
- Quando uma versão do CentOS e uma versão do .NET Core têm ✔️, há suporte para essa combinação de so e .NET.

| CentOS                   | .NET Core 2.1 | .NET Core 3.1 | Versão prévia do .NET 5 (somente instalação manual) |
|--------------------------|---------------|---------------|----------------|
| ✔️ [8](#centos-8-) | ✔️ 2,1        | ✔️ 3,1        | versão prévia do ✔️ 5,0 |
| ✔️ [7](#centos-7-) | ✔️ 2,1        | ✔️ 3,1        | versão prévia do ✔️ 5,0 |

Não há mais suporte para as seguintes versões do .NET Core. Os downloads para eles ainda permanecem publicados:

- 3.0
- 2.2
- 2,0

[!INCLUDE [linux-install-package-manager-x64-vs-arm](includes/linux-install-package-manager-x64-vs-arm.md)]

## <a name="how-to-install-other-versions"></a>Como instalar outras versões

[!INCLUDE [package-manager-switcher](./includes/package-manager-heading-hack-pkgname.md)]

## <a name="centos-8-"></a>CentOS 8 ✔️

O .NET Core 3,1 está disponível nos repositórios de pacotes padrão do CentOS 8.

[!INCLUDE [linux-dnf-install-31](includes/linux-install-31-dnf.md)]

## <a name="centos-7-"></a>CentOS 7 ✔️

[!INCLUDE [linux-prep-intro-generic](includes/linux-prep-intro-generic.md)]

```bash
sudo rpm -Uvh https://packages.microsoft.com/config/centos/7/packages-microsoft-prod.rpm
```

[!INCLUDE [linux-yum-install-31](includes/linux-install-31-yum.md)]

## <a name="troubleshoot-the-package-manager"></a>Solucionar problemas do Gerenciador de pacotes

Esta seção fornece informações sobre erros comuns que você pode obter ao usar o Gerenciador de pacotes para instalar o .NET Core.

### <a name="failed-to-fetch"></a>Falha ao buscar

[!INCLUDE [package-manager-failed-to-fetch-rpm](includes/package-manager-failed-to-fetch-rpm.md)]

## <a name="snap"></a>Snap

[!INCLUDE [linux-install-snap](includes/linux-install-snap.md)]

## <a name="dependencies"></a>Dependências

[!INCLUDE [linux-rpm-install-dependencies](includes/linux-rpm-install-dependencies.md)]

## <a name="scripted-install"></a>Instalação com script

[!INCLUDE [linux-install-scripted](includes/linux-install-scripted.md)]

## <a name="manual-install"></a>Instalação manual

[!INCLUDE [linux-install-manual](includes/linux-install-manual.md)]

## <a name="next-steps"></a>Próximas etapas

- [Tutorial: criar um aplicativo de console com SDK do .NET Core usando Visual Studio Code](../tutorials/with-visual-studio-code.md)
