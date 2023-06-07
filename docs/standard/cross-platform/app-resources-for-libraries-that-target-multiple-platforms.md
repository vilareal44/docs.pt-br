---
title: Recursos do aplicativo para bibliotecas direcionadas a várias plataformas
ms.date: 07/18/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- multiple platforms, resources for
- resources [.NET Framework]
- .NET Framework, resources when targeting multiple platforms
- resources, for multiple platforms
- targeting multiple platforms, resources for
ms.assetid: 72c76f0b-7255-4576-9261-3587f949669c
ms.openlocfilehash: a2d02a8ebe5e2611db3bc284bb022470ff77f601
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84290351"
---
# <a name="app-resources-for-libraries-that-target-multiple-platforms"></a>Recursos do aplicativo para bibliotecas direcionadas a várias plataformas
Você pode usar o .NET Framework tipo de projeto de [biblioteca de classes portátil](cross-platform-development-with-the-portable-class-library.md) para garantir que os recursos em suas bibliotecas de classes possam ser acessados de várias plataformas. Esse tipo de projeto está disponível no Visual Studio 2012 e tem como alvo o subconjunto portátil da biblioteca de classes de .NET Framework. O uso de uma biblioteca de classes portátil garante que sua biblioteca possa ser acessada de aplicativos da área de trabalho, aplicativos do Silverlight, Windows Phone aplicativos e aplicativos da loja do Windows 8. x.

[!INCLUDE[standard](../../../includes/pcl-to-standard.md)]

 O projeto de biblioteca de classes portátil torna apenas um subconjunto muito limitado dos tipos no <xref:System.Resources> namespace disponível para seu aplicativo, mas permite que você use a <xref:System.Resources.ResourceManager> classe para recuperar recursos. No entanto, se estiver criando um aplicativo com o Visual Studio, você deverá usar o wrapper fortemente tipado criado pelo Visual Studio em vez de usar a classe <xref:System.Resources.ResourceManager> diretamente.

 Para criar um wrapper com rigidez de tipos no Visual Studio, defina o **modificador de acesso** do arquivo de recurso principal no designer de recursos do Visual Studio como **público**. Isso cria um arquivo [resourceFileName].designer.cs ou [resourceFileName].designer.vb que contém o wrapper ResourceManager fortemente tipado. Para obter mais informações sobre como usar um wrapper de recursos com rigidez de tipos, consulte a seção "gerando uma classe de recurso fortemente tipado" no tópico [Resgen. exe (gerador de arquivo de recurso)](../../framework/tools/resgen-exe-resource-file-generator.md) .

## <a name="resource-manager-in-the-portable-class-library"></a>Gerenciador de recursos na biblioteca de classes portátil
 Em um projeto de biblioteca de classes portátil, todo o acesso a recursos é manipulado pela <xref:System.Resources.ResourceManager> classe. Como os tipos no <xref:System.Resources> namespace, como <xref:System.Resources.ResourceReader> e <xref:System.Resources.ResourceSet> , não são acessíveis de um projeto de biblioteca de classes portátil, eles não podem ser usados para acessar recursos.

 O projeto de biblioteca de classes portátil inclui os quatro <xref:System.Resources.ResourceManager> Membros listados na tabela a seguir. Esses construtores e métodos permitem que você crie uma instância de um objeto <xref:System.Resources.ResourceManager> e recupere recursos de cadeia de caracteres.

|Membro do `ResourceManager`|Description|
|------------------------------|-----------------|
|<xref:System.Resources.ResourceManager.%23ctor%28System.String%2CSystem.Reflection.Assembly%29>|Cria uma instância de <xref:System.Resources.ResourceManager> para acessar o arquivo de recurso nomeado encontrado no assembly especificado.|
|<xref:System.Resources.ResourceManager.%23ctor%28System.Type%29>|Cria uma instância de <xref:System.Resources.ResourceManager> que corresponde ao tipo especificado.|
|<xref:System.Resources.ResourceManager.GetString%28System.String%29>|Recupera um recurso nomeado para a cultura atual.|
|<xref:System.Resources.ResourceManager.GetString%28System.String%2CSystem.Globalization.CultureInfo%29>|Recupera um recurso nomeado que pertence à cultura especificada.|

 A exclusão de outros <xref:System.Resources.ResourceManager> membros da biblioteca de classes portátil significa que objetos serializados, dados não de cadeia de caracteres e imagens não podem ser recuperados de um arquivo de recurso. Para usar recursos de uma biblioteca de classes portátil, você deve armazenar todos os dados de objeto no formulário de cadeia de caracteres. Por exemplo, você pode armazenar valores numéricos em um arquivo de recurso convertendo-os em cadeias de caracteres, e pode recuperá-los e convertê-los novamente em números ao usar o método `Parse` ou `TryParse` do tipo de dados numéricos. Você pode converter imagens ou outros dados binários em uma representação de cadeia de caracteres ao chamar o método <xref:System.Convert.ToBase64String%2A?displayProperty=nameWithType> e restaurá-los para uma matriz de bytes ao chamar o método <xref:System.Convert.FromBase64String%2A?displayProperty=nameWithType>.

