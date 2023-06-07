---
title: Investigação padrão-.NET Core
description: Visão geral da lógica de investigação System. Runtime. Loader. AssemblyLoadContext. padrão do .NET Core para localizar dependências.
ms.date: 08/09/2019
author: sdmaclea
ms.author: stmaclea
ms.openlocfilehash: 13ce4c7de5f6ce1b76b2e61db810c0f19717540f
ms.sourcegitcommit: cbb19e56d48cf88375d35d0c27554d4722761e0d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88608426"
---
# <a name="default-probing"></a>Investigação padrão

A <xref:System.Runtime.Loader.AssemblyLoadContext.Default%2A?displayProperty=nameWithType> instância é responsável por localizar as dependências de um assembly. Este artigo descreve a <xref:System.Runtime.Loader.AssemblyLoadContext.Default%2A?displayProperty=nameWithType> lógica de investigação da instância.

## <a name="host-configured-probing-properties"></a>Propriedades de investigação configuradas pelo host

Quando o tempo de execução é iniciado, o host de tempo de execução fornece um conjunto de propriedades de investigação nomeadas que configuram <xref:System.Runtime.Loader.AssemblyLoadContext.Default%2A?displayProperty=nameWithType> caminhos de investigação.

Cada propriedade de investigação é opcional. Se houver, cada propriedade será um valor de cadeia de caracteres que contém uma lista delimitada de caminhos absolutos. O delimitador é '; ' no Windows e ': ' em todas as outras plataformas.

|Nome da propriedade                 |Descrição  |
|------------------------------|---------|
|`TRUSTED_PLATFORM_ASSEMBLIES`   | Lista de caminhos de arquivo de assembly de aplicativo e plataforma. |
|`PLATFORM_RESOURCE_ROOTS`       | Lista de caminhos de diretório para pesquisar por assemblies de recurso satélite. |
|`NATIVE_DLL_SEARCH_DIRECTORIES` | Lista de caminhos de diretório para pesquisar bibliotecas não gerenciadas (nativas).        |
|`APP_PATHS`                     | Lista de caminhos de diretório para pesquisar assemblies gerenciados. |
|`APP_NI_PATHS`                  | Lista de caminhos de diretório para pesquisar imagens nativas de assemblies gerenciados. |

### <a name="how-are-the-properties-populated"></a>Como as propriedades são populadas?

Há dois cenários principais para popular as propriedades dependendo se o * \<myapp>.deps.jsno* arquivo existe.

- Quando o * \*.deps.jsno* arquivo estiver presente, ele será analisado para preencher as propriedades de investigação.
- Quando o * \*.deps.jsno* arquivo não estiver presente, o diretório do aplicativo será considerado para conter todas as dependências. O conteúdo do diretório é usado para preencher as propriedades de investigação.

Além disso, a * \*.deps.jsem* arquivos para todas as estruturas referenciadas é analisada da mesma forma.

Por fim, a variável de ambiente `ADDITIONAL_DEPS` pode ser usada para adicionar outras dependências.  `dotnet.exe` também contém um `--additional-deps` parâmetro opcional para definir esse valor na inicialização do aplicativo.

As `APP_PATHS` `APP_NI_PATHS` Propriedades e não são populadas por padrão e são omitidas para a maioria dos aplicativos.

A lista de todos os * \*.deps.jsem* arquivos usados pelo aplicativo pode ser acessada via `System.AppContext.GetData("APP_CONTEXT_DEPS_FILES")` .

### <a name="how-do-i-see-the-probing-properties-from-managed-code"></a>Como fazer ver as propriedades de investigação do código gerenciado?

Cada propriedade está disponível chamando a <xref:System.AppContext.GetData(System.String)?displayProperty=nameWithType> função com o nome da propriedade da tabela acima.

### <a name="how-do-i-debug-the-probing-properties-construction"></a>Como fazer depurar a construção das propriedades de investigação?

O host de tempo de execução do .NET Core produzirá mensagens de rastreamento úteis quando determinadas variáveis de ambiente estiverem habilitadas:

|Variável de ambiente        |Descrição  |
|----------------------------|---------|
|`COREHOST_TRACE=1`          |Habilita o rastreamento.|
|`COREHOST_TRACEFILE=<path>` |Rastreia um caminho de arquivo em vez do padrão `stderr` .|
|`COREHOST_TRACE_VERBOSITY`  |Define o detalhamento de 1 (menor) para 4 (mais alto).|

## <a name="managed-assembly-default-probing"></a>Investigação padrão do assembly gerenciado

Ao investigar para localizar um assembly gerenciado, o <xref:System.Runtime.Loader.AssemblyLoadContext.Default%2A?displayProperty=nameWithType> procura na ordem em:

- Arquivos que correspondem <xref:System.Reflection.AssemblyName.Name?displayProperty=nameWithType> ao `TRUSTED_PLATFORM_ASSEMBLIES` (após a remoção de extensões de arquivo).
- Arquivos de assembly de imagem nativa no `APP_NI_PATHS` com extensões de arquivo comuns.
- Arquivos de assembly no `APP_PATHS` com extensões de arquivo comuns.

## <a name="satellite-resource-assembly-probing"></a>Investigação de assembly satélite (recurso)

Para localizar um assembly satélite para uma cultura específica, construa um conjunto de caminhos de arquivo.

Para cada caminho no `PLATFORM_RESOURCE_ROOTS` e, em seguida `APP_PATHS` , acrescente a <xref:System.Globalization.CultureInfo.Name?displayProperty=nameWithType> cadeia de caracteres, um separador de diretório, a <xref:System.Reflection.AssemblyName.Name?displayProperty=nameWithType> cadeia de caracteres e a extensão '. dll '.

Se existir algum arquivo correspondente, tente carregá-lo e retorná-lo.

## <a name="unmanaged-native-library-probing"></a>Investigação de biblioteca não gerenciada (nativa)

Ao investigar para localizar uma biblioteca não gerenciada, o `NATIVE_DLL_SEARCH_DIRECTORIES` é pesquisado procurando por uma biblioteca correspondente.
