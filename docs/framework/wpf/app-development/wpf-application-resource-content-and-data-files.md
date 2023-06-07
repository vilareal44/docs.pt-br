---
title: Recurso de aplicativo, conteúdo e arquivos de dados
description: Saiba mais sobre o suporte especial para configurar, identificar e usar arquivos de dados de aplicativo no Windows Presentation Foundation (WPF).
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WPF application [WPF], files
- loose resources [WPF]
- content files [WPF]
- Site of Origin files [WPF]
- resource files [WPF]
- remote files [WPF]
- embedded resources [WPF]
- files [WPF]
- referencing application files [WPF]
- application development [WPF], files
- application management [WPF]
ms.assetid: 7ad2943b-3961-41d3-8fc6-1582d43f5d99
ms.openlocfilehash: 324b3eb922f0fd1d1d9ad00105a06a7fbdb8effd
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85622855"
---
# <a name="wpf-application-resource-content-and-data-files"></a>Arquivos de recurso, conteúdo e dados do aplicativo WPF
Os aplicativos do Microsoft Windows geralmente dependem de arquivos que contêm dados não executáveis, como [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] imagens, vídeos e áudio. O Windows Presentation Foundation (WPF) oferece suporte especial para configurar, identificar e usar esses tipos de arquivos de dados, que são chamados de arquivos de dados de aplicativo. Esse suporte gira em torno de um conjunto específico de tipos de arquivo de dados do aplicativo, incluindo:  
  
- **Arquivos de recurso**: Arquivos de dados que são compilados em um assembly executável ou de biblioteca do WPF.  
  
- **Arquivos de conteúdo**: Arquivos de dados autônomos que têm uma associação explícita com um assembly executável do WPF.  
  
- **Arquivos de site de origem**: Arquivos de dados autônomos que não têm associação com um assembly do WPF executável.  
  
 Uma distinção importante a se fazer entre esses três tipos de arquivos é que os arquivos de recurso e os arquivos de conteúdo são conhecidos em tempo de build; um assembly tem o conhecimento explícito sobre eles. Para arquivos de site de origem, no entanto, um assembly pode não ter nenhum conhecimento sobre eles ou conhecimento implícito por meio de uma referência de URI (identificador de recurso uniforme) de pacote; o caso do último, não há nenhuma garantia de que o site de origem do arquivo referenciado realmente existe.  
  
 Para fazer referência a arquivos de dados de aplicativo, Windows Presentation Foundation (WPF) usa o esquema do URI (Uniform Resource Identifier) do Pack, que é descrito em detalhes em [URIs de pacote no WPF](pack-uris-in-wpf.md).  
  
 Este tópico descreve como configurar e usar arquivos de dados do aplicativo.  

<a name="Resource_Files"></a>
## <a name="resource-files"></a>Arquivos de recursos  
 Se um arquivo de dados do aplicativo precisar estar sempre disponível para um aplicativo, a única maneira de garantir essa disponibilidade será compilá-lo em um assembly executável principal do aplicativo ou em um de seus assemblies referenciados. Esse tipo de arquivo de dados de aplicativo é conhecido como um *arquivo de recurso*.  
  
 Você deve usar arquivos de recurso quando:  
  
- Você não precisa atualizar o conteúdo do arquivo de recurso depois que ele é compilado em um assembly.  
  
- Você deseja simplificar a complexidade da distribuição de aplicativos, reduzindo o número de dependências de arquivo.  
  
- O arquivo de dados do aplicativo precisa ser localizável (consulte [visão geral de globalização e localização do WPF](../advanced/wpf-globalization-and-localization-overview.md)).  
  
> [!NOTE]
> Os arquivos de recursos descritos nesta seção são diferentes dos arquivos de recursos descritos em [recursos XAML](../../../desktop-wpf/fundamentals/xaml-resources-define.md) e diferentes dos recursos incorporados ou vinculados descritos em [gerenciar recursos do aplicativo (.net)](/visualstudio/ide/managing-application-resources-dotnet).  
  
