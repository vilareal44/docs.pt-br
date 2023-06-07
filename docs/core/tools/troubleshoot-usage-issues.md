---
title: Solucionar problemas de uso da ferramenta .NET Core
description: Descubra os problemas comuns ao executar as ferramentas do .NET Core e as possíveis soluções.
author: kdollard
ms.topic: troubleshooting
ms.date: 02/14/2020
ms.openlocfilehash: db88958e1605fef589c5dbcb12065a6318183705
ms.sourcegitcommit: cbb19e56d48cf88375d35d0c27554d4722761e0d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88608311"
---
# <a name="troubleshoot-net-core-tool-usage-issues"></a>Solucionar problemas de uso da ferramenta .NET Core

Você pode encontrar problemas ao tentar instalar ou executar uma ferramenta .NET Core, que pode ser uma ferramenta global ou uma ferramenta local. Este artigo descreve as causas raiz comuns e algumas soluções possíveis.

## <a name="installed-net-core-tool-fails-to-run"></a>Falha na execução da ferramenta .NET Core instalada

Quando uma ferramenta do .NET Core não é executada, é muito provável que você tenha executado um dos seguintes problemas:

* O arquivo executável da ferramenta não foi encontrado.
* A versão correta do tempo de execução do .NET Core não foi encontrada.

### <a name="executable-file-not-found"></a>Arquivo executável não encontrado

Se o arquivo executável não for encontrado, você verá uma mensagem semelhante à seguinte:

```console
Could not execute because the specified command or file was not found.
Possible reasons for this include:
  * You misspelled a built-in dotnet command.
  * You intended to execute a .NET Core program, but dotnet-xyz does not exist.
  * You intended to run a global tool, but a dotnet-prefixed executable with this name could not be found on the PATH.
```

O nome do executável determina como você invoca a ferramenta. A tabela a seguir descreve o formato:

| Formato do nome do executável  | Formato de invocação   |
|-------------------------|---------------------|
| `dotnet-<toolName>.exe` | `dotnet <toolName>` |
| `<toolName>.exe`        | `<toolName>`        |

* Ferramentas globais

  As ferramentas globais podem ser instaladas no diretório padrão ou em um local específico. Os diretórios padrão são:

  | Sistema operacional          | Caminho                          |
  |-------------|-------------------------------|
  | Linux/macOS | `$HOME/.dotnet/tools`         |
  | Windows     | `%USERPROFILE%\.dotnet\tools` |

  Se você estiver tentando executar uma ferramenta global, verifique se a `PATH` variável de ambiente em seu computador contém o caminho em que você instalou a ferramenta global e se o executável está nesse caminho.

  O CLI do .NET Core tenta adicionar o local padrão à variável de ambiente PATH em seu primeiro uso. No entanto, há alguns cenários em que o local pode não ser adicionado automaticamente ao caminho:

  * Se você estiver usando o Linux e tiver instalado o SDK do .NET Core usando arquivos *. tar. gz* e não apt-get ou RPM.
  * Se você estiver usando o macOS 10,15 "Catalina" ou versões posteriores.
  * Se você estiver usando o macOS 10,14 "Mojave" ou versões anteriores, e tiver instalado o SDK do .NET Core usando arquivos *. tar. gz* e não *. pkg*.
  * Se você instalou o SDK do .NET Core 3,0 e definiu a `DOTNET_ADD_GLOBAL_TOOLS_TO_PATH` variável de ambiente como `false` .
  * Se você instalou o SDK do .NET Core 2,2 ou versões anteriores, e definiu a `DOTNET_SKIP_FIRST_TIME_EXPERIENCE` variável de ambiente como `true` .

  Nesses cenários ou se você tiver especificado a `--tool-path` opção, a `PATH` variável de ambiente em seu computador não conterá automaticamente o caminho onde você instalou a ferramenta global. Nesse caso, acrescente o local da ferramenta (por exemplo, `$HOME/.dotnet/tools` ) à `PATH` variável de ambiente usando qualquer método que o seu Shell fornece para atualizar as variáveis de ambiente. Para obter mais informações, consulte [Ferramentas do .NET Core](global-tools.md).

* Ferramentas locais

  Se você estiver tentando executar uma ferramenta local, verifique se há um arquivo de manifesto chamado *dotnet-tools.js* no diretório atual ou em qualquer um de seus diretórios pai. Esse arquivo também pode residir em uma pasta chamada *. config* em qualquer lugar na hierarquia de pastas do projeto, em vez da pasta raiz. Se *dotnet-tools.js* houver, abra-o e verifique a ferramenta que você está tentando executar. Se o arquivo não contiver uma entrada para `"isRoot": true` , verifique também a hierarquia de arquivos para arquivos de manifesto da ferramenta adicional.

  Se você estiver tentando executar uma ferramenta do .NET Core que foi instalada com um caminho especificado, precisará incluir esse caminho ao usar a ferramenta. Um exemplo de como usar uma ferramenta instalada por caminho de ferramenta é:

  ```console
  ..\<toolDirectory>\dotnet-<toolName>
  ```

### <a name="runtime-not-found"></a>Tempo de execução não encontrado

