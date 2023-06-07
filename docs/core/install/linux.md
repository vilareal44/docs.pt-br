---
title: Instalar o .NET Core em distribuições do Linux
description: Saiba mais sobre o que as distribuições do Linux dão suporte à instalação do .NET Core no Linux.
author: adegeo
ms.author: adegeo
ms.date: 06/01/2020
ms.openlocfilehash: 368ee879e5ab9027bd37c3ced12ed377276de507
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86863767"
---
# <a name="install-net-core-on-linux"></a>Instalar o .NET Core no Linux

> [!div class="op_single_selector"]
>
> - [Instalar no Windows](windows.md)
> - [Instalar no macOS](macos.md)
> - [Instalar no Linux](linux.md)

O .NET Core está disponível em diferentes distribuições do Linux. A maioria das plataformas e distribuições do Linux tem uma versão principal a cada ano, e a maioria fornece um Gerenciador de pacotes usado para instalar o .NET Core. Este artigo descreve o que tem suporte no momento e qual Gerenciador de pacotes é usado.

O restante deste artigo é uma análise de cada grande distribuição do Linux com suporte do .NET Core. Todas as versões do .NET Core permanecem com suporte até que a versão do [.NET Core atinja o fim do suporte](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) ou a distribuição do Linux atinja o fim da vida útil.

Para obter a melhor compatibilidade, escolha uma versão de LTS (versão de longo prazo).

## <a name="unsupported-releases"></a>Versões sem suporte

Não há mais suporte para as seguintes versões do .NET Core ❌ . Os downloads para eles ainda permanecem publicados:

- 3.0
- 2.2
- 2.0

Essas versões sem suporte não são detalhadas nas seções abaixo e sua quilometragem pode variar se você tentar instalá-las.

## <a name="alpine"></a>Alpine

