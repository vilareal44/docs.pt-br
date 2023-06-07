---
title: Comando dotnet publish
description: O comando dotnet publish publica um projeto do .NET Core ou uma solução em um diretório.
ms.date: 02/24/2020
ms.openlocfilehash: 45bf8504fd882286041794d27ecb56464fc8d13d
ms.sourcegitcommit: c4a15c6c4ecbb8a46ad4e67d9b3ab9b8b031d849
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88656659"
---
# <a name="dotnet-publish"></a>dotnet publish

**Este artigo aplica-se a:** ✔️ SDK do .net Core 2,1 e versões posteriores

## <a name="name"></a>Nome

`dotnet publish` – Publica o aplicativo e suas dependências em uma pasta para implantação em um sistema de hospedagem.

## <a name="synopsis"></a>Sinopse

```dotnetcli
dotnet publish [<PROJECT>|<SOLUTION>] [-c|--configuration <CONFIGURATION>]
    [-f|--framework <FRAMEWORK>] [--force] [--interactive]
    [--manifest <PATH_TO_MANIFEST_FILE>] [--no-build] [--no-dependencies]
    [--no-restore] [--nologo] [-o|--output <OUTPUT_DIRECTORY>]
    [-p:PublishReadyToRun=true] [-p:PublishSingleFile=true] [-p:PublishTrimmed=true]
    [-r|--runtime <RUNTIME_IDENTIFIER>] [--self-contained [true|false]]
    [--no-self-contained] [-v|--verbosity <LEVEL>]
    [--version-suffix <VERSION_SUFFIX>]

dotnet publish -h|--help
```

## <a name="description"></a>Descrição

`dotnet publish` compila o aplicativo, lê suas dependências especificadas no arquivo de projeto e publica o conjunto de arquivos resultantes em um diretório. A saída inclui os seguintes ativos:

- Código IL (Linguagem Intermediária) em um assembly com uma extensão *dll*.
- Um *.deps.jsno* arquivo que inclui todas as dependências do projeto.
- Um *.runtimeconfig.jsno* arquivo que especifica o tempo de execução compartilhado que o aplicativo espera, bem como outras opções de configuração para o tempo de execução (por exemplo, tipo de coleta de lixo).
- As dependências do aplicativo, que são copiadas do cache NuGet para a pasta de saída.

A saída do comando `dotnet publish` está pronta para implantação em um sistema de hospedagem (por exemplo, um servidor, um computador, um Mac, um laptop) para execução. É a única maneira com suporte oficial de preparar o aplicativo para implantação. Dependendo do tipo de implantação especificado pelo projeto, talvez o sistema de hospedagem não tenha o runtime compartilhado do .NET Core instalado. Para obter mais informações, consulte [publicar aplicativos .NET Core com o CLI do .NET Core](../deploying/deploy-with-cli.md).

### <a name="implicit-restore"></a>Restauração implícita

[!INCLUDE[dotnet restore note](~/includes/dotnet-restore-note.md)]

### <a name="msbuild"></a>MSBuild

O comando `dotnet publish` chama MSBuild, que invoca o destino `Publish`. Quaisquer parâmetros passados para `dotnet publish` são passados para o MSBuild. Os parâmetros `-c` e `-o` mapeiam as propriedades `Configuration` e `PublishDir` do MSBuild, respectivamente.

O `dotnet publish` comando aceita opções do MSBuild, como `-p` para configurar propriedades e `-l` definir um agente de log. Por exemplo, você pode definir uma propriedade do MSBuild usando o formato: `-p:<NAME>=<VALUE>` .

Você também pode definir propriedades relacionadas à publicação fazendo referência a um arquivo *. pubxml* (disponível desde o SDK do .net Core 3,1). Por exemplo:

```dotnetcli
dotnet publish -p:PublishProfile=FolderProfile
```

