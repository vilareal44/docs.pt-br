---
title: Visão geral do pop-up
description: Saiba mais sobre o controle pop-up Windows Presentation Foundation, que fornece uma maneira de exibir o conteúdo em uma janela que flutua sobre o aplicativo atual.
ms.date: 03/30/2017
helpviewer_keywords:
- controls [WPF], Popup
- Popup control [WPF], about Popup control
ms.assetid: 774f53ca-bff8-470e-9ce9-3928b4cf3d4c
ms.openlocfilehash: 84eaddc53366df6d1da1a0a005d3618268f8cce2
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87167540"
---
# <a name="popup-overview"></a>Visão geral do pop-up
O <xref:System.Windows.Controls.Primitives.Popup> controle fornece uma maneira de exibir o conteúdo em uma janela separada que flutua sobre a janela do aplicativo atual em relação a um elemento ou uma coordenada de tela designada. Este tópico apresenta o <xref:System.Windows.Controls.Primitives.Popup> controle e fornece informações sobre seu uso.  

<a name="What_Is_a_Popup_"></a>
## <a name="what-is-a-popup"></a>O que é um pop-up?  
 Um <xref:System.Windows.Controls.Primitives.Popup> controle exibe o conteúdo em uma janela separada em relação a um elemento ou ponto na tela. Quando o <xref:System.Windows.Controls.Primitives.Popup> estiver visível, a <xref:System.Windows.Controls.Primitives.Popup.IsOpen%2A> propriedade será definida como `true` .  
  
> [!NOTE]
> Um não <xref:System.Windows.Controls.Primitives.Popup> é aberto automaticamente quando o ponteiro do mouse se move sobre seu objeto pai. Se você quiser que um <xref:System.Windows.Controls.Primitives.Popup> seja aberto automaticamente, use <xref:System.Windows.Controls.ToolTip> a <xref:System.Windows.Controls.ToolTipService> classe ou. Para obter mais informações, consulte [Visão geral de ToolTip](tooltip-overview.md).  
  
