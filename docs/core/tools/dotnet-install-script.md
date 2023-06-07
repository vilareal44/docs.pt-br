---
title: Scripts dotnet-install
description: Saiba mais sobre os scripts dotnet-install para instalar o SDK do .NET Core e o tempo de execução compartilhado.
ms.date: 04/30/2020
ms.openlocfilehash: c3aa6549a0b521db7fc19c6ff44665e3c4ba0c5f
ms.sourcegitcommit: 1e6439ec4d5889fc08cf3bfb4dac2b91931eb827
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/08/2020
ms.locfileid: "88024648"
---
# <a name="dotnet-install-scripts-reference"></a>referência de scripts dotnet-install

## <a name="name"></a>Nome

`dotnet-install.ps1` | `dotnet-install.sh`-Script usado para instalar o SDK do .NET Core e o tempo de execução compartilhado.

## <a name="synopsis"></a>Sinopse

Windows:

```powershell
dotnet-install.ps1 [-Architecture <ARCHITECTURE>] [-AzureFeed]
    [-Channel <CHANNEL>] [-DryRun] [-FeedCredential]
    [-InstallDir <DIRECTORY>] [-JSonFile <JSONFILE>]
    [-NoCdn] [-NoPath] [-ProxyAddress] [-ProxyBypassList <LIST_OF_URLS>]
    [-ProxyUseDefaultCredentials] [-Runtime <RUNTIME>]
    [-SkipNonVersionedFiles] [-UncachedFeed] [-Verbose]
    [-Version <VERSION>]

Get-Help ./dotnet-install.ps1
```

Linux/macOS:

```bash
dotnet-install.sh  [--architecture <ARCHITECTURE>] [--azure-feed]
    [--channel <CHANNEL>] [--dry-run] [--feed-credential]
    [--install-dir <DIRECTORY>] [--jsonfile <JSONFILE>]
    [--no-cdn] [--no-path] [--runtime <RUNTIME>] [--runtime-id <RID>]
    [--skip-non-versioned-files] [--uncached-feed] [--verbose]
    [--version <VERSION>]

dotnet-install.sh --help
```

O script de bash também lê comutadores do PowerShell. Portanto, você pode usar comutadores do PowerShell com o script nos sistemas Linux/macOS.

## <a name="description"></a>Descrição

Os `dotnet-install` scripts executam uma instalação não administrativa do SDK do .NET Core, que inclui o CLI do .NET Core e o tempo de execução compartilhado. Há dois scripts:

* Um script do PowerShell que funciona no Windows.
* Um script bash que funciona no Linux/macOS.

### <a name="purpose"></a>Finalidade

 O uso pretendido dos scripts é para cenários de CI (integração contínua), em que:

* O SDK precisa ser instalado sem interação do usuário e sem direitos de administrador.
* A instalação do SDK não precisa persistir em várias execuções de CI.

  A sequência típica de eventos:
  * CI é disparado.
  * O CI instala o SDK usando um desses scripts.
  * O CI conclui seu trabalho e limpa os dados temporários, incluindo a instalação do SDK.

Para configurar um ambiente de desenvolvimento ou executar aplicativos, use os instaladores em vez desses scripts.

### <a name="recommended-version"></a>Versão recomendada

Recomendamos que você use a versão estável dos scripts:

- Bash (Linux/macOS):<https://dot.net/v1/dotnet-install.sh>
- PowerShell (Windows):<https://dot.net/v1/dotnet-install.ps1>

### <a name="script-behavior"></a>Comportamento do script

Ambos os scripts têm o mesmo comportamento. Eles baixam o arquivo ZIP/tarball da CLI Build Descartes e prosseguem para instalá-lo no local padrão ou em um local especificado por `-InstallDir|--install-dir` .

Por padrão, os scripts de instalação baixam o SDK e o instalam. Se você quiser obter somente o runtime compartilhado, especifique o argumento `-Runtime|--runtime`.

Por padrão, o script adiciona o local de instalação ao $PATH da sessão atual. Substitua esse comportamento padrão especificando o argumento `-NoPath|--no-path`. O script não define a `DOTNET_ROOT` variável de ambiente.

