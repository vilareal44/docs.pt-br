---
title: Comando dotnet restore
description: Saiba como restaurar as dependências e ferramentas específicas de projeto com o comando dotnet restore.
ms.date: 02/27/2020
ms.openlocfilehash: 7b456e28505a07c03936c9006c8631848fd4672c
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86925470"
---
# <a name="dotnet-restore"></a>dotnet restore

**Este artigo aplica-se a:** ✔️ SDK do .net Core 2,1 e versões posteriores

## <a name="name"></a>Nome

`dotnet restore` – Restaura as dependências e as ferramentas de um projeto.

## <a name="synopsis"></a>Sinopse

```dotnetcli
dotnet restore [<ROOT>] [--configfile <FILE>] [--disable-parallel]
    [-f|--force] [--force-evaluate] [--ignore-failed-sources]
    [--interactive] [--lock-file-path <LOCK_FILE_PATH>] [--locked-mode]
    [--no-cache] [--no-dependencies] [--packages <PACKAGES_DIRECTORY>]
    [-r|--runtime <RUNTIME_IDENTIFIER>] [-s|--source <SOURCE>]
    [--use-lock-file] [-v|--verbosity <LEVEL>]

dotnet restore -h|--help
```

## <a name="description"></a>Descrição

O comando `dotnet restore` usa o NuGet para restaurar as dependências e ferramentas específicas de projeto especificadas no arquivo de projeto.  Na maioria dos casos, você não precisa usar o `dotnet restore` comando explicitamente, uma vez que uma restauração do NuGet é executada implicitamente, se necessário, quando você executa os seguintes comandos:

- [`dotnet new`](dotnet-new.md)
- [`dotnet build`](dotnet-build.md)
- [`dotnet build-server`](dotnet-build-server.md)
- [`dotnet run`](dotnet-run.md)
- [`dotnet test`](dotnet-test.md)
- [`dotnet publish`](dotnet-publish.md)
- [`dotnet pack`](dotnet-pack.md)

Às vezes, pode ser inconveniente executar a restauração implícita do NuGet com estes comandos. Por exemplo, alguns sistemas automatizados, como os sistemas de compilação, precisam chamar o `dotnet restore` explicitamente para controlar o momento em que a restauração ocorre para que possam controlar o uso de rede. Para evitar a restauração implícita do NuGet, você pode usar o `--no-restore` sinalizador com qualquer um desses comandos para desabilitar a restauração implícita.

### <a name="specify-feeds"></a>Especificar feeds

Para restaurar as dependências, o NuGet precisa dos feeds nos quais os pacotes estão localizados. Os feeds são geralmente fornecidos por meio do arquivo de configuração *nuget.config*. Um arquivo de configuração padrão é fornecido quando o SDK do .NET Core é instalado. Para especificar Feeds adicionais, siga um destes procedimentos:

- Crie seu próprio arquivo de *nuget.config* no diretório do projeto. Para obter mais informações, consulte [configurações comuns do NuGet](/nuget/consume-packages/configuring-nuget-behavior) e [diferenças denuget.config](#nugetconfig-differences) mais adiante neste artigo.
- Use `dotnet nuget` comandos como [`dotnet nuget add source`](dotnet-nuget-add-source.md) .

Você pode substituir os feeds *nuget.config* com a `-s` opção.

Para obter informações sobre como usar feeds autenticados, consulte [consumindo pacotes de feeds autenticados](/nuget/consume-packages/consuming-packages-authenticated-feeds).

### <a name="global-packages-folder"></a>Pasta de pacotes globais

Para dependências, você pode especificar onde os pacotes restaurados são colocados durante a operação de restauração usando o argumento `--packages`. Se não é especificado, o cache do pacote NuGet padrão é usado, o qual pode ser encontrado no diretório `.nuget/packages` do diretório base do usuário em todos os sistemas operacionais. Por exemplo, */home/user1* no Linux ou *C:\Usuário\user1* no Windows.

### <a name="project-specific-tooling"></a>Ferramentas específicas do projeto

Para ferramentas específicas do projeto, o `dotnet restore` primeiro restaura o pacote no qual a ferramenta foi empacotada e prossegue com a restauração das dependências da ferramenta conforme especificado no seu arquivo de projeto.

### <a name="nugetconfig-differences"></a>Diferenças do nuget.config

O comportamento do comando `dotnet restore` será afetado pelas configurações no arquivo *nuget.config*, se estiver presente. Por exemplo, a definição da `globalPackagesFolder` em *nuget.config* coloca os pacotes NuGet restaurados na pasta especificada. Essa é uma alternativa para especificar a opção `--packages` no comando `dotnet restore`. Para obter mais informações, confira a [Referência do nuget.config](/nuget/schema/nuget-config-file).

Há três configurações específicas que são ignoradas por `dotnet restore`:

- [bindingRedirects](/nuget/schema/nuget-config-file#bindingredirects-section)

  Os redirecionamentos de associação não funcionam com elementos `<PackageReference>` e o .NET Core só dá suporte a elementos `<PackageReference>` em pacotes NuGet.

- [solução](/nuget/schema/nuget-config-file#solution-section)

  Essa configuração é específica do Visual Studio e não se aplica ao .NET Core. O .NET Core não usa um arquivo `packages.config` e, em vez disso, usa elementos `<PackageReference>` para pacotes NuGet.

- [trustedSigners](/nuget/schema/nuget-config-file#trustedsigners-section)

  Essa configuração não é aplicável, pois o [NuGet ainda não dá suporte à verificação de multiplataforma](https://github.com/NuGet/Home/issues/7939) de pacotes confiáveis.

## <a name="arguments"></a>Argumentos

- **`ROOT`**

  Caminho opcional para o arquivo de projeto a ser restaurado.

## <a name="options"></a>Opções

- **`--configfile <FILE>`**

  O arquivo de configuração NuGet (*nuget.config*) a ser usado para a operação de restauração.

- **`--disable-parallel`**

  Desabilita a restauração de vários projetos paralelamente.

- **`--force`**

  Forçará todas as dependências a serem resolvidas mesmo se última restauração tiver sido bem-sucedida. A especificação desse sinalizador é o mesmo que a exclusão do arquivo *project.assets.json*.

- **`--force-evaluate`**

  Força a restauração a reavaliar todas as dependências mesmo se um arquivo de bloqueio já existir.

- **`-h|--help`**

  Imprime uma ajuda breve para o comando.

- **`--ignore-failed-sources`**

  Avise somente sobre fontes com falha se houver pacotes que atendem ao requisito de versão.

- **`--interactive`**

  Permite que o comando pare e aguarde a entrada ou uma ação do usuário (por exemplo, para concluir a autenticação). A partir do .NET Core 2.1.400.

- **`--lock-file-path <LOCK_FILE_PATH>`**

  Local de saída onde o arquivo de bloqueio do projeto é gravado. Por padrão, isso é *PROJECT_ROOT\packages.lock.jsem*.

- **`--locked-mode`**

  Não permitir atualização do arquivo de bloqueio do projeto.

- **`--no-cache`**

  Especifica para não armazenar em cache solicitações HTTP.

- **`--no-dependencies`**

  Ao restaurar um projeto com referências de P2P (projeto a projeto), restaura o projeto raiz, não as referências.

- **`--packages <PACKAGES_DIRECTORY>`**

  Especifica o diretório para os pacotes restaurados.

- **`-r|--runtime <RUNTIME_IDENTIFIER>`**

  Especifica um runtime para a restauração do pacote. Isso é usado para restaurar pacotes para runtimes não listados explicitamente na marca `<RuntimeIdentifiers>` no arquivo *.csproj*. Para obter uma lista de RIDs (Identificadores de Runtime), veja o [Catálogo de RIDs](../rid-catalog.md). Forneça diversas RIDs especificando essa opção várias vezes.

- **`-s|--source <SOURCE>`**

  Especifica o URI da origem do pacote NuGet a ser usado durante a operação de restauração. Essa configuração substitui todas as fontes especificadas nos arquivos *nuget.config*. Diversas fontes podem ser fornecidas especificando essa opção várias vezes.

- **`--use-lock-file`**

  Habilita o arquivo de bloqueio do projeto a ser gerado e usado com a restauração.

- **`-v|--verbosity <LEVEL>`**

  Define o nível de detalhes do comando. Os valores permitidos são `q[uiet]`, `m[inimal]`, `n[ormal]`, `d[etailed]` e `diag[nostic]`. O valor padrão é `minimal`.

## <a name="examples"></a>Exemplos

- Restaure as dependências e as ferramentas para o projeto no diretório atual:

  ```dotnetcli
  dotnet restore
  ```

- Restaure as dependências e as ferramentas para o projeto `app1` encontrado no caminho fornecido:

  ```dotnetcli
  dotnet restore ./projects/app1/app1.csproj
  ```

- Restaure as dependências e as ferramentas para o projeto no diretório atual usando o caminho de arquivo fornecido como a origem:

  ```dotnetcli
  dotnet restore -s c:\packages\mypackages
  ```

- Restaure as dependências e as ferramentas para o projeto no diretório atual usando os dois caminhos de arquivo fornecidos como fontes:

  ```dotnetcli
  dotnet restore -s c:\packages\mypackages -s c:\packages\myotherpackages
  ```

- Restaure as dependências e as ferramentas para o projeto no diretório atual mostrando a saída detalhada:

  ```dotnetcli
  dotnet restore --verbosity detailed
  ```