Não há instaladores para o Alpine. Você deve usar o [script de instalação](linux-alpine.md#scripted-install) ou seguir as instruções de [instalação manual](linux-alpine.md#manual-install) .

A tabela a seguir é uma lista de versões do .NET Core com suporte no momento e as versões do Alpine para as quais têm suporte. Essas versões permanecem com suporte até que a versão do [.NET Core atinja o fim do suporte](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) ou a versão do [Alpine alcance o fim da vida útil](https://wiki.alpinelinux.org/wiki/Alpine_Linux:Releases).

- Um ✔️ indica que a versão do Alpine ou .NET Core ainda tem suporte.
- Um ❌ indica que a versão do Alpine ou .NET Core não tem suporte nessa versão do Alpine.
- Quando uma versão do Alpine e uma versão do .NET Core têm ✔️, há suporte para essa combinação de so e .NET.

| Alpine                      | .NET Core 2.1 | .NET Core 3.1 | Versão prévia do .NET 5 |
|-----------------------------|---------------|---------------|----------------|
| ✔️ [3,12](linux-alpine.md)  | ✔️ 2,1        | ✔️ 3,1        | versão prévia do ✔️ 5,0 |
| ✔️ [3,11](linux-alpine.md)  | ✔️ 2,1        | ✔️ 3,1        | versão prévia do ✔️ 5,0 |
| ✔️ [3,10](linux-alpine.md)  | ✔️ 2,1        | ✔️ 3,1        | versão prévia do ✔️ 5,0 |
| ✔️ [3,9](linux-alpine.md)   | ✔️ 2,1        | ✔️ 3,1        | versão prévia do ✔️ 5,0 |
| ❌ [3.8](linux-alpine.md)   | ✔️ 2,1        | ❌3,1        | ❌visualização de 5,0 |

Para obter mais informações, consulte [instalar o .NET Core no Alpine Ski](linux-alpine.md).

## <a name="centos"></a>CentOS

O CentOS 7 usa o yum como um Gerenciador de pacotes e o CentOS 8 usa o DNF.

A tabela a seguir é uma lista de versões do .NET Core com suporte atualmente no CentOS 7 e no CentOS 8. Essas versões permanecem com suporte até que a versão do [.NET Core atinja o fim do suporte](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) ou a versão do CentOS não seja mais suportada.

| CentOS                   | .NET Core 2.1 | .NET Core 3.1 | Versão prévia do .NET 5 (somente instalação manual) |
|--------------------------|---------------|---------------|----------------|
| ✔️ [8](linux-centos.md#centos-8-) | ✔️ 2,1        | ✔️ 3,1        | versão prévia do ✔️ 5,0 |
| ✔️ [7](linux-centos.md#centos-7-) | ✔️ 2,1        | ✔️ 3,1        | versão prévia do ✔️ 5,0 |

Para obter mais informações, consulte [instalar o .NET Core no CentOS](linux-centos.md).

## <a name="debian"></a>Debian

Debian usa APT (ferramenta de pacote avançado) como um Gerenciador de pacotes.

A tabela a seguir é uma lista de versões do .NET Core com suporte no momento e as versões do Debian nas quais elas têm suporte. Essas versões permanecem com suporte até que a versão do [.NET Core atinja o fim do suporte](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) ou a versão do [Debian atinja o fim da vida útil](https://wiki.debian.org/DebianReleases).

- Um ✔️ indica que a versão do Debian ou do .NET Core ainda tem suporte.
- Um ❌ indica que a versão do Debian ou do .NET Core não tem suporte nessa versão do Debian.
- Quando uma versão do Debian e uma versão do .NET Core têm ✔️, há suporte para essa combinação de so e .NET.

| Debian                   | .NET Core 2.1 | .NET Core 3.1 | Versão prévia do .NET 5 (somente instalação manual) |
|--------------------------|---------------|---------------|----------------|
| ✔️ [10](linux-debian.md#debian-10-)     | ✔️ 2,1        | ✔️ 3,1        | versão prévia do ✔️ 5,0 |
| ✔️ [9](linux-debian.md#debian-9-)       | ✔️ 2,1        | ✔️ 3,1        | versão prévia do ✔️ 5,0 |
| ❌ [8](linux-debian.md#debian-8-)       | ✔️ 2,1        | ❌3,1        | ❌visualização de 5,0 |

Para obter mais informações, consulte [instalar o .NET Core no Debian](linux-debian.md).

## <a name="fedora"></a>Fedora

Fedora usa DNF como seu Gerenciador de pacotes.

A tabela a seguir é uma lista de versões do .NET Core com suporte no momento e as versões do Fedora em que têm suporte. Essas versões permanecem com suporte até que a versão do [.NET Core atinja o fim do suporte](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) ou a versão do [Fedora atinja o fim da vida útil](https://fedoraproject.org/wiki/End_of_life).

- Um ✔️ indica que a versão do Fedora ou do .NET Core ainda tem suporte.
- Um ❌ indica que a versão do Fedora ou do .NET Core não tem suporte nessa versão do Fedora.
- Quando uma versão do Fedora e uma versão do .NET Core têm ✔️, há suporte para essa combinação de so e .NET.

| Fedora                   | .NET Core 2.1 | .NET Core 3.1 | Versão prévia do .NET 5 (somente instalação manual) |
|--------------------------|---------------|---------------|----------------|
| ✔️ [32](linux-fedora.md#fedora-32-) | ✔️ 2,1        | ✔️ 3,1        | versão prévia do ✔️ 5,0 |
| ✔️ [31](linux-fedora.md#fedora-31-) | ✔️ 2,1        | ✔️ 3,1        | versão prévia do ✔️ 5,0 |
| ❌[30](linux-fedora.md#fedora-30-) | ✔️ 2,1        | ✔️ 3,1        | ❌visualização de 5,0 |
| ❌[29](linux-fedora.md#fedora-29-) | ✔️ 2,1        | ✔️ 3,1        | ❌visualização de 5,0 |
| ❌[28](linux-fedora.md#fedora-28-) | ✔️ 2,1        | ❌3,1        | ❌visualização de 5,0 |
| ❌[27](linux-fedora.md#fedora-27-) | ✔️ 2,1        | ❌3,1        | ❌visualização de 5,0 |

Para obter mais informações, consulte [instalar o .NET Core em Fedora](linux-fedora.md).

## <a name="opensuse"></a>openSUSE

openSUSE usa zypper como o Gerenciador de pacotes.

A tabela a seguir é uma lista de versões do .NET Core com suporte no momento no openSUSE 15. Essas versões permanecem com suporte até que a versão do [.NET Core atinja o fim do suporte](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) ou a versão do openSUSE não seja mais suportada.

- Um ✔️ indica que a versão do openSUSE ou do .NET Core ainda tem suporte.
- Um ❌ indica que a versão do openSUSE ou do .NET Core não tem suporte nessa versão do openSUSE.
- Quando uma versão do openSUSE e uma versão do .NET Core têm ✔️, há suporte para essa combinação de so e .NET.

| openSUSE                   | .NET Core 2.1 | .NET Core 3.1 | Versão prévia do .NET 5 (somente instalação manual) |
|----------------------------|---------------|---------------|----------------|
| ✔️ [15](linux-opensuse.md#opensuse-15-)     | ✔️ 2,1        | ✔️ 3,1        | versão prévia do ✔️ 5,0 |

Para obter mais informações, consulte [instalar o .NET Core em openSUSE](linux-opensuse.md).

## <a name="red-hat"></a>Red Hat

Red Hat Enterprise Linux (RHEL) usa yum (RHEL 7) e DNF (RHEL 8) como o Gerenciador de pacotes.

A tabela a seguir é uma lista de versões do .NET Core com suporte no momento no RHEL 7 e RHEL 8. Essas versões permanecem com suporte até que a versão do [.NET Core atinja o fim do suporte](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) ou a versão do RHEL não seja mais suportada.

- Um ✔️ indica que a versão do RHEL ou do .NET Core ainda tem suporte.
- Um ❌ indica que a versão do RHEL ou do .NET Core não tem suporte nessa versão do RHEL.
- Quando uma versão do RHEL e uma versão do .NET Core têm ✔️, há suporte para essa combinação de so e .NET.

| RHEL                   | .NET Core 2.1 | .NET Core 3.1 | Versão prévia do .NET 5 (somente instalação manual) |
|--------------------------|---------------|---------------|----------------|
| ✔️ [8](linux-rhel.md#rhel-8-) | ✔️ 2,1        | ✔️ 3,1        | versão prévia do ✔️ 5,0 |
| ✔️ [7](linux-rhel.md#rhel-7-) | ✔️ 2,1        | ✔️ 3,1        | versão prévia do ✔️ 5,0 |

Para obter mais informações, consulte [instalar o .NET Core no RHEL](linux-rhel.md).

## <a name="sles"></a>SLES

O SLES usa zypper como o Gerenciador de pacotes.

A tabela a seguir é uma lista de versões do .NET Core com suporte no momento no SLES 12 SP2 e no SLES 15. Essas versões permanecem com suporte até que a versão do [.NET Core atinja o fim do suporte](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) ou a versão do SLES não seja mais suportada.

- Um ✔️ indica que a versão do SLES ou do .NET Core ainda tem suporte.
- Um ❌ indica que a versão do SLES ou do .NET Core não tem suporte nessa versão do SLES.
- Quando uma versão do SLES e uma versão do .NET Core têm ✔️, há suporte para essa combinação de so e .NET.

| SLES                   | .NET Core 2.1 | .NET Core 3.1 | Versão prévia do .NET 5 (somente instalação manual) |
|------------------------|---------------|---------------|----------------|
| ✔️ [15](linux-sles.md#sles-15-)     | ✔️ 2,1        | ✔️ 3,1        | versão prévia do ✔️ 5,0 |
| ✔️ [12 SP2](linux-sles.md#sles-12-) | ✔️ 2,1        | ✔️ 3,1        | versão prévia do ✔️ 5,0 |

Para obter mais informações, consulte [instalar o .NET Core no SLES](linux-sles.md).

## <a name="ubuntu"></a>Ubuntu

O Ubuntu usa a APT (ferramenta de pacote avançado) como um Gerenciador de pacotes.

A tabela a seguir representa o status de suporte do Ubuntu e do .NET Core.

- Um ✔️ indica que a versão do Ubuntu ou do .NET Core ainda tem suporte.
- Um ❌ indica que a versão do Ubuntu ou do .NET Core não tem suporte nessa versão do Ubuntu.
- Quando uma versão do Ubuntu e uma versão do .NET Core têm ✔️, há suporte para essa combinação de so e .NET.

| Ubuntu                   | .NET Core 2.1 | .NET Core 3.1 | Versão prévia do .NET 5 (somente instalação manual) |
|--------------------------|---------------|---------------|----------------|
| ✔️ [20, 4 (LTS)](linux-ubuntu.md#2004-) | ✔️ 2,1        | ✔️ 3,1        | versão prévia do ✔️ 5,0 |
| ❌[19,10](linux-ubuntu.md#1910-)       | ✔️ 2,1        | ✔️ 3,1        | versão prévia do ✔️ 5,0 |
| ❌[19, 4](linux-ubuntu.md#1904-)       | ✔️ 2,1        | ✔️ 3,1        | ❌visualização de 5,0 |
| ❌[18,10](linux-ubuntu.md#1810-)       | ✔️ 2,1        | ❌3,1        | ❌visualização de 5,0 |
| ✔️ [18, 4 (LTS)](linux-ubuntu.md#1804-) | ✔️ 2,1        | ✔️ 3,1        | versão prévia do ✔️ 5,0 |
| ❌[17,10](linux-ubuntu.md#1710-)       | ✔️ 2,1        | ❌3,1        | ❌visualização de 5,0 |
| ❌ [17.04](linux-ubuntu.md#1704-)       | ✔️ 2,1        | ❌3,1        | ❌visualização de 5,0 |
| ❌[16,10](linux-ubuntu.md#1610-)       | ❌2,1        | ❌3,1        | ❌visualização de 5,0 |
| ✔️ [16, 4 (LTS)](linux-ubuntu.md#1604-) | ✔️ 2,1        | ✔️ 3,1        | versão prévia do ✔️ 5,0 |

Para obter mais informações, consulte [instalar o .NET Core no Ubuntu](linux-ubuntu.md).

## <a name="next-steps"></a>Próximas etapas

- [Como verificar se o .NET Core já está instalado](how-to-detect-installed-versions.md?pivots=os-linux).
- [Tutorial: criar um novo aplicativo com Visual Studio Code](../tutorials/with-visual-studio-code.md).
- [Tutorial: colocar em contêiner um aplicativo .NET Core](../docker/build-container.md).
