---
title: Visão geral de ScrollViewer
description: Saiba mais sobre o controle ScrollViewer, que permite a rolagem de conteúdo em aplicativos Windows Presentation Foundation. Consulte os exemplos de uso.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- controls [WPF], ScrollViewer
- ScrollViewer control [WPF], about ScrollViewer control
ms.assetid: 94a13b94-cfdf-4b12-a1aa-90cb50c6e9b9
ms.openlocfilehash: 1ef680bb004315a2b523814714d5533535d044e5
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87164639"
---
# <a name="scrollviewer-overview"></a>Visão geral de ScrollViewer
O conteúdo dentro de uma interface do usuário geralmente é maior do que a área de exibição de uma tela de computador. O <xref:System.Windows.Controls.ScrollViewer> controle fornece uma maneira conveniente de habilitar a rolagem de conteúdo em [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] aplicativos. Este tópico apresenta o <xref:System.Windows.Controls.ScrollViewer> elemento e fornece vários exemplos de uso.  
  
<a name="what_is_a_scrollviewer_element"></a>
## <a name="the-scrollviewer-control"></a>O controle ScrollViewer  
 Há dois elementos predefinidos que permitem a rolagem em [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] aplicativos: <xref:System.Windows.Controls.Primitives.ScrollBar> e <xref:System.Windows.Controls.ScrollViewer> . O <xref:System.Windows.Controls.ScrollViewer> controle encapsula elementos horizontais e verticais <xref:System.Windows.Controls.Primitives.ScrollBar> e um contêiner de conteúdo (como um <xref:System.Windows.Controls.Panel> elemento) a fim de exibir outros elementos visíveis em uma área rolável. Você deve criar um objeto personalizado para usar o <xref:System.Windows.Controls.Primitives.ScrollBar> elemento para a rolagem de conteúdo. No entanto, você pode usar o <xref:System.Windows.Controls.ScrollViewer> elemento por si só porque ele é um controle composto que encapsula a <xref:System.Windows.Controls.Primitives.ScrollBar> funcionalidade.  
  
 O <xref:System.Windows.Controls.ScrollViewer> controle responde a comandos do mouse e do teclado e define vários métodos com os quais rolar o conteúdo por incrementos predeterminados. Você pode usar o <xref:System.Windows.Controls.ScrollViewer.ScrollChanged> evento para detectar uma alteração em um <xref:System.Windows.Controls.ScrollViewer> estado.  
  
 Um <xref:System.Windows.Controls.ScrollViewer> só pode ter um filho, normalmente um <xref:System.Windows.Controls.Panel> elemento que pode hospedar uma <xref:System.Windows.Controls.Panel.Children%2A> coleção de elementos. A <xref:System.Windows.Controls.ContentPresenter.Content%2A> propriedade define o único filho do <xref:System.Windows.Controls.ScrollViewer> .  
  
<a name="scrollviewer_physical_vs_logical"></a>
## <a name="physical-vs-logical-scrolling"></a>Rolagem física versus lógica  
 Rolagem física é usada para rolar conteúdo por um incremento físico predeterminado, geralmente por um valor declarado em pixels. Rolagem lógica é usada para rolar para o próximo item na árvore lógica. A rolagem física é o comportamento de rolagem padrão para a maioria dos <xref:System.Windows.Controls.Panel> elementos. O [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] dá suporte a ambos os tipos de rolagem.  
  
