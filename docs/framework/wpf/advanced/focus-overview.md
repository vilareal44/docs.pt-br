---
title: Visão geral do foco
description: 'Saiba mais sobre os dois conceitos principais do Windows Presentation Foundation que pertencem ao foco: foco do teclado e foco lógico.'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- applications [WPF], focus
- focus in applications [WPF]
ms.assetid: 0230c4eb-0c8a-462b-ac4b-ae3e511659f4
ms.openlocfilehash: 71aeb577cccd2c3b16df1c5a3cb772dd1f5479e2
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87165107"
---
# <a name="focus-overview"></a>Visão geral do foco
Em [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)], há dois conceitos principais relativos ao foco: foco do teclado e foco lógico.  O foco do teclado refere-se ao elemento que recebe entrada do teclado e foco lógico refere-se ao elemento em um escopo de foco que tem foco.  Esses conceitos são discutidos em detalhes nesta visão geral.  Noções básicas sobre a diferença entre esses conceitos é importante para a criação de aplicativos complexos que têm várias regiões no qual o foco pode ser obtido.  
  
 As classes principais que participam do gerenciamento de foco são a <xref:System.Windows.Input.Keyboard> classe, a <xref:System.Windows.Input.FocusManager> classe e as classes de elementos base, como <xref:System.Windows.UIElement> e <xref:System.Windows.ContentElement> .  Para obter mais informações sobre os elementos base, consulte [Visão geral de elementos base](base-elements-overview.md).  
  
 A <xref:System.Windows.Input.Keyboard> classe está preocupada principalmente com o foco do teclado e o <xref:System.Windows.Input.FocusManager> está preocupado principalmente com o foco lógico, mas essa não é uma distinção absoluta.  Um elemento que tem o foco do teclado também terá foco lógico, mas não necessariamente um elemento que tenha foco lógico tem o foco do teclado.  Isso é aparente quando você usa a <xref:System.Windows.Input.Keyboard> classe para definir o elemento que tem o foco do teclado, para ele também define o foco lógico no elemento.  