O exemplo anterior usa o arquivo *FolderProfile. pubxml* que é encontrado na pasta * \<project_folder> /Properties/PublishProfiles* . Se você especificar um caminho e uma extensão de arquivo ao definir a `PublishProfile` propriedade, eles serão ignorados. Por padrão, o MSBuild procura na pasta *Properties/PublishProfiles* e assume a extensão de arquivo *pubxml* . Para especificar o caminho e o nome de arquivo, incluindo a extensão, defina a `PublishProfileFullPath` propriedade em vez da `PublishProfile` propriedade.

Para saber mais, consulte os recursos a seguir:

- [Referência de linha de comando do MSBuild](/visualstudio/msbuild/msbuild-command-line-reference)
- [Perfis de publicação do Visual Studio (. pubxml) para implantação de aplicativo ASP.NET Core](/aspnet/core/host-and-deploy/visual-studio-publish-profiles)
- [dotnet msbuild](dotnet-msbuild.md)

## <a name="arguments"></a>Argumentos

- **`PROJECT|SOLUTION`**

  O projeto ou solução a ser publicada.
  
  * `PROJECT` é o caminho e o nome de arquivo de um Visual Basic de projeto em [c#](csproj.md), f # ou Visual Basic, ou o caminho para um diretório que contém um arquivo de projeto C#, f # ou. Se o diretório não for especificado, o padrão será o diretório atual.

  * `SOLUTION` é o caminho e o nome de arquivo de uma solução (extensão *. sln* ) ou o caminho para um diretório que contém um arquivo de solução. Se o diretório não for especificado, o padrão será o diretório atual. Disponível desde o SDK do .NET Core 3.0.

## <a name="options"></a>Opções

- **`-c|--configuration <CONFIGURATION>`**

  Define a configuração da compilação. O padrão para a maioria dos projetos é `Debug` , mas você pode substituir as definições de configuração de compilação em seu projeto.

- **`-f|--framework <FRAMEWORK>`**

  Publica o aplicativo para a [estrutura de destino](../../standard/frameworks.md) especificada. Especifique a estrutura de destino no arquivo de projeto.

- **`--force`**

  Forçará todas as dependências a serem resolvidas mesmo se última restauração tiver sido bem-sucedida. A especificação desse sinalizador é o mesmo que a exclusão do arquivo *project.assets.json*.

- **`-h|--help`**

  Imprime uma ajuda breve para o comando.

- **`--interactive`**

  Permite que o comando pare e aguarde entrada ou ação do usuário. Por exemplo, para concluir a autenticação. Disponível desde o SDK do .NET Core 3.0.

- **`--manifest <PATH_TO_MANIFEST_FILE>`**

  Especifica um ou vários [manifestos de destino](../deploying/runtime-store.md) a serem usados para cortar o conjunto de pacotes publicados com o aplicativo. O arquivo de manifesto faz parte da saída do [ `dotnet store` comando](dotnet-store.md). Para especificar vários manifestos, adicione uma opção `--manifest` para cada manifesto.

- **`--no-build`**

  Não compila o projeto antes da publicação. Também define o sinalizador `--no-restore` implicitamente.

- **`--no-dependencies`**

  Ignora as referências projeto a projeto e só restaura o projeto raiz.

- **`--nologo`**

  Não exibe a faixa de inicialização nem a mensagem de direitos autorais. Disponível desde o SDK do .NET Core 3.0.

- **`--no-restore`**

  Não executa uma restauração implícita ao executar o comando.

- **`-o|--output <OUTPUT_DIRECTORY>`**

  Especifica o caminho para o diretório de saída.
  
  Se não for especificado, o padrão é *[project_file_folder]./bin/[Configuration]/[Framework]/Publish/* para um executável dependente de estrutura e binários de plataforma cruzada. O padrão é *[project_file_folder]/bin/[Configuration]/[Framework]/[Runtime]/Publish/* para um executável independente.

  Em um projeto Web, se a pasta de saída estiver na pasta do projeto, os `dotnet publish` comandos sucessivos resultarão em pastas de saída aninhadas. Por exemplo, se a pasta do projeto *for MyProject*e a pasta de saída de publicação for *MyProject/Publish*e você `dotnet publish` executar duas vezes, a segunda execução colocará arquivos de conteúdo como arquivos *. config* e *. JSON* no *MyProject/publicar/Publish*. Para evitar o aninhamento de pastas de publicação, especifique uma pasta de publicação que não esteja **diretamente** sob a pasta do projeto ou exclua a pasta de publicação do projeto. Para excluir uma pasta de publicação chamada *publishoutput*, adicione o seguinte elemento a um `PropertyGroup` elemento no arquivo *. csproj* :

  ```xml
  <DefaultItemExcludes>$(DefaultItemExcludes);publishoutput**</DefaultItemExcludes>
  ```

  - SDK do .NET Core 3. x e posterior
  
    Se um caminho relativo for especificado durante a publicação de um projeto, o diretório de saída gerado será relativo ao diretório de trabalho atual, não ao local do arquivo do projeto.

    Se um caminho relativo for especificado durante a publicação de uma solução, toda a saída de todos os projetos entrará na pasta especificada em relação ao diretório de trabalho atual. Para fazer com que a saída de publicação vá para pastas separadas para cada projeto, especifique um caminho relativo usando a `PublishDir` Propriedade MSBuild em vez da `--output` opção. Por exemplo, `dotnet publish -p:PublishDir=.\publish` envia a saída de publicação para cada projeto em uma `publish` pasta sob a pasta que contém o arquivo de projeto.

  - SDK do .NET Core 2. x
  
    Se um caminho relativo for especificado durante a publicação de um projeto, o diretório de saída gerado será relativo ao local do arquivo de projeto, não ao diretório de trabalho atual.

    Se um caminho relativo for especificado durante a publicação de uma solução, a saída de cada projeto entrará em uma pasta separada relativa ao local do arquivo de projeto. Se um caminho absoluto for especificado durante a publicação de uma solução, toda a saída de publicação de todos os projetos vai para a pasta especificada.

- **`-p:PublishReadyToRun=true`**

  Compila assemblies de aplicativo como o formato ReadyToRun (R2R). R2R é uma forma de compilação antecipada (AOT). Para obter mais informações, consulte [ReadyToRun images](../whats-new/dotnet-core-3-0.md#readytorun-images). Disponível desde o SDK do .NET Core 3.0.

  Recomendamos que você especifique essa opção em um perfil de publicação em vez de na linha de comando. Para mais informações, consulte [MSBuild](#msbuild).

- **`-p:PublishSingleFile=true`**

  Empacota o aplicativo em um executável de arquivo único específico da plataforma. O executável é de extração automática e contém todas as dependências (incluindo as nativas) que são necessárias para executar o aplicativo. Quando o aplicativo é executado pela primeira vez, o aplicativo é extraído para um diretório com base no nome do aplicativo e no identificador do build. A inicialização é mais rápida quando o aplicativo é executado novamente. O aplicativo não precisa extrair a si mesmo uma segunda vez, a menos que uma nova versão seja usada. Disponível desde o SDK do .NET Core 3.0.

  Para obter mais informações sobre a publicação de arquivo único, consulte o [documento de design de empacotador de arquivo único](https://github.com/dotnet/designs/blob/master/accepted/2020/single-file/design.md).

  Recomendamos que você especifique essa opção em um perfil de publicação em vez de na linha de comando. Para mais informações, consulte [MSBuild](#msbuild).

- **`-p:PublishTrimmed=true`**

  Apara as bibliotecas não usadas para reduzir o tamanho da implantação de um aplicativo ao publicar um executável independente. Para obter mais informações, consulte [aparar implantações e executáveis independentes](../deploying/trim-self-contained.md). Disponível desde o SDK do .NET Core 3,0 como um recurso de visualização.

  Recomendamos que você especifique essa opção em um perfil de publicação em vez de na linha de comando. Para mais informações, consulte [MSBuild](#msbuild).

- **`--self-contained [true|false]`**

  Publica o runtime do .NET Core com seu aplicativo para que não seja necessário instalar o runtime no computador de destino. O padrão é `true` se um identificador de tempo de execução for especificado e o projeto for um projeto executável (não um projeto de biblioteca). Para obter mais informações, consulte [publicação de aplicativos do .NET Core](../deploying/index.md) e [publicar aplicativos .net core com o CLI do .NET Core](../deploying/deploy-with-cli.md).

  Se essa opção for usada sem especificar `true` ou `false` , o padrão será `true` . Nesse caso, não coloque a solução ou o argumento do projeto imediatamente após `--self-contained` , porque `true` ou `false` é esperado nessa posição.

- **`--no-self-contained`**

  Equivalente a `--self-contained false`. Disponível desde o SDK do .NET Core 3.0.

- **`-r|--runtime <RUNTIME_IDENTIFIER>`**

  Publica o aplicativo para um determinado runtime. Para obter uma lista de RIDs (Identificadores de Runtime), veja o [Catálogo de RIDs](../rid-catalog.md). Para obter mais informações, consulte [publicação de aplicativos do .NET Core](../deploying/index.md) e [publicar aplicativos .net core com o CLI do .NET Core](../deploying/deploy-with-cli.md).

- **`-v|--verbosity <LEVEL>`**

  Define o nível de detalhes do comando. Os valores permitidos são `q[uiet]`, `m[inimal]`, `n[ormal]`, `d[etailed]` e `diag[nostic]`. O valor padrão é `minimal`.

- **`--version-suffix <VERSION_SUFFIX>`**

  Define o sufixo da versão para substituir o asterisco (`*`) no campo de versão do arquivo de projeto.

## <a name="examples"></a>Exemplos

- Crie um [binário de plataforma cruzada dependente de estrutura](../deploying/index.md#produce-a-cross-platform-binary) para o projeto no diretório atual:

  ```dotnetcli
  dotnet publish
  ```

  A partir do SDK do .NET Core 3,0, este exemplo também cria um [executável dependente da estrutura](../deploying/index.md#publish-framework-dependent) para a plataforma atual.

- Crie um [executável independente](../deploying/index.md#publish-self-contained) para o projeto no diretório atual, para um tempo de execução específico:

  ```dotnetcli
  dotnet publish --runtime osx.10.11-x64
  ```

  O RID deve estar no arquivo de projeto.

- Crie um [executável dependente de estrutura](../deploying/index.md#publish-framework-dependent) para o projeto no diretório atual, para uma plataforma específica:

  ```dotnetcli
  dotnet publish --runtime osx.10.11-x64 --self-contained false
  ```

  O RID deve estar no arquivo de projeto. Este exemplo se aplica ao SDK do .NET Core 3,0 e versões posteriores.

- Publicar o projeto no diretório atual, para um tempo de execução e estrutura de destino específicos:

  ```dotnetcli
  dotnet publish --framework netcoreapp3.1 --runtime osx.10.11-x64
  ```

- Publicar o arquivo de projeto especificado:

  ```dotnetcli
  dotnet publish ~/projects/app1/app1.csproj
  ```

- Publicar o aplicativo atual, mas não restaurar referências ponto a projeto (P2P), apenas o projeto raiz durante a operação de restauração:

  ```dotnetcli
  dotnet publish --no-dependencies
  ```

## <a name="see-also"></a>Consulte também

- [Visão geral da publicação de aplicativos do .NET Core](../deploying/index.md)
- [Publicar aplicativos .NET Core com o CLI do .NET Core](../deploying/deploy-with-cli.md)
- [Estruturas de destino](../../standard/frameworks.md)
- [Catálogo de RID (Identificador de Runtime)](../rid-catalog.md)
- [Trabalhando com o notarization Catalina do macOS](../install/macos-notarization-issues.md)
- [Estrutura de diretórios de um aplicativo publicado](/aspnet/core/hosting/directory-structure)
- [Referência de linha de comando do MSBuild](/visualstudio/msbuild/msbuild-command-line-reference)
- [Perfis de publicação do Visual Studio (. pubxml) para implantação de aplicativo ASP.NET Core](/aspnet/core/host-and-deploy/visual-studio-publish-profiles)
- [dotnet msbuild](dotnet-msbuild.md)
- [ILLInk. Tasks](https://aka.ms/dotnet-illink)
