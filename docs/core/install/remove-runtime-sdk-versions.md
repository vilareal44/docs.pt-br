---
title: Remover o SDK e o runtime do .NET Core
description: Este artigo descreve como determinar quais versões do runtime e do SDK do .NET Core estão instaladas e, em seguida, como removê-las no Windows, Mac e Linux.
author: adegeo
ms.author: adegeo
ms.date: 04/28/2020
zone_pivot_groups: operating-systems-set-one
ms.openlocfilehash: 1e4a846cf5e3d0209f5ade873520ef64abc12e7c
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85324650"
---
# <a name="how-to-remove-the-net-core-runtime-and-sdk"></a>Como remover o SDK e o Runtime do .NET Core

Ao longo do tempo, ao instalar as versões atualizadas do runtime e do SDK do .NET Core, você poderá desejar remover versões desatualizadas do .NET Core do seu computador. Remover versões mais antigas do runtime pode mudar o runtime escolhido para executar aplicativos de estrutura compartilhada, conforme detalhado no artigo [Seleção de versão do .NET Core](../versions/selection.md).

## <a name="should-i-remove-a-version"></a>Devo remover uma versão?

Os comportamentos de [seleção de versão do .NET Core](../versions/selection.md) e a compatibilidade do runtime do .NET Core com as atualizações permitem a remoção segura de versões anteriores. As atualizações de runtime do .NET core são compatíveis com uma “banda” de versão principal como 1.x e 2.x. Além disso, versões mais recentes do SDK do .NET Core geralmente mantêm a capacidade de compilar aplicativos destinados a versões anteriores do runtime de uma maneira compatível.

Em geral, você precisa apenas do SDK mais recente e da versão de patch mais recente dos runtimes necessários para seu aplicativo. As instâncias em que você talvez queira manter versões mais antigas do SDK ou *do*Runtime incluem manterproject.jsaplicativos baseados em. A menos que seu aplicativo tenha motivos específicos para runtimes ou SDKs anteriores, você pode remover com segurança as versões mais antigas.

## <a name="determine-what-is-installed"></a>Determinar o que está instalado

