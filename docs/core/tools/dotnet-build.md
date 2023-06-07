---
title: Comando dotnet build
description: O comando dotnet build compila um projeto e todas as suas dependências.
ms.date: 02/14/2020
ms.openlocfilehash: 6f33b449301f40949ff5dfe4077564344a9de8ec
ms.sourcegitcommit: c8c3e1c63a00b7d27f76f5e50ee6469e6bdc8987
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87251160"
---
# <a name="dotnet-build"></a>dotnet build

**Este artigo aplica-se a:** ✔️ SDK do .NET Core 2. x e versões posteriores

## <a name="name"></a>Nome

`dotnet build` – Compila um projeto e todas as suas dependências.

## <a name="synopsis"></a>Sinopse

```dotnetcli
dotnet build [<PROJECT>|<SOLUTION>] [-c|--configuration <CONFIGURATION>]
    [-f|--framework <FRAMEWORK>] [--force] [--interactive] [--no-dependencies]
    [--no-incremental] [--no-restore] [--nologo] [-o|--output <OUTPUT_DIRECTORY>]
    [-r|--runtime <RUNTIME_IDENTIFIER>] [--source <SOURCE>]
    [-v|--verbosity <LEVEL>] [--version-suffix <VERSION_SUFFIX>]

dotnet build -h|--help
```

## <a name="description"></a>Descrição

O comando `dotnet build` compila o projeto e suas dependências em um conjunto de binários. Os binários incluem o código do projeto em arquivos de IL (linguagem intermediária) com uma extensão *. dll* .  Dependendo do tipo de projeto e das configurações, outros arquivos podem ser incluídos, como:

- Um executável que pode ser usado para executar o aplicativo, se o tipo de projeto for um executável direcionado ao .NET Core 3,0 ou posterior.
- Arquivos de símbolo usados para depuração com uma extensão *. pdb* .
- Um *.deps.jsno* arquivo, que lista as dependências do aplicativo ou da biblioteca.
- Um *.runtimeconfig.jsno* arquivo, que especifica o tempo de execução compartilhado e sua versão para um aplicativo.
- Outras bibliotecas das quais o projeto depende (por meio de referências de projeto ou referências de pacote NuGet).

Para projetos executáveis que visam versões anteriores ao .NET Core 3,0, as dependências de biblioteca do NuGet normalmente não são copiadas para a pasta de saída.  Eles são resolvidos na pasta de pacotes globais do NuGet em tempo de execução. Com isso em mente, o produto de `dotnet build` não está pronto para ser transferido para outro computador para execução. Para criar uma versão do aplicativo que pode ser implantada, você precisa publicá-la (por exemplo, com o comando [dotnet Publish](dotnet-publish.md) ). Para saber mais, confira [Implantação de aplicativos .NET Core](../deploying/index.md).

Para projetos executáveis destinados ao .NET Core 3,0 e posterior, as dependências de biblioteca são copiadas para a pasta de saída. Isso significa que, se não houver nenhuma outra lógica específica de publicação (como os projetos Web têm), a saída da compilação deverá ser implantável.

### <a name="implicit-restore"></a>Restauração implícita

A compilação exige o arquivo *project.assets.json*, que lista as dependências do seu aplicativo. O arquivo é criado quando [`dotnet restore`](dotnet-restore.md) é executado. Sem o arquivo de ativos em vigor, as ferramentas não conseguem resolver os assemblies de referência, o que resulta em erros.

[!INCLUDE[dotnet restore note + options](~/includes/dotnet-restore-note-options.md)]

### <a name="executable-or-library-output"></a>Saída de biblioteca ou executável

O fato de o projeto ser executável ou não é determinado pela propriedade `<OutputType>` do arquivo de projeto. O seguinte exemplo mostra um projeto que produz um código executável:

```xml
<PropertyGroup>
  <OutputType>Exe</OutputType>
</PropertyGroup>
```

Para produzir uma biblioteca, omita a `<OutputType>` propriedade ou altere seu valor para `Library` . A DLL de IL para uma biblioteca não contém pontos de entrada e não pode ser executada.

### <a name="msbuild"></a>MSBuild

O `dotnet build` usa o MSBuild para compilar o projeto e, portanto, dá suporte a builds paralelos e incrementais. Para obter mais informações, consulte [Compilações incrementais](/visualstudio/msbuild/incremental-builds).