#### <a name="the-iscrollinfo-interface"></a>A interface IScrollInfo  
 A <xref:System.Windows.Controls.Primitives.IScrollInfo> interface representa a região de rolagem principal dentro de um <xref:System.Windows.Controls.ScrollViewer> controle ou derivado. A interface define propriedades de rolagem e métodos que podem ser implementados por <xref:System.Windows.Controls.Panel> elementos que exigem a rolagem por unidade lógica, em vez de um incremento físico. Converter uma instância do <xref:System.Windows.Controls.Primitives.IScrollInfo> em um derivado <xref:System.Windows.Controls.Panel> e, em seguida, usar seus métodos de rolagem fornece uma maneira útil de rolar para a próxima unidade lógica em uma coleção filho, em vez de por incremento de pixel. Por padrão, o <xref:System.Windows.Controls.ScrollViewer> controle dá suporte à rolagem por unidades físicas.  
  
 <xref:System.Windows.Controls.StackPanel>e <xref:System.Windows.Controls.VirtualizingStackPanel> implementam <xref:System.Windows.Controls.Primitives.IScrollInfo> e oferecem suporte nativo à rolagem lógica. Para controles de layout que oferecem suporte nativo à rolagem lógica, você ainda pode obter a rolagem física encapsulando o elemento de host <xref:System.Windows.Controls.Panel> em um <xref:System.Windows.Controls.ScrollViewer> e definindo a <xref:System.Windows.Controls.ScrollViewer.CanContentScroll%2A> propriedade como `false` .  
  
 O exemplo de código a seguir demonstra como converter uma instância do <xref:System.Windows.Controls.Primitives.IScrollInfo> em a <xref:System.Windows.Controls.StackPanel> e usar os métodos de rolagem de conteúdo ( <xref:System.Windows.Controls.Primitives.IScrollInfo.LineUp%2A> e <xref:System.Windows.Controls.Primitives.IScrollInfo.LineDown%2A> ) definidos pela interface.  
  
 [!code-csharp[IScrollInfoMethods#3](~/samples/snippets/csharp/VS_Snippets_Wpf/IScrollInfoMethods/CSharp/Window1.xaml.cs#3)]
 [!code-vb[IScrollInfoMethods#3](~/samples/snippets/visualbasic/VS_Snippets_Wpf/IScrollInfoMethods/VisualBasic/Window1.xaml.vb#3)]  
  
<a name="scrollviewer_markup_syntax_and_sample"></a>
## <a name="defining-and-using-a-scrollviewer-element"></a>Definindo e usando um elemento ScrollViewer  
 O exemplo a seguir cria um <xref:System.Windows.Controls.ScrollViewer> em uma janela que contém um texto e um retângulo. <xref:System.Windows.Controls.Primitives.ScrollBar>os elementos aparecem somente quando são necessários. Quando você redimensiona a janela, os <xref:System.Windows.Controls.Primitives.ScrollBar> elementos aparecem e desaparecem, devido aos valores atualizados das <xref:System.Windows.Controls.ScrollViewer.ComputedHorizontalScrollBarVisibility%2A> <xref:System.Windows.Controls.ScrollViewer.ComputedVerticalScrollBarVisibility%2A> Propriedades e.  
  
 [!code-cpp[ScrollViewer#1](~/samples/snippets/cpp/VS_Snippets_Wpf/ScrollViewer/CPP/ScrollViewer_wcp.cpp#1)]
 [!code-csharp[ScrollViewer#1](~/samples/snippets/csharp/VS_Snippets_Wpf/ScrollViewer/CSharp/ScrollViewer_wcp.cs#1)]
 [!code-vb[ScrollViewer#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ScrollViewer/VisualBasic/ScrollViewer.vb#1)]
 [!code-xaml[ScrollViewer#1](~/samples/snippets/xaml/VS_Snippets_Wpf/ScrollViewer/XAML/Pane1.xaml#1)]  
  
<a name="scrollviewer_styling_scrollviewer"></a>
## <a name="styling-a-scrollviewer"></a>Aplicando estilo a um ScrollViewer  
 Como todos os controles em Windows Presentation Foundation, o <xref:System.Windows.Controls.ScrollViewer> pode ser estilizado para alterar o comportamento de renderização padrão do controle. Para obter informações adicionais sobre aplicação de estilo de controle, consulte [Aplicação de estilo e modelagem](../../../desktop-wpf/fundamentals/styles-templates-overview.md).  
  
<a name="scrollviewer_scroll_vs_paginate"></a>
## <a name="paginating-documents"></a>Paginando documentos  
 Para o conteúdo do documento, uma alternativa à rolagem é escolher um contêiner de documento que dá suporte à paginação. <xref:System.Windows.Documents.FlowDocument>o é destinado a documentos que são projetados para serem hospedados em um controle de exibição, como <xref:System.Windows.Controls.FlowDocumentPageViewer> o, que dá suporte à paginação de conteúdo em várias páginas, evitando a necessidade de rolagem. <xref:System.Windows.Controls.DocumentViewer>fornece uma solução para exibir o <xref:System.Windows.Documents.FixedDocument> conteúdo, que usa a rolagem tradicional para exibir o conteúdo fora do Realm da área de exibição.  
  
 Para obter informações adicionais sobre formatos de documento e opções de apresentação, consulte [Documentos no WPF](../advanced/documents-in-wpf.md).  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.Controls.ScrollViewer>
- <xref:System.Windows.Controls.Primitives.ScrollBar>
- <xref:System.Windows.Controls.Primitives.IScrollInfo>
- [Como: criar um visualizador de rolagem](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms752352(v=vs.90))
- [Documentos no WPF](../advanced/documents-in-wpf.md)
- [Estilos e modelos ScrollBar](scrollbar-styles-and-templates.md)
- [Controles](../advanced/optimizing-performance-controls.md)