<a name="APopupExample"></a>
## <a name="creating-a-popup"></a>Criar um pop-up  
 O exemplo a seguir mostra como definir um <xref:System.Windows.Controls.Primitives.Popup> controle que é o elemento filho de um <xref:System.Windows.Controls.Button> controle. Como um <xref:System.Windows.Controls.Button> só pode ter um elemento filho, este exemplo coloca o texto para o <xref:System.Windows.Controls.Button> e os <xref:System.Windows.Controls.Primitives.Popup> controles em um <xref:System.Windows.Controls.StackPanel> . O conteúdo do <xref:System.Windows.Controls.Primitives.Popup> aparece em um <xref:System.Windows.Controls.TextBlock> controle, que exibe seu texto em uma janela separada que flutua sobre a janela do aplicativo próximo ao controle relacionado <xref:System.Windows.Controls.Button> .  
  
 [!code-xaml[PopupSimple#1](~/samples/snippets/csharp/VS_Snippets_Wpf/PopupSimple/CSharp/Window1.xaml#1)]  
  
 [!code-xaml[PopupSimple#CreatePopupCodeXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/PopupSimple/CSharp/Window1.xaml#createpopupcodexaml)]  
  
<a name="PopupUses"></a>
## <a name="controls-that-implement-a-popup"></a>Controles que implementam um pop-up  
 Você pode criar <xref:System.Windows.Controls.Primitives.Popup> controles em outros controles. Os seguintes controles implementam o <xref:System.Windows.Controls.Primitives.Popup> controle para usos específicos:  
  
- <xref:System.Windows.Controls.ToolTip>. Se você quiser criar uma dica de ferramenta para um elemento, use <xref:System.Windows.Controls.ToolTip> as <xref:System.Windows.Controls.ToolTipService> classes e. Para obter mais informações, consulte [Visão geral de ToolTip](tooltip-overview.md).  
  
- <xref:System.Windows.Controls.ContextMenu>. Se você quiser criar um menu de contexto para um elemento, use o <xref:System.Windows.Controls.ContextMenu> controle. Para mais informações, consulte [Visão geral do ContextMenu](contextmenu-overview.md).  
  
- <xref:System.Windows.Controls.ComboBox>. Se você quiser criar um controle de seleção que tenha uma caixa de listagem suspensa que possa ser exibida ou oculta, use o <xref:System.Windows.Controls.ComboBox> controle.  
  
- <xref:System.Windows.Controls.Expander>. Se você quiser criar um controle que exibe um cabeçalho com uma área recolhível que exibe o conteúdo, use o <xref:System.Windows.Controls.Expander> controle. Para mais informações, consulte [Visão geral do expansor](expander-overview.md).  
  
<a name="PopupBehaviorandAppearance"></a>
## <a name="popup-behavior-and-appearance"></a>Aparência e comportamento de pop-up  
 O <xref:System.Windows.Controls.Primitives.Popup> controle fornece funcionalidade que permite personalizar seu comportamento e aparência. Por exemplo, você pode definir comportamento de abrir e fechar, animação, opacidade e efeitos de bitmap, e <xref:System.Windows.Controls.Primitives.Popup> tamanho e posição.  
  
<a name="OpenandCloseBehavior"></a>
### <a name="open-and-close-behavior"></a>Comportamento de abertura e fechamento  
 Um <xref:System.Windows.Controls.Primitives.Popup> controle exibe seu conteúdo quando a <xref:System.Windows.Controls.Primitives.Popup.IsOpen%2A> propriedade é definida como `true` . Por padrão, <xref:System.Windows.Controls.Primitives.Popup> permanece aberto até que a <xref:System.Windows.Controls.Primitives.Popup.IsOpen%2A> propriedade seja definida como `false` . No entanto, você pode alterar o comportamento padrão definindo a <xref:System.Windows.Controls.Primitives.Popup.StaysOpen%2A> propriedade como `false` . Quando você define essa propriedade como `false` , a <xref:System.Windows.Controls.Primitives.Popup> janela de conteúdo tem a captura do mouse. O <xref:System.Windows.Controls.Primitives.Popup> perde a captura do mouse e a janela fecha quando um evento do mouse ocorre fora da <xref:System.Windows.Controls.Primitives.Popup> janela.  
  
 Os <xref:System.Windows.Controls.Primitives.Popup.Opened> <xref:System.Windows.Controls.Primitives.Popup.Closed> eventos e são gerados quando a <xref:System.Windows.Controls.Primitives.Popup> janela de conteúdo é aberta ou fechada.  
  
<a name="Animation"></a>
### <a name="animation"></a>Animação  
 O <xref:System.Windows.Controls.Primitives.Popup> controle tem suporte interno para as animações que normalmente são associadas a comportamentos como fade-in e deslizamento. Você pode ativar essas animações definindo a <xref:System.Windows.Controls.Primitives.Popup.PopupAnimation%2A> propriedade como um valor de <xref:System.Windows.Controls.Primitives.PopupAnimation> enumeração. Para que <xref:System.Windows.Controls.Primitives.Popup> as animações funcionem corretamente, você deve definir a <xref:System.Windows.Controls.Primitives.Popup.AllowsTransparency%2A> propriedade como `true` .  
  
 Você também pode aplicar animações como <xref:System.Windows.Media.Animation.Storyboard> ao <xref:System.Windows.Controls.Primitives.Popup> controle.  
  
<a name="OpacityandBitmapEffects"></a>
### <a name="opacity-and-bitmap-effects"></a>Efeitos de bitmap e opacidade  
 A <xref:System.Windows.UIElement.Opacity%2A> propriedade de um <xref:System.Windows.Controls.Primitives.Popup> controle não tem nenhum efeito sobre seu conteúdo. Por padrão, a <xref:System.Windows.Controls.Primitives.Popup> janela de conteúdo é opaca. Para criar uma transparente <xref:System.Windows.Controls.Primitives.Popup> , defina a <xref:System.Windows.Controls.Primitives.Popup.AllowsTransparency%2A> propriedade como `true` .  
  
 O conteúdo de um não <xref:System.Windows.Controls.Primitives.Popup> herda efeitos de bitmap, como <xref:System.Windows.Media.Effects.DropShadowBitmapEffect> , que você define diretamente no <xref:System.Windows.Controls.Primitives.Popup> controle ou em qualquer outro elemento na janela pai. Para que os efeitos de bitmap apareçam no conteúdo de um <xref:System.Windows.Controls.Primitives.Popup> , você deve definir o efeito de bitmap diretamente em seu conteúdo. Por exemplo, se o filho de um <xref:System.Windows.Controls.Primitives.Popup> for um <xref:System.Windows.Controls.StackPanel> , defina o efeito de bitmap no <xref:System.Windows.Controls.StackPanel> .  
  
<a name="PopupSize"></a>
### <a name="popup-size"></a>Tamanho de pop-up  
 Por padrão, um <xref:System.Windows.Controls.Primitives.Popup> é dimensionado automaticamente para seu conteúdo. Quando ocorre o dimensionamento automático, alguns efeitos de bitmap podem estar ocultos porque o tamanho padrão da área da tela que é definido para o <xref:System.Windows.Controls.Primitives.Popup> conteúdo não fornece espaço suficiente para que os efeitos de bitmap sejam exibidos.  
  
 <xref:System.Windows.Controls.Primitives.Popup>o conteúdo também pode ser obscurecido quando você define um <xref:System.Windows.UIElement.RenderTransform%2A> no conteúdo. Nesse cenário, algum conteúdo pode estar oculto se o conteúdo da transformação se <xref:System.Windows.Controls.Primitives.Popup> estender além da área do original <xref:System.Windows.Controls.Primitives.Popup> . Se um efeito de bitmap ou transformação exigir mais espaço, você poderá definir uma margem em volta do <xref:System.Windows.Controls.Primitives.Popup> conteúdo para fornecer mais área para o controle.  
  
<a name="DefiningPopupPosition"></a>
## <a name="defining-the-popup-position"></a>Definindo a posição de pop-up  
 Você pode posicionar um pop-up definindo as <xref:System.Windows.Controls.Primitives.Popup.PlacementTarget%2A> Propriedades,, <xref:System.Windows.Controls.Primitives.Popup.PlacementRectangle%2A> <xref:System.Windows.Controls.Primitives.Popup.Placement%2A> , <xref:System.Windows.Controls.Primitives.Popup.HorizontalOffset%2A> e <xref:System.Windows.Controls.Primitives.Popup.VerticalOffsetProperty> . Para mais informações, consulte [Comportamento de posicionamento de pop-up](popup-placement-behavior.md). Quando <xref:System.Windows.Controls.Primitives.Popup> é exibido na tela, ele não se reposicionará se o seu pai for reposicionado.  
  
<a name="CustomizingPopupPlacement"></a>
### <a name="customizing-popup-placement"></a>Personalizando o posicionamento do pop-up  
 Você pode personalizar o posicionamento de um <xref:System.Windows.Controls.Primitives.Popup> controle especificando um conjunto de coordenadas que são relativas ao local em que <xref:System.Windows.Controls.Primitives.Popup.PlacementTarget%2A> você deseja que <xref:System.Windows.Controls.Primitives.Popup> apareçam.  
  
 Para personalizar o posicionamento, defina a <xref:System.Windows.Controls.Primitives.Popup.Placement%2A> propriedade como <xref:System.Windows.Controls.Primitives.PlacementMode.Custom> . Em seguida, defina um <xref:System.Windows.Controls.Primitives.CustomPopupPlacementCallback> delegado que retorne um conjunto de possíveis pontos de posicionamento e eixos primários (em ordem de preferência) para o <xref:System.Windows.Controls.Primitives.Popup> . O ponto que mostra a maior parte do <xref:System.Windows.Controls.Primitives.Popup> é selecionado automaticamente. Para ver um exemplo, consulte [Especificar uma posição de pop-up personalizada](how-to-specify-a-custom-popup-position.md).  
  
<a name="PopupandtheVisualTree"></a>
## <a name="popup-and-the-visual-tree"></a>Pop-up e árvore visual  
 Um <xref:System.Windows.Controls.Primitives.Popup> controle não tem sua própria árvore visual; em vez disso, ele retorna um tamanho de 0 (zero) quando o <xref:System.Windows.Controls.Primitives.Popup.MeasureOverride%2A> método para <xref:System.Windows.Controls.Primitives.Popup> é chamado. No entanto, quando você define a <xref:System.Windows.Controls.Primitives.Popup.IsOpen%2A> propriedade de <xref:System.Windows.Controls.Primitives.Popup> como `true` , uma nova janela com sua própria árvore visual é criada. A nova janela contém o <xref:System.Windows.Controls.Primitives.Popup.Child%2A> conteúdo de <xref:System.Windows.Controls.Primitives.Popup> . A largura e altura da nova janela não podem ser maiores do que 75 por cento da largura ou altura da tela.  
  
 O <xref:System.Windows.Controls.Primitives.Popup> controle mantém uma referência ao seu <xref:System.Windows.Controls.Primitives.Popup.Child%2A> conteúdo como um filho lógico. Quando a nova janela é criada, o conteúdo de <xref:System.Windows.Controls.Primitives.Popup> se torna um filho visual da janela e permanece o filho lógico de <xref:System.Windows.Controls.Primitives.Popup> . Por outro lado, <xref:System.Windows.Controls.Primitives.Popup> permanece o pai lógico de seu <xref:System.Windows.Controls.Primitives.Popup.Child%2A> conteúdo.  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.Controls.Primitives.Popup>
- <xref:System.Windows.Controls.Primitives.PopupPrimaryAxis>
- <xref:System.Windows.Controls.Primitives.PlacementMode>
- <xref:System.Windows.Controls.Primitives.CustomPopupPlacement>
- <xref:System.Windows.Controls.Primitives.CustomPopupPlacementCallback>
- <xref:System.Windows.Controls.ToolTip>
- <xref:System.Windows.Controls.ToolTipService>
- [Tópicos explicativos](popup-how-to-topics.md)
- [Tópicos explicativos](tooltip-how-to-topics.md)