<a name="Keyboard_Focus"></a>
## <a name="keyboard-focus"></a>Foco do teclado  
 O foco do teclado refere-se ao elemento que está recebendo atualmente entrada do teclado.  Em toda a área de trabalho, pode haver apenas um elemento que tem o foco do teclado.  No [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] , o elemento que tem o foco do teclado terá <xref:System.Windows.IInputElement.IsKeyboardFocused%2A> definido como `true` .  A propriedade estática <xref:System.Windows.Input.Keyboard.FocusedElement%2A> na <xref:System.Windows.Input.Keyboard> classe obtém o elemento que atualmente tem o foco do teclado.  
  
 Para que um elemento obtenha o foco do teclado, as <xref:System.Windows.UIElement.Focusable%2A> Propriedades e <xref:System.Windows.UIElement.IsVisible%2A> nos elementos base devem ser definidas como `true` .  Algumas classes, como a <xref:System.Windows.Controls.Panel> classe base, foram <xref:System.Windows.UIElement.Focusable%2A> definidas como `false` por padrão; portanto, você deve definir <xref:System.Windows.UIElement.Focusable%2A> como `true` se desejar que esse elemento seja capaz de obter o foco do teclado.  
  
 O foco do teclado pode ser obtido por meio de interação do usuário com o [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)], como tabulações para um elemento ou clique do mouse sobre determinados elementos.  O foco do teclado também pode ser obtido programaticamente usando o <xref:System.Windows.Input.Keyboard.Focus%2A> método na <xref:System.Windows.Input.Keyboard> classe.  O <xref:System.Windows.Input.Keyboard.Focus%2A> método tenta dar o foco do teclado do elemento especificado.  O elemento retornado é o elemento que tem o foco do teclado, que pode ser um elemento diferente do solicitado se o velho ou o novo objeto de foco bloquear a solicitação.  
  
 O exemplo a seguir usa o <xref:System.Windows.Input.Keyboard.Focus%2A> método para definir o foco do teclado em um <xref:System.Windows.Controls.Button> .  
  
 [!code-csharp[focussample#FocusSampleSetFocus](~/samples/snippets/csharp/VS_Snippets_Wpf/FocusSample/CSharp/Window1.xaml.cs#focussamplesetfocus)]
 [!code-vb[focussample#FocusSampleSetFocus](~/samples/snippets/visualbasic/VS_Snippets_Wpf/FocusSample/visualbasic/window1.xaml.vb#focussamplesetfocus)]  
  
 A <xref:System.Windows.UIElement.IsKeyboardFocused%2A> Propriedade nas classes do elemento base Obtém um valor que indica se o elemento tem o foco do teclado.  A <xref:System.Windows.UIElement.IsKeyboardFocusWithin%2A> Propriedade nas classes do elemento base Obtém um valor que indica se o elemento ou qualquer um de seus elementos filho visuais tem o foco do teclado.  
  
 Ao definir o foco inicial na inicialização do aplicativo, o elemento para receber o foco deve estar na árvore visual da janela inicial carregada pelo aplicativo e o elemento deve ter <xref:System.Windows.UIElement.Focusable%2A> e <xref:System.Windows.UIElement.IsVisible%2A> definido como `true` .  O local recomendado para definir o foco inicial é no <xref:System.Windows.FrameworkElement.Loaded> manipulador de eventos.  Um <xref:System.Windows.Threading.Dispatcher> retorno de chamada também pode ser usado chamando <xref:System.Windows.Threading.Dispatcher.Invoke%2A> ou <xref:System.Windows.Threading.Dispatcher.BeginInvoke%2A> .  
  
<a name="Logical_Focus"></a>
## <a name="logical-focus"></a>Foco lógico  
 Foco lógico refere-se a <xref:System.Windows.Input.FocusManager.FocusedElement%2A?displayProperty=nameWithType> em um escopo de foco.  Um escopo de foco é um elemento que controla o <xref:System.Windows.Input.FocusManager.FocusedElement%2A> dentro de seu escopo.  Quando o foco de teclado deixar um escopo de foco, o elemento com foco perderá o foco do teclado mas manterá o foco lógico.  Quando o foco de teclado retornar para o escopo de foco, o elemento com foco obterá o foco do teclado.  Isso permite que o foco do teclado seja alterado entre vários escopos de foco, mas assegura que o elemento focalizado no escopo de foco recupere o foco do teclado quando o foco retornar para o escopo de foco.  
  
 Pode haver vários elementos que têm foco lógico em um aplicativo, mas pode haver apenas um elemento com foco lógico em um escopo de foco específico.  
  
 Um elemento que tem o foco do teclado tem foco lógico para o escopo de foco a que pertence.  
  
 Um elemento pode ser transformado em um escopo de foco no [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] definindo a <xref:System.Windows.Input.FocusManager> Propriedade anexada <xref:System.Windows.Input.FocusManager.IsFocusScope%2A> como `true` .  No código, um elemento pode ser transformado em um escopo de foco chamando <xref:System.Windows.Input.FocusManager.SetIsFocusScope%2A> .  
  
 O exemplo a seguir coloca um <xref:System.Windows.Controls.StackPanel> em um escopo de foco definindo a <xref:System.Windows.Input.FocusManager.IsFocusScope%2A> Propriedade anexada.  
  
 [!code-xaml[MarkupSnippets#MarkupIsFocusScopeXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/MarkupSnippets/CSharp/Window1.xaml#markupisfocusscopexaml)]  
  
 [!code-csharp[FocusSnippets#FocusSetIsFocusScope](~/samples/snippets/csharp/VS_Snippets_Wpf/FocusSnippets/CSharp/Window1.xaml.cs#focussetisfocusscope)]
 [!code-vb[FocusSnippets#FocusSetIsFocusScope](~/samples/snippets/visualbasic/VS_Snippets_Wpf/FocusSnippets/visualbasic/window1.xaml.vb#focussetisfocusscope)]  
  
 <xref:System.Windows.Input.FocusManager.GetFocusScope%2A>Retorna o escopo de foco para o elemento especificado.  
  
 As classes nas [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] quais os escopos de foco são, por padrão <xref:System.Windows.Window> , são, <xref:System.Windows.Controls.MenuItem> , <xref:System.Windows.Controls.ToolBar> e <xref:System.Windows.Controls.ContextMenu> .  
  
 <xref:System.Windows.Input.FocusManager.GetFocusedElement%2A>Obtém o elemento focalizado para o escopo de foco especificado.  <xref:System.Windows.Input.FocusManager.SetFocusedElement%2A>define o elemento focalizado no escopo de foco especificado.  <xref:System.Windows.Input.FocusManager.SetFocusedElement%2A>normalmente é usado para definir o elemento focalizado inicial.  
  
 O exemplo a seguir define o elemento focalizado em um escopo de foco e obtém o elemento focalizado de um escopo de foco.  
  
 [!code-csharp[FocusSnippets#FocusGetSetFocusedElement](~/samples/snippets/csharp/VS_Snippets_Wpf/FocusSnippets/CSharp/Window1.xaml.cs#focusgetsetfocusedelement)]
 [!code-vb[FocusSnippets#FocusGetSetFocusedElement](~/samples/snippets/visualbasic/VS_Snippets_Wpf/FocusSnippets/visualbasic/window1.xaml.vb#focusgetsetfocusedelement)]  
  
<a name="Keyboard_Navigation"></a>
## <a name="keyboard-navigation"></a>Navegação por teclado  
 A <xref:System.Windows.Input.KeyboardNavigation> classe é responsável pela implementação da navegação de foco de teclado padrão quando uma das teclas de navegação é pressionada.  As teclas de navegação são: TAB, SHIFT + TAB, CTRL + TAB, CTRL + SHIFT + TAB, setas para cima, para baixo, esquerda e direita.  
  
 O comportamento de navegação de um contêiner de navegação pode ser alterado definindo as <xref:System.Windows.Input.KeyboardNavigation> Propriedades anexadas <xref:System.Windows.Input.KeyboardNavigation.TabNavigation%2A> , <xref:System.Windows.Input.KeyboardNavigation.ControlTabNavigation%2A> e <xref:System.Windows.Input.KeyboardNavigation.DirectionalNavigation%2A> .  Essas propriedades são do tipo <xref:System.Windows.Input.KeyboardNavigationMode> e os valores possíveis são <xref:System.Windows.Input.KeyboardNavigationMode.Continue> , <xref:System.Windows.Input.KeyboardNavigationMode.Local> ,,, <xref:System.Windows.Input.KeyboardNavigationMode.Contained> <xref:System.Windows.Input.KeyboardNavigationMode.Cycle> <xref:System.Windows.Input.KeyboardNavigationMode.Once> e <xref:System.Windows.Input.KeyboardNavigationMode.None> .  O valor padrão é <xref:System.Windows.Input.KeyboardNavigationMode.Continue> , que significa que o elemento não é um contêiner de navegação.  
  
 O exemplo a seguir cria um <xref:System.Windows.Controls.Menu> com um número de <xref:System.Windows.Controls.MenuItem> objetos.  A <xref:System.Windows.Input.KeyboardNavigation.TabNavigation%2A> Propriedade anexada está definida como <xref:System.Windows.Input.KeyboardNavigationMode.Cycle> no <xref:System.Windows.Controls.Menu> .  Quando o foco é alterado usando a tecla Tab dentro de <xref:System.Windows.Controls.Menu> , o foco se moverá de cada elemento e quando o último elemento for atingido, o foco retornará ao primeiro elemento.  
  
 [!code-xaml[MarkupSnippets#MarkupKeyboardNavigationTabNavigationXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/MarkupSnippets/CSharp/Window1.xaml#markupkeyboardnavigationtabnavigationxaml)]  
  
 [!code-csharp[MarkupSnippets#MarkupKeyboardNavigationTabNavigationCODE](~/samples/snippets/csharp/VS_Snippets_Wpf/MarkupSnippets/CSharp/Window1.xaml.cs#markupkeyboardnavigationtabnavigationcode)]
 [!code-vb[MarkupSnippets#MarkupKeyboardNavigationTabNavigationCODE](~/samples/snippets/visualbasic/VS_Snippets_Wpf/MarkupSnippets/visualbasic/window1.xaml.vb#markupkeyboardnavigationtabnavigationcode)]  
  
<a name="Manipulating_Focus_Programmatically"></a>
## <a name="navigating-focus-programmatically"></a>Foco de navegação por meio de programação  
 A API adicional para trabalhar com o foco são <xref:System.Windows.UIElement.MoveFocus%2A> e <xref:System.Windows.UIElement.PredictFocus%2A> .  
  
 <xref:System.Windows.FrameworkElement.MoveFocus%2A>altera o foco para o próximo elemento no aplicativo.  Um <xref:System.Windows.Input.TraversalRequest> é usado para especificar a direção.   O <xref:System.Windows.Input.FocusNavigationDirection> passado para <xref:System.Windows.UIElement.MoveFocus%2A> especifica o foco de direções diferentes que pode ser movido, <xref:System.Windows.Input.FocusNavigationDirection.First> como <xref:System.Windows.Input.FocusNavigationDirection.Last> , <xref:System.Windows.Input.FocusNavigationDirection.Up> e <xref:System.Windows.Input.FocusNavigationDirection.Down> .  
  
 O exemplo a seguir usa <xref:System.Windows.FrameworkElement.MoveFocus%2A> para alterar o elemento focalizado.  
  
 [!code-csharp[focussample#FocusSampleMoveFocus](~/samples/snippets/csharp/VS_Snippets_Wpf/FocusSample/CSharp/Window1.xaml.cs#focussamplemovefocus)]
 [!code-vb[focussample#FocusSampleMoveFocus](~/samples/snippets/visualbasic/VS_Snippets_Wpf/FocusSample/visualbasic/window1.xaml.vb#focussamplemovefocus)]  
  
 <xref:System.Windows.FrameworkElement.PredictFocus%2A>Retorna o objeto que receberia o foco se o foco fosse alterado.  Atualmente, somente o, o, o <xref:System.Windows.Input.FocusNavigationDirection.Up> <xref:System.Windows.Input.FocusNavigationDirection.Down> e o <xref:System.Windows.Input.FocusNavigationDirection.Left> <xref:System.Windows.Input.FocusNavigationDirection.Right> têm suporte do <xref:System.Windows.FrameworkElement.PredictFocus%2A> .  
  
<a name="Focus_Events"></a>
## <a name="focus-events"></a>Eventos de foco  
 Os eventos relacionados ao foco do teclado são <xref:System.Windows.Input.Keyboard.PreviewGotKeyboardFocus> , <xref:System.Windows.Input.Keyboard.GotKeyboardFocus> e <xref:System.Windows.Input.Keyboard.PreviewLostKeyboardFocus> , <xref:System.Windows.Input.Keyboard.LostKeyboardFocus> .  Os eventos são definidos como eventos anexados na <xref:System.Windows.Input.Keyboard> classe, mas são mais prontamente acessíveis como eventos roteados equivalentes nas classes de elemento base.  Para obter mais informações sobre os eventos, consulte a [Visão geral de eventos roteados](routed-events-overview.md).  
  
 <xref:System.Windows.Input.Keyboard.GotKeyboardFocus>é gerado quando o elemento Obtém o foco do teclado.  <xref:System.Windows.Input.Keyboard.LostKeyboardFocus>é gerado quando o elemento perde o foco do teclado.  Se o <xref:System.Windows.Input.Keyboard.PreviewGotKeyboardFocus> evento ou o <xref:System.Windows.Input.Keyboard.PreviewLostKeyboardFocusEvent> evento for manipulado e <xref:System.Windows.RoutedEventArgs.Handled%2A> definido como `true` , o foco não será alterado.  
  
 O exemplo a seguir anexa <xref:System.Windows.UIElement.GotKeyboardFocus> e <xref:System.Windows.UIElement.LostKeyboardFocus> manipuladores de eventos a um <xref:System.Windows.Controls.TextBox> .  
  
 [!code-xaml[keyboardsample#KeyboardSampleXAMLHandlerHookup](~/samples/snippets/csharp/VS_Snippets_Wpf/KeyboardSample/CSharp/Window1.xaml#keyboardsamplexamlhandlerhookup)]  
  
 Quando o <xref:System.Windows.Controls.TextBox> foco do teclado é obtido, a <xref:System.Windows.Controls.Control.Background%2A> Propriedade do <xref:System.Windows.Controls.TextBox> é alterada para <xref:System.Windows.Media.Brushes.LightBlue%2A> .  
  
 [!code-csharp[keyboardsample#KeyboardSampleGotFocus](~/samples/snippets/csharp/VS_Snippets_Wpf/KeyboardSample/CSharp/Window1.xaml.cs#keyboardsamplegotfocus)]
 [!code-vb[keyboardsample#KeyboardSampleGotFocus](~/samples/snippets/visualbasic/VS_Snippets_Wpf/KeyboardSample/visualbasic/window1.xaml.vb#keyboardsamplegotfocus)]  
  
 Quando o <xref:System.Windows.Controls.TextBox> foco do teclado é perdida, a <xref:System.Windows.Controls.Control.Background%2A> Propriedade do <xref:System.Windows.Controls.TextBox> é alterada de volta para branco.  
  
 [!code-csharp[keyboardsample#KeyboardSampleLostFocus](~/samples/snippets/csharp/VS_Snippets_Wpf/KeyboardSample/CSharp/Window1.xaml.cs#keyboardsamplelostfocus)]
 [!code-vb[keyboardsample#KeyboardSampleLostFocus](~/samples/snippets/visualbasic/VS_Snippets_Wpf/KeyboardSample/visualbasic/window1.xaml.vb#keyboardsamplelostfocus)]  
  
 Os eventos relacionados ao foco lógico são <xref:System.Windows.UIElement.GotFocus> e <xref:System.Windows.UIElement.LostFocus> .  Esses eventos são definidos no <xref:System.Windows.Input.FocusManager> como eventos anexados, mas o não <xref:System.Windows.Input.FocusManager> expõe os wrappers de eventos CLR.  <xref:System.Windows.UIElement>e <xref:System.Windows.ContentElement> expor esses eventos de forma mais conveniente.  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.Input.FocusManager>
- <xref:System.Windows.UIElement>
- <xref:System.Windows.ContentElement>
- [Visão geral da entrada](input-overview.md)
- [Visão geral de elementos base](base-elements-overview.md)