## <a name="the-portable-class-library-and-windows-store-apps"></a>A biblioteca de classes portátil e os aplicativos da Windows Store
 Os projetos de biblioteca de classes portáteis armazenam recursos em arquivos. resx, que são então compilados em arquivos. Resources e inseridos no assembly principal ou em assemblies satélites em tempo de compilação. Por outro lado, os aplicativos da loja do Windows 8. x exigem que os recursos sejam armazenados em arquivos. resw, que são compilados em um único arquivo PRI (índice de recursos de pacote). No entanto, apesar dos formatos de arquivo incompatíveis, sua biblioteca de classes portátil funcionará em um aplicativo de armazenamento do Windows 8. x.

 Para consumir sua biblioteca de classes de um aplicativo da loja do Windows 8. x, adicione uma referência a ela em seu projeto de aplicativo da Windows Store. O Visual Studio extrairá de forma transparente os recursos do assembly para um arquivo. resw e o usará para gerar um arquivo PRI do qual o Windows Runtime pode extrair recursos. Em tempo de execução, o Windows Runtime executa o código em sua biblioteca de classes portátil, mas recupera os recursos da sua biblioteca de classes portáteis do arquivo PRI.

 Se o seu projeto de biblioteca de classes portátil incluir recursos localizados, você usará o modelo Hub e spoke para implantá-los exatamente como faria para uma biblioteca em um aplicativo de área de trabalho. Para consumir o arquivo de recurso principal e todos os arquivos de recursos localizados em seu aplicativo da loja do Windows 8. x, você adiciona uma referência ao assembly principal. No tempo de compilação, o Visual Studio extrai os recursos do arquivo de recursos principal e quaisquer arquivos de recursos localizados em arquivos .resw separados. Em seguida, ele compila os arquivos. resw em um único arquivo PRI que o Windows Runtime acessa em tempo de execução.

<a name="NonLoc"></a>
## <a name="example-non-localized-portable-class-library"></a>Exemplo: biblioteca de classes portátil não localizada
 O exemplo de biblioteca de classes portátil simples e não localizado a seguir usa recursos para armazenar os nomes de colunas e para determinar o número de caracteres a serem reservados para dados tabulares. O exemplo usa um arquivo chamado LibResources.resx para armazenar os recursos de cadeia de caracteres listados na tabela a seguir.

|Nome do recurso|Valor do recurso|
|-------------------|--------------------|
|Born|Birthdate|
|BornLength|12|
|Hired|Hire Date|
|HiredLength|12|
|ID|ID|
|ID.Length|12|
|Nome|Nome|
|NameLength|25|
|Título|Employee Database|

 O código a seguir define uma `UILibrary` classe que usa o wrapper do Gerenciador de recursos chamado `resources` gerado pelo Visual Studio quando o **modificador de acesso** do arquivo é alterado para **público**. A classe UILibrary analisa os dados de cadeia de caracteres conforme o necessário. . Observe que a classe está no namespace `MyCompany.Employees`.

 [!code-csharp[Conceptual.Resources.Portable#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.portable/cs/uilibrary.cs#1)]
 [!code-vb[Conceptual.Resources.Portable#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.portable/vb/uilibrary.vb#1)]

 O código a seguir ilustra como a classe `UILibrary` e seus recursos podem ser acessados de um aplicativo no modo de console. Ele requer uma referência a UILibrary. dll a ser adicionada ao projeto de aplicativo de console.

 [!code-csharp[Conceptual.Resources.Portable#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.portable/cs/program.cs#2)]
 [!code-vb[Conceptual.Resources.Portable#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.portable/vb/module1.vb#2)]

 O código a seguir ilustra como a `UILibrary` classe e seus recursos podem ser acessados de um aplicativo da loja do Windows 8. x. Ele requer uma referência a UILibrary. dll para ser adicionado ao projeto de aplicativo da Windows Store.

 [!code-csharp[Conceptual.Resources.PortableMetro#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.portablemetro/cs/blankpage.xaml.cs#1)]

## <a name="example-localized-portable-class-library"></a>Exemplo: biblioteca de classes portátil localizada
 O exemplo de biblioteca de classes portátil localizada a seguir inclui recursos para as culturas francesa (França) e inglês (Estados Unidos). A cultura em inglês (Estados Unidos) é a cultura padrão do aplicativo; seus recursos são mostrados na tabela na [seção anterior](app-resources-for-libraries-that-target-multiple-platforms.md#NonLoc). O arquivo de recursos para francês (França) é chamado LibResources.fr-FR.resx e consiste nos recursos de cadeia de caracteres listados na tabela a seguir. O código-fonte para a classe `UILibrary` é o mesmo mostrado na seção anterior.

|Nome do recurso|Valor do recurso|
|-------------------|--------------------|
|Born|Date de naissance|
|BornLength|20|
|Hired|Date embauché|
|HiredLength|16|
|ID|ID|
|Nome|Nom|
|Título|Base de données des employés|

 O código a seguir ilustra como a classe `UILibrary` e seus recursos podem ser acessados de um aplicativo no modo de console. Ele requer uma referência a UILibrary. dll a ser adicionada ao projeto de aplicativo de console.

 [!code-csharp[Conceptual.Resources.Portable#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.portable/cs/program2.cs#3)]
 [!code-vb[Conceptual.Resources.Portable#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.portable/vb/module2.vb#3)]

 O código a seguir ilustra como a `UILibrary` classe e seus recursos podem ser acessados de um aplicativo da loja do Windows 8. x. Ele requer uma referência a UILibrary. dll para ser adicionado ao projeto de aplicativo da Windows Store. Usa a propriedade estática `ApplicationLanguages.PrimaryLanguageOverride` para definir o idioma preferencial do aplicativo como francês.

 [!code-csharp[Conceptual.Resources.PortableMetroLoc#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.portablemetroloc/cs/blankpage.xaml.cs#1)]
 [!code-vb[Conceptual.Resources.PortableMetroLoc#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.portablemetroloc/vb/blankpage.xaml.vb#1)]  
  
## <a name="see-also"></a>Veja também

- <xref:System.Resources.ResourceManager>
- [Recursos em aplicativos da área de trabalho](../../framework/resources/index.md)
- [Empacotando e implantando recursos](../../framework/resources/packaging-and-deploying-resources-in-desktop-apps.md)
