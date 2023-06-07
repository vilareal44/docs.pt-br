---
title: Instalar o .NET Framework no Windows 10
description: Saiba como instalar o .NET Framework no Windows 10 ou no Windows Server 2016.
ms.date: 04/18/2019
ms.openlocfilehash: 8bd54133f67395d0f110dd24e9e77521bbfe2bab
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88558771"
---
# <a name="install-the-net-framework-on-windows-10-and-windows-server-2016-and-later"></a>Instalar o .NET Framework no Windows 10 e no Windows Server 2016 e posterior

O .NET Framework é necessário para executar muitos aplicativos no Windows. As instruções neste artigo devem ajudá-lo a instalar as versões do .NET Framework que você precisa. O [.NET Framework 4.8](https://github.com/Microsoft/dotnet/tree/master/releases/net48) é a versão mais recente disponível.

Você pode ter chegado nesta página depois de tentar executar um aplicativo e ver uma caixa de diálogo em seu computador semelhante à seguinte:

![Não foi possível iniciar o aplicativo](./media/this-application-could-not-be-started.png)

## <a name="net-framework-48"></a>.NET Framework 4.8

O .NET Framework 4.8 está incluído na:

- [Atualização de maio de 2019 para Windows 10](https://support.microsoft.com/help/4028685/windows-10-get-the-update)

> [!div class="button"]
> [Baixar o .NET Framework 4.8](https://dotnet.microsoft.com/download/dotnet-framework/net48)

O [.NET Framework 4.8](https://dotnet.microsoft.com/download/dotnet-framework/net48) pode ser usado para executar aplicativos criados para as versões 4.0 a 4.7.2 do .NET Framework.

Instale o [.NET Framework 4.8](https://dotnet.microsoft.com/download/dotnet-framework/net48) na:

- Atualização de outubro de 2018 para Windows 10 (Versão 1809)
- Atualização de abril de 2018 para o Windows 10 (versão 1803)
- Windows 10 Fall Creators Update (versão 1709)
- No Windows 10 Creators Update (versão 1703)
- Na Atualização de Aniversário do Windows 10 (versão 1607)
- Windows Server 2019
- Windows Server, versão 1809
- Windows Server, versão 1803
- Windows Server 2016

O .NET Framework 4.8 não é compatível com:

- Windows 10 1507
- Com o Windows 10 1511

Se estiver usando o Windows 10 1507 ou 1511 e quiser instalar o .NET Framework 4.8, primeiro, você precisará atualizar para uma versão posterior do Windows 10.

## <a name="net-framework-462"></a>.NET Framework 4.6.2

O [.NET Framework 4.6.2](https://dotnet.microsoft.com/download/dotnet-framework/net462) é a versão mais recente do .NET Framework compatível com o Windows 10 1507 e o 1511.

O .NET Framework 4.6.2 é compatível com aplicativos compilados para as versões 4.0 até 4.6.2 do .NET Framework.

## <a name="net-framework-35"></a>.NET Framework 3.5

Siga as instruções para instalar o [.NET Framework 3.5 no Windows 10](dotnet-35-windows-10.md).

O .NET Framework 3.5 é compatível com aplicativos compilados para as versões 1.0 até 3.5 do .NET Framework.

## <a name="additional-information"></a>Informações adicionais

As versões 4.x do .NET Framework são atualizações in-loco de versões anteriores. Isso significa o seguinte:

- Você pode ter somente uma versão do .NET Framework 4. x instalada em seu computador.

- Você não poderá instalar uma versão anterior do .NET Framework em seu computador se uma versão mais recente já tiver sido instalada.

- As versões 4.x do .NET Framework podem ser usadas para executar aplicativos compilados para o .NET Framework 4.0 por meio dessa versão. Por exemplo, .NET Framework 4.7 pode ser usado para executar aplicativos compilados para as versões 4.0 até 4.7 do .NET Framework. A versão mais recente (.NET Framework 4.8) pode ser usada para executar aplicativos compilados para todas as versões do .NET Framework da 4.0 em diante.

Para obter uma lista de todas as versões do .NET Framework disponíveis para download, consulte a página [Downloads do .NET](https://dotnet.microsoft.com/download).

## <a name="help"></a>Ajuda

Se não for possível instalar a versão correta do .NET Framework, [entre em contato com a Microsoft para obter ajuda](mailto:dotnet-install-help@service.microsoft.com?subject=Install-Help).

## <a name="see-also"></a>Confira também

- [Downloads do .NET](https://dotnet.microsoft.com/download)
- [Solução de problemas de instalações e desinstalações bloqueadas do .NET Framework](troubleshoot-blocked-installations-and-uninstallations.md)
- [Instalar o .NET Framework para desenvolvedores](guide-for-developers.md)
- [Determinar quais versões do .NET Framework estão instaladas](../migration-guide/how-to-determine-which-versions-are-installed.md)
