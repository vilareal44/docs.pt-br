---
title: Recursos em aplicativos .NET
description: Entenda os recursos nos aplicativos .NET. Um recurso é qualquer dado não executável que é implantado logicamente com um aplicativo.
ms.date: 07/25/2018
helpviewer_keywords:
- deploying applications [.NET Framework], resources
- deploying applications [.NET Core], resources
- application resources
- resource files
- satellite assemblies
- localization
- packaging application resources
- localizing resources
ms.assetid: 8ad495d4-2941-40cf-bf64-e82e85825890
ms.openlocfilehash: 105325170389917bfb2022314791aa1ed5923db3
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86865158"
---
# <a name="resources-in-net-apps"></a>Recursos em aplicativos .NET

Quase todos os aplicativos de qualidade de produção precisam usar recursos. Um recurso é qualquer dado não executável que está implantado de modo lógico com um aplicativo. Um recurso pode ser exibido em um aplicativo como mensagens de erro ou como parte da interface do usuário. Os recursos podem conter dados em vários formatos, incluindo cadeias de caracteres, imagens e objetos persistentes. (Para gravar objetos persistentes em um arquivo de recurso, os objetos devem ser serializáveis.) O armazenamento de dados em um arquivo de recurso permite que você altere os dados sem recompilar o aplicativo inteiro. Também é possível armazenar dados em um único local e eliminar a necessidade de depender de dados embutidos em código que são armazenados em vários locais.

O .NET Framework e o .NET Core fornecem suporte abrangente para a criação e a localização de recursos. Além disso, o .NET dá suporte a um modelo simples de empacotamento e implantação de recursos localizados.

