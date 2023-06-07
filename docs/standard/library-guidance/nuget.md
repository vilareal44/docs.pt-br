---
title: Bibliotecas do NuGet e .NET
description: Recomendações de melhor prática para o empacotamento com o NuGet para bibliotecas do .NET.
ms.date: 01/15/2019
ms.openlocfilehash: d9f8d7cc4402a87e1429791b57a0306b318dfbe4
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87382107"
---
# <a name="nuget"></a>NuGet

O NuGet é um gerenciador de pacotes para o ecossistema do .NET e é a principal maneira como os desenvolvedores descobrem e adquirem bibliotecas de código-fonte aberto do .NET. O [NuGet.org](https://www.nuget.org/), um serviço gratuito fornecido pela Microsoft para hospedar pacotes do NuGet, é o host primário para os pacotes públicos do NuGet, mas você pode publicar em serviços personalizados do NuGet, como [MyGet](https://www.myget.org/) e [Azure Artifacts](https://azure.microsoft.com/services/devops/artifacts/).

![NuGet](./media/nuget/nuget-logo.png "NuGet")

## <a name="create-a-nuget-package"></a>Criar um pacote NuGet

Um pacote do NuGet (`*.nupkg`) é um arquivo zip que contém os assemblies do .NET e os metadados associados.

Há duas maneiras principais de criar um pacote do NuGet. A maneira recomendada e mais recente é criar um pacote de um projeto no estilo do SDK (arquivo de projeto cujo conteúdo começa com `<Project Sdk="Microsoft.NET.Sdk">`). Assemblies e destinos são automaticamente adicionados ao pacote e o restante dos metadados é adicionado ao arquivo MSBuild, como o número de versão e o nome do pacote. A compilação com o [`dotnet pack`](../../core/tools/dotnet-pack.md) comando gera um `*.nupkg` arquivo em vez de assemblies.

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>netstandard2.0</TargetFramework>
    <AssemblyName>Contoso.Api</AssemblyName>
    <PackageVersion>1.1.0</PackageVersion>
    <Authors>John Doe</Authors>
  </PropertyGroup>
</Project>
```

A maneira mais antiga de criar um pacote do NuGet é com um arquivo `*.nuspec` e a ferramenta de linha de comando `nuget.exe`. Um arquivo nuspec dá excelente controle, mas você deve especificar com cuidado quais assemblies e destinos incluir no pacote final do NuGet. É fácil cometer um erro ou alguém se esquecer de atualizar o nuspec ao fazer alterações. A vantagem de um nuspec é que você pode usá-lo criar pacotes do NuGet para estruturas que ainda não dão suporte a um arquivo de projeto de estilo do SDK.

✔️ CONSIDERE usar um arquivo de projeto no estilo SDK para criar o pacote do NuGet.

## <a name="package-dependencies"></a>Dependências do pacote

Dependências de pacotes NuGet são abordadas detalhadamente no artigo [Dependências](./dependencies.md).

## <a name="important-nuget-package-metadata"></a>Metadados de pacote do NuGet importantes

Um pacote do NuGet dá suporte a muitas [propriedades de metadados](/nuget/reference/nuspec). A seguinte tabela contém os principais metadados que todo projeto do NuGet.org deve fornecer:

| Nome da Propriedade do MSBuild              | Nome Nuspec              | Descrição  |
| ---------------------------------- | ------------------------ | ------------ |
| `PackageId`                        | `id`                       | O identificador de pacote. Um prefixo do identificador poderá ser reservado se ele cumprir os [critérios](/nuget/reference/id-prefix-reservation). |
| `PackageVersion`                   | `version`                  | Versão do pacote NuGet. Para obter mais informações, veja [Controle de versão de pacote do NuGet](./versioning.md#nuget-package-version).             |
| `Title`                            | `title`                    | Um título amigável a humanos do pacote. Usa como padrão `PackageId`.             |
| `Description`                      | `description`              | Uma descrição longa do pacote exibida na interface do usuário.             |
| `Authors`                          | `authors`                  | Uma lista separada por vírgulas de autores de pacote que correspondem aos nomes de perfil em nuget.org.             |
| `PackageTags`                      | `tags`                     | Uma lista delimitada por espaço de marcas e palavras-chave que descrevem o pacote. Marcas são usadas ao pesquisar pacotes.             |
| `PackageIcon`                   | `icon`                  | Um caminho para uma imagem no pacote a ser usado como um ícone de pacote. Leia mais sobre [ `icon` metadados](/nuget/reference/nuspec#icon). |
| `PackageProjectUrl`                | `projectUrl`               | Uma URL para o repositório de origem ou página inicial do projeto.             |
| `PackageLicenseExpression`         | `license`                  | O [identificador SPDX](https://spdx.org/licenses/) da licença de projeto. Somente licenças aprovadas por OSI e FSF podem usar um identificador. Outras licenças devem usar `PackageLicenseFile`. Leia mais sobre [ `license` metadados](/nuget/reference/nuspec#license). |

> [!IMPORTANT]
> Um projeto sem uma licença usa como padrão [direitos autorais exclusivos](https://choosealicense.com/no-permission/), tornando legalmente impossível seu uso por outras pessoas.

✔️ CONSIDERE escolher um nome de pacote do NuGet com um prefixo que cumpra os [critérios](/nuget/reference/id-prefix-reservation) de reserva de prefixo do NuGet.

✔️ FAÇA uso de um href HTTPS com o ícone do pacote.

> Sites, como o NuGet.org são executados com HTTPS habilitado e exibir uma imagem não HTTPS criará um aviso de conteúdo misto.

✔️ FAÇA uso de uma imagem de ícone do pacote de 64 x 64 e com um segundo plano transparente para melhor visualização dos resultados.

✔️ CONSIDERE a configuração do [Source Link](./sourcelink.md) para adicionar metadados de controle do código-fonte aos assemblies e ao pacote NuGet.

> O Source Link adiciona automaticamente os metadados `RepositoryUrl` e `RepositoryType` ao pacote NuGet. O Source Link também adiciona informações sobre o código-fonte exato do qual o pacote foi criado. Por exemplo, um pacote criado de um repositório Git terá o hash de confirmação adicionado como metadados.

## <a name="pre-release-packages"></a>Pacotes de pré-lançamento

Pacotes do NuGet com um sufixo de versão são considerados [pré-lançamento](/nuget/create-packages/prerelease-packages). Por padrão, a interface do usuário do Gerenciador de Pacotes do NuGet mostra versões estáveis, a menos que um usuário aceite pacotes de pré-lançamento, tornando os pacotes de pré-lançamento ideais para testes de usuários limitados.

```xml
<PackageVersion>1.0.1-beta1</PackageVersion>
```

> [!NOTE]
> Um pacote estável não pode depender de um pacote de pré-lançamento. Você deve criar seu próprio pacote de pré-lançamento ou depender de uma versão estável mais antiga.

![Dependência do pacote de pré-lançamento do NuGet](./media/nuget/nuget-prerelease-package.png "Dependência do pacote de pré-lançamento do NuGet")

✔️ FAÇA a publicação de um pacote de pré-lançamento ao testar, visualizar ou experimentar.

✔️ FAÇA a publicação de um pacote estável quando ele estiver pronto para que outros pacotes estáveis possam fazer referência a ele.

## <a name="symbol-packages"></a>Pacotes de símbolo

Arquivos de símbolo (`*.pdb`) são produzidos pelo compilador do .NET junto com assemblies. Arquivos de símbolo mapeiam locais de execução para o código-fonte original para que você possa percorrer o código-fonte conforme ele é executado usando um depurador. O NuGet é compatível com a [geração de um pacote de símbolos separado (`*.snupkg`)](/nuget/create-packages/symbol-packages-snupkg) contendo arquivos de símbolo junto com o pacote principal contendo assemblies do .NET. A ideia de pacotes de símbolos é que eles estão hospedados em um servidor de símbolos e são baixados somente por uma ferramenta como o Visual Studio sob demanda.

O NuGet.org hospeda seu próprio [repositório do servidor de símbolos](/nuget/create-packages/symbol-packages-snupkg#nugetorg-symbol-server). Os desenvolvedores podem usar os símbolos publicados no servidor de símbolos do NuGet.org adicionando `https://symbols.nuget.org/download/symbols` às suas [fontes de símbolos no Visual Studio](/visualstudio/debugger/specify-symbol-dot-pdb-and-source-files-in-the-visual-studio-debugger).

> [!IMPORTANT]
> O servidor de símbolos NuGet.org só dá suporte aos novos [arquivos de símbolo portáteis](https://github.com/dotnet/core/blob/master/Documentation/diagnostics/portable_pdb.md) ( `*.pdb` ) criados por projetos em estilo SDK.
>
> Para usar o servidor de símbolos NuGet.org ao depurar uma biblioteca .NET, os desenvolvedores devem ter o Visual Studio 2017 versão 15,9 ou posterior.

Uma alternativa para criar um pacote de símbolos é inserir arquivos de símbolo no pacote do NuGet principal. O pacote do NuGet principal será maior, mas os arquivos de símbolos inseridos significam que os desenvolvedores não precisam configurar o servidor de símbolos do NuGet.org. Se você estiver criando seu pacote do NuGet usando um projeto de estilo do SDK, poderá inserir arquivos de símbolo definindo a propriedade `AllowedOutputExtensionsInPackageBuildOutputFolder`:

```xml
<Project Sdk="Microsoft.NET.Sdk">
 <PropertyGroup>
    <!-- Include symbol files (*.pdb) in the built .nupkg -->
    <AllowedOutputExtensionsInPackageBuildOutputFolder>$(AllowedOutputExtensionsInPackageBuildOutputFolder);.pdb</AllowedOutputExtensionsInPackageBuildOutputFolder>
  </PropertyGroup>
</Project>
```

A desvantagem de inserir arquivos de símbolo é que eles aumentam o tamanho do pacote em cerca de 30% para bibliotecas .NET compiladas usando projetos de estilo SDK. Se o tamanho do pacote é uma preocupação, você deve publicar símbolos em um pacote de símbolos em vez disso.

✔️ CONSIDERE a possibilidade de publicar símbolos como um pacote de símbolos (`*.snupkg`) em NuGet.org

> Os pacotes de símbolos (`*.snupkg`) oferecem aos desenvolvedores uma boa experiência de depuração sob demanda, sem sobrecarregar o tamanho do pacote principal nem afetar o desempenho de restauração para aqueles que não pretendem depurar o pacote NuGet.
>
> A limitação é que os usuários podem precisar localizar e configurar o servidor de símbolos NuGet em seu IDE (como uma configuração única) para obter arquivos de símbolo. O Visual Studio 2019 versão 16,1 adicionou o servidor de símbolos do NuGet. org à lista de servidores de símbolos padrão.

>[!div class="step-by-step"]
>[Anterior](strong-naming.md) 
> [Avançar](dependencies.md)
