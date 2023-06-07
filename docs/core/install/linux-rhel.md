---
title: Instalar o .NET Core no RHEL-.NET Core
description: Demonstra as várias maneiras de instalar o SDK do .NET Core e o tempo de execução do .NET Core no RHEL.
author: adegeo
ms.author: adegeo
ms.date: 06/04/2020
ms.openlocfilehash: 9e4d0ab86355329b898a82f135b9eeb839eab1cb
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619436"
---
# <a name="install-net-core-sdk-or-net-core-runtime-on-rhel"></a>Instalar o SDK do .NET Core ou o tempo de execução do .NET Core no RHEL

O .NET Core tem suporte no RHEL. Este artigo descreve como instalar o .NET Core no RHEL.

[!INCLUDE [linux-intro-sdk-vs-runtime](includes/linux-intro-sdk-vs-runtime.md)]

## <a name="register-your-red-hat-subscription"></a>Registrar sua assinatura do Red Hat

Para instalar o .NET Core do Red Hat no RHEL, primeiro você precisa se registrar usando o Gerenciador de assinaturas do Red Hat. Se isso não tiver sido feito em seu sistema ou se você não tiver certeza, consulte a [documentação do produto Red Hat para .NET Core](https://access.redhat.com/documentation/net_core/).

## <a name="supported-distributions"></a>Distribuições com suporte

A tabela a seguir é uma lista de versões do .NET Core com suporte no momento no RHEL 7 e RHEL 8. Essas versões permanecem com suporte até que a versão do [.NET Core atinja o fim do suporte](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) ou a versão do RHEL não seja mais suportada.

- Um ✔️ indica que a versão do RHEL ou do .NET Core ainda tem suporte.
- Um ❌ indica que a versão do RHEL ou do .NET Core não tem suporte nessa versão do RHEL.
- Quando uma versão do RHEL e uma versão do .NET Core têm ✔️, há suporte para essa combinação de so e .NET.

| RHEL                   | .NET Core 2.1 | .NET Core 3.1 | Versão prévia do .NET 5 (somente instalação manual) |
|--------------------------|---------------|---------------|----------------|
| ✔️ [8](#rhel-8-) | ✔️ 2,1        | ✔️ 3,1        | versão prévia do ✔️ 5,0 |
| ✔️ [7](#rhel-7-) | ✔️ 2,1        | ✔️ 3,1        | versão prévia do ✔️ 5,0 |

Não há mais suporte para as seguintes versões do .NET Core. Os downloads para eles ainda permanecem publicados:

- 3.0
- 2.2
- 2,0

## <a name="how-to-install-other-versions"></a>Como instalar outras versões

Consulte a [documentação do Red Hat para .NET Core](https://access.redhat.com/documentation/net_core/) sobre as etapas necessárias para instalar outras versões do .NET Core.

## <a name="rhel-8-"></a>RHEL 8 ✔️

O .NET Core está incluído nos repositórios do AppStream para RHEL 8.

[!INCLUDE [linux-dnf-install-31](includes/linux-install-31-dnf.md)]

## <a name="rhel-7-"></a>RHEL 7 ✔️

[!INCLUDE [linux-prep-intro-generic](includes/linux-prep-intro-generic.md)]

O comando a seguir instala o `scl-utils` pacote:

```bash
sudo yum install scl-utils
```

### <a name="install-the-sdk"></a>Instalar o SDK

SDK do .NET Core permite que você desenvolva aplicativos com o .NET Core. Se você instalar SDK do .NET Core, não será necessário instalar o tempo de execução correspondente. Para instalar o SDK do .NET Core, execute os seguintes comandos:

```bash
subscription-manager repos --enable=rhel-7-server-dotnet-rpms
yum install rh-dotnet31 -y
scl enable rh-dotnet31 bash
```

O Red Hat não recomenda a habilitação permanente `rh-dotnet31` porque pode afetar outros programas. Por exemplo, `rh-dotnet31` inclui uma versão do `libcurl` que difere da versão base do RHEL. Isso pode levar a problemas em programas que não esperam uma versão diferente do `libcurl` . Se você quiser habilitar `rh-dotnet` permanentemente, adicione a seguinte linha ao seu arquivo _~/.bashrc_ .

```bash
source scl_source enable rh-dotnet31
```

### <a name="install-the-runtime"></a>Instalar o runtime

O tempo de execução do .NET Core permite executar aplicativos que foram feitos com o .NET Core que não incluiu o tempo de execução. Os comandos a seguir instalam o tempo de execução de ASP.NET Core, que é o tempo de execução mais compatível para o .NET Core. Em seu terminal, execute os comandos a seguir.

```bash
subscription-manager repos --enable=rhel-7-server-dotnet-rpms
yum install rh-dotnet31-aspnetcore-runtime-3.1 -y
scl enable rh-dotnet31-aspnetcore-runtime-3.1 bash
```

O Red Hat não recomenda a habilitação permanente `rh-dotnet31-aspnetcore-runtime-3.1` porque pode afetar outros programas. Por exemplo, `rh-dotnet31-aspnetcore-runtime-3.1` inclui uma versão do `libcurl` que difere da versão base do RHEL. Isso pode levar a problemas em programas que não esperam uma versão diferente do `libcurl` . Se você quiser habilitar `rh-dotnet31-aspnetcore-runtime-3.1` permanentemente, adicione a seguinte linha ao seu arquivo _~/.bashrc_ .

```bash
source scl_source enable rh-dotnet31-aspnetcore-runtime-3.1
```

Como alternativa ao tempo de execução de ASP.NET Core, você pode instalar o tempo de execução do .NET Core que não inclui suporte a ASP.NET Core: substitua `rh-dotnet31-aspnetcore-runtime-3.1` nos comandos acima por `rh-dotnet31-dotnet-runtime-3.1` .

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
