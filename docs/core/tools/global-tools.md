---
title: " Ferramentas do .NET Core"
description: Como instalar, usar, atualizar e remover as ferramentas do .NET Core. Aborda ferramentas globais, ferramentas de caminho de ferramenta e ferramentas locais.
author: KathleenDollard
ms.topic: how-to
ms.date: 02/12/2020
ms.openlocfilehash: 00c0317fcfc4da0e7205c23faa7b355c20882ec9
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88062724"
---
# <a name="how-to-manage-net-core-tools"></a>Como gerenciar as ferramentas do .NET Core

**Este artigo aplica-se a:** ✔️ SDK do .net Core 2,1 e versões posteriores

Uma ferramenta .NET Core é um pacote NuGet especial que contém um aplicativo de console. Uma ferramenta pode ser instalada em seu computador das seguintes maneiras:

* Como uma ferramenta global.

  Os binários de ferramenta são instalados em um diretório padrão que é adicionado à variável de ambiente PATH. Você pode invocar a ferramenta de qualquer diretório no computador sem especificar seu local. Uma versão de uma ferramenta é usada para todos os diretórios no computador.

* Como uma ferramenta global em um local personalizado (também conhecido como ferramenta de caminho de ferramenta).

  Os binários de ferramenta são instalados em um local que você especificar. Você pode invocar a ferramenta no diretório de instalação ou fornecendo o diretório com o nome do comando ou adicionando o diretório à variável de ambiente PATH. Uma versão de uma ferramenta é usada para todos os diretórios no computador.

* Como uma ferramenta local (aplica-se a SDK do .NET Core 3,0 e posterior).

  Os binários de ferramenta são instalados em um diretório padrão. Você invoca a ferramenta no diretório de instalação ou em qualquer um de seus subdiretórios. Diretórios diferentes podem usar versões diferentes da mesma ferramenta.
  
  A CLI do .NET usa arquivos de manifesto para controlar quais ferramentas são instaladas como locais em um diretório. Quando o arquivo de manifesto é salvo no diretório raiz de um repositório de código-fonte, um colaborador pode clonar o repositório e invocar um único comando CLI do .NET Core que instala todas as ferramentas listadas nos arquivos de manifesto.

> [!IMPORTANT]
> As ferramentas do .NET Core são executadas com confiança total. Não instale uma ferramenta do .NET Core, a menos que você confie no autor.

## <a name="find-a-tool"></a>Encontrar uma ferramenta

Atualmente, o .NET Core não tem um recurso de pesquisa de ferramenta. Aqui estão algumas maneiras de encontrar ferramentas:

* Pesquise o site do [NuGet](https://www.nuget.org) usando o filtro de tipo de pacote "ferramenta .net". Para obter mais informações, consulte [Localizando e escolhendo pacotes](/nuget/consume-packages/finding-and-choosing-packages).
* Consulte a lista de ferramentas no repositório GitHub [natemcmaster/dotnet-Tools](https://github.com/natemcmaster/dotnet-tools) .
* Use [ToolGet](https://www.toolget.net/) para procurar ferramentas .net.
* Consulte o código-fonte para as ferramentas criadas pela equipe de ASP.NET Core no [diretório de ferramentas do repositório do GitHub dotnet/aspnetcore](https://github.com/dotnet/aspnetcore/tree/master/src/Tools).
* Saiba mais sobre as ferramentas de diagnóstico nas [ferramentas de diagnóstico do .NET Core dotnet](../diagnostics/index.md#net-core-dotnet-diagnostic-global-tools).

## <a name="check-the-author-and-statistics"></a>Verificar o autor e as estatísticas

Como as ferramentas do .NET Core são executadas com confiança total e as ferramentas globais são adicionadas à variável de ambiente PATH, elas podem ser muito poderosas. Não baixe ferramentas de pessoas em quem você não confia.

Se a ferramenta estiver hospedada no NuGet, você pode verificar o autor e as estatísticas pesquisando a ferramenta.

## <a name="install-a-global-tool"></a>Instalar uma ferramenta global

Para instalar uma ferramenta como uma ferramenta global, use a `-g` `--global` opção ou da [instalação da ferramenta dotnet](dotnet-tool-install.md), conforme mostrado no exemplo a seguir:

```dotnetcli
dotnet tool install -g dotnetsay
```

A saída mostra o comando usado para invocar a ferramenta e a versão instalada, semelhante ao exemplo a seguir:

```output
You can invoke the tool using the following command: dotnetsay
Tool 'dotnetsay' (version '2.1.4') was successfully installed.
```

O local padrão para os binários de uma ferramenta depende do sistema operacional:

| Sistema operacional          | Caminho                          |
|-------------|-------------------------------|
| Linux/macOS | `$HOME/.dotnet/tools`         |
| Windows     | `%USERPROFILE%\.dotnet\tools` |

Esse local é adicionado ao caminho do usuário quando o SDK é executado pela primeira vez, portanto, as ferramentas globais podem ser invocadas de qualquer diretório sem especificar o local da ferramenta.

O acesso à ferramenta é específico do usuário, não do computador global. Uma ferramenta global só está disponível para o usuário que instalou a ferramenta.

### <a name="install-a-global-tool-in-a-custom-location"></a>Instalar uma ferramenta global em um local personalizado

Para instalar uma ferramenta como uma ferramenta global em um local personalizado, use a `--tool-path` opção de [instalação da ferramenta dotnet](dotnet-tool-install.md), conforme mostrado nos exemplos a seguir.

No Windows:

```dotnetcli
dotnet tool install dotnetsay --tool-path c:\dotnet-tools
```

No Linux ou macOS:

```dotnetcli
dotnet tool install dotnetsay --tool-path ~/bin
```

O SDK do .NET Core não adiciona esse local automaticamente à variável de ambiente PATH. Para [invocar uma ferramenta de caminho de ferramenta](#invoke-a-tool-path-tool), você precisa certificar-se de que o comando está disponível usando um dos seguintes métodos:

* Adicione o diretório de instalação à variável de ambiente PATH.
* Especifique o caminho completo para a ferramenta ao chamá-lo.
* Invoque a ferramenta de dentro do diretório de instalação.

## <a name="install-a-local-tool"></a>Instalar uma ferramenta local

**Aplica-se ao SDK do .NET Core 3,0 e posterior.**

Para instalar uma ferramenta somente para acesso local (para o diretório e subdiretórios atuais), ela deve ser adicionada a um arquivo de manifesto da ferramenta. Para criar um arquivo de manifesto da ferramenta, execute o `dotnet new tool-manifest` comando:

```dotnetcli
dotnet new tool-manifest
```

Este comando cria um arquivo de manifesto chamado *dotnet-tools.jsno* diretório *. config* . Para adicionar uma ferramenta local ao arquivo de manifesto, use o comando de [instalação da ferramenta dotnet](dotnet-tool-install.md) e **omita** as `--global` `--tool-path` Opções e, conforme mostrado no exemplo a seguir:

```dotnetcli
dotnet tool install dotnetsay
```

A saída do comando mostra em qual arquivo de manifesto a ferramenta recém-instalada está, semelhante ao exemplo a seguir:

```console
You can invoke the tool from this directory using the following command:
dotnet tool run dotnetsay
Tool 'dotnetsay' (version '2.1.4') was successfully installed.
Entry is added to the manifest file /home/name/botsay/.config/dotnet-tools.json.
```

O exemplo a seguir mostra um arquivo de manifesto com duas ferramentas locais instaladas:

```json
{
  "version": 1,
  "isRoot": true,
  "tools": {
    "botsay": {
      "version": "1.0.0",
      "commands": [
        "botsay"
      ]
    },
    "dotnetsay": {
      "version": "2.1.3",
      "commands": [
        "dotnetsay"
      ]
    }
  }
}
```

Normalmente, você adiciona uma ferramenta local ao diretório raiz do repositório. Depois de fazer o check-in do arquivo de manifesto para o repositório, os desenvolvedores que confiram o código do repositório obtêm o arquivo de manifesto mais recente. Para instalar todas as ferramentas listadas no arquivo de manifesto, elas executam o `dotnet tool restore` comando:

```dotnetcli
dotnet tool restore
```

A saída indica quais ferramentas foram restauradas:

```console
Tool 'botsay' (version '1.0.0') was restored. Available commands: botsay
Tool 'dotnetsay' (version '2.1.3') was restored. Available commands: dotnetsay
Restore was successful.
```

## <a name="install-a-specific-tool-version"></a>Instalar uma versão específica da ferramenta

Para instalar uma versão de pré-lançamento ou uma versão específica de uma ferramenta, especifique o número de versão usando a `--version` opção, conforme mostrado no exemplo a seguir:

```dotnetcli
dotnet tool install dotnetsay --version 2.1.3
```

## <a name="use-a-tool"></a>Usar uma ferramenta

O comando que você usa para invocar uma ferramenta pode ser diferente do nome do pacote que você instalar. Para exibir todas as ferramentas atualmente instaladas no computador para o usuário atual, use o comando [dotnet da lista de ferramentas](dotnet-tool-list.md) :

```dotnetcli
dotnet tool list
```

A saída mostra a versão e o comando de cada ferramenta, semelhante ao exemplo a seguir:

```console
Package Id      Version      Commands       Manifest
-------------------------------------------------------------------------------------------
botsay          1.0.0        botsay         /home/name/repository/.config/dotnet-tools.json
dotnetsay       2.1.3        dotnetsay      /home/name/repository/.config/dotnet-tools.json
```

Conforme mostrado neste exemplo, a lista mostra as ferramentas locais. Para ver as ferramentas globais, use a `--global` opção e para ver as ferramentas de caminho de ferramenta, use a `--tool-path` opção.

### <a name="invoke-a-global-tool"></a>Invocar uma ferramenta global

Para ferramentas globais, use o comando de ferramenta por si só. Por exemplo, se o comando for `dotnetsay` ou `dotnet-doc` , é isso que você usa para invocar o comando:

```console
dotnetsay
dotnet-doc
```

Se o comando começar com o prefixo `dotnet-` , uma maneira alternativa de invocar a ferramenta é usar o `dotnet` comando e omitir o prefixo de comando da ferramenta. Por exemplo, se o comando for `dotnet-doc` , o comando a seguir invocará a ferramenta:

```dotnetcli
dotnet doc
```

No entanto, no cenário a seguir, você não pode usar o `dotnet` comando para invocar uma ferramenta global:

* Uma ferramenta global e uma ferramenta local têm o mesmo comando prefixado pelo `dotnet-` .
* Você deseja invocar a ferramenta global de um diretório que está no escopo da ferramenta local.

Nesse cenário, `dotnet doc` e `dotnet dotnet-doc` invoca a ferramenta local. Para invocar a ferramenta global, use o comando por si só:

```dotnetcli
dotnet-doc
```

### <a name="invoke-a-tool-path-tool"></a>Invocar uma ferramenta de caminho de ferramenta

Para invocar uma ferramenta global que é instalada usando a `tool-path` opção, verifique se o comando está disponível, conforme explicado [anteriormente neste artigo](#install-a-global-tool-in-a-custom-location).

### <a name="invoke-a-local-tool"></a>Invocar uma ferramenta local

Para invocar uma ferramenta local, você precisa usar o `dotnet` comando de dentro do diretório de instalação. Você pode usar a forma longa ( `dotnet tool run <COMMAND_NAME>` ) ou a forma abreviada ( `dotnet <COMMAND_NAME>` ), conforme mostrado nos exemplos a seguir:

```dotnetcli
dotnet tool run dotnetsay
dotnet dotnetsay
```

Se o comando for prefixado pelo `dotnet-` , você poderá incluir ou omitir o prefixo ao invocar a ferramenta. Por exemplo, se o comando for `dotnet-doc` , qualquer um dos seguintes exemplos invocará a ferramenta local:

```dotnetcli
dotnet tool run dotnet-doc
dotnet dotnet-doc
dotnet doc
```

## <a name="update-a-tool"></a>Atualizar uma ferramenta

A atualização de uma ferramenta envolve a desinstalação e a reinstalação com a versão estável mais recente. Para atualizar uma ferramenta, use o comando [dotnet ferramenta de atualização](dotnet-tool-update.md) com a mesma opção usada para instalar a ferramenta:

```dotnetcli
dotnet tool update --global <packagename>
dotnet tool update --tool-path <packagename>
dotnet tool update <packagename>
```

Para uma ferramenta local, o SDK encontra o primeiro arquivo de manifesto que contém a ID do pacote examinando o diretório atual e os diretórios pai. Se não houver nenhuma ID de pacote em nenhum arquivo de manifesto, o SDK adicionará uma nova entrada ao arquivo de manifesto mais próximo.

## <a name="uninstall-a-tool"></a>Desinstalar uma ferramenta

Remova uma ferramenta usando o comando [dotnet ferramenta de desinstalação](dotnet-tool-uninstall.md) com a mesma opção usada para instalar a ferramenta:

```dotnetcli
dotnet tool uninstall --global <packagename>
dotnet tool uninstall --tool-path <packagename>
dotnet tool uninstall <packagename>
```

Para uma ferramenta local, o SDK encontra o primeiro arquivo de manifesto que contém a ID do pacote examinando o diretório atual e os diretórios pai.

## <a name="get-help-and-troubleshoot"></a>Obter ajuda e solucionar problemas

Para obter uma lista de `dotnet tool` comandos disponíveis, digite o seguinte comando:

```dotnetcli
dotnet tool --help
```

Para obter instruções de uso da ferramenta, insira um dos comandos a seguir ou consulte o site da ferramenta:

```dotnetcli
<command> --help
dotnet <command> --help
```

Se uma ferramenta não for instalada ou executada, consulte [solucionar problemas de uso da ferramenta .NET Core](troubleshoot-usage-issues.md).

## <a name="see-also"></a>Consulte também

- [Tutorial: criar uma ferramenta do .NET Core usando o CLI do .NET Core](global-tools-how-to-create.md)
- [Tutorial: instalar e usar uma ferramenta global do .NET Core usando o CLI do .NET Core](global-tools-how-to-use.md)
- [Tutorial: instalar e usar uma ferramenta local do .NET Core usando o CLI do .NET Core](local-tools-how-to-use.md)