Além das próprias opções, o comando `dotnet build` também aceita opções do MSBuild, como `-p` para configurar propriedades ou `-l` para definir um agente. Para obter mais informações sobre essas opções, confira a [Referência de linha de comando do MSBuild](/visualstudio/msbuild/msbuild-command-line-reference). Ou você também pode usar o comando [dotnet msbuild](dotnet-msbuild.md).

A execução `dotnet build` é equivalente à execução `dotnet msbuild -restore` ; no entanto, o detalhamento padrão da saída é diferente.

## <a name="arguments"></a>Argumentos

`PROJECT | SOLUTION`

O arquivo de projeto ou solução a ser compilado. Se um arquivo de solução ou projeto não for especificado, o MSBuild pesquisará o diretório de trabalho atual em busca de um arquivo que tenha uma extensão terminada em *proj* ou *sln* e usará esse arquivo.

## <a name="options"></a>Opções

- **`-c|--configuration <CONFIGURATION>`**

  Define a configuração da compilação. O padrão para a maioria dos projetos é `Debug` , mas você pode substituir as definições de configuração de compilação em seu projeto.

- **`-f|--framework <FRAMEWORK>`**

  Compila para uma [estrutura](../../standard/frameworks.md) específica. A estrutura precisa ser definida no [arquivo de projeto](csproj.md).

- **`--force`**

  Forçará todas as dependências a serem resolvidas mesmo se última restauração tiver sido bem-sucedida. A especificação desse sinalizador é o mesmo que a exclusão do arquivo *project.assets.json*.

- **`-h|--help`**

  Imprime uma ajuda breve para o comando.

- **`--interactive`**

  Permite que o comando pare e aguarde entrada ou ação do usuário. Por exemplo, para concluir a autenticação. Disponível desde o SDK do .NET Core 3.0.

- **`--no-dependencies`**

  Ignora as referências P2P (projeto a projeto) e compila apenas o projeto raiz especificado.

- **`--no-incremental`**

  Marca o build como não segura para build incremental. Esse sinalizador desativa a compilação incremental e força uma nova recompilação do grafo de dependência do projeto.

- **`--no-restore`**

  Não executa uma restauração implícita durante o build.

- **`--nologo`**

  Não exibe a faixa de inicialização nem a mensagem de direitos autorais. Disponível desde o SDK do .NET Core 3.0.

- **`-o|--output <OUTPUT_DIRECTORY>`**

  Diretório no qual os binários compilados são colocados. Se não for especificado, o caminho padrão será `./bin/<configuration>/<framework>/`.  Para projetos com várias estruturas de destino (por meio da `TargetFrameworks` Propriedade), também é necessário definir `--framework` quando você especifica essa opção.

- **`-r|--runtime <RUNTIME_IDENTIFIER>`**

  Especifica o runtime de destino. Para obter uma lista de RIDs (Identificadores de Runtime), veja o [Catálogo de RIDs](../rid-catalog.md).

- **`--source <SOURCE>`**

  O URI da origem do pacote NuGet a ser usado durante a operação de restauração.

- **`-v|--verbosity <LEVEL>`**

  Define o nível de detalhamento do MSBuild. Os valores permitidos são `q[uiet]`, `m[inimal]`, `n[ormal]`, `d[etailed]` e `diag[nostic]`. O padrão é `minimal`.

- **`--version-suffix <VERSION_SUFFIX>`**

  Define o valor da propriedade `$(VersionSuffix)` a ser usada ao compilar o projeto. Isso funcionará apenas se a propriedade `$(Version)` não estiver definida. Em seguida, `$(Version)` é definido como o `$(VersionPrefix)` combinado com o `$(VersionSuffix)`, separado por um traço.

## <a name="examples"></a>Exemplos

- Compile um projeto e suas dependências:

  ```dotnetcli
  dotnet build
  ```

- Compile um projeto e suas dependências usando a configuração da Versão:

  ```dotnetcli
  dotnet build --configuration Release
  ```

- Compile um projeto e suas dependências para um runtime específico (no exemplo, Ubuntu 18.04):

  ```dotnetcli
  dotnet build --runtime ubuntu.18.04-x64
  ```

- Compile o projeto e use a fonte do pacote NuGet especificada durante a operação de restauração (SDK do .NET Core 2.0 e versões posteriores):

  ```dotnetcli
  dotnet build --source c:\packages\mypackages
  ```

- Compile o projeto e defina a versão 1.2.3.4 como um parâmetro de compilação usando a `-p` [opção MSBuild](#msbuild):

  ```dotnetcli
  dotnet build -p:Version=1.2.3.4
  ```
