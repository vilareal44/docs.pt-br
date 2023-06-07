---
title: Comando dotnet add package
description: O comando 'dotnet add package' fornece uma opção conveniente para adicionar uma referência de pacote NuGet a um projeto.
ms.date: 02/14/2020
ms.openlocfilehash: bc79fe8adf5f775ddce62f3877a8de945c6a18ab
ms.sourcegitcommit: e5772b3ddcc114c80b4c9767ffdb3f6c7fad8f05
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83840891"
---
# <a name="dotnet-add-package"></a>dotnet add package

**Este artigo aplica-se a:** ✔️ SDK do .NET Core 2. x e versões posteriores

## <a name="name"></a>Nome

`dotnet add package` – adiciona uma referência de pacote a um arquivo de projeto.

## <a name="synopsis"></a>Sinopse

```dotnetcli
dotnet add [<PROJECT>] package <PACKAGE_NAME>
    [-f|--framework <FRAMEWORK>] [--interactive]
    [-n|--no-restore] [--package-directory <PACKAGE_DIRECTORY>]
    [-s|--source <SOURCE>] [-v|--version <VERSION>]

dotnet add package -h|--help
```

## <a name="description"></a>Descrição

O comando `dotnet add package` fornece uma opção conveniente para adicionar uma referência de pacote a um arquivo de projeto. Depois de executar o comando, há uma verificação de compatibilidade para garantir que o pacote seja compatível com as estruturas do projeto. Se for aprovado na verificação, um elemento `<PackageReference>` será adicionado ao arquivo de projeto e [dotnet restore](dotnet-restore.md) será executada.

Por exemplo, adicionar `Newtonsoft.Json` a *ToDo.csproj* produz uma saída semelhante ao exemplo a seguir:

```console
Writing C:\Users\me\AppData\Local\Temp\tmp95A8.tmp
info : Adding PackageReference for package 'Newtonsoft.Json' into project 'C:\projects\ToDo\ToDo.csproj'.
log  : Restoring packages for C:\Temp\projects\consoleproj\consoleproj.csproj...
info :   GET https://api.nuget.org/v3-flatcontainer/newtonsoft.json/index.json
info :   OK https://api.nuget.org/v3-flatcontainer/newtonsoft.json/index.json 79ms
info :   GET https://api.nuget.org/v3-flatcontainer/newtonsoft.json/12.0.1/newtonsoft.json.12.0.1.nupkg
info :   OK https://api.nuget.org/v3-flatcontainer/newtonsoft.json/12.0.1/newtonsoft.json.12.0.1.nupkg 232ms
log  : Installing Newtonsoft.Json 12.0.1.
info : Package 'Newtonsoft.Json' is compatible with all the specified frameworks in project 'C:\projects\ToDo\ToDo.csproj'.
info : PackageReference for package 'Newtonsoft.Json' version '12.0.1' added to file 'C:\projects\ToDo\ToDo.csproj'.
```

O arquivo *ToDo.csproj* agora contém um elemento [`<PackageReference>`](/nuget/consume-packages/package-references-in-project-files) para o pacote referenciado.

```xml
<PackageReference Include="Newtonsoft.Json" Version="12.0.1" />
```

### <a name="implicit-restore"></a>Restauração implícita

[!INCLUDE[DotNet Restore Note](../../../includes/dotnet-restore-note.md)]

## <a name="arguments"></a>Argumentos

- **`PROJECT`**

  Especifica o arquivo do projeto. Se não for especificado, o comando pesquisará um no diretório atual.

- **`PACKAGE_NAME`**

  A referência de pacote a ser adicionada.

## <a name="options"></a>Opções

- **`-f|--framework <FRAMEWORK>`**

  Adiciona uma referência de pacote somente quando há uma [estrutura](../../standard/frameworks.md) específica como destino.

- **`-h|--help`**

  Imprime uma ajuda breve para o comando.

- **`--interactive`**

  Permite que o comando pare e aguarde a entrada ou uma ação do usuário (por exemplo, para concluir a autenticação). Disponível desde o SDK 2.1 do .NET Core, versão 2.1.400 ou posterior.

- **`-n|--no-restore`**

  Adiciona uma referência de pacote sem executar a visualização de restauração e a verificação de compatibilidade.

- **`--package-directory <PACKAGE_DIRECTORY>`**

  O diretório no qual restaurar os pacotes. O local de restauração de pacote padrão é `%userprofile%\.nuget\packages` no Windows e `~/.nuget/packages` no macOS e Linux. Para obter mais informações, consulte [Como gerenciar as pastas de pacotes globais, de cache e temporárias no NuGet](https://docs.microsoft.com/nuget/consume-packages/managing-the-global-packages-and-cache-folders).

- **`-s|--source <SOURCE>`**

  O URI da origem do pacote NuGet a ser usado durante a operação de restauração.

- **`-v|--version <VERSION>`**

  Versão do pacote. Consulte [Controle de versão do pacote NuGet](https://docs.microsoft.com/nuget/reference/package-versioning).

## <a name="examples"></a>Exemplos

- Adicionar um pacote NuGet `Newtonsoft.Json` a um projeto:

  ```dotnetcli
  dotnet add package Newtonsoft.Json
  ```

- Adicionar uma versão específica de um pacote a um projeto:

  ```dotnetcli
  dotnet add ToDo.csproj package Microsoft.Azure.DocumentDB.Core -v 1.0.0
  ```

- Adicionar um pacote usando uma fonte específica do NuGet:

  ```dotnetcli
  dotnet add package Microsoft.AspNetCore.StaticFiles -s https://dotnet.myget.org/F/dotnet-core/api/v3/index.json
  ```

## <a name="see-also"></a>Consulte também

- [Como gerenciar as pastas de pacotes globais, de cache e temporárias no NuGet](https://docs.microsoft.com/nuget/consume-packages/managing-the-global-packages-and-cache-folders)
- [Controle de versão do pacote NuGet](https://docs.microsoft.com/nuget/reference/package-versioning)