Começando com o .NET Core 2.1, a CLI do .NET tem as opções que você pode usar para listar as versões do SDK e do runtime que estão instaladas em seu computador.  Use [`dotnet --list-sdks`](../tools/dotnet.md#options) para ver a lista de SDKs instalados em seu computador. Use [`dotnet --list-runtimes`](../tools/dotnet.md#options) para ver a lista de tempos de execução instalados em seu computador. Para obter mais informações, consulte [como verificar se o .NET Core já está instalado](how-to-detect-installed-versions.md).

## <a name="uninstall-net-core"></a>Desinstalar o .NET Core

::: zone pivot="os-windows"

O .NET Core usa a caixa de diálogo **recursos de & de aplicativos** do Windows para remover versões do SDK e do tempo de execução do .NET Core. A figura a seguir mostra a caixa de diálogo **aplicativos & recursos** . Você pode pesquisar o **SDK do core** para filtrar e mostrar as versões instaladas do .NET Core.

![Adicionar ou remover programas para remover o .NET Core](./media/remove-runtime-sdk-versions/programs-and-features.png)

Selecione todas as versões que você deseja remover do seu computador e clique em **Desinstalar**.

::: zone-end

::: zone pivot="os-linux"

Há mais opções para desinstalar o .NET Core (SDK ou runtime) no Linux. A melhor maneira para desinstalar o .NET Core é espelhar a ação que você usou para instalar o .NET Core. As especificações dependem de sua distribuição escolhida e do método de instalação.

> [!IMPORTANT]
> Para instalações do Red Hat, consulte o [Guia de introdução do Red Hat](https://access.redhat.com/documentation/en-us/net_core/2.0/html/getting_started_guide/gs_install_dotnet#install_register_rehel) para obter informações sobre como instalar e desinstalar o .NET Core.

A partir do .NET Core 2,1, não é necessário desinstalar o SDK do .NET Core ao atualizá-lo usando um Gerenciador de pacotes. Os comandos `update` ou `refresh` do gerenciador de pacotes removerão automaticamente a versão mais antiga após a instalação bem-sucedida de uma versão mais recente.

Se instalou o .NET Core usando um gerenciador de pacotes, você usará esse mesmo gerenciador de pacotes para desinstalar o SDK ou o runtime do .NET. As instalações do .NET core dão suporte a gerenciadores de pacotes mais populares. Consulte a documentação do Gerenciador de pacotes de distribuição para obter a sintaxe precisa em seu ambiente:

- [apt-get(8)](https://linux.die.net/man/8/apt-get) é usado por sistemas baseados em Debian, incluindo o Ubuntu.
- [yum(8)](https://linux.die.net/man/8/yum) é usado no Fedora, CentOS e Oracle Linux.
- [zypper(8)](https://en.opensuse.org/SDB:Zypper_manual_(plain)) é usado no openSUSE e no SLES (SUSE Linux Enterprise System).
- [dnf(8)](https://dnf.readthedocs.io/en/latest/command_ref.html) é usado no Fedora.

Em quase todos os casos, o comando para remover um pacote é `remove`.

O nome do pacote para a instalação do SDK do .NET Core para a maioria dos gerenciadores de pacotes é `dotnet-sdk`, seguido pelo número de versão. Começando com a versão 2.1.300 do SDK do .NET Core e a versão `2.1` do runtime, somente os números de versão principal e secundária são necessários: por exemplo, o SDK do .NET Core versão 2.1.300 pode ser referenciado como o pacote `dotnet-sdk-2.1`. As versões anteriores exigem a cadeia de caracteres de versão inteira: por exemplo, `dotnet-sdk-2.1.200` seria necessária para a versão 2.1.200 do SDK do .NET Core.

Para computadores apenas com o runtime instalado, e não o SDK, o nome do pacote é `dotnet-runtime-<version>`, para o runtime do .NET Core, e `aspnetcore-runtime-<version>`, para a pilha em runtime inteira.

As instalações do .NET Core anteriores a 2,0 não desinstalaram o aplicativo host quando o SDK foi desinstalado usando o Gerenciador de pacotes. Usando `apt-get`, o comando é:

```bash
apt-get remove dotnet-host
```

Observe que não há nenhuma versão anexada a `dotnet-host` .

Se instalou usando um tarball, você deverá remover o .NET Core usando o método manual.

No Linux, você deve remover os SDKs e tempos de execução separadamente, removendo os diretórios com controle de versão. Removê-los exclui o SDK e o tempo de execução do disco. Por exemplo, para remover o SDK 1.0.1 e o runtime, você usaria os seguintes comandos de bash:

```bash
version="1.0.1"
sudo rm -rf /usr/local/share/dotnet/sdk/$version
sudo rm -rf /usr/local/share/dotnet/shared/Microsoft.NETCore.App/$version
sudo rm -rf /usr/local/share/dotnet/shared/Microsoft.AspNetCore.All/$version
sudo rm -rf /usr/local/share/dotnet/shared/Microsoft.AspNetCore.App/$version
sudo rm -rf /usr/local/share/dotnet/host/fxr/$version
```

Os diretórios pais para o SDK e o runtime são listados na saída dos comandos `dotnet --list-sdks` e `dotnet --list-runtimes`, conforme mostrado na tabela anterior.

::: zone-end

::: zone pivot="os-macos"

No Mac, você deve remover os SDKs e tempos de execução separadamente, removendo os diretórios com controle de versão. Removê-los exclui o SDK e o tempo de execução do disco. Por exemplo, para remover o SDK 1.0.1 e o runtime, você usaria os seguintes comandos de bash:

```bash
version="1.0.1"
sudo rm -rf /usr/local/share/dotnet/sdk/$version
sudo rm -rf /usr/local/share/dotnet/shared/Microsoft.NETCore.App/$version
sudo rm -rf /usr/local/share/dotnet/shared/Microsoft.AspNetCore.All/$version
sudo rm -rf /usr/local/share/dotnet/shared/Microsoft.AspNetCore.App/$version
sudo rm -rf /usr/local/share/dotnet/host/fxr/$version
```

Os diretórios pais para o SDK e o runtime são listados na saída dos comandos `dotnet --list-sdks` e `dotnet --list-runtimes`, conforme mostrado na tabela anterior.

::: zone-end

## <a name="net-core-uninstall-tool"></a>Ferramenta de Desinstalação do .NET Core

A [ferramenta de desinstalação do .NET Core](../additional-tools/uninstall-tool.md) ( `dotnet-core-uninstall` ) permite que você remova os SDKs do .NET Core e os tempos de execução de um sistema. Uma coleção de opções está disponível para especificar quais versões devem ser desinstaladas.

## <a name="visual-studio-dependency-on-net-core-sdk-versions"></a>Dependência do Visual Studio em versões de SDK do .NET Core

Antes do Visual Studio 2019 versão 16,3, os instaladores do Visual Studio chamaram o instalador de SDK do .NET Core autônomo. Como resultado, as versões do SDK são exibidas na caixa de diálogo **recursos de & de aplicativos** do Windows. Remover SDKs do .NET Core que foram instalados pelo Visual Studio usando o instalador autônomo pode interromper o Visual Studio. Se o Visual Studio tiver problemas depois de desinstalar os SDKs, execute o reparo nessa versão específica do Visual Studio. A tabela a seguir mostra algumas das dependências do Visual Studio em SDK do .NET Core versões:

| Versão do Visual Studio           | Versão do SDK do .NET Core          |
|---------------------------------|--------------------------------|
| Visual Studio 2019 versão 16.2 | SDK do .NET Core 2.2.4 XX, 2.1.8 XX |
| Visual Studio 2019 versão 16.1 | SDK do .NET Core 2.2.3 XX, 2.1.7 XX |
| Visual Studio 2019 versão 16,0 | SDK do .NET Core 2.2.2 XX, 2.1.6 XX |
| Visual Studio 2017 versão 15,9 | SDK do .NET Core 2.2.1 XX, 2.1.5 XX |
| Visual Studio 2017 versão 15.8 | SDK do .NET Core 2.1.4 XX          |

A partir do Visual Studio 2019 versão 16,3, o Visual Studio é responsável por sua própria cópia do SDK do .NET Core. Por esse motivo, você não vê mais essas versões do SDK na caixa de diálogo **aplicativos & recursos** .

## <a name="remove-the-nuget-fallback-folder"></a>Remover a pasta de fallback do NuGet

Antes do SDK do .NET Core 3,0, os instaladores de SDK do .NET Core usaram uma pasta chamada *NuGetFallbackFolder* para armazenar um cache de pacotes NuGet. Esse cache foi usado durante operações como `dotnet restore` ou `dotnet build /t:Restore` . O *NuGetFallbackFolder* está localizado em *C:\Program Files\dotnet\sdk* no Windows e em */usr/local/share/dotnet/SDK* no MacOS.

Talvez você queira remover essa pasta, se:

- Você está desenvolvendo apenas usando o SDK do .NET Core 3,0 ou versões posteriores.
- Você está desenvolvendo usando versões SDK do .NET Core anteriores a 3,0, mas pode trabalhar online.

Se você quiser remover a pasta de fallback do NuGet, poderá excluí-la, mas precisará de privilégios de administrador para fazer isso.

Não é recomendável excluir a pasta *dotnet* . Isso removeria todas as ferramentas globais que você instalou anteriormente. Além disso, no Windows:

- Você interromperá o Visual Studio 2019 versão 16,3 e versões posteriores. Você pode executar o **reparo** para recuperar.
- Se houver SDK do .NET Core entradas na caixa de diálogo **aplicativos & recursos** , eles ficarão órfãos.
