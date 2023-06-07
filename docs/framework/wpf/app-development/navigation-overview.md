---
title: Visão geral de navegação
description: Saiba mais sobre o suporte para navegação em estilo de navegador usado em aplicativos autônomos e aplicativos de navegador XAML no Windows Presentation Foundation (WPF).
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- loose XAML files [WPF]
- windows [WPF]
- Start page [WPF]
- HTML files [WPF]
- structured navigation [WPF]
- fragment navigation [WPF]
- URIs (Uniform Resource Identifiers)
- custom objects [WPF]
- Uniform Resource Identifiers (URIs)
- pages [WPF]
- frames [WPF]
- navigation hosts [WPF]
- journals [WPF]
- lifetimes [WPF]
- retaining content state [WPF]
- content state [WPF]
- programmatic navigation [WPF]
- hyperlinks [WPF]
ms.assetid: 86ad2143-606a-4e34-bf7e-51a2594248b8
ms.openlocfilehash: 2aac1b3a38b6240bfba1b90983fd9cbd18b31b6f
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621698"
---
# <a name="navigation-overview"></a>Visão geral de navegação

O Windows Presentation Foundation (WPF) dá suporte à navegação em estilo de navegador que pode ser usada em dois tipos de aplicativos: aplicativos autônomos e aplicativos de navegador XAML (XBAPs). Para empacotar conteúdo para navegação, o WPF fornece a <xref:System.Windows.Controls.Page> classe. Você pode navegar de um <xref:System.Windows.Controls.Page> para outro de forma declarativa, usando um <xref:System.Windows.Documents.Hyperlink> , ou programaticamente, usando o <xref:System.Windows.Navigation.NavigationService> . O WPF usa o diário para lembrar páginas das quais foram navegadas e para navegar de volta para elas.

<xref:System.Windows.Controls.Page>, <xref:System.Windows.Documents.Hyperlink> , <xref:System.Windows.Navigation.NavigationService> e o diário formam o núcleo do suporte de navegação oferecido pelo WPF. Esta visão geral explora esses recursos em detalhes antes de abordar o suporte de navegação avançada que inclui a navegação para arquivos soltos [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] , arquivos HTML e objetos.

> [!NOTE]
> Neste tópico, o termo "navegador" refere-se apenas a navegadores que podem hospedar aplicativos WPF, que atualmente incluem o Microsoft Internet Explorer e o Firefox. Quando há suporte para recursos específicos do WPF apenas por um navegador específico, a versão do navegador é referida.

## <a name="navigation-in-wpf-applications"></a>Navegação em aplicativos WPF

Este tópico fornece uma visão geral dos principais recursos de navegação no WPF. Esses recursos estão disponíveis para aplicativos autônomos e XBAPs, embora este tópico os apresente dentro do contexto de um XBAP.

> [!NOTE]
> Este tópico não aborda como compilar e implantar XBAPs. Para obter mais informações sobre XBAPs, consulte [visão geral de aplicativos de navegador XAML WPF](wpf-xaml-browser-applications-overview.md).

Esta seção explica e demonstra os seguintes aspectos da navegação:

- [Implementar uma página](#CreatingAXAMLPage)

- [Configurar uma página inicial](#Configuring_a_Start_Page)

- [Configurar o título, largura e altura da janela do host](#ConfiguringAXAMLPage)

- [Navegação de hiperlink](#NavigatingBetweenXAMLPages)

- [Navegação de fragmento](#FragmentNavigation)

- [Serviço de navegação](#NavigationService)

- [Navegação programática com o serviço de navegação](#Programmatic_Navigation_with_the_Navigation_Service)

- [Tempo de vida de navegação](#Navigation_Lifetime)

- [Memorizar a navegação com o diário](#NavigationHistory)

- [Tempo de vida da página e o diário](#PageLifetime)

- [Reter o estado de conteúdo com o histórico de navegação](#RetainingContentStateWithNavigationHistory)

- [Cookies](#Cookies)

- [Navegação estruturada](#Structured_Navigation)

<a name="CreatingAXAMLPage"></a>

### <a name="implementing-a-page"></a>Implementar uma página

No WPF, você pode navegar para vários tipos de conteúdo que incluem .NET Framework objetos, objetos personalizados, valores de enumeração, controles de usuário, [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] arquivos e arquivos HTML. No entanto, você descobrirá que a maneira mais comum e conveniente de empacotar conteúdo é usando <xref:System.Windows.Controls.Page> . Além disso, <xref:System.Windows.Controls.Page> o implementa recursos específicos de navegação para aprimorar sua aparência e simplificar o desenvolvimento.

Usando <xref:System.Windows.Controls.Page> o, você pode implementar declarativamente uma página navegável de [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] conteúdo usando marcação semelhante à seguinte.

[!code-xaml[NavigationOverviewSnippets#Page1XAML](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/Page1.xaml#page1xaml)]

Um <xref:System.Windows.Controls.Page> que é implementado na [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] marcação tem `Page` como seu elemento raiz e requer a declaração de namespace XML do WPF. O `Page` elemento contém o conteúdo que você deseja navegar e exibir. Você adiciona conteúdo definindo o `Page.Content` elemento de propriedade, conforme mostrado na marcação a seguir.

[!code-xaml[NavigationOverviewSnippets#Page2XAML](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/Page2.xaml#page2xaml)]

`Page.Content` só pode conter um elemento filho; no exemplo anterior, o conteúdo é uma única cadeia de caracteres, "Hello, Page!" Na prática, você geralmente usará um controle de layout como o elemento filho (consulte [layout](../advanced/layout.md)) para conter e compor seu conteúdo.

Os elementos filho de um `Page` elemento são considerados como o conteúdo de <xref:System.Windows.Controls.Page> e, consequentemente, você não precisa usar a `Page.Content` declaração explícita. A marcação a seguir é o equivalente declarativo da amostra anterior.

[!code-xaml[NavigationOverviewSnippets#Page3XAML](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/Page3.xaml#page3xaml)]

Nesse caso, `Page.Content` é definido automaticamente com os elementos filho do `Page` elemento. Para obter mais informações, consulte [Modelo de conteúdo do WPF](../controls/wpf-content-model.md).

Um somente marcação <xref:System.Windows.Controls.Page> é útil para exibir o conteúdo. No entanto, um <xref:System.Windows.Controls.Page> também pode exibir controles que permitem que os usuários interajam com a página e possam responder à interação do usuário manipulando eventos e chamando a lógica do aplicativo. Um interativo <xref:System.Windows.Controls.Page> é implementado usando uma combinação de marcação e code-behind, conforme mostrado no exemplo a seguir.

[!code-xaml[XBAPAppDefSnippets#HomePageMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/XBAPAppDefSnippets/CSharp/HomePage.xaml#homepagemarkup)]

[!code-csharp[XBAPAppDefSnippets#HomePageCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/XBAPAppDefSnippets/CSharp/HomePage.xaml.cs#homepagecodebehind)]
[!code-vb[XBAPAppDefSnippets#HomePageCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/XBAPAppDefSnippets/VisualBasic/HomePage.xaml.vb#homepagecodebehind)]

Para permitir que um arquivo de marcação e o arquivo code-behind funcionem juntos, a seguinte configuração é necessária:

- Na marcação, o `Page` elemento deve incluir o `x:Class` atributo. Quando o aplicativo é compilado, a existência do `x:Class` no arquivo de marcação faz com que o Microsoft Build Engine (MSBuild) crie uma `partial` classe derivada de <xref:System.Windows.Controls.Page> e que tenha o nome especificado pelo `x:Class` atributo. Isso requer a adição de uma declaração de namespace XML para o [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] esquema ( `xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"` ). A `partial` classe gerada implementa `InitializeComponent` , que é chamada para registrar os eventos e definir as propriedades que são implementadas na marcação.

- No code-behind, a classe deve ser uma `partial` classe com o mesmo nome que é especificado pelo `x:Class` atributo na marcação e deve derivar de <xref:System.Windows.Controls.Page> . Isso permite que o arquivo code-behind seja associado à `partial` classe gerada para o arquivo de marcação quando o aplicativo é compilado (consulte [criando um aplicativo WPF](building-a-wpf-application-wpf.md)).

- No code-behind, a <xref:System.Windows.Controls.Page> classe deve implementar um construtor que chama o `InitializeComponent` método. `InitializeComponent`é implementado pela classe gerada do arquivo de marcação `partial` para registrar eventos e definir propriedades que são definidas na marcação.

> [!NOTE]
> Quando você adiciona um novo <xref:System.Windows.Controls.Page> ao seu projeto usando o Visual Studio, o <xref:System.Windows.Controls.Page> é implementado usando marcação e code-behind e inclui a configuração necessária para criar a associação entre os arquivos de marcação e code-behind, conforme descrito aqui.

Depois de ter um <xref:System.Windows.Controls.Page> , você pode navegar para ele. Para especificar o primeiro <xref:System.Windows.Controls.Page> para o qual um aplicativo navega, você precisa configurar o início <xref:System.Windows.Controls.Page> .

<a name="Configuring_a_Start_Page"></a>

### <a name="configuring-a-start-page"></a>Configurar uma página inicial

XBAPs exigem uma determinada quantidade de infraestrutura de aplicativo a ser hospedada em um navegador. No WPF, a <xref:System.Windows.Application> classe faz parte de uma definição de aplicativo que estabelece a infraestrutura de aplicativo necessária (consulte [visão geral do gerenciamento de aplicativos](application-management-overview.md)).

Uma definição de aplicativo geralmente é implementada usando marcação e code-behind, com o arquivo de marcação configurado como um item do MSBuild `ApplicationDefinition` . A seguir está uma definição de aplicativo para um XBAP.

[!code-xaml[XBAPAppDefSnippets#XBAPApplicationDefinitionMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/XBAPAppDefSnippets/CSharp/App.xaml#xbapapplicationdefinitionmarkup)]

[!code-csharp[XBAPAppDefSnippets#XBAPApplicationDefinitionCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/XBAPAppDefSnippets/CSharp/App.xaml.cs#xbapapplicationdefinitioncodebehind)]
[!code-vb[XBAPAppDefSnippets#XBAPApplicationDefinitionCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/XBAPAppDefSnippets/VisualBasic/Application.xaml.vb#xbapapplicationdefinitioncodebehind)]

Um XBAP pode usar sua definição de aplicativo para especificar um início <xref:System.Windows.Controls.Page> , que é o <xref:System.Windows.Controls.Page> que é carregado automaticamente quando o XBAP é iniciado. Você faz isso definindo a <xref:System.Windows.Application.StartupUri%2A> propriedade com o URI (Uniform Resource Identifier) para o desejado <xref:System.Windows.Controls.Page> .

> [!NOTE]
> Na maioria dos casos, o <xref:System.Windows.Controls.Page> é compilado ou implantado com um aplicativo. Nesses casos, o URI que identifica um <xref:System.Windows.Controls.Page> é um pacote URI, que é um URI que está de acordo com o esquema do *pacote* . URIs de pacote são abordados mais detalhadamente em [URIs de pacote no WPF](pack-uris-in-wpf.md). Você também pode navegar para o conteúdo usando o esquema http, que é discutido abaixo.

Você pode definir <xref:System.Windows.Application.StartupUri%2A> declarativamente na marcação, conforme mostrado no exemplo a seguir.

[!code-xaml[NavigationOverviewSnippets#XBAPApplicationDefinitionMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/App.xaml#xbapapplicationdefinitionmarkup)]

Neste exemplo, o `StartupUri` atributo é definido com um URI de pacote relativo que identifica homepage. XAML. Quando o XBAP é iniciado, HomePage. XAML é automaticamente navegado para e exibido. Isso é demonstrado pela figura a seguir, que mostra um XBAP que foi iniciado a partir de um servidor Web.

![Página XBAP](./media/navigation-overview/xbap-launched-from-a-web-server.png "Isso mostra um XBAP iniciado a partir de um servidor Web.")

> [!NOTE]
> Para obter mais informações sobre o desenvolvimento e a implantação de XBAPs, consulte [visão geral dos aplicativos de navegador XAML WPF](wpf-xaml-browser-applications-overview.md) e [implantando um aplicativo do WPF](deploying-a-wpf-application-wpf.md).

<a name="ConfiguringAXAMLPage"></a>

### <a name="configuring-the-host-windows-title-width-and-height"></a>Configurar o título, largura e altura da janela do host

Uma coisa que você pode ter observado na figura anterior é que o título do navegador e do painel de guias é o URI para o XBAP. Além de ser longo, o título não é atraente nem informativo. Por esse motivo, <xref:System.Windows.Controls.Page> o oferece uma maneira de alterar o título definindo a <xref:System.Windows.Controls.Page.WindowTitle%2A> propriedade. Além disso, você pode configurar a largura e a altura da janela do navegador definindo <xref:System.Windows.Controls.Page.WindowWidth%2A> e <xref:System.Windows.Controls.Page.WindowHeight%2A> , respectivamente.

<xref:System.Windows.Controls.Page.WindowTitle%2A>, <xref:System.Windows.Controls.Page.WindowWidth%2A> e <xref:System.Windows.Controls.Page.WindowHeight%2A> pode ser definido declarativamente na marcação, conforme mostrado no exemplo a seguir.

[!code-xaml[NavigationOverviewSnippets#HomePageMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/HomePage.xaml#homepagemarkup)]

O resultado é mostrado na figura a seguir.

![Título da janela, altura, largura](./media/navigation-overview/window-title-width-height.png "Isso mostra o título, a altura e a largura da janela que você pode configurar.")

<a name="NavigatingBetweenXAMLPages"></a>

### <a name="hyperlink-navigation"></a>Navegação de hiperlink

Um XBAP típico é composto por várias páginas. A maneira mais simples de navegar de uma página para outra é usar um <xref:System.Windows.Documents.Hyperlink> . Você pode adicionar declarativamente um <xref:System.Windows.Documents.Hyperlink> a um <xref:System.Windows.Controls.Page> usando o `Hyperlink` elemento, que é mostrado na marcação a seguir.

[!code-xaml[NavigationOverviewSnippets#HyperlinkXAML1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithHyperlink.xaml#hyperlinkxaml1)]
[!code-xaml[NavigationOverviewSnippets#HyperlinkXAML2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithHyperlink.xaml#hyperlinkxaml2)]
[!code-xaml[NavigationOverviewSnippets#HyperlinkXAML3](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithHyperlink.xaml#hyperlinkxaml3)]

Um `Hyperlink` elemento requer o seguinte:

- O pacote URI do <xref:System.Windows.Controls.Page> para navegar até, conforme especificado pelo `NavigateUri` atributo.

- O conteúdo no qual um usuário pode clicar para iniciar a navegação, como texto e imagens (para o conteúdo que o `Hyperlink` elemento pode conter, consulte <xref:System.Windows.Documents.Hyperlink> ).

A figura a seguir mostra um XBAP com um <xref:System.Windows.Controls.Page> que tem um <xref:System.Windows.Documents.Hyperlink> .

![Página com hiperlink](./media/navigation-overview/xbap-with-a-page-with-a-hyperlink.png "Isso mostra um XBAP com uma página com um hiperlink.")

Como esperado, clicar no <xref:System.Windows.Documents.Hyperlink> faz com que o XBAP navegue para o <xref:System.Windows.Controls.Page> que é identificado pelo `NavigateUri` atributo. Além disso, o XBAP adiciona uma entrada para o anterior <xref:System.Windows.Controls.Page> à lista de páginas recentes no Internet Explorer. Isso será mostrado na figura a seguir.

![Botões voltar e avançar](./media/navigation-overview/back-and-forward-navigation.png "Navegue com os botões voltar e avançar.")

Além de dar suporte à navegação de um <xref:System.Windows.Controls.Page> para outro, o <xref:System.Windows.Documents.Hyperlink> também dá suporte à navegação de fragmento.

<a name="FragmentNavigation"></a>

### <a name="fragment-navigation"></a>Navegação de fragmento

A *navegação de fragmento* é a navegação para um fragmento de conteúdo no atual ou em <xref:System.Windows.Controls.Page> outra <xref:System.Windows.Controls.Page> . No WPF, um fragmento de conteúdo é o conteúdo contido por um elemento nomeado. Um elemento nomeado é um elemento que tem seu `Name` atributo definido. A marcação a seguir mostra um `TextBlock` elemento nomeado que contém um fragmento de conteúdo.

[!code-xaml[NavigationOverviewSnippets#PageWithContentFragmentsMARKUP1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithFragments.xaml#pagewithcontentfragmentsmarkup1)]
[!code-xaml[NavigationOverviewSnippets#PageWithContentFragmentsMARKUP2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithFragments.xaml#pagewithcontentfragmentsmarkup2)]
[!code-xaml[NavigationOverviewSnippets#PageWithContentFragmentsMARKUP3](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithFragments.xaml#pagewithcontentfragmentsmarkup3)]

Para um <xref:System.Windows.Documents.Hyperlink> navegar até um fragmento de conteúdo, o `NavigateUri` atributo deve incluir o seguinte:

- O URI do <xref:System.Windows.Controls.Page> com o fragmento de conteúdo para o qual navegar.

- Um caractere "#".

- O nome do elemento no <xref:System.Windows.Controls.Page> que contém o fragmento de conteúdo.

Um URI de fragmento tem o formato a seguir.

*PageURI* `#` *ElementName*

Veja a seguir um exemplo de um `Hyperlink` que é configurado para navegar até um fragmento de conteúdo.

[!code-xaml[NavigationOverviewSnippets#PageThatNavigatesXAML1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageThatNavigatesToFragment.xaml#pagethatnavigatesxaml1)]
[!code-xaml[NavigationOverviewSnippets#PageThatNavigatesXAML2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageThatNavigatesToFragment.xaml#pagethatnavigatesxaml2)]
[!code-xaml[NavigationOverviewSnippets#PageThatNavigatesXAML3](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageThatNavigatesToFragment.xaml#pagethatnavigatesxaml3)]

> [!NOTE]
> Esta seção descreve a implementação de navegação de fragmento padrão no WPF. O WPF também permite que você implemente seu próprio esquema de navegação de fragmento que, em parte, requer o tratamento do <xref:System.Windows.Navigation.NavigationService.FragmentNavigation?displayProperty=nameWithType> evento.

> [!IMPORTANT]
> Você pode navegar para fragmentos em [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] páginas flexíveis (arquivos somente de marcação [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] com `Page` o elemento raiz) somente se as páginas puderem ser navegadas via http.
>
> No entanto, uma [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] página flexível pode navegar para seus próprios fragmentos.

<a name="NavigationService"></a>

### <a name="navigation-service"></a>Serviço de navegação

Embora <xref:System.Windows.Documents.Hyperlink> permita que um usuário inicie a navegação para um determinado <xref:System.Windows.Controls.Page> , o trabalho de localizar e baixar a página é executado pela <xref:System.Windows.Navigation.NavigationService> classe. Essencialmente, <xref:System.Windows.Navigation.NavigationService> o fornece a capacidade de processar uma solicitação de navegação em nome do código do cliente, como o <xref:System.Windows.Documents.Hyperlink> . Além disso, o <xref:System.Windows.Navigation.NavigationService> implementa o suporte de nível mais alto para acompanhar e influenciar uma solicitação de navegação.

Quando um <xref:System.Windows.Documents.Hyperlink> é clicado, o WPF chama <xref:System.Windows.Navigation.NavigationService.Navigate%2A?displayProperty=nameWithType> para localizar e baixar o <xref:System.Windows.Controls.Page> no URI do pacote especificado. O baixado <xref:System.Windows.Controls.Page> é convertido em uma árvore de objetos cujo objeto raiz é uma instância do baixada <xref:System.Windows.Controls.Page> . Uma referência ao objeto raiz <xref:System.Windows.Controls.Page> é armazenada na <xref:System.Windows.Navigation.NavigationService.Content%2A?displayProperty=nameWithType> propriedade. O URI do pacote para o conteúdo que navegou é armazenado na <xref:System.Windows.Navigation.NavigationService.Source%2A?displayProperty=nameWithType> propriedade, enquanto o <xref:System.Windows.Navigation.NavigationService.CurrentSource%2A?displayProperty=nameWithType> armazena o URI do pacote para a última página à qual foi navegada.

> [!NOTE]
> É possível que um aplicativo do WPF tenha mais de um ativo atualmente <xref:System.Windows.Navigation.NavigationService> . Para obter mais informações, consulte os [hosts de navegação](#Navigation_Hosts) mais adiante neste tópico.

<a name="Programmatic_Navigation_with_the_Navigation_Service"></a>

### <a name="programmatic-navigation-with-the-navigation-service"></a>Navegação programática com o serviço de navegação

Você não precisa saber se a <xref:System.Windows.Navigation.NavigationService> navegação é implementada declarativamente na marcação usando <xref:System.Windows.Documents.Hyperlink> , porque <xref:System.Windows.Documents.Hyperlink> o usa o <xref:System.Windows.Navigation.NavigationService> em seu nome. Isso significa que, desde que o pai direto ou indireto de um <xref:System.Windows.Documents.Hyperlink> seja um host de navegação (consulte [hosts de navegação](#Navigation_Hosts)), <xref:System.Windows.Documents.Hyperlink> será capaz de localizar e usar o serviço de navegação do host de navegação para processar uma solicitação de navegação.

No entanto, há situações em que você precisa usar <xref:System.Windows.Navigation.NavigationService> o diretamente, incluindo o seguinte:

- Quando você precisa criar uma instância de um <xref:System.Windows.Controls.Page> usando um construtor sem parâmetros.

- Quando você precisa definir as propriedades no <xref:System.Windows.Controls.Page> antes de navegar para ele.

- Quando o <xref:System.Windows.Controls.Page> que precisa ser navegado só pode ser determinado em tempo de execução.

Nessas situações, você precisa escrever código para iniciar a navegação programaticamente chamando o <xref:System.Windows.Navigation.NavigationService.Navigate%2A> método do <xref:System.Windows.Navigation.NavigationService> objeto. Isso requer obter uma referência a um <xref:System.Windows.Navigation.NavigationService> .

#### <a name="getting-a-reference-to-the-navigationservice"></a>Obter uma referência para o NavigationService

Por motivos que são abordados na seção [hosts de navegação](#Navigation_Hosts) , um aplicativo WPF pode ter mais de um <xref:System.Windows.Navigation.NavigationService> . Isso significa que seu código precisa de uma maneira de localizar um <xref:System.Windows.Navigation.NavigationService> , que geralmente é o <xref:System.Windows.Navigation.NavigationService> que navegou para o atual <xref:System.Windows.Controls.Page> . Você pode obter uma referência a um <xref:System.Windows.Navigation.NavigationService> chamando o `static` <xref:System.Windows.Navigation.NavigationService.GetNavigationService%2A?displayProperty=nameWithType> método. Para obter o <xref:System.Windows.Navigation.NavigationService> que navegou para um específico <xref:System.Windows.Controls.Page> , você passa uma referência para o <xref:System.Windows.Controls.Page> como o argumento do <xref:System.Windows.Navigation.NavigationService.GetNavigationService%2A> método. O código a seguir mostra como obter o <xref:System.Windows.Navigation.NavigationService> para o atual <xref:System.Windows.Controls.Page> .

[!code-csharp[NavigationOverviewSnippets#GetNSCODEBEHIND1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/GetNSPage.xaml.cs#getnscodebehind1)]
[!code-csharp[NavigationOverviewSnippets#GetNSCODEBEHIND2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/GetNSPage.xaml.cs#getnscodebehind2)]
[!code-vb[NavigationOverviewSnippets#GetNSCODEBEHIND2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/GetNSPage.xaml.vb#getnscodebehind2)]

Como um atalho para localizar o <xref:System.Windows.Navigation.NavigationService> para um <xref:System.Windows.Controls.Page> , <xref:System.Windows.Controls.Page> o implementa a <xref:System.Windows.Controls.Page.NavigationService%2A> propriedade. Isso é mostrado no exemplo a seguir.

[!code-csharp[NavigationOverviewSnippets#GetNSShortcutCODEBEHIND1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/GetNSPageShortCut.xaml.cs#getnsshortcutcodebehind1)]
[!code-csharp[NavigationOverviewSnippets#GetNSShortcutCODEBEHIND2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/GetNSPageShortCut.xaml.cs#getnsshortcutcodebehind2)]
[!code-vb[NavigationOverviewSnippets#GetNSShortcutCODEBEHIND2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/GetNSPageShortCut.xaml.vb#getnsshortcutcodebehind2)]

> [!NOTE]
> Um <xref:System.Windows.Controls.Page> só pode obter uma referência ao seu <xref:System.Windows.Navigation.NavigationService> quando <xref:System.Windows.Controls.Page> o gerar o <xref:System.Windows.FrameworkElement.Loaded> evento.

#### <a name="programmatic-navigation-to-a-page-object"></a>Navegação programática para um objeto de página

O exemplo a seguir mostra como usar o <xref:System.Windows.Navigation.NavigationService> para navegar programaticamente para um <xref:System.Windows.Controls.Page> . A navegação programática é necessária porque o <xref:System.Windows.Controls.Page> que está sendo navegado só pode ser instanciado usando um único Construtor sem parâmetros. O <xref:System.Windows.Controls.Page> com o construtor sem parâmetros é mostrado na seguinte marcação e código.

[!code-xaml[NavigationOverviewSnippets#PageWithNonDefaultConstructorXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithNonDefaultConstructor.xaml#pagewithnondefaultconstructorxaml)]

[!code-csharp[NavigationOverviewSnippets#PageWithNonDefaultConstructorCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithNonDefaultConstructor.xaml.cs#pagewithnondefaultconstructorcodebehind)]
[!code-vb[NavigationOverviewSnippets#PageWithNonDefaultConstructorCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/PageWithNonDefaultConstructor.xaml.vb#pagewithnondefaultconstructorcodebehind)]

O <xref:System.Windows.Controls.Page> que navega para o <xref:System.Windows.Controls.Page> com o construtor sem parâmetros é mostrado na seguinte marcação e código.

[!code-xaml[NavigationOverviewSnippets#NSNavigationPageXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NSNavigationPage.xaml#nsnavigationpagexaml)]

[!code-csharp[NavigationOverviewSnippets#NSNavigationPageCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NSNavigationPage.xaml.cs#nsnavigationpagecodebehind)]
[!code-vb[NavigationOverviewSnippets#NSNavigationPageCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/NSNavigationPage.xaml.vb#nsnavigationpagecodebehind)]

Quando o <xref:System.Windows.Documents.Hyperlink> <xref:System.Windows.Controls.Page> é clicado, a navegação é iniciada instanciando o <xref:System.Windows.Controls.Page> para navegar para o usando o construtor sem parâmetros e chamar o <xref:System.Windows.Navigation.NavigationService.Navigate%2A?displayProperty=nameWithType> método. <xref:System.Windows.Navigation.NavigationService.Navigate%2A>aceita uma referência ao objeto para o qual o <xref:System.Windows.Navigation.NavigationService> irá navegar, em vez de um URI de pacote.

#### <a name="programmatic-navigation-with-a-pack-uri"></a>Navegação programática com um URI "pack://"

Se você precisar construir um pacote URI programaticamente (quando você só puder determinar o URI do pacote em tempo de execução, por exemplo), poderá usar o <xref:System.Windows.Navigation.NavigationService.Navigate%2A?displayProperty=nameWithType> método. Isso é mostrado no exemplo a seguir.

[!code-xaml[NavigationOverviewSnippets#NSUriNavigationPageXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NSUriNavigationPage.xaml#nsurinavigationpagexaml)]

[!code-csharp[NavigationOverviewSnippets#NSUriNavigationPageCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NSUriNavigationPage.xaml.cs#nsurinavigationpagecodebehind)]
[!code-vb[NavigationOverviewSnippets#NSUriNavigationPageCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/NSUriNavigationPage.xaml.vb#nsurinavigationpagecodebehind)]

#### <a name="refreshing-the-current-page"></a>Atualizar a página atual

Um <xref:System.Windows.Controls.Page> não será baixado se tiver o mesmo URI de pacote que o pack URI armazenado na <xref:System.Windows.Navigation.NavigationService.Source%2A?displayProperty=nameWithType> propriedade. Para forçar o WPF a baixar a página atual novamente, você pode chamar o <xref:System.Windows.Navigation.NavigationService.Refresh%2A?displayProperty=nameWithType> método, conforme mostrado no exemplo a seguir.

[!code-xaml[NavigationOverviewSnippets#NSRefreshNavigationPageXAML1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NSRefreshNavigationPage.xaml#nsrefreshnavigationpagexaml1)]

[!code-csharp[NavigationOverviewSnippets#NSRefreshNavigationPageCODEBEHIND1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NSRefreshNavigationPage.xaml.cs#nsrefreshnavigationpagecodebehind1)]
[!code-vb[NavigationOverviewSnippets#NSRefreshNavigationPageCODEBEHIND1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/NSRefreshNavigationPage.xaml.vb#nsrefreshnavigationpagecodebehind1)]
[!code-csharp[NavigationOverviewSnippets#NSRefreshNavigationPageCODEBEHIND2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NSRefreshNavigationPage.xaml.cs#nsrefreshnavigationpagecodebehind2)]
[!code-vb[NavigationOverviewSnippets#NSRefreshNavigationPageCODEBEHIND2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/NSRefreshNavigationPage.xaml.vb#nsrefreshnavigationpagecodebehind2)]

<a name="Navigation_Lifetime"></a>

### <a name="navigation-lifetime"></a>Tempo de vida de navegação

Como você viu, há várias maneiras de iniciar a navegação. Quando a navegação é iniciada e enquanto a navegação está em andamento, você pode acompanhar e influenciar a navegação usando os seguintes eventos que são implementados pelo <xref:System.Windows.Navigation.NavigationService> :

- <xref:System.Windows.Navigation.NavigationService.Navigating>. Ocorre quando uma nova navegação é solicitada. Pode ser usado para cancelar a navegação.

- <xref:System.Windows.Navigation.NavigationService.NavigationProgress>. Ocorre periodicamente durante um download para fornecer informações sobre o andamento da navegação.

- <xref:System.Windows.Navigation.NavigationService.Navigated>. Ocorre quando a página foi localizada e baixada.

- <xref:System.Windows.Navigation.NavigationService.NavigationStopped>. Ocorre quando a navegação é interrompida (chamando <xref:System.Windows.Navigation.NavigationService.StopLoading%2A> ) ou quando uma nova navegação é solicitada enquanto uma navegação atual está em andamento.

- <xref:System.Windows.Navigation.NavigationService.NavigationFailed>. Ocorre quando um erro é gerado ao negar para o conteúdo solicitado.

- <xref:System.Windows.Navigation.NavigationService.LoadCompleted>. Ocorre após carregar, analisar o conteúdo para o qual se navegou e iniciar sua renderização.

- <xref:System.Windows.Navigation.NavigationService.FragmentNavigation>. Ocorre quando a navegação para um fragmento de conteúdo começa, o que ocorre:

  - Imediatamente, se o fragmento desejado estiver no conteúdo atual.

  - Depois que o conteúdo de origem tiver sido carregado, se o fragmento desejado estiver em outro conteúdo.

Os eventos de navegação são acionados na ordem em que são ilustrados pela figura a seguir.

![Gráfico de fluxo de navegação de página](./media/navigation-overview/order-of-navigation-events.png "Gráfico de fluxo de eventos de navegação de página")

Em geral, um <xref:System.Windows.Controls.Page> não se preocupa com esses eventos. É mais provável que um aplicativo esteja preocupado com eles e, por esse motivo, esses eventos também sejam gerados pela <xref:System.Windows.Application> classe:

- <xref:System.Windows.Application.Navigating?displayProperty=nameWithType>

- <xref:System.Windows.Application.NavigationProgress?displayProperty=nameWithType>

- <xref:System.Windows.Application.Navigated?displayProperty=nameWithType>

- <xref:System.Windows.Application.NavigationFailed?displayProperty=nameWithType>

- <xref:System.Windows.Application.NavigationStopped?displayProperty=nameWithType>

- <xref:System.Windows.Application.LoadCompleted?displayProperty=nameWithType>

- <xref:System.Windows.Application.FragmentNavigation?displayProperty=nameWithType>

Toda vez <xref:System.Windows.Navigation.NavigationService> que o gera um evento, a <xref:System.Windows.Application> classe gera o evento correspondente. <xref:System.Windows.Controls.Frame>e <xref:System.Windows.Navigation.NavigationWindow> oferecem os mesmos eventos para detectar a navegação dentro de seus respectivos escopos.

Em alguns casos, um <xref:System.Windows.Controls.Page> pode estar interessado nesses eventos. Por exemplo, um <xref:System.Windows.Controls.Page> pode manipular o <xref:System.Windows.Navigation.NavigationService.Navigating?displayProperty=nameWithType> evento para determinar se deseja ou não cancelar a navegação de si mesmo. Isso é mostrado no exemplo a seguir.

[!code-xaml[NavigationOverviewSnippets#CancelNavigationPageXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/CancelNavigationPage.xaml#cancelnavigationpagexaml)]

[!code-csharp[NavigationOverviewSnippets#CancelNavigationPageCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/CancelNavigationPage.xaml.cs#cancelnavigationpagecodebehind)]
[!code-vb[NavigationOverviewSnippets#CancelNavigationPageCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/CancelNavigationPage.xaml.vb#cancelnavigationpagecodebehind)]

Se você registrar um manipulador com um evento de navegação de um <xref:System.Windows.Controls.Page> , como o exemplo anterior, você também deverá cancelar o registro do manipulador de eventos. Caso contrário, pode haver efeitos colaterais em relação à forma como a navegação do WPF lembra <xref:System.Windows.Controls.Page> a navegação usando o diário.

<a name="NavigationHistory"></a>

### <a name="remembering-navigation-with-the-journal"></a>Memorizar a navegação com o diário

O WPF usa duas pilhas para lembrar as páginas das quais você navegou: uma pilha voltar e uma pilha de avanço. Quando você navega do atual <xref:System.Windows.Controls.Page> para um novo <xref:System.Windows.Controls.Page> ou para um existente <xref:System.Windows.Controls.Page> , o atual <xref:System.Windows.Controls.Page> é adicionado à *pilha voltar*. Quando você navega do atual de <xref:System.Windows.Controls.Page> volta para o anterior <xref:System.Windows.Controls.Page> , o atual <xref:System.Windows.Controls.Page> é adicionado à *pilha de encaminhamento*. Nos referimos ao conjunto composto pela pilha voltar, a pilha avançar e a funcionalidade para gerenciá-las como o diário. Cada item na pilha voltar e a pilha de encaminhamento é uma instância da <xref:System.Windows.Navigation.JournalEntry> classe e é chamado de *entrada de diário*.

#### <a name="navigating-the-journal-from-internet-explorer"></a>Navegando pelo diário no Internet Explorer

Conceitualmente, o diário opera da mesma forma que os botões **voltar** e **Avançar** no Internet Explorer. Eles serão mostrados na figura a seguir.

![Botões voltar e avançar](./media/navigation-overview/back-and-forward-navigation.png "Navegue com os botões voltar e avançar.")

Para XBAPs hospedados pelo Internet Explorer, o WPF integra o diário à navegação [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] do Internet Explorer. Isso permite que os usuários naveguem por páginas em um XBAP usando os botões **voltar**, **Avançar**e **recente** no Internet Explorer.

> [!IMPORTANT]
> No Internet Explorer, quando um usuário navega para fora de e para um XBAP, somente as entradas de diário para páginas que não são mantidas ativas são mantidas no diário. Para discussão sobre como manter as páginas ativas, consulte [tempo de vida da página e o diário](#PageLifetime) mais adiante neste tópico.

Por padrão, o texto para cada um <xref:System.Windows.Controls.Page> que aparece na lista **páginas recentes** do Internet Explorer é o URI para o <xref:System.Windows.Controls.Page> . Em muitos casos, isso não é especialmente significativo para o usuário. Felizmente, você pode alterar o texto usando uma das seguintes opções:

1. O `JournalEntry.Name` valor do atributo anexado.

2. O `Page.Title` valor do atributo.

3. O `Page.WindowTitle` valor do atributo e o URI para o atual <xref:System.Windows.Controls.Page> .

4. O URI para o atual <xref:System.Windows.Controls.Page> . (Padrão)

A ordem na qual as opções estão listadas coincide com a ordem de precedência para localizar o texto. Por exemplo, se `JournalEntry.Name` for definido, os outros valores serão ignorados.

O exemplo a seguir usa o `Page.Title` atributo para alterar o texto que aparece para uma entrada de diário.

[!code-xaml[NavigationOverviewSnippets#PageTitleMARKUP1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithTitle.xaml#pagetitlemarkup1)]
[!code-xaml[NavigationOverviewSnippets#PageTitleMARKUP2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithTitle.xaml#pagetitlemarkup2)]

[!code-csharp[NavigationOverviewSnippets#PageTitleCODEBEHIND1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithTitle.xaml.cs#pagetitlecodebehind1)]
[!code-vb[NavigationOverviewSnippets#PageTitleCODEBEHIND1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/PageWithTitle.xaml.vb#pagetitlecodebehind1)]
[!code-csharp[NavigationOverviewSnippets#PageTitleCODEBEHIND2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithTitle.xaml.cs#pagetitlecodebehind2)]
[!code-vb[NavigationOverviewSnippets#PageTitleCODEBEHIND2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/PageWithTitle.xaml.vb#pagetitlecodebehind2)]

#### <a name="navigating-the-journal-using-wpf"></a>Navegando pelo diário usando WPF

Embora um usuário possa navegar pelo diário usando as páginas **voltar**, **Avançar**e **recentes** no Internet Explorer, você também pode navegar pelo diário usando mecanismos declarativos e programáticos fornecidos pelo WPF. Um motivo para fazer isso é fornecer UIs de navegação personalizadas em suas páginas.

Você pode adicionar declarativamente suporte à navegação do diário usando os comandos de navegação expostos pelo <xref:System.Windows.Input.NavigationCommands> . O exemplo a seguir demonstra como usar o `BrowseBack` comando de navegação.

[!code-xaml[NavigationOverviewSnippets#NavigationCommandsPageXAML1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NavigationCommandsPage.xaml#navigationcommandspagexaml1)]
[!code-xaml[NavigationOverviewSnippets#NavigationCommandsPageXAML2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NavigationCommandsPage.xaml#navigationcommandspagexaml2)]
[!code-xaml[NavigationOverviewSnippets#NavigationCommandsPageXAML3](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NavigationCommandsPage.xaml#navigationcommandspagexaml3)]
[!code-xaml[NavigationOverviewSnippets#NavigationCommandsPageXAML4](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NavigationCommandsPage.xaml#navigationcommandspagexaml4)]

Você pode navegar por programação no diário usando um dos seguintes membros da <xref:System.Windows.Navigation.NavigationService> classe:

- <xref:System.Windows.Navigation.NavigationService.GoBack%2A>

- <xref:System.Windows.Navigation.NavigationService.GoForward%2A>

- <xref:System.Windows.Navigation.NavigationService.CanGoBack%2A>

- <xref:System.Windows.Navigation.NavigationService.CanGoForward%2A>

O diário também pode ser manipulado programaticamente, conforme discutido em [retendo o estado do conteúdo com histórico de navegação](#RetainingContentStateWithNavigationHistory) mais adiante neste tópico.

<a name="PageLifetime"></a>

### <a name="page-lifetime-and-the-journal"></a>Tempo de vida da página e o diário

Considere um XBAP com várias páginas que contêm conteúdo avançado, incluindo gráficos, animações e mídia. O volume de memória de páginas como essas poderá ser muito grande, especialmente se mídia de áudio e vídeo for usada. Considerando que as páginas "Remembers" do diário que foram navegadas, tal XBAP poderia consumir rapidamente uma quantidade grande e perceptível de memória.

Por esse motivo, o comportamento padrão do diário é armazenar <xref:System.Windows.Controls.Page> metadados em cada entrada de diário, em vez de uma referência a um <xref:System.Windows.Controls.Page> objeto. Quando uma entrada de diário é navegada, seus <xref:System.Windows.Controls.Page> metadados são usados para criar uma nova instância do especificada <xref:System.Windows.Controls.Page> . Como consequência, cada <xref:System.Windows.Controls.Page> uma navegada tem o tempo de vida que é ilustrado pela figura a seguir.

![Tempo de vida da página](./media/navigation-overview/navigated-page-lifetime.png "Isso mostra o tempo de vida quando uma página é navegada.")

Embora o uso do comportamento de registro em diário padrão possa economizar no consumo de memória, o desempenho de renderização por página pode ser reduzido; a reinstanciação de um <xref:System.Windows.Controls.Page> pode ser muito demorada, especialmente se tiver muito conteúdo. Se você precisar manter uma <xref:System.Windows.Controls.Page> instância no diário, poderá desenhar duas técnicas para fazer isso. Primeiro, você pode navegar programaticamente para um <xref:System.Windows.Controls.Page> objeto chamando o <xref:System.Windows.Navigation.NavigationService.Navigate%2A?displayProperty=nameWithType> método.

Em segundo lugar, você pode especificar que o WPF mantenha uma instância de a <xref:System.Windows.Controls.Page> no diário definindo a <xref:System.Windows.Controls.Page.KeepAlive%2A> propriedade como `true` (o padrão é `false` ). Conforme mostrado no exemplo a seguir, você pode definir <xref:System.Windows.Controls.Page.KeepAlive%2A> declarativamente na marcação.

[!code-xaml[NavigationOverviewSnippets#KeepAlivePageXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/KeepAlivePage.xaml#keepalivepagexaml)]

O tempo de vida de um <xref:System.Windows.Controls.Page> que é mantido ativo é ligeiramente diferente de um que não é. Na primeira vez <xref:System.Windows.Controls.Page> em que o que é mantido ativo é navegado para, ele é instanciado da mesma forma <xref:System.Windows.Controls.Page> que não é mantido ativo. No entanto, como uma instância do <xref:System.Windows.Controls.Page> é mantida no diário, ela nunca é instanciada novamente pelo tempo que permanecer no diário. Consequentemente, se uma <xref:System.Windows.Controls.Page> tiver uma lógica de inicialização que precisa ser chamada toda vez que o <xref:System.Windows.Controls.Page> for navegado, você deverá movê-la do construtor para um manipulador para o <xref:System.Windows.FrameworkElement.Loaded> evento. Conforme mostrado na figura a seguir, os <xref:System.Windows.FrameworkElement.Loaded> <xref:System.Windows.FrameworkElement.Unloaded> eventos e ainda são gerados cada vez <xref:System.Windows.Controls.Page> que um é navegado para e de, respectivamente.

![Quando os eventos Loaded e Unloaded são gerados](./media/navigation-overview/loaded-and-unloaded-events.png "Eventos carregados e descarregados são gerados quando uma página é navegada para e de.")

Quando um <xref:System.Windows.Controls.Page> não é mantido ativo, você não deve fazer o seguinte:

- Armazenar uma referência nela ou qualquer parte dela.

- Registrar manipuladores de eventos com eventos que não são implementados por ela.

Fazer uma delas criará referências que forçam o <xref:System.Windows.Controls.Page> a ser retido na memória, mesmo após terem sido removidas do diário.

Em geral, você deve preferir o <xref:System.Windows.Controls.Page> comportamento padrão de não manter um <xref:System.Windows.Controls.Page> vivo. No entanto, isso tem implicações de estado que são discutidas na próxima seção.

<a name="RetainingContentStateWithNavigationHistory"></a>

### <a name="retaining-content-state-with-navigation-history"></a>Reter o estado de conteúdo com o histórico de navegação

Se um <xref:System.Windows.Controls.Page> não for mantido ativo e tiver controles que coletam dados do usuário, o que acontecerá com os dados se um usuário sair de volta e voltar para o <xref:System.Windows.Controls.Page> ? De uma perspectiva de experiência do usuário, o usuário deve ter a expectativa de ver os dados que inseriu anteriormente. Infelizmente, como uma nova instância do <xref:System.Windows.Controls.Page> é criada com cada navegação, os controles que coletavam os dados são instanciados novamente e os dados são perdidos.

Felizmente, o diário fornece suporte para memorizar dados entre <xref:System.Windows.Controls.Page> navegações, incluindo dados de controle. Especificamente, a entrada de diário para cada <xref:System.Windows.Controls.Page> atua como um contêiner temporário para o <xref:System.Windows.Controls.Page> estado associado. As etapas a seguir descrevem como esse suporte é usado quando um <xref:System.Windows.Controls.Page> é navegado em:

1. Uma entrada para a atual <xref:System.Windows.Controls.Page> é adicionada ao diário.

2. O estado do <xref:System.Windows.Controls.Page> é armazenado com a entrada de diário para essa página, que é adicionada à pilha voltar.

3. O novo <xref:System.Windows.Controls.Page> é navegado para.

Quando a página <xref:System.Windows.Controls.Page> é navegada de volta para, usando o diário, ocorrem as seguintes etapas:

1. O <xref:System.Windows.Controls.Page> (a entrada de diário superior na pilha voltar) é instanciada.

2. O <xref:System.Windows.Controls.Page> é atualizado com o estado que foi armazenado com a entrada de diário para o <xref:System.Windows.Controls.Page> .

3. O <xref:System.Windows.Controls.Page> é navegado de volta para.

O WPF usa esse suporte automaticamente quando os seguintes controles são usados em um <xref:System.Windows.Controls.Page> :

- <xref:System.Windows.Controls.CheckBox>

- <xref:System.Windows.Controls.ComboBox>

- <xref:System.Windows.Controls.Expander>

- <xref:System.Windows.Controls.Frame>

- <xref:System.Windows.Controls.ListBox>

- <xref:System.Windows.Controls.ListBoxItem>

- <xref:System.Windows.Controls.MenuItem>

- <xref:System.Windows.Controls.ProgressBar>

- <xref:System.Windows.Controls.RadioButton>

- <xref:System.Windows.Controls.Slider>

- <xref:System.Windows.Controls.TabControl>

- <xref:System.Windows.Controls.TabItem>

- <xref:System.Windows.Controls.TextBox>

Se um <xref:System.Windows.Controls.Page> usar esses controles, os dados inseridos neles serão lembrados entre as <xref:System.Windows.Controls.Page> navegações, conforme demonstrado pela **cor favorita** <xref:System.Windows.Controls.ListBox> na figura a seguir.

![Página com controles que lembram o estado](./media/navigation-overview/data-remembered-across-page-navigations.png "Os dados inseridos são lembrados entre as navegações de página.")

Quando um <xref:System.Windows.Controls.Page> tem controles diferentes daqueles na lista anterior, ou quando o estado é armazenado em objetos personalizados, você precisa escrever código para fazer com que o diário memorize o estado entre as <xref:System.Windows.Controls.Page> navegações.

Se você precisar se lembrar de pequenas partes de estado entre <xref:System.Windows.Controls.Page> navegações, poderá usar propriedades de dependência (consulte <xref:System.Windows.DependencyProperty> ) configuradas com o <xref:System.Windows.FrameworkPropertyMetadata.Journal%2A?displayProperty=nameWithType> sinalizador de metadados.

Se o estado que você <xref:System.Windows.Controls.Page> precisa se lembrar entre as navegações abranger vários dados, talvez você ache menos intensivos de código para encapsular seu estado em uma única classe e implementar a <xref:System.Windows.Navigation.IProvideCustomContentState> interface.

Se você precisar navegar por vários Estados de um único <xref:System.Windows.Controls.Page> , sem navegar a partir dele <xref:System.Windows.Controls.Page> , você pode usar <xref:System.Windows.Navigation.IProvideCustomContentState> e <xref:System.Windows.Navigation.NavigationService.AddBackEntry%2A?displayProperty=nameWithType> .

<a name="Cookies"></a>

### <a name="cookies"></a>Cookies

Outra maneira que os aplicativos do WPF podem armazenar dados é com cookies, que são criados, atualizados e excluídos usando os <xref:System.Windows.Application.SetCookie%2A> <xref:System.Windows.Application.GetCookie%2A> métodos e. Os cookies que você pode criar no WPF são os mesmos cookies que outros tipos de aplicativos Web usam; cookies são partes arbitrárias de dados que são armazenados por um aplicativo em um computador cliente durante ou entre sessões de aplicativo. Os dados do cookie normalmente assumem a forma de um par nome/valor no formato a seguir.

*Nome* `=` do *Valor* do

Quando os dados são passados para o <xref:System.Windows.Application.SetCookie%2A> , junto com o <xref:System.Uri> do local para o qual o cookie deve ser definido, um cookie é criado na memória e só está disponível durante a sessão atual do aplicativo. Esse tipo de cookie é conhecido como um *cookie de sessão*.

Para armazenar um cookie entre sessões de aplicativo, uma data de validade deve ser adicionada ao cookie, usando o formato a seguir.

*NOME* `=` *VALOR* `; expires=DAY, DD-MMM-YYYY HH:MM:SS GMT`

Um cookie com uma data de validade é armazenado na pasta Temporary Internet Files atual da instalação do Windows até que o cookie expire. Esse cookie é conhecido como um *cookie persistente* porque ele persiste entre as sessões do aplicativo.

Você recupera os cookies persistentes e de sessão chamando o <xref:System.Windows.Application.GetCookie%2A> método, passando o <xref:System.Uri> do local onde o cookie foi definido com o <xref:System.Windows.Application.SetCookie%2A> método.

A seguir estão algumas das maneiras como os cookies têm suporte no WPF:

- Os aplicativos e XBAPs autônomos do WPF podem criar e gerenciar cookies.

- Os cookies criados por um XBAP podem ser acessados no navegador.

- XBAPs do mesmo domínio podem criar e compartilhar cookies.

- XBAPs e páginas HTML do mesmo domínio podem criar e compartilhar cookies.

- Os cookies são expedidos quando os XBAPs e [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] as páginas flexíveis fazem solicitações da Web.

- XBAPs de nível superior e XBAPs hospedados em IFRAMEs podem acessar cookies.

- O suporte a cookies no WPF é o mesmo para todos os navegadores com suporte.

- No Internet Explorer, a política de P3P que pertence a cookies é respeitada pelo WPF, particularmente em relação aos XBAPs primários e de terceiros.

<a name="Structured_Navigation"></a>

### <a name="structured-navigation"></a>Navegação estruturada

Se você precisar passar dados de um <xref:System.Windows.Controls.Page> para outro, poderá passar os dados como argumentos para um construtor sem parâmetros do <xref:System.Windows.Controls.Page> . Observe que, se você usar essa técnica, deverá manter o <xref:System.Windows.Controls.Page> vivo; se não, na próxima vez que navegar para o <xref:System.Windows.Controls.Page> , o WPF reinstanciará o <xref:System.Windows.Controls.Page> usando o construtor sem parâmetros.

Como alternativa, você <xref:System.Windows.Controls.Page> pode implementar propriedades que são definidas com os dados que precisam ser passados. As coisas se tornam complicadas, no entanto, quando <xref:System.Windows.Controls.Page> é necessário passar dados de volta para o <xref:System.Windows.Controls.Page> que navegou para ele. O problema é que a navegação não oferece suporte nativo a mecanismos para garantir que um <xref:System.Windows.Controls.Page> será retornado após a sua navegação. Essencialmente, a navegação não dá suporte à semântica de chamada/retorno. Para resolver esse problema, o WPF fornece a <xref:System.Windows.Navigation.PageFunction%601> classe que você pode usar para garantir que um <xref:System.Windows.Controls.Page> seja retornado de maneira previsível e estruturada. Para obter mais informações, consulte [visão geral de navegação estruturada](structured-navigation-overview.md).

<a name="The_NavigationWindow_Class"></a>

## <a name="the-navigationwindow-class"></a>A classe NavigationWindow

Neste ponto, você viu a gama de serviços de navegação que você provavelmente utilizará para criar aplicativos com conteúdo navegável. Esses serviços foram discutidos no contexto de XBAPs, embora não estejam limitados a XBAPs. Os sistemas operacionais modernos e os aplicativos do Windows aproveitam a experiência do navegador de usuários modernos para incorporar a navegação no estilo de navegador em aplicativos autônomos. Exemplos comuns incluem:

- **Dicionário de sinônimos de palavras**: navegue por opções de palavras.

- **Explorador de Arquivos**: navegar por arquivos e pastas.

- **Assistentes**: dividir uma tarefa complexa em várias páginas entre as quais se pode navegar. Um exemplo é o assistente de componentes do Windows que lida com a adição e remoção de recursos do Windows.

Para incorporar a navegação no estilo de navegador em seus aplicativos autônomos, você pode usar a <xref:System.Windows.Navigation.NavigationWindow> classe. <xref:System.Windows.Navigation.NavigationWindow>deriva de <xref:System.Windows.Window> e estende-o com o mesmo suporte para navegação que XBAPs fornecem. Você pode usar <xref:System.Windows.Navigation.NavigationWindow> o como a janela principal do seu aplicativo autônomo ou como uma janela secundária, como uma caixa de diálogo.

Para implementar um <xref:System.Windows.Navigation.NavigationWindow> , como com a maioria das classes de nível superior no WPF ( <xref:System.Windows.Window> , <xref:System.Windows.Controls.Page> e assim por diante), você usa uma combinação de marcação e code-behind. Isso é mostrado no exemplo a seguir.

[!code-xaml[IntroToNavNavigationWindowSnippets#NavigationWindowMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/IntroToNavNavigationWindowSnippets/CSharp/MainWindow.xaml#navigationwindowmarkup)]

[!code-csharp[IntroToNavNavigationWindowSnippets#NavigationWindowCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/IntroToNavNavigationWindowSnippets/CSharp/MainWindow.xaml.cs#navigationwindowcodebehind)]
[!code-vb[IntroToNavNavigationWindowSnippets#NavigationWindowCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/IntroToNavNavigationWindowSnippets/VisualBasic/MainWindow.xaml.vb#navigationwindowcodebehind)]

Esse código cria um <xref:System.Windows.Navigation.NavigationWindow> que navega automaticamente para um <xref:System.Windows.Controls.Page> (homepage. XAML) quando o <xref:System.Windows.Navigation.NavigationWindow> é aberto. Se o <xref:System.Windows.Navigation.NavigationWindow> for a janela principal do aplicativo, você poderá usar o `StartupUri` atributo para iniciá-lo. Isso é mostrado na marcação a seguir.

[!code-xaml[IntroToNavNavigationWindowSnippets#AppLaunchNavWindow](~/samples/snippets/csharp/VS_Snippets_Wpf/IntroToNavNavigationWindowSnippets/CSharp/App.xaml#applaunchnavwindow)]

A figura a seguir mostra o <xref:System.Windows.Navigation.NavigationWindow> como a janela principal de um aplicativo autônomo.

![Uma janela principal](./media/navigation-overview/navigation-window-as-main-window.png "Janela de navegação como a janela principal")

Na figura, você pode ver que o <xref:System.Windows.Navigation.NavigationWindow> tem um título, mesmo que não tenha sido definido no <xref:System.Windows.Navigation.NavigationWindow> código de implementação do exemplo anterior. Em vez disso, o título é definido usando a <xref:System.Windows.Controls.Page.WindowTitle%2A> propriedade, que é mostrada no código a seguir.

[!code-xaml[IntroToNavNavigationWindowSnippets#HomePageMARKUP1](~/samples/snippets/csharp/VS_Snippets_Wpf/IntroToNavNavigationWindowSnippets/CSharp/HomePage.xaml#homepagemarkup1)]
[!code-xaml[IntroToNavNavigationWindowSnippets#HomePageMARKUP2](~/samples/snippets/csharp/VS_Snippets_Wpf/IntroToNavNavigationWindowSnippets/CSharp/HomePage.xaml#homepagemarkup2)]

Definir as <xref:System.Windows.Controls.Page.WindowWidth%2A> <xref:System.Windows.Controls.Page.WindowHeight%2A> Propriedades e também afeta o <xref:System.Windows.Navigation.NavigationWindow> .

Normalmente, você implementa sua própria <xref:System.Windows.Navigation.NavigationWindow> quando precisa personalizar seu comportamento ou sua aparência. Se nada disso é necessário, você pode usar um atalho. Se você especificar o pacote URI de a <xref:System.Windows.Controls.Page> como <xref:System.Windows.Application.StartupUri%2A> em um aplicativo autônomo, o <xref:System.Windows.Application> criará automaticamente um <xref:System.Windows.Navigation.NavigationWindow> para hospedar o <xref:System.Windows.Controls.Page> . A marcação a seguir mostra como habilitar isso.

[!code-xaml[IntroToNavNavigationWindowSnippets#AppLaunchPage](~/samples/snippets/csharp/VS_Snippets_Wpf/IntroToNavNavigationWindowSnippets/CSharp/AnotherApp.xaml#applaunchpage)]

Se desejar que uma janela de aplicativo secundário, como uma caixa de diálogo, seja um <xref:System.Windows.Navigation.NavigationWindow> , você poderá usar o código no exemplo a seguir para abri-lo.

[!code-csharp[IntroToNavNavigationWindowSnippets#CreateNWDialogBox](~/samples/snippets/csharp/VS_Snippets_Wpf/IntroToNavNavigationWindowSnippets/CSharp/DialogOwnerWindow.xaml.cs#createnwdialogbox)]
[!code-vb[IntroToNavNavigationWindowSnippets#CreateNWDialogBox](~/samples/snippets/visualbasic/VS_Snippets_Wpf/IntroToNavNavigationWindowSnippets/VisualBasic/DialogOwnerWindow.xaml.vb#createnwdialogbox)]

A figura a seguir mostra o resultado.

![Uma caixa de diálogo](./media/navigation-overview/navigation-window-as-dialog-box.png "Janela de navegação como uma caixa de diálogo")

Como você pode ver, o <xref:System.Windows.Navigation.NavigationWindow> exibe os botões **voltar** e **Avançar** no estilo do Internet Explorer que permitem que os usuários naveguem no diário. Esses botões fornecem a mesma experiência do usuário, conforme mostrado na figura a seguir.

![Botões voltar e avançar em um NavigationWindow](./media/navigation-overview/back-and-forward-buttons-in-navigation-window.png "Botões voltar e avançar em uma janela de navegação")

Se suas páginas fornecem seu próprio suporte de navegação de diário e interface do usuário, você pode ocultar os botões **voltar** e **Avançar** exibidos pelo <xref:System.Windows.Navigation.NavigationWindow> definindo o valor da <xref:System.Windows.Navigation.NavigationWindow.ShowsNavigationUI%2A> propriedade como `false` .

Como alternativa, você pode usar o suporte à personalização no WPF para substituir o [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] <xref:System.Windows.Navigation.NavigationWindow> próprio.

<a name="Frame_in_Standalone_Applications"></a>

## <a name="the-frame-class"></a>A classe Frame

O navegador e o <xref:System.Windows.Navigation.NavigationWindow> Windows que hospedam conteúdo navegável. Em alguns casos, os aplicativos têm conteúdo que não precisa ser hospedado por uma janela inteira. Em vez disso, esse tipo de conteúdo pode ser hospedado dentro de outro conteúdo. Você pode inserir conteúdo navegável em outro conteúdo usando a <xref:System.Windows.Controls.Frame> classe. <xref:System.Windows.Controls.Frame>fornece o mesmo suporte que <xref:System.Windows.Navigation.NavigationWindow> e XBAPs.

O exemplo a seguir mostra como adicionar um <xref:System.Windows.Controls.Frame> a um de forma <xref:System.Windows.Controls.Page> declarativa usando o `Frame` elemento.

[!code-xaml[NavigationOverviewSnippets#FrameHostPageXAML1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHostPage.xaml#framehostpagexaml1)]
[!code-xaml[NavigationOverviewSnippets#FrameHostPageXAML2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHostPage.xaml#framehostpagexaml2)]
[!code-xaml[NavigationOverviewSnippets#FrameHostPageXAML3](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHostPage.xaml#framehostpagexaml3)]

Essa marcação define o `Source` atributo do `Frame` elemento com um URI de pacote para o <xref:System.Windows.Controls.Page> que o <xref:System.Windows.Controls.Frame> deve navegar inicialmente. A figura a seguir mostra um XBAP com um <xref:System.Windows.Controls.Page> que tem um que <xref:System.Windows.Controls.Frame> navegou entre várias páginas.

![Um quadro que navegou entre várias páginas](./media/navigation-overview/frame-navigation-between-multiple-pages.png "Isso mostra uma navegação de quadro entre várias páginas.")

Você não precisa apenas usar <xref:System.Windows.Controls.Frame> dentro do conteúdo de um <xref:System.Windows.Controls.Page> . Também é comum hospedar um <xref:System.Windows.Controls.Frame> dentro do conteúdo de um <xref:System.Windows.Window> .

Por padrão, <xref:System.Windows.Controls.Frame> o usa apenas seu próprio diário na ausência de outro diário. Se um <xref:System.Windows.Controls.Frame> for parte do conteúdo hospedado dentro de um <xref:System.Windows.Navigation.NavigationWindow> ou XBAP, <xref:System.Windows.Controls.Frame> o usará o diário que pertence ao <xref:System.Windows.Navigation.NavigationWindow> XBAP ou. Às vezes, no entanto, <xref:System.Windows.Controls.Frame> talvez seja necessário ser responsável por seu próprio diário. Um motivo para fazer isso é permitir a navegação do diário dentro das páginas que são hospedadas por um <xref:System.Windows.Controls.Frame> . Isso é ilustrado pela figura a seguir.

![Diagrama de quadro e de página](./media/navigation-overview/journal-navigation-within-pages-hosted-by-a-frame.png "Isso mostra a navegação do diário em páginas hospedadas por um quadro.")

Nesse caso, você pode configurar o <xref:System.Windows.Controls.Frame> para usar seu próprio diário definindo a <xref:System.Windows.Controls.Frame.JournalOwnership%2A> Propriedade do <xref:System.Windows.Controls.Frame> para <xref:System.Windows.Navigation.JournalOwnership.OwnsJournal> . Isso é mostrado na marcação a seguir.

[!code-xaml[NavigationOverviewSnippets#FrameHostPageOwnJournalXAML1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHostPageOwnJournal.xaml#framehostpageownjournalxaml1)]
[!code-xaml[NavigationOverviewSnippets#FrameHostPageOwnJournalXAML2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHostPageOwnJournal.xaml#framehostpageownjournalxaml2)]
[!code-xaml[NavigationOverviewSnippets#FrameHostPageOwnJournalXAML3](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHostPageOwnJournal.xaml#framehostpageownjournalxaml3)]

A figura a seguir ilustra o efeito de navegar em um <xref:System.Windows.Controls.Frame> que usa seu próprio diário.

![Um quadro que usa seu próprio diário](./media/navigation-overview/frame-uses-its-own-journal.png "Isso mostra o efeito de navegar em um quadro que usa seu próprio diário.")

Observe que as entradas de diário são mostradas pela navegação [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] no <xref:System.Windows.Controls.Frame> , em vez de pelo Internet Explorer.

> [!NOTE]
> Se um <xref:System.Windows.Controls.Frame> fizer parte do conteúdo hospedado em um <xref:System.Windows.Window> , <xref:System.Windows.Controls.Frame> o usará seu próprio diário e, consequentemente, exibirá sua própria navegação [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] .

Se sua experiência de usuário exigir um <xref:System.Windows.Controls.Frame> para fornecer seu próprio diário sem mostrar a navegação [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] , você poderá ocultar a navegação [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] definindo o <xref:System.Windows.Controls.Frame.NavigationUIVisibility%2A> para <xref:System.Windows.Visibility.Hidden> . Isso é mostrado na marcação a seguir.

[!code-xaml[NavigationOverviewSnippets#FrameHostPageHidesUIXAML1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHostPageOwnHiddenJournal.xaml#framehostpagehidesuixaml1)]
[!code-xaml[NavigationOverviewSnippets#FrameHostPageHidesUIXAML2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHostPageOwnHiddenJournal.xaml#framehostpagehidesuixaml2)]
[!code-xaml[NavigationOverviewSnippets#FrameHostPageHidesUIXAML3](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHostPageOwnHiddenJournal.xaml#framehostpagehidesuixaml3)]

<a name="Navigation_Hosts"></a>

## <a name="navigation-hosts"></a>Hosts de navegação

<xref:System.Windows.Controls.Frame>e <xref:System.Windows.Navigation.NavigationWindow> são classes conhecidas como hosts de navegação. Um *host de navegação* é uma classe que pode navegar e exibir o conteúdo. Para fazer isso, cada host de navegação usa seu próprio <xref:System.Windows.Navigation.NavigationService> e seu diário. A construção básica de um host de navegação é mostrada na figura a seguir.

![Diagramas de navegador](./media/navigation-overview/navigation-host-construction.png "Construção básica de um host de navegação")

Essencialmente, isso permite <xref:System.Windows.Navigation.NavigationWindow> e <xref:System.Windows.Controls.Frame> fornece o mesmo suporte de navegação que um XBAP fornece quando hospedado no navegador.

Além de usar <xref:System.Windows.Navigation.NavigationService> o e um diário, os hosts de navegação implementam os mesmos membros que o <xref:System.Windows.Navigation.NavigationService> implementa. Isso é ilustrado pela figura a seguir.

![Um diário em um quadro e em um NavigationWindow](./media/navigation-overview/navigation-window-and-frame.png "Janela de navegação e quadro")

Isso permite que você programe suporte a navegação diretamente em relação a eles. Você pode considerar isso se precisar fornecer uma navegação personalizada [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] para um <xref:System.Windows.Controls.Frame> que esteja hospedado em um <xref:System.Windows.Window> . Além disso, os dois tipos implementam membros adicionais relacionados à navegação, incluindo `BackStack` ( <xref:System.Windows.Navigation.NavigationWindow.BackStack%2A?displayProperty=nameWithType> , <xref:System.Windows.Controls.Frame.BackStack%2A?displayProperty=nameWithType> ) e `ForwardStack` ( <xref:System.Windows.Navigation.NavigationWindow.ForwardStack%2A?displayProperty=nameWithType> , <xref:System.Windows.Controls.Frame.ForwardStack%2A?displayProperty=nameWithType> ), que permitem enumerar as entradas de diário na pilha voltar e na pilha de encaminhamento, respectivamente.

Conforme mencionado anteriormente, mais de um diário pode existir dentro de um aplicativo. A figura a seguir fornece um exemplo de quando isso pode acontecer.

![Vários diários dentro de um aplicativo](./media/navigation-overview/multiple-journals-in-one-application.png "Este é um exemplo de mais de um diário em um aplicativo.")

<a name="Navigating_to_Content_Other_than_Pages"></a>

## <a name="navigating-to-content-other-than-xaml-pages"></a>Navegar para conteúdo que não seja páginas XAML

Ao longo deste tópico, <xref:System.Windows.Controls.Page> o pacote XBAPs foi usado para demonstrar os vários recursos de navegação do WPF. No entanto, um <xref:System.Windows.Controls.Page> que é compilado em um aplicativo não é o único tipo de conteúdo que pode ser navegado e Pack XBAPs não é a única maneira de identificar o conteúdo.

Como esta seção demonstra, você também pode navegar para arquivos soltos [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] , arquivos HTML e objetos.

<a name="Navigating_to_Loose_XAML_Files"></a>

### <a name="navigating-to-loose-xaml-files"></a>Navegar para arquivos XAML flexíveis

Um [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] arquivo flexível é um arquivo com as seguintes características:

- Contém apenas [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] (ou seja, nenhum código).

- Tem uma declaração de namespace apropriada.

- Tem a extensão de nome de arquivo .xaml.

Por exemplo, considere o conteúdo a seguir que é armazenado como um [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] arquivo flexível, Person. XAML.

[!code-xaml[NavigationOverviewSnippets#LooseXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/Person.xaml#loosexaml)]

Quando você clica duas vezes no arquivo, o navegador é aberto e então navega para o conteúdo e o exibe. Isso será mostrado na figura a seguir.

![Exibição do conteúdo no arquivo Person. XAML](./media/navigation-overview/contents-of-person-xaml-file.png "Mostra o conteúdo do arquivo Person. XAML.")

Você pode exibir um [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] arquivo livre do seguinte:

- Um site da Web no computador local, a intranet ou a Internet.

- Um compartilhamento de arquivos UNC (Convenção de nomenclatura universal).

- O disco local.

Um [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] arquivo flexível pode ser adicionado aos favoritos do navegador ou ser o home page do navegador.

> [!NOTE]
> Para obter mais informações sobre como publicar e iniciar [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] páginas flexíveis, consulte [implantando um aplicativo WPF](deploying-a-wpf-application-wpf.md).

Uma limitação em relação à flexível [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] é que você só pode hospedar conteúdo que seja seguro para ser executado em confiança parcial. Por exemplo, `Window` não pode ser o elemento raiz de um [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] arquivo flexível. Para obter mais informações, consulte [Segurança parcialmente confiável do WPF](../wpf-partial-trust-security.md).

<a name="Navigating_to_HTML_Files_Using_Frame"></a>

### <a name="navigating-to-html-files-by-using-frame"></a>Navegar para arquivos HTML usando o quadro

Como você pode esperar, você também pode navegar para HTML. Você simplesmente precisa fornecer um URI que usa o esquema http. Por exemplo, o seguinte [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] mostra um <xref:System.Windows.Controls.Frame> que navega para uma página HTML.

[!code-xaml[NavigationOverviewSnippets#FrameHtmlNavMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHTMLNavPage.xaml#framehtmlnavmarkup)]

Navegar para HTML requer permissões especiais. Por exemplo, você não pode navegar de um XBAP que está em execução na área restrita de segurança de confiança parcial da zona da Internet. Para obter mais informações, consulte [Segurança parcialmente confiável do WPF](../wpf-partial-trust-security.md).

<a name="Navigating_to_HTML_Files_Using_WebBrowser"></a>

### <a name="navigating-to-html-files-by-using-the-webbrowser-control"></a>Navegar para arquivos HTML usando o controle WebBrowser

O <xref:System.Windows.Controls.WebBrowser> controle dá suporte à Hospedagem de documentos HTML, navegação e interoperabilidade de código gerenciado/script. Para obter informações detalhadas sobre o <xref:System.Windows.Controls.WebBrowser> controle, consulte <xref:System.Windows.Controls.WebBrowser> .

Como <xref:System.Windows.Controls.Frame> , navegar para HTML usando <xref:System.Windows.Controls.WebBrowser> requer permissões especiais. Por exemplo, de um aplicativo de confiança parcial, você pode navegar apenas para HTML localizado no site de origem. Para obter mais informações, consulte [Segurança parcialmente confiável do WPF](../wpf-partial-trust-security.md).

<a name="Navigating_to_Objects"></a>

### <a name="navigating-to-custom-objects"></a>Navegar para objetos personalizados

Se você tiver dados armazenados como objetos personalizados, uma maneira de exibir esses dados é criar um <xref:System.Windows.Controls.Page> com conteúdo associado a esses objetos (consulte [visão geral de ligação de dados](../../../desktop-wpf/data/data-binding-overview.md)). Se você não precisar da sobrecarga resultante da criação de uma página inteira apenas para exibir os objetos, você poderá navegar diretamente para eles em vez disso.

Considere a `Person` classe implementada no código a seguir.

[!code-csharp[NavigateToObjectSnippets#PersonClassCODE](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigateToObjectSnippets/CSharp/Person.cs#personclasscode)]
[!code-vb[NavigateToObjectSnippets#PersonClassCODE](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigateToObjectSnippets/VisualBasic/Person.vb#personclasscode)]

Para navegar até ele, você chama o <xref:System.Windows.Navigation.NavigationWindow.Navigate%2A?displayProperty=nameWithType> método, conforme demonstrado pelo código a seguir.

[!code-xaml[NavigateToObjectSnippets#PageThatNavsToObject1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigateToObjectSnippets/CSharp/HomePage.xaml#pagethatnavstoobject1)]
[!code-xaml[NavigateToObjectSnippets#PageThatNavsToObject2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigateToObjectSnippets/CSharp/HomePage.xaml#pagethatnavstoobject2)]
[!code-xaml[NavigateToObjectSnippets#PageThatNavsToObject3](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigateToObjectSnippets/CSharp/HomePage.xaml#pagethatnavstoobject3)]

[!code-csharp[NavigateToObjectSnippets#PageThatNavsToObjectCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigateToObjectSnippets/CSharp/HomePage.xaml.cs#pagethatnavstoobjectcodebehind)]
[!code-vb[NavigateToObjectSnippets#PageThatNavsToObjectCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigateToObjectSnippets/VisualBasic/HomePage.xaml.vb#pagethatnavstoobjectcodebehind)]

A figura a seguir mostra o resultado.

![Uma página que navega para uma classe](./media/navigation-overview/page-navigates-to-an-object.png "Este é um exemplo de uma página que navega para um objeto.")

Nessa figura, você pode ver que nada útil é exibido. Na verdade, o valor que é exibido é o valor de retorno do `ToString` método para o objeto **Person** ; por padrão, esse é o único valor que o WPF pode usar para representar seu objeto. Você pode substituir o `ToString` método para retornar informações mais significativas, embora ele ainda seja um valor de cadeia de caracteres. Uma técnica que você pode usar que aproveita os recursos de apresentação do WPF é usar um modelo de dados. Você pode implementar um modelo de dados que o WPF possa associar a um objeto de um tipo específico. O código a seguir mostra um modelo de dados para o `Person` objeto.

[!code-xaml[NavigateToObjectSnippets#DataTemplateMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigateToObjectSnippets/CSharp/App.xaml#datatemplatemarkup)]

Aqui, o modelo de dados é associado ao `Person` tipo usando a `x:Type` extensão de marcação no `DataType` atributo. Em seguida, o modelo de dados associa `TextBlock` os elementos (consulte <xref:System.Windows.Controls.TextBlock> ) às propriedades da `Person` classe. A figura a seguir mostra a aparência atualizada do `Person` objeto.

![Navegando para uma classe que tem um modelo de dados](./media/navigation-overview/navigating-to-a-class.png "Navegando para uma classe que tem um modelo de dados.")

Uma vantagem dessa técnica é a consistência você obtém sendo capaz de reutilizar o modelo de dados para exibir os objetos de forma consistente em qualquer lugar no aplicativo.

Para obter mais informações sobre modelos de dados, consulte [visão geral de modelagem de dados](../data/data-templating-overview.md).

<a name="Security"></a>

## <a name="security"></a>Segurança

O suporte à navegação do WPF permite que XBAPs sejam navegados pela Internet e permite que os aplicativos hospedem conteúdo de terceiros. Para proteger aplicativos e usuários do comportamento prejudicial, o WPF fornece uma variedade de recursos de segurança que são discutidos [em segurança](../security-wpf.md) e [segurança de confiança parcial do WPF](../wpf-partial-trust-security.md).

## <a name="see-also"></a>Consulte também

- <xref:System.Windows.Application.SetCookie%2A>
- <xref:System.Windows.Application.GetCookie%2A>
- [Visão geral de gerenciamento do aplicativo](application-management-overview.md)
- [URIs "pack://" no WPF](pack-uris-in-wpf.md)
- [Visão geral da navegação estruturada](structured-navigation-overview.md)
- [Visão geral de topologias da navegação](navigation-topologies-overview.md)
- [Tópicos explicativos](navigation-how-to-topics.md)
- [Implantando um aplicativo WPF](deploying-a-wpf-application-wpf.md)