Para saber mais sobre recursos no ASP.NET, confira [Visão Geral sobre Recursos de Página da Web ASP.NET](https://docs.microsoft.com/previous-versions/aspnet/ms227427(v=vs.100)).

## <a name="create-and-localize-resources"></a>Criar e localizar recursos

Em um aplicativo não localizado, você pode usar arquivos de recurso como um repositório para dados de aplicativo, especialmente para cadeias de caracteres que poderiam ser de alguma forma embutidas em código em vários locais no código-fonte. Normalmente, você cria recursos como arquivos de texto (.txt) ou XML (.resx) e usa [Resgen.exe (Gerador de Arquivo de Recurso)](../tools/resgen-exe-resource-file-generator.md) para compilá-los em arquivos de recursos binários. Esses arquivos podem ser inseridos no arquivo executável do aplicativo por um compilador de linguagem. Para saber mais sobre como criar recursos, confira [Criando arquivos de recurso](creating-resource-files-for-desktop-apps.md).

Você também pode localizar os recursos do seu aplicativo para culturas específicas. Isso permite que criar versões localizadas (traduzidas) dos aplicativos. Ao desenvolver um aplicativo que usa recursos localizados, você designa uma cultura que atua como a cultura neutra ou de fallback cujos recursos serão usados se não houver recursos adequados disponíveis. Normalmente, os recursos da cultura neutra são armazenados no executável do aplicativo. Os demais recursos para culturas localizadas individuais são armazenados nos assemblies satélite autônomos. Para saber mais, confira [Criando assemblies satélite](creating-satellite-assemblies-for-desktop-apps.md).

## <a name="package-and-deploy-resources"></a>Empacotar e implantar recursos

Implante recursos de aplicativo localizados em [assemblies satélite](packaging-and-deploying-resources-in-desktop-apps.md). Um assembly satélite contém os recursos de uma única cultura; ele não contém qualquer código de aplicativo. No modelo de implantação do assembly satélite, você cria um aplicativo com um assembly padrão (que geralmente é o assembly principal) e um assembly satélite para cada cultura com a qual o aplicativo é compatível. Como os assemblies satélite não fazem parte do assembly principal, você pode substituir ou atualizar facilmente os recursos correspondentes a uma cultura específica sem substituir o assembly principal do aplicativo.

Determine cuidadosamente quais recursos vão compor o assembly do recurso padrão do aplicativo. Como ele faz parte do assembly principal, qualquer mudança nele exigirá que você substitua o assembly principal. Caso você não forneça um recurso padrão, uma exceção será gerada quando o [processo de fallback do recurso](packaging-and-deploying-resources-in-desktop-apps.md) tentar encontrá-lo. Em um aplicativo bem projetado, o uso de recursos nunca deve gerar uma exceção.

Para saber mais, confira o artigo [Empacotamento e implantação de recursos](packaging-and-deploying-resources-in-desktop-apps.md).

## <a name="retrieve-resources"></a>Recuperar recursos

No tempo de execução, um aplicativo carrega os recursos localizados apropriados por thread, com base na cultura especificada pela propriedade <xref:System.Globalization.CultureInfo.CurrentUICulture%2A?displayProperty=nameWithType>. Esse valor da propriedade é derivado da seguinte maneira:

- Atribuindo diretamente um objeto <xref:System.Globalization.CultureInfo> que representa a cultura localizada para a propriedade <xref:System.Threading.Thread.CurrentUICulture%2A?displayProperty=nameWithType>.

- Se não houver uma cultura explicitamente designada, recuperando a cultura padrão da interface do usuário do thread pela propriedade <xref:System.Globalization.CultureInfo.DefaultThreadCurrentUICulture%2A?displayProperty=nameWithType>.

- Se uma cultura de interface do usuário do thread padrão não for explicitamente atribuída, recuperando a cultura para o usuário atual no computador local. As implementações do .NET em execução no Windows fazem isso chamando a função do Windows [`GetUserDefaultUILanguage`](/windows/desktop/api/winnls/nf-winnls-getuserdefaultuilanguage) .

Para saber mais sobre a configuração da cultura da interface do usuário atual, confira as páginas de referência <xref:System.Globalization.CultureInfo> e <xref:System.Globalization.CultureInfo.CurrentUICulture%2A?displayProperty=nameWithType>.

Você pode recuperar recursos para a cultura da interface do usuário atual ou para uma cultura específica usando a classe <xref:System.Resources.ResourceManager?displayProperty=nameWithType>. Embora a classe <xref:System.Resources.ResourceManager> seja mais frequentemente usada para recuperar recursos, o namespace <xref:System.Resources?displayProperty=nameWithType> contém tipos adicionais que podem ser usados para recuperar recursos. Elas incluem:

- A classe <xref:System.Resources.ResourceReader>, que permite enumerar recursos inseridos em um assembly ou armazenados em um arquivo de recurso binário autônomo. Ela é útil quando você desconhece os nomes exatos dos recursos que estão disponíveis no tempo de execução.

- A classe <xref:System.Resources.ResXResourceReader>, que permite recuperar recursos de um arquivo XML (.resx).

- A classe <xref:System.Resources.ResourceSet>, que permite recuperar os recursos de uma cultura específica sem observar regras de fallback. Os recursos podem ser armazenados em um assembly ou em um arquivo de recursos binário autônomo. Você também pode desenvolver uma implementação <xref:System.Resources.IResourceReader> que permite usar a classe <xref:System.Resources.ResourceSet> para recuperar recursos de outra fonte.

- A classe <xref:System.Resources.ResXResourceSet>, que permite recuperar todos os itens em um arquivo de recurso XML na memória.

## <a name="see-also"></a>Veja também

- <xref:System.Globalization.CultureInfo>
- <xref:System.Globalization.CultureInfo.CurrentUICulture%2A?displayProperty=nameWithType>
- [Criação de arquivos de recurso](creating-resource-files-for-desktop-apps.md)
- [Empacotando e implantando recursos](packaging-and-deploying-resources-in-desktop-apps.md)
- [Criação de assemblies satélite](creating-satellite-assemblies-for-desktop-apps.md)
- [Recuperando recursos](retrieving-resources-in-desktop-apps.md)
