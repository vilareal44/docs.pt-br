---
title: Instalar e gerenciar modelos de SDK-.NET Core
description: Saiba como instalar modelos do .NET Core no Windows, Linux e macOS.
author: adegeo
ms.author: adegeo
ms.date: 04/24/2020
zone_pivot_groups: operating-systems-set-one
no-loc:
- dotnet new
- dotnet nuget add source
ms.openlocfilehash: 09acae1409eb0492be10bd3a61b14da5be57c6c7
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85324492"
---
# <a name="manage-net-project-and-item-templates"></a>Gerenciar modelos de projeto e item do .NET

O .NET Core fornece um sistema de modelo que permite aos usuários instalar ou desinstalar modelos do NuGet, um arquivo de pacote NuGet ou um diretório do sistema de arquivos. Este artigo descreve como gerenciar modelos do .NET Core por meio da CLI do SDK do .NET Core.

Para obter mais informações sobre como criar modelos, consulte [tutorial: criar modelos](../tutorials/cli-templates-create-item-template.md).

## <a name="install-template"></a>Instalar modelo

Os modelos são instalados por meio do [dotnet new](../tools/dotnet-new.md) comando do SDK com o `-i` parâmetro. Você pode fornecer o identificador de pacote NuGet de um modelo ou uma pasta que contém os arquivos de modelo.

### <a name="nuget-hosted-package"></a>Pacote hospedado do NuGet