As ferramentas do .NET Core são [aplicativos dependentes da estrutura](../deploying/index.md#publish-framework-dependent), o que significa que eles dependem de um tempo de execução do .NET Core instalado em seu computador. Se o tempo de execução esperado não for encontrado, siga as regras normais de roll-forward do tempo de execução do .NET Core, como:

* Um aplicativo efetua roll forward até a versão de patch mais recente da versão principal e secundária especificadas.
* Se não houver nenhum tempo de execução correspondente com um número de versão principal e secundária correspondente, a próxima versão secundária mais alta será usada.
* O roll forward não ocorre entre versões prévias do runtime ou entre versões prévias e versões de lançamento. Portanto, as ferramentas do .NET Core criadas usando versões prévias devem ser recriadas e republicadas pelo autor e reinstaladas.

O roll-forward não ocorrerá por padrão em dois cenários comuns:

* Somente versões inferiores do tempo de execução estão disponíveis. O roll-forward só seleciona versões posteriores do tempo de execução.
* Somente as versões principais mais altas do tempo de execução estão disponíveis. Roll forward não cruza os limites de versão principais.

Se um aplicativo não conseguir encontrar um tempo de execução apropriado, ele não executará e relatará um erro.

Você pode descobrir quais tempos de execução do .NET Core estão instalados em seu computador usando um dos seguintes comandos:

```dotnetcli
dotnet --list-runtimes
dotnet --info
```

Se você considerar que a ferramenta deve dar suporte à versão de tempo de execução instalada atualmente, entre em contato com o autor da ferramenta e veja se eles podem atualizar o número de versão ou vários destinos. Depois de recompilar e republicar seu pacote de ferramentas no NuGet com um número de versão atualizado, você pode atualizar sua cópia. Embora isso não aconteça, a solução mais rápida para você é instalar uma versão do tempo de execução que funcionaria com a ferramenta que você está tentando executar. Para baixar uma versão específica do tempo de execução do .NET Core, visite a [página de download do .NET Core](https://dotnet.microsoft.com/download/dotnet-core).

Se você instalar o SDK do .NET Core em um local não padrão, precisará definir a variável de ambiente `DOTNET_ROOT` para o diretório que contém o `dotnet` executável.

## <a name="net-core-tool-installation-fails"></a>Falha na instalação da ferramenta .NET Core

Há vários motivos pelos quais a instalação de uma ferramenta global ou local do .NET Core pode falhar. Quando a instalação da ferramenta falhar, você verá uma mensagem semelhante à seguinte:

```console
Tool '{0}' failed to install. This failure may have been caused by:

* You are attempting to install a preview release and did not use the --version option to specify the version.
* A package by this name was found, but it was not a .NET Core tool.
* The required NuGet feed cannot be accessed, perhaps because of an Internet connection problem.
* You mistyped the name of the tool.

For more reasons, including package naming enforcement, visit https://aka.ms/failure-installing-tool
```

Para ajudar a diagnosticar essas falhas, as mensagens do NuGet são mostradas diretamente para o usuário, juntamente com a mensagem anterior. A mensagem do NuGet pode ajudá-lo a identificar o problema.

### <a name="package-naming-enforcement"></a>Imposição de nomenclatura de pacote

A Microsoft alterou suas diretrizes sobre a ID do pacote para ferramentas, resultando em várias ferramentas não encontradas com o nome previsto. A nova diretriz é que todas as ferramentas da Microsoft sejam prefixadas com "Microsoft". Esse prefixo é reservado e só pode ser usado para pacotes assinados com um certificado autorizado da Microsoft.

Durante a transição, algumas ferramentas da Microsoft terão a forma antiga da ID do pacote, enquanto outras terão o novo formulário:

```dotnetcli
dotnet tool install -g Microsoft.<toolName>
dotnet tool install -g <toolName>
```

Como as IDs de pacote são atualizadas, você precisará alterar para a nova ID do pacote para obter as atualizações mais recentes. Os pacotes com o nome da ferramenta simplificada serão preteridos.

### <a name="preview-releases"></a>Versões de visualização

* Você está tentando instalar uma versão prévia e não usou a `--version` opção para especificar a versão.

As ferramentas do .NET Core que estão em visualização devem ser especificadas com uma parte do nome para indicar que estão na versão prévia. Você não precisa incluir a visualização inteira. Supondo que os números de versão estejam no formato esperado, você pode usar algo semelhante ao exemplo a seguir:

```dotnetcli
dotnet tool install -g --version 1.1.0-pre <toolName>
```

### <a name="package-isnt-a-net-core-tool"></a>O pacote não é uma ferramenta .NET Core

* Um pacote NuGet com esse nome foi encontrado, mas não era uma ferramenta .NET Core.

Se você tentar instalar um pacote NuGet que é um pacote NuGet regular e não uma ferramenta .NET Core, verá um erro semelhante ao seguinte:

> NU1212: combinação de pacote de projeto inválida para `<ToolName>` . O estilo de projeto DotnetToolReference só pode conter referências do tipo DotnetTool.

### <a name="nuget-feed-cant-be-accessed"></a>O feed do NuGet não pode ser acessado

* O feed NuGet necessário não pode ser acessado, talvez devido a um problema de conexão com a Internet.

A instalação da ferramenta requer acesso ao feed do NuGet que contém o pacote de ferramentas. Ele falhará se o feed não estiver disponível. Você pode alterar os feeds com `nuget.config` o, solicitar um `nuget.config` arquivo específico ou especificar Feeds adicionais com a `--add-source` opção. Por padrão, o NuGet gera um erro para qualquer feed que não pode se conectar. O sinalizador `--ignore-failed-sources` pode ignorar essas fontes não acessíveis.

### <a name="package-id-incorrect"></a>ID de pacote incorreta

* Você digitou indigitadamente o nome da ferramenta.

Um motivo comum para a falha é que o nome da ferramenta não está correto. Isso pode ocorrer devido a digitação incorreta ou porque a ferramenta foi movida ou foi preterida. Para ferramentas no NuGet.org, uma maneira de garantir que você tenha o nome correto é Pesquisar a ferramenta em NuGet.org e copiar o comando de instalação.

## <a name="see-also"></a>Confira também

* [Ferramentas do .NET Core](global-tools.md)