### <a name="configuring-resource-files"></a>Configurando arquivos de recurso  
 No WPF, um arquivo de recurso é um arquivo que é incluído em um projeto do Microsoft Build Engine (MSBuild) como um `Resource` Item.  
  
```xml  
<Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003" ... >  
  ...  
  <ItemGroup>  
    <Resource Include="ResourceFile.xaml" />  
  </ItemGroup>  
  ...  
</Project>  
```  
  
> [!NOTE]
> No Visual Studio, você cria um arquivo de recurso adicionando um arquivo a um projeto e definindo seu `Build Action` como `Resource` .  
  
 Quando o projeto é compilado, o MSBuild compila o recurso no assembly.  
  
### <a name="using-resource-files"></a>Usando arquivos de recurso  
 Para carregar um arquivo de recurso, você pode chamar o <xref:System.Windows.Application.GetResourceStream%2A> método da <xref:System.Windows.Application> classe, passando um URI de pacote que identifica o arquivo de recurso desejado. <xref:System.Windows.Application.GetResourceStream%2A>Retorna um <xref:System.Windows.Resources.StreamResourceInfo> objeto, que expõe o arquivo de recurso como um <xref:System.IO.Stream> e descreve seu tipo de conteúdo.  
  
 Por exemplo, o código a seguir mostra como usar o <xref:System.Windows.Application.GetResourceStream%2A> para carregar um <xref:System.Windows.Controls.Page> arquivo de recurso e defini-lo como o conteúdo de um <xref:System.Windows.Controls.Frame> ( `pageFrame` ):  
  
 [!code-csharp[WPFAssemblyResourcesSnippets#LoadAPageResourceFileManuallyCODE](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFAssemblyResourcesSnippets/CSharp/ResourcesSample/ApplicationGetResourceStreamSnippetWindow.xaml.cs#loadapageresourcefilemanuallycode)]
 [!code-vb[WPFAssemblyResourcesSnippets#LoadAPageResourceFileManuallyCODE](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WPFAssemblyResourcesSnippets/VisualBasic/ResourcesSample/ApplicationGetResourceStreamSnippetWindow.xaml.vb#loadapageresourcefilemanuallycode)]  
  
 Embora <xref:System.Windows.Application.GetResourceStream%2A> a chamada forneça acesso ao <xref:System.IO.Stream> , você precisa executar o trabalho adicional de convertê-lo para o tipo da propriedade com a qual você irá configurá-lo. Em vez disso, você pode deixar o WPF cuidar de abrir e converter o <xref:System.IO.Stream> carregando um arquivo de recurso diretamente na propriedade de um tipo usando código.  
  
 O exemplo a seguir mostra como carregar um <xref:System.Windows.Controls.Page> diretamente em um <xref:System.Windows.Controls.Frame> ( `pageFrame` ) usando código.  
  
 [!code-csharp[WPFAssemblyResourcesSnippets#LoadPageResourceFileFromCODE](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFAssemblyResourcesSnippets/CSharp/ResourcesSample/ApplicationGetResourceStreamSnippetWindow.xaml.cs#loadpageresourcefilefromcode)]
 [!code-vb[WPFAssemblyResourcesSnippets#LoadPageResourceFileFromCODE](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WPFAssemblyResourcesSnippets/VisualBasic/ResourcesSample/ApplicationGetResourceStreamSnippetWindow.xaml.vb#loadpageresourcefilefromcode)]  
  
 O exemplo a seguir é um exemplo de marcação equivalente ao anterior.  
  
 [!code-xaml[WPFAssemblyResourcesSnippets#LoadPageResourceFileFromXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFAssemblyResourcesSnippets/CSharp/ResourcesSample/ApplicationGetResourceStreamSnippetWindow.xaml#loadpageresourcefilefromxaml)]  
  
### <a name="application-code-files-as-resource-files"></a>Arquivos de código de aplicativo como arquivos de recurso  
 Um conjunto especial de arquivos de código do aplicativo WPF pode ser referenciado usando URIs de pacote, incluindo janelas, páginas, documentos de fluxo e dicionários de recursos. Por exemplo, você pode definir a <xref:System.Windows.Application.StartupUri%2A?displayProperty=nameWithType> propriedade com um pacote URI que faz referência à janela ou à página que você deseja carregar quando um aplicativo é iniciado.  
  
 [!code-xaml[WPFAssemblyResourcesSnippets#SetApplicationStartupURI](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFAssemblyResourcesSnippets/CSharp/ResourcesSample/App.xaml#setapplicationstartupuri)]  
  
 Você pode fazer isso quando um arquivo XAML é incluído em um projeto do MSBuild como um `Page` Item.  
  
```xml  
<Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003" ... >  
  ...  
  <ItemGroup>  
    <Page Include="MainWindow.xaml" />  
  </ItemGroup>  
  ...  
</Project>  
```  
  
> [!NOTE]
> No Visual Studio, você adiciona um novo <xref:System.Windows.Window> , <xref:System.Windows.Navigation.NavigationWindow> , <xref:System.Windows.Controls.Page> , <xref:System.Windows.Documents.FlowDocument> ou <xref:System.Windows.ResourceDictionary> a um projeto, o `Build Action` para o arquivo de marcação usará como padrão `Page` .  
  
 Quando um projeto com `Page` itens é compilado, os [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] itens são convertidos em formato binário e compilados no assembly associado. Consequentemente, esses arquivos podem ser usados da mesma maneira que arquivos de recurso típicos.  
  
> [!NOTE]
> Se um [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] arquivo estiver configurado como um `Resource` Item e não tiver um arquivo code-behind, o RAW [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] será compilado em um assembly em vez de uma versão binária do RAW [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] .  
  
<a name="Content_Files"></a>
## <a name="content-files"></a>Arquivos de Conteúdo  
 Um *arquivo de conteúdo* é distribuído como um arquivo flexível junto com um assembly executável. Embora eles não sejam compilados em um assembly, os assemblies são compilados com metadados que estabelecem uma associação com cada arquivo de conteúdo.  
  
 Você deve usar arquivos de conteúdo quando o aplicativo requer um conjunto específico de arquivos de dados do aplicativo que você deseja ser capaz de atualizar sem recompilar o assembly que os consome.  
  
### <a name="configuring-content-files"></a>Configurando arquivos de conteúdo  
 Para adicionar um arquivo de conteúdo a um projeto, um arquivo de dados do aplicativo deve ser incluído como um `Content` Item. Além disso, como um arquivo de conteúdo não é compilado diretamente no assembly, você precisa definir o elemento de metadados do MSBuild `CopyToOutputDirectory` para especificar que o arquivo de conteúdo seja copiado para um local relativo ao assembly compilado. Se você quiser que o recurso seja copiado para a pasta de saída de compilação toda vez que um projeto for compilado, defina o `CopyToOutputDirectory` elemento de metadados com o `Always` valor. Caso contrário, você pode garantir que apenas a versão mais recente do recurso seja copiada para a pasta de saída de compilação usando o `PreserveNewest` valor.  
  
 A seguir é mostrado um arquivo que está configurado como um arquivo de conteúdo que é copiado para a pasta de saída de build somente quando uma nova versão do recurso é adicionada ao projeto.  
  
```xml  
<Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003" ... >  
  ...  
  <ItemGroup>  
    <Content Include="ContentFile.xaml">  
      <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>  
    </Content>  
  </ItemGroup>  
  ...  
</Project>  
```  
  
> [!NOTE]
> No Visual Studio, você cria um arquivo de conteúdo adicionando um arquivo a um projeto e definindo seu como e `Build Action` `Content` define seu `Copy to Output Directory` como `Copy always` (igual a `Always` ) e `Copy if newer` (o mesmo que `PreserveNewest` ).  
  
 Quando o projeto é compilado, um <xref:System.Windows.Resources.AssemblyAssociatedContentFileAttribute> atributo é compilado nos metadados do assembly para cada arquivo de conteúdo.  
  
 `[assembly: AssemblyAssociatedContentFile("ContentFile.xaml")]`  
  
 O valor de <xref:System.Windows.Resources.AssemblyAssociatedContentFileAttribute> implica o caminho para o arquivo de conteúdo relativo à sua posição no projeto. Por exemplo, se um arquivo de conteúdo estava localizado em uma subpasta do projeto, as informações de caminho adicionais seriam incorporadas ao <xref:System.Windows.Resources.AssemblyAssociatedContentFileAttribute> valor.  
  
 `[assembly: AssemblyAssociatedContentFile("Resources/ContentFile.xaml")]`  
  
 O <xref:System.Windows.Resources.AssemblyAssociatedContentFileAttribute> valor também é o valor do caminho para o arquivo de conteúdo na pasta de saída de compilação.  
  
### <a name="using-content-files"></a>Usando arquivos de conteúdo  
 Para carregar um arquivo de conteúdo, você pode chamar o <xref:System.Windows.Application.GetContentStream%2A> método da <xref:System.Windows.Application> classe, passando um URI de pacote que identifica o arquivo de conteúdo desejado. <xref:System.Windows.Application.GetContentStream%2A>Retorna um <xref:System.Windows.Resources.StreamResourceInfo> objeto, que expõe o arquivo de conteúdo como um <xref:System.IO.Stream> e descreve seu tipo de conteúdo.  
  
 Por exemplo, o código a seguir mostra como usar o <xref:System.Windows.Application.GetContentStream%2A> para carregar um <xref:System.Windows.Controls.Page> arquivo de conteúdo e defini-lo como o conteúdo de um <xref:System.Windows.Controls.Frame> ( `pageFrame` ).  
  
 [!code-csharp[WPFAssemblyResourcesSnippets#LoadAPageContentFileManuallyCODE](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFAssemblyResourcesSnippets/CSharp/ResourcesSample/ApplicationGetContentStreamSnippetWindow.xaml.cs#loadapagecontentfilemanuallycode)]
 [!code-vb[WPFAssemblyResourcesSnippets#LoadAPageContentFileManuallyCODE](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WPFAssemblyResourcesSnippets/VisualBasic/ResourcesSample/ApplicationGetContentStreamSnippetWindow.xaml.vb#loadapagecontentfilemanuallycode)]  
  
 Embora <xref:System.Windows.Application.GetContentStream%2A> a chamada forneça acesso ao <xref:System.IO.Stream> , você precisa executar o trabalho adicional de convertê-lo para o tipo da propriedade com a qual você irá configurá-lo. Em vez disso, você pode deixar o WPF cuidar de abrir e converter o <xref:System.IO.Stream> carregando um arquivo de recurso diretamente na propriedade de um tipo usando código.  
  
 O exemplo a seguir mostra como carregar um <xref:System.Windows.Controls.Page> diretamente em um <xref:System.Windows.Controls.Frame> ( `pageFrame` ) usando código.  
  
 [!code-csharp[WPFAssemblyResourcesSnippets#LoadPageContentFileFromCODE](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFAssemblyResourcesSnippets/CSharp/ResourcesSample/ApplicationGetContentStreamSnippetWindow.xaml.cs#loadpagecontentfilefromcode)]
 [!code-vb[WPFAssemblyResourcesSnippets#LoadPageContentFileFromCODE](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WPFAssemblyResourcesSnippets/VisualBasic/ResourcesSample/ApplicationGetContentStreamSnippetWindow.xaml.vb#loadpagecontentfilefromcode)]  
  
 O exemplo a seguir é um exemplo de marcação equivalente ao anterior.  
  
 [!code-xaml[WPFAssemblyResourcesSnippets#LoadPageContentFileFromXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFAssemblyResourcesSnippets/CSharp/ResourcesSample/ApplicationGetContentStreamSnippetWindow.xaml#loadpagecontentfilefromxaml)]  
  
<a name="Site_of_Origin_Files"></a>
## <a name="site-of-origin-files"></a>Arquivos de site de origem  
 Os arquivos de recurso têm uma relação explícita com os assemblies que eles são distribuídos juntamente com o, conforme definido pelo <xref:System.Windows.Resources.AssemblyAssociatedContentFileAttribute> . Mas, às vezes você deseja estabelecer um relacionamento implícito ou inexistente entre um assembly e um arquivo de dados do aplicativo, como quando:  
  
- Um arquivo não existe no momento da compilação.  
  
- Você não sabe quais arquivos seu assembly vai requerer até o tempo de execução.  
  
- Você deseja ser capaz de atualizar os arquivos sem recompilar o assembly ao qual eles estão associados.  
  
- O aplicativo usa arquivos de dados grandes, como áudio e vídeo, e você só deseja que os usuários os baixem caso desejem.  
  
 É possível carregar esses tipos de arquivos usando esquemas de URI tradicionais, como os `file:///` `http://` esquemas e.  
  
 [!code-xaml[WPFAssemblyResourcesSnippets#AbsolutePackUriFileHttpReferenceXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFAssemblyResourcesSnippets/CSharp/ResourcesSample/AbsolutePackUriPage.xaml#absolutepackurifilehttpreferencexaml)]  
  
 No entanto, os `file:///` `http://` esquemas e exigem que seu aplicativo tenha confiança total. Se seu aplicativo for um aplicativo de navegador XAML (XBAP) que foi iniciado pela Internet ou intranet e solicitar somente o conjunto de permissões permitido para aplicativos iniciados nesses locais, os arquivos flexíveis só poderão ser carregados a partir do site de origem do aplicativo (local de inicialização). Esses arquivos são conhecidos como arquivos *de site de origem* .  
  
 Os arquivos de site de origem são a única opção para aplicativos parcialmente confiáveis, apesar de não serem limitados a aplicativos parcialmente confiáveis. Os aplicativos de confiança total ainda podem precisar carregar arquivos de dados do aplicativo que eles não conhecem em tempo de build. Embora os aplicativos de confiança total possam utilizar file:///, é provável que os arquivos de dados do aplicativo sejam instalados na mesma pasta ou em uma subpasta do assembly do aplicativo. Nesse caso, usar a referência do site de origem é mais fácil do que usar file:///, porque o uso de file:/// requer que você descubra o caminho completo do arquivo.  
  
> [!NOTE]
> Os arquivos de site de origem não são armazenados em cache com um aplicativo de navegador XAML (XBAP) em um computador cliente, enquanto os arquivos de conteúdo são. Consequentemente, eles são baixados somente quando solicitado especificamente. Se um aplicativo XBAP (aplicativo de navegador XAML) tiver arquivos de mídia grandes, configurá-los como arquivos de site de origem significa que a inicialização inicial do aplicativo é muito mais rápida e os arquivos só serão baixados sob demanda.  
  
### <a name="configuring-site-of-origin-files"></a>Configurando arquivos de site de origem  
 Se os arquivos do site de origem forem inexistentes ou desconhecidos no momento da compilação, você precisará usar mecanismos de implantação tradicionais para garantir que os arquivos necessários estejam disponíveis em tempo de execução, incluindo o uso do `XCopy` programa de linha de comando ou do Microsoft Windows Installer.  
  
 Se você souber em tempo de compilação os arquivos que deseja que estejam localizados no site de origem, mas ainda quiser evitar uma dependência explícita, poderá adicionar esses arquivos a um projeto do MSBuild como `None` Item. Assim como os arquivos de conteúdo, você precisa definir o `CopyToOutputDirectory` atributo MSBuild para especificar que o arquivo de site de origem seja copiado para um local relativo ao assembly compilado, especificando o `Always` valor ou o `PreserveNewest` valor.  
  
```xml  
<Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003" ... >  
  ...  
  <None Include="PageSiteOfOriginFile.xaml">  
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>  
  </None>  
  ...  
</Project>  
```  
  
> [!NOTE]
> No Visual Studio, você cria um arquivo de site de origem adicionando um arquivo a um projeto e definindo seu `Build Action` como `None` .  
  
 Quando o projeto é compilado, o MSBuild copia os arquivos especificados para a pasta de saída de compilação.  
  
### <a name="using-site-of-origin-files"></a>Usando arquivos de site de origem  
 Para carregar um arquivo de site de origem, você pode chamar o <xref:System.Windows.Application.GetRemoteStream%2A> método da <xref:System.Windows.Application> classe, passando um URI de pacote que identifica o arquivo de site de origem desejado. <xref:System.Windows.Application.GetRemoteStream%2A>Retorna um <xref:System.Windows.Resources.StreamResourceInfo> objeto, que expõe o site do arquivo de origem como um <xref:System.IO.Stream> e descreve seu tipo de conteúdo.  
  
 Por exemplo, o código a seguir mostra como usar o <xref:System.Windows.Application.GetRemoteStream%2A> para carregar um <xref:System.Windows.Controls.Page> arquivo de site de origem e defini-lo como o conteúdo de um <xref:System.Windows.Controls.Frame> ( `pageFrame` ).  
  
 [!code-csharp[WPFAssemblyResourcesSnippets#LoadAPageSOOFileManuallyCODE](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFAssemblyResourcesSnippets/CSharp/ResourcesSample/SOOPage.xaml.cs#loadapagesoofilemanuallycode)]
 [!code-vb[WPFAssemblyResourcesSnippets#LoadAPageSOOFileManuallyCODE](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WPFAssemblyResourcesSnippets/VisualBasic/ResourcesSample/SOOPage.xaml.vb#loadapagesoofilemanuallycode)]  
  
 Embora <xref:System.Windows.Application.GetRemoteStream%2A> a chamada forneça acesso ao <xref:System.IO.Stream> , você precisa executar o trabalho adicional de convertê-lo para o tipo da propriedade com a qual você irá configurá-lo. Em vez disso, você pode deixar o WPF cuidar de abrir e converter o <xref:System.IO.Stream> carregando um arquivo de recurso diretamente na propriedade de um tipo usando código.  
  
 O exemplo a seguir mostra como carregar um <xref:System.Windows.Controls.Page> diretamente em um <xref:System.Windows.Controls.Frame> ( `pageFrame` ) usando código.  
  
 [!code-csharp[WPFAssemblyResourcesSnippets#LoadPageSOOFileFromCODE](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFAssemblyResourcesSnippets/CSharp/ResourcesSample/SOOPage.xaml.cs#loadpagesoofilefromcode)]
 [!code-vb[WPFAssemblyResourcesSnippets#LoadPageSOOFileFromCODE](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WPFAssemblyResourcesSnippets/VisualBasic/ResourcesSample/SOOPage.xaml.vb#loadpagesoofilefromcode)]  
  
 O exemplo a seguir é um exemplo de marcação equivalente ao anterior.  
  
 [!code-xaml[WPFAssemblyResourcesSnippets#LoadPageSOOFileFromXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFAssemblyResourcesSnippets/CSharp/ResourcesSample/SOOPage.xaml#loadpagesoofilefromxaml)]  
  
<a name="Rebuilding_after_Changing_Build_Type"></a>
## <a name="rebuilding-after-changing-build-type"></a>Recompilando após modificar o tipo de build  
 Depois de alterar o tipo de build de um arquivo de dados do aplicativo, você precisa recompilar o aplicativo inteiro para garantir que essas alterações sejam aplicadas. Se você apenas compilar o aplicativo, as alterações não serão aplicadas.  
  
## <a name="see-also"></a>Consulte também

- [URIs "pack://" no WPF](pack-uris-in-wpf.md)