Os modelos da CLI do .NET são carregados no [NuGet](https://www.nuget.org/) para ampla distribuição. Os modelos também podem ser instalados de um feed particular. Em vez de carregar um modelo em um feed do NuGet, os arquivos de modelo *nupkg* podem ser distribuídos e instalados manualmente, conforme descrito na seção [pacote do NuGet local](#local-nuget-package) .

Para obter mais informações sobre a configuração de feeds do NuGet, consulte [dotnet nuget add source](../tools/dotnet-nuget-add-source.md) .

Para instalar um pacote de modelos do feed do NuGet padrão, use o `dotnet new -i {package-id}` comando:

```dotnetcli
dotnet new -i Microsoft.DotNet.Web.Spa.ProjectTemplates
```

Para instalar um pacote de modelos do feed do NuGet padrão com uma versão específica, use o `dotnet new -i {package-id}::{version}` comando:

```dotnetcli
dotnet new -i Microsoft.DotNet.Web.Spa.ProjectTemplates::2.2.6
```

### <a name="local-nuget-package"></a>Pacote NuGet local

Quando um pacote de modelos é criado, um arquivo *nupkg* é gerado. Se você tiver um arquivo *nupkg* contendo modelos, poderá instalá-lo com o `dotnet new -i {path-to-package}` comando:

::: zone pivot="os-windows"

```dotnetcli
dotnet new -i c:\code\nuget-packages\Some.Templates.1.0.0.nupkg
```

::: zone-end

::: zone pivot="os-linux,os-macos"

```dotnetcli
dotnet new -i ~/code/nuget-packages/Some.Templates.1.0.0.nupkg
```

::: zone-end

### <a name="folder"></a>Pasta

Como alternativa à instalação do modelo a partir de um arquivo *nupkg* , você também pode instalar modelos de uma pasta diretamente com o `dotnet new -i {folder-path}` comando. A pasta especificada é tratada como o identificador do pacote de modelos para qualquer modelo encontrado. Qualquer modelo encontrado na hierarquia da pasta especificada é instalado.

::: zone pivot="os-windows"

```dotnetcli
dotnet new -i c:\code\nuget-packages\some-folder\
```

::: zone-end

::: zone pivot="os-linux,os-macos"

```dotnetcli
dotnet new -i ~/code/nuget-packages/some-folder/
```

::: zone-end

O `{folder-path}` especificado no comando torna-se o identificador do pacote de modelos para todos os modelos encontrados. Conforme especificado na seção [modelos de lista](#list-templates) , você pode obter uma lista de modelos instalados com o `dotnet new -u` comando. Neste exemplo, o identificador do pacote de modelos é mostrado como a pasta usada para instalação:

::: zone pivot="os-windows"

```console
dotnet new -u
Template Instantiation Commands for .NET Core CLI

Currently installed items:

... cut to save space ...

  c:\code\nuget-packages\some-folder
    Templates:
      A Template Console Class (templateconsole) C#
      Project for some technology (contosoproject) C#
    Uninstall Command:
      dotnet new -u c:\code\nuget-packages\some-folder
```

::: zone-end

::: zone pivot="os-linux,os-macos"

```console
dotnet new -u
Template Instantiation Commands for .NET Core CLI

Currently installed items:

... cut to save space ...

  /home/username/code/templates
    Templates:
      A Template Console Class (templateconsole) C#
      Project for some technology (contosoproject) C#
    Uninstall Command:
      dotnet new -u /home/username/code/templates
```

::: zone-end

## <a name="uninstall-template"></a>Desinstalar modelo

Os modelos são desinstalados por meio do [dotnet new](../tools/dotnet-new.md) comando do SDK com o `-u` parâmetro. Você pode fornecer o identificador de pacote NuGet de um modelo ou uma pasta que contém os arquivos de modelo.

### <a name="nuget-package"></a>Pacote NuGet

Depois que um pacote de modelos NuGet é instalado, de um feed NuGet ou de um arquivo *nupkg* , você pode desinstalá-lo referenciando o identificador do pacote NuGet.

Para desinstalar um pacote de modelos, use o `dotnet new -u {package-id}` comando:

```dotnetcli
dotnet new -u Microsoft.DotNet.Web.Spa.ProjectTemplates
```

### <a name="folder"></a>Pasta

Quando os modelos são instalados por meio de um [caminho de pasta](#folder), o caminho da pasta se torna o identificador do pacote de modelos.

Para desinstalar um pacote de modelos, use o `dotnet new -u {package-folder-path}` comando:

::: zone pivot="os-windows"

```dotnetcli
dotnet new -u c:\code\nuget-packages\some-folder
```

::: zone-end

::: zone pivot="os-linux,os-macos"

```dotnetcli
dotnet new -u /home/username/code/templates
```

::: zone-end

## <a name="list-templates"></a>Listar modelos

Usando o comando de desinstalação padrão sem um identificador de pacote, você pode ver uma lista de modelos instalados junto com o comando que desinstala cada modelo.

```console
dotnet new -u
Template Instantiation Commands for .NET Core CLI

Currently installed items:

... cut to save space ...

  c:\code\nuget-packages\some-folder
    Templates:
      A Template Console Class (templateconsole) C#
      Project for some technology (contosoproject) C#
    Uninstall Command:
      dotnet new -u c:\code\nuget-packages\some-folder
```

## <a name="install-templates-from-other-sdks"></a>Instalar modelos de outros SDKs

Se você instalou cada versão do SDK em sequência, por exemplo, instalou o SDK 2,0, o SDK 2,1 e assim por diante, terá todos os modelos do SDK instalados. No entanto, se você começar com uma versão mais recente do SDK, como 3,1, somente os modelos para [versões LTS (suporte a longo prazo)](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) serão incluídos, que no momento da versão do SDK 3,1 é SDK 2,1 e SDK 3,1. Os modelos para qualquer outra versão não estão incluídos.

Os modelos do .NET Core estão disponíveis no NuGet e você pode instalá-los como qualquer outro modelo. Para obter mais informações, consulte [instalar o pacote hospedado do NuGet](#nuget-hosted-package).

| .              | Identificador do pacote NuGet                                                                                                      |
|------------------|-------------------------------------------------------------------------------------------------------------------------------|
| .NET Core 2.1    | [`Microsoft.DotNet.Common.ProjectTemplates.2.1`](https://www.nuget.org/packages/Microsoft.DotNet.Common.ProjectTemplates.2.1) |
| .NET Core 2.2    | [`Microsoft.DotNet.Common.ProjectTemplates.2.2`](https://www.nuget.org/packages/Microsoft.DotNet.Common.ProjectTemplates.2.2) |
| .NET Core 3.0    | [`Microsoft.DotNet.Common.ProjectTemplates.3.0`](https://www.nuget.org/packages/Microsoft.DotNet.Common.ProjectTemplates.3.0) |
| .NET Core 3.1    | [`Microsoft.DotNet.Common.ProjectTemplates.3.1`](https://www.nuget.org/packages/Microsoft.DotNet.Common.ProjectTemplates.3.1) |
| ASP.NET Core 2.1 | [`Microsoft.DotNet.Web.ProjectTemplates.2.1`](https://www.nuget.org/packages/Microsoft.DotNet.Web.ProjectTemplates.2.1)       |
| ASP.NET Core 2,2 | [`Microsoft.DotNet.Web.ProjectTemplates.2.2`](https://www.nuget.org/packages/Microsoft.DotNet.Web.ProjectTemplates.2.2)       |
| ASP.NET Core 3,0 | [`Microsoft.DotNet.Web.ProjectTemplates.3.0`](https://www.nuget.org/packages/Microsoft.DotNet.Web.ProjectTemplates.3.0)       |
| ASP.NET Core 3,1 | [`Microsoft.DotNet.Web.ProjectTemplates.3.1`](https://www.nuget.org/packages/Microsoft.DotNet.Web.ProjectTemplates.3.1)       |

Por exemplo, a SDK do .NET Core inclui modelos para um aplicativo de console destinado ao .NET Core 2,1 e ao .NET Core 3,1. Se você quisesse direcionar o .NET Core 3,0, precisaria instalar os modelos de 3,0.

01. Tente criar um aplicativo que tenha como destino o .NET Core 3,0.

    ```dotnetcli
    dotnet new console --framework netcoreapp3.0
    ```

    Se você vir uma mensagem de erro, precisará instalar os modelos.

    > Não foi possível encontrar um modelo instalado que corresponda à entrada, pesquisando online para um que faça isso...

01. Instale os modelos de projeto do .NET Core 3,0.

    ```dotnetcli
    dotnet new -i Microsoft.DotNet.Common.ProjectTemplates.3.0
    ```

01. Tente criar o aplicativo uma segunda vez.

    ```dotnetcli
    dotnet new console --framework netcoreapp3.0
    ```

    E você deverá ver uma mensagem indicando que o projeto foi criado.

    > O modelo "aplicativo de console" foi criado com êxito.
    >
    > Processando ações de pós-criação... Executando ' dotnet restore ' em path-to-Project-File. csproj... Determinando os projetos a serem restaurados... Restauração concluída em 1, 5 s para Path-to-Project-File. csproj.
    >
    > Restauração bem-sucedida.

## <a name="see-also"></a>Veja também

- [Tutorial: criar um modelo de item](../tutorials/cli-templates-create-item-template.md)
- [dotnet new](../tools/dotnet-new.md)
- [dotnet nuget add source](../tools/dotnet-nuget-add-source.md)