Antes de executar o script, instale as [dependências](../install/windows.md#dependencies) necessárias.

Você pode instalar uma versão específica usando o argumento `-Version|--version`. A versão deve ser especificada como um número de versão de três partes, como `2.1.0` . Se a versão não for especificada, o script instalará a `latest` versão.

Os scripts de instalação não atualizam o registro no Windows. Eles apenas baixam os binários zipados e os copiam para uma pasta. Se você quiser que os valores de chave do registro sejam atualizados, use os instaladores do .NET Core.

## <a name="options"></a>Opções

- **`-Architecture|--architecture <ARCHITECTURE>`**

  Arquitetura dos binários do .NET Core para instalação. Os valores possíveis são `<auto>`, `amd64`, `x64`, `x86`, `arm64` e `arm`. O valor padrão é `<auto>`, que representa a arquitetura do sistema operacional em execução no momento.

- **`-AzureFeed|--azure-feed`**

  Especifica a URL para o feed do Azure para o instalador. Não recomendamos alterar esse valor. O valor padrão é `https://dotnetcli.azureedge.net/dotnet`.

- **`-Channel|--channel <CHANNEL>`**

  Especifica o canal de origem da instalação. Os valores possíveis são:

  - `Current`: versão mais atual.
  - `LTS`: canal de suporte de longo prazo (versão mais atual compatível).
  - Versão de duas partes no formato X.Y que representa uma versão específica (por exemplo, `2.1` ou `3.0`).
  - Nome do Branch: por exemplo, `release/3.1.1xx` ou `master` (para versões noturnas). Use esta opção para instalar uma versão de um canal de visualização. Use o nome do canal, conforme listado em [instaladores e binários](https://github.com/dotnet/core-sdk#installers-and-binaries).

  O valor padrão é `LTS`. Para saber mais sobre os canais de suporte do .NET, consulte a página [Política de suporte do .NET](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).

- **`-DryRun|--dry-run`**

  Se definido, o script não executará a instalação. Em vez disso, exibirá qual linha de comando deve ser usada para instalar de forma consistente a versão atualmente solicitada da CLI do .NET Core. Por exemplo, se você especificar a versão `latest`, será exibido um link com a versão específica, para que este comando possa ser usado de forma determinista em um script de build. Ele também exibirá o local dos binários, caso você prefira instalá-lo ou baixá-lo por conta própria.

- **`-FeedCredential|--feed-credential`**

  Usado como uma cadeia de caracteres de consulta para acrescentar ao feed do Azure. Permite alterar a URL para usar contas de armazenamento de blobs não públicos.

- **`--help`**

  Imprime a ajuda do script. Aplica-se somente ao script bash. Para o PowerShell, use `Get-Help ./dotnet-install.ps1` .

- **`-InstallDir|--install-dir <DIRECTORY>`**

  Especifica o caminho da instalação. O diretório será criado se não existir. O valor padrão é *%LocalAppData%\Microsoft\dotnet* no Windows e no */usr/share/dotnet* no linux/MacOS. Os binários são colocados diretamente nesse diretório.

- **`-JSonFile|--jsonfile <JSONFILE>`**

  Especifica um caminho para um [global.jsno](global-json.md) arquivo que será usado para determinar a versão do SDK. O *global.jsno* arquivo deve ter um valor para `sdk:version` .

- **`-NoCdn|--no-cdn`**

  Desabilita o download da [Rede de Distribuição de Conteúdo (CDN) Azure](https://docs.microsoft.com/azure/cdn/cdn-overview) e usa o feed não armazenado em cache diretamente.

- **`-NoPath|--no-path`**

  Se definida, a pasta de instalação não será exportada para o caminho da sessão atual. Por padrão, o script modifica o caminho, o que torna o CLI do .NET Core disponível imediatamente após a instalação.

- **`-ProxyAddress`**

  Se for definido, o instalador usará o proxy ao fazer solicitações da Web. (Válido somente para Windows.)

- **`-ProxyBypassList <LIST_OF_URLS>`**

  Se definido com `ProxyAddress` , fornece uma lista de URLs separadas por vírgula que ignorarão o proxy. (Válido somente para Windows.)

- **`ProxyUseDefaultCredentials`**

  Se definido, o instalador usará as credenciais do usuário atual ao usar o endereço de proxy. (Válido somente para Windows.)

- **`-Runtime|--runtime <RUNTIME>`**

  Instala apenas o runtime compartilhado, não todo o SDK. Os valores possíveis são:

  - `dotnet`: o runtime compartilhado `Microsoft.NETCore.App`.
  - `aspnetcore`: o runtime compartilhado `Microsoft.AspNetCore.App`.
  - `windowsdesktop`: o runtime compartilhado `Microsoft.WindowsDesktop.App`.

- **`--runtime-id <RID>`**

  Especifica o [identificador de tempo de execução](../rid-catalog.md) para o qual as ferramentas estão sendo instaladas. Use `linux-x64` para Linux portátil. (Válido somente para Linux/macOS.)

- **`-SharedRuntime|--shared-runtime`**

  > [!NOTE]
  > Esse parâmetro está obsoleto e pode ser removido em uma versão futura do script. A alternativa recomendada é a opção `-Runtime|--runtime`.

  Instala apenas os bits de runtime compartilhado, não todo o SDK. Essa opção é equivalente a especificar `-Runtime|--runtime dotnet` .

- **`-SkipNonVersionedFiles|--skip-non-versioned-files`**

  Ignora a instalação de arquivos sem controle de versão, como *dotnet.exe*, se já existirem.

- **`-UncachedFeed|--uncached-feed`**

  Permite alterar a URL do feed não armazenado em cache usado por esse instalador. Não recomendamos alterar esse valor.

- **`-Verbose|--verbose`**

  Exibe informações de diagnóstico.

- **`-Version|--version <VERSION>`**

  Representa uma versão específica do build. Os valores possíveis são:

  - `latest`: build mais recente no canal (usado com a opção `-Channel`).
  - `coherent`: build coerente mais recente no canal; usa a combinação de pacotes estáveis mais recente (usado com as opções `-Channel` do nome do Branch).
  - Versão de três partes no formato X.Y.Z que representa uma determinada versão do build; substitui a opção `-Channel`. Por exemplo: `2.0.0-preview2-006120`.

  Se não for especificada, a `-Version` assumirá o padrão `latest`.

## <a name="examples"></a>Exemplos

- Instale a versão LTS (suportada a longo prazo) no local padrão:

  Windows:

  ```powershell
  ./dotnet-install.ps1 -Channel LTS
  ```

  macOS/Linux:

  ```bash
  ./dotnet-install.sh --channel LTS
  ```

- Instale a versão mais recente do canal 3,1 no local especificado:

  Windows:

  ```powershell
  ./dotnet-install.ps1 -Channel 3.1 -InstallDir C:\cli
  ```

  macOS/Linux:

  ```bash
  ./dotnet-install.sh --channel 3.1 --install-dir ~/cli
  ```

- Instale a versão 3.0.0 do tempo de execução compartilhado:

  Windows:

  ```powershell
  ./dotnet-install.ps1 -Runtime dotnet -Version 3.0.0
  ```

  macOS/Linux:

  ```bash
  ./dotnet-install.sh --runtime dotnet --version 3.0.0
  ```

- Obtenha o script e instale a versão 2.1.2 atrás de um proxy corporativo (somente Windows):

  ```powershell
  Invoke-WebRequest 'https://dot.net/v1/dotnet-install.ps1' -Proxy $env:HTTP_PROXY -ProxyUseDefaultCredentials -OutFile 'dotnet-install.ps1';
  ./dotnet-install.ps1 -InstallDir '~/.dotnet' -Version '2.1.2' -ProxyAddress $env:HTTP_PROXY -ProxyUseDefaultCredentials;
  ```

- Obtenha o script e instale exemplos de uma linha da CLI do .NET Core:

  Windows:

  ```powershell
  # Run a separate PowerShell process because the script calls exit, so it will end the current PowerShell session.
  &powershell -NoProfile -ExecutionPolicy unrestricted -Command "[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12; &([scriptblock]::Create((Invoke-WebRequest -UseBasicParsing 'https://dot.net/v1/dotnet-install.ps1'))) <additional install-script args>"
  ```

  macOS/Linux:

  ```bash
  curl -sSL https://dot.net/v1/dotnet-install.sh | bash /dev/stdin <additional install-script args>
  ```

## <a name="see-also"></a>Consulte também

- [Versões do .NET Core](https://github.com/dotnet/core/releases)
- [Arquivo de download de runtime e SDK do .NET Core](https://github.com/dotnet/core/blob/master/release-notes/download-archive.md)
