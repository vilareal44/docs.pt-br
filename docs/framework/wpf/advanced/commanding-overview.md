---
title: Visão geral dos comandos
description: Saiba mais sobre comandos, um mecanismo de entrada no Windows Presentation Foundation que fornece tratamento de entrada em um nível mais semântico do que a entrada do dispositivo.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- interfaces [WPF], ICommandSource
- command library [WPF]
- commands [WPF], definition of
- CommandBindings [WPF]
- ICommandSource interfaces [WPF]
- commanding [WPF]
- CommandManager [WPF]
ms.assetid: bc208dfe-367d-426a-99de-52b7e7511e81
ms.openlocfilehash: 4f7d12fbf0de9b1546f15061ab7eb1318378bbbb
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87168121"
---
# <a name="commanding-overview"></a>Visão geral dos comandos
<a name="introduction"></a> Os comandos são um mecanismo de entrada do [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] que oferecem manipulação de entrada em um nível mais semântico do que a entrada do dispositivo. Os exemplos de comandos são as operações **Copiar**, **Recortar** e **Colar**, encontradas em muitos aplicativos.  
  
 Esta visão geral define quais comandos estão no [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)], quais classes fazem parte do modelo de comandos e como utilizar e criar comandos nos seus aplicativos.  
  
 Este tópico contém as seguintes seções:  
  
- [O que são comandos?](#commands_at_10000_feet)  
  
- [Exemplo de comando simples no WPF](#simple_command)  
  
- [Quatro conceitos principais em comandos do WPF](#Four_main_Concepts)  
  
- [Biblioteca de comandos](#Command_Library)  
  
- [Criando comandos personalizados](#creating_commands)  
  
<a name="commands_at_10000_feet"></a>
## <a name="what-are-commands"></a>O que são comandos?  
 Os comandos têm várias finalidades. A primeira finalidade é separar a semântica e o objeto que invoca um comando da lógica que o executa. Isso permite que fontes múltiplas e distintas invoquem a mesma lógica de comando, além de permitir que a lógica de comando seja personalizada para destinos diferentes. Por exemplo, as operações de edição **Copiar**, **Recortar** e **Colar**, que são encontradas em muitos aplicativos, podem ser invocadas por meio de ações do usuário diferentes se forem implementadas por meio do uso de comandos. Um aplicativo pode permitir que um usuário recorte texto ou objetos selecionados ao clicar em um botão, escolher um item em um menu ou usar uma combinação de teclas, como CTRL+X. Usando comandos, é possível associar cada tipo de ação do usuário à mesma lógica.  
  
 Outra finalidade dos comandos é indicar se uma ação está disponível. Para continuar o exemplo de recortar um objeto ou texto, a ação só faz sentido quando algo está selecionado. Se um usuário tentar recortar um objeto ou texto sem ter selecionado alguma coisa, nada acontece. Para indicar isso para o usuário, muitos aplicativos desabilitam botões e itens de menu para indicar se é possível executar uma ação. Um comando pode indicar se uma ação é possível pela implementação do método <xref:System.Windows.Input.ICommand.CanExecute%2A>. Um botão pode assinar o evento <xref:System.Windows.Input.ICommand.CanExecuteChanged> e ser desabilitado se <xref:System.Windows.Input.ICommand.CanExecute%2A> retorna `false` ou ser habilitado se <xref:System.Windows.Input.ICommand.CanExecute%2A> retorna `true`.  
  
 A semântica de um comando pode ser consistente entre aplicativos e classes; porém, a lógica da ação é específica para o objeto específico da ação. A combinação de teclas CTRL+X invoca o comando **Recortar** em classes de texto, classes de imagens e navegadores da Web, enquanto a lógica real para executar a operação **Recortar** é definida pelo aplicativo que faz o recorte. Um <xref:System.Windows.Input.RoutedCommand> permite que os clientes implementem a lógica. Um objeto de texto poderá recortar o texto selecionado na área de transferência, enquanto um objeto de imagem poderá recortar a imagem selecionada. Quando um aplicativo manipula o evento <xref:System.Windows.Input.CommandManager.Executed>, ele tem acesso ao destino do comando e pode executar a ação adequada, dependendo do tipo de destino.  
  
<a name="simple_command"></a>
## <a name="simple-command-example-in-wpf"></a>Exemplo de comando simples no WPF  
 A maneira mais simples de usar um comando no [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] é usar um <xref:System.Windows.Input.RoutedCommand> predefinido de uma das classes da biblioteca de comandos, usar um controle que tenha suporte nativo para manipular o comando e usar um controle que tenha suporte nativo para invocar um comando.  O comando <xref:System.Windows.Input.ApplicationCommands.Paste%2A> é um dos comandos predefinidos na classe <xref:System.Windows.Input.ApplicationCommands>.  O controle <xref:System.Windows.Controls.TextBox> tem uma lógica interna para manipular o comando <xref:System.Windows.Input.ApplicationCommands.Paste%2A>.  Além disso, a classe <xref:System.Windows.Controls.MenuItem> tem suporte nativo para invocar comandos.  
  
 O exemplo a seguir mostra como configurar um <xref:System.Windows.Controls.MenuItem> para que, quando ele receber um clique, ele invoque o comando <xref:System.Windows.Input.ApplicationCommands.Paste%2A> em uma <xref:System.Windows.Controls.TextBox>, supondo que a <xref:System.Windows.Controls.TextBox> tenha o foco do teclado.  
  
 [!code-xaml[CommandingOverviewSnippets#CommandingOverviewSimpleCommand](~/samples/snippets/csharp/VS_Snippets_Wpf/CommandingOverviewSnippets/CSharp/Window1.xaml#commandingoverviewsimplecommand)]  
  
 [!code-csharp[CommandingOverviewSnippets#CommandingOverviewCommandTargetCodeBehind](~/samples/snippets/csharp/VS_Snippets_Wpf/CommandingOverviewSnippets/CSharp/Window1.xaml.cs#commandingoverviewcommandtargetcodebehind)]
 [!code-vb[CommandingOverviewSnippets#CommandingOverviewCommandTargetCodeBehind](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CommandingOverviewSnippets/visualbasic/window1.xaml.vb#commandingoverviewcommandtargetcodebehind)]  
  
<a name="Four_main_Concepts"></a>
## <a name="four-main-concepts-in-wpf-commanding"></a>Quatro conceitos principais em comandos do WPF  
 O modelo de comando roteado no [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] pode ser dividido em quatro conceitos principais: o comando, a fonte do comando, o destino do comando e a associação do comando:  
  
- O *comando* é a ação que será executada.  
  
- A *fonte do comando* é o objeto que invoca o comando.  
  
- O *destino do comando* é o objeto no qual o comando está sendo executado.  
  
- A *associação do comando* é o objeto que mapeia a lógica de comando até o comando.  
  
 No exemplo anterior, o comando <xref:System.Windows.Input.ApplicationCommands.Paste%2A> é o comando, o <xref:System.Windows.Controls.MenuItem> é a origem de comando, a <xref:System.Windows.Controls.TextBox> é o destino de comando, e a associação de comando é fornecida pelo controle <xref:System.Windows.Controls.TextBox>.  Vale a pena observar que nem sempre a <xref:System.Windows.Input.CommandBinding> é fornecida pelo controle que é a classe de destino de comando.  Com muita frequência, a <xref:System.Windows.Input.CommandBinding> precisa ser criada pelo desenvolvedor de aplicativos, ou a <xref:System.Windows.Input.CommandBinding> pode ser anexada a um ancestral do destino de comando.  
  
<a name="Commands"></a>
### <a name="commands"></a>Comandos  
 Os comandos do [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] são criados pela implementação da interface <xref:System.Windows.Input.ICommand>.  <xref:System.Windows.Input.ICommand> expõe dois métodos, <xref:System.Windows.Input.ICommand.Execute%2A>, e <xref:System.Windows.Input.ICommand.CanExecute%2A>e um evento <xref:System.Windows.Input.ICommand.CanExecuteChanged>. <xref:System.Windows.Input.ICommand.Execute%2A> executa as ações associadas ao comando. <xref:System.Windows.Input.ICommand.CanExecute%2A> determina se o comando pode ser executado no destino de comando atual. <xref:System.Windows.Input.ICommand.CanExecuteChanged> é acionado quando o gerenciador de comandos que centraliza as operações de comandos detecta uma alteração na origem de comando que pode invalidar um comando que foi acionado, mas que ainda não foi executado pela associação de comando.  A implementação [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] de <xref:System.Windows.Input.ICommand> é a classe <xref:System.Windows.Input.RoutedCommand> e é o foco desta visão geral.  
  
 As principais fontes de entrada no [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] são o mouse, o teclado, tinta e comandos roteados.  As entradas mais orientadas ao dispositivo usam um <xref:System.Windows.RoutedEvent> para notificar aos objetos em uma página de aplicativo que ocorreu um evento de entrada.  Um <xref:System.Windows.Input.RoutedCommand> não é diferente.  Os métodos <xref:System.Windows.Input.RoutedCommand.Execute%2A> e <xref:System.Windows.Input.RoutedCommand.CanExecute%2A> de um <xref:System.Windows.Input.RoutedCommand> não contêm a lógica do aplicativo para o comando, mas acionam eventos roteados que passam por um túnel e se propagam pela árvore de elementos até encontrarem um objeto com uma <xref:System.Windows.Input.CommandBinding>.  A <xref:System.Windows.Input.CommandBinding> contém os manipuladores para esses eventos e são os manipuladores que executam o comando.  Para obter mais informações sobre o roteamento de eventos no [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)], consulte [Visão geral de eventos roteados](routed-events-overview.md).  
  
 O método <xref:System.Windows.Input.RoutedCommand.Execute%2A> em um <xref:System.Windows.Input.RoutedCommand> aciona os eventos <xref:System.Windows.Input.CommandManager.PreviewExecuted> e <xref:System.Windows.Input.CommandManager.Executed> no destino de comando.  O método <xref:System.Windows.Input.RoutedCommand.CanExecute%2A> em um <xref:System.Windows.Input.RoutedCommand> aciona os eventos <xref:System.Windows.Input.CommandManager.CanExecute> e <xref:System.Windows.Input.CommandManager.PreviewCanExecute> no destino de comando.  Esses eventos passam por um túnel e se propagam pela árvore de elementos até encontrarem um objeto que tem uma <xref:System.Windows.Input.CommandBinding> para esse comando específico.  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] fornece um conjunto de comandos roteados comuns distribuídos em várias classes: <xref:System.Windows.Input.MediaCommands>, <xref:System.Windows.Input.ApplicationCommands>, <xref:System.Windows.Input.NavigationCommands>, <xref:System.Windows.Input.ComponentCommands> e <xref:System.Windows.Documents.EditingCommands>.  Essas classes consistem apenas nos objetos <xref:System.Windows.Input.RoutedCommand> e não na lógica de implementação do comando.  A lógica de implementação é de responsabilidade do objeto no qual o comando está sendo executado.  
  
<a name="Command_Sources"></a>
### <a name="command-sources"></a>Fontes de comando  
 Uma fonte do comando é o objeto que invoca o comando.  Alguns exemplos de origem de comando são <xref:System.Windows.Controls.MenuItem>, <xref:System.Windows.Controls.Button> e <xref:System.Windows.Input.KeyGesture>.  
  
 Em geral, as origens de comando do [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] implementam a interface <xref:System.Windows.Input.ICommandSource>.  
  
 <xref:System.Windows.Input.ICommandSource> expõe três propriedades: <xref:System.Windows.Input.ICommandSource.Command%2A>, <xref:System.Windows.Input.ICommandSource.CommandTarget%2A> e <xref:System.Windows.Input.ICommandSource.CommandParameter%2A>:  
  
- <xref:System.Windows.Input.ICommandSource.Command%2A> é o comando a ser executado quando a origem de comando é invocada.  
  
- <xref:System.Windows.Input.ICommandSource.CommandTarget%2A> é o objeto no qual executar o comando.  Vale a pena observar que, no [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)], a propriedade <xref:System.Windows.Input.ICommandSource.CommandTarget%2A> em <xref:System.Windows.Input.ICommandSource> só é aplicável quando o <xref:System.Windows.Input.ICommand> é um <xref:System.Windows.Input.RoutedCommand>.  Se o <xref:System.Windows.Input.ICommandSource.CommandTarget%2A> estiver definido em uma <xref:System.Windows.Input.ICommandSource> e o comando correspondente não for um <xref:System.Windows.Input.RoutedCommand>, o destino de comando será ignorado. Se o <xref:System.Windows.Input.ICommandSource.CommandTarget%2A> não estiver definido, o elemento com o foco do teclado será o destino de comando.  
  
- <xref:System.Windows.Input.ICommandSource.CommandParameter%2A> é um tipo de dados definido pelo usuário usado para passar informações para os manipuladores que implementam o comando.  
  
 As classes [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] que implementam <xref:System.Windows.Input.ICommandSource> são <xref:System.Windows.Controls.Primitives.ButtonBase>, <xref:System.Windows.Controls.MenuItem>, <xref:System.Windows.Documents.Hyperlink> e <xref:System.Windows.Input.InputBinding>.  <xref:System.Windows.Controls.Primitives.ButtonBase>, <xref:System.Windows.Controls.MenuItem> e <xref:System.Windows.Documents.Hyperlink> invocam um comando quando eles recebem um clique, e uma <xref:System.Windows.Input.InputBinding> invoca um comando quando o <xref:System.Windows.Input.InputGesture> associado a ela é executado.  
  
 O exemplo a seguir mostra como usar um <xref:System.Windows.Controls.MenuItem> em um <xref:System.Windows.Controls.ContextMenu> como uma origem de comando para o comando <xref:System.Windows.Input.ApplicationCommands.Properties%2A>.  
  
 [!code-xaml[CommandingOverviewSnippets#CommandingOverviewCmdSourceXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/CommandingOverviewSnippets/CSharp/Window1.xaml#commandingoverviewcmdsourcexaml)]  
  
 [!code-csharp[CommandingOverviewSnippets#CommandingOverviewCmdSource](~/samples/snippets/csharp/VS_Snippets_Wpf/CommandingOverviewSnippets/CSharp/Window1.xaml.cs#commandingoverviewcmdsource)]
 [!code-vb[CommandingOverviewSnippets#CommandingOverviewCmdSource](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CommandingOverviewSnippets/visualbasic/window1.xaml.vb#commandingoverviewcmdsource)]  
  
 Normalmente, uma origem de comando escutará o evento <xref:System.Windows.Input.RoutedCommand.CanExecuteChanged>.  Esse evento informa à fonte de comando que a capacidade do comando de executar o destino do comando atual pode ter sido alterada.  A origem de comando pode consultar o status atual do <xref:System.Windows.Input.RoutedCommand> usando o método <xref:System.Windows.Input.RoutedCommand.CanExecute%2A>.  Em seguida, ela poderá ser desabilitada se não for possível executar o comando.  Um exemplo disso é um <xref:System.Windows.Controls.MenuItem> esmaecendo quando um comando não pode ser executado.  
  
 Um <xref:System.Windows.Input.InputGesture> pode ser usado como uma fonte do comando.  Dois tipos de gestos de entrada no [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] são o <xref:System.Windows.Input.KeyGesture> e o <xref:System.Windows.Input.MouseGesture>.  Considere um <xref:System.Windows.Input.KeyGesture> como um atalho de teclado, como CTRL+C.  Um <xref:System.Windows.Input.KeyGesture> é composto por uma <xref:System.Windows.Input.Key> e um conjunto de <xref:System.Windows.Input.ModifierKeys>.  Um <xref:System.Windows.Input.MouseGesture> é composto por uma <xref:System.Windows.Input.MouseAction> e um conjunto opcional de <xref:System.Windows.Input.ModifierKeys>.  
  
 Para que um <xref:System.Windows.Input.InputGesture> funcione como uma origem de comando, ele deve estar associado a um comando. Existem algumas maneiras de fazer isso.  Uma maneira é usar uma <xref:System.Windows.Input.InputBinding>.  
  
 O exemplo a seguir mostra como criar uma <xref:System.Windows.Input.KeyBinding> entre um <xref:System.Windows.Input.KeyGesture> e um <xref:System.Windows.Input.RoutedCommand>.  
  
 [!code-xaml[CommandingOverviewSnippets#CommandingOverviewXAMLKeyBinding](~/samples/snippets/csharp/VS_Snippets_Wpf/CommandingOverviewSnippets/CSharp/Window1.xaml#commandingoverviewxamlkeybinding)]  
  
 [!code-csharp[CommandingOverviewSnippets#CommandingOverviewKeyBinding](~/samples/snippets/csharp/VS_Snippets_Wpf/CommandingOverviewSnippets/CSharp/Window1.xaml.cs#commandingoverviewkeybinding)]
 [!code-vb[CommandingOverviewSnippets#CommandingOverviewKeyBinding](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CommandingOverviewSnippets/visualbasic/window1.xaml.vb#commandingoverviewkeybinding)]  
  
 Outra maneira de associar um <xref:System.Windows.Input.InputGesture> a um <xref:System.Windows.Input.RoutedCommand> é adicionar o <xref:System.Windows.Input.InputGesture> ao <xref:System.Windows.Input.InputGestureCollection> no <xref:System.Windows.Input.RoutedCommand>.  
  
 O exemplo a seguir mostra como adicionar um <xref:System.Windows.Input.KeyGesture> à <xref:System.Windows.Input.InputGestureCollection> de um <xref:System.Windows.Input.RoutedCommand>.  
  
 [!code-csharp[CommandingOverviewSnippets#CommandingOverviewKeyGestureOnCmd](~/samples/snippets/csharp/VS_Snippets_Wpf/CommandingOverviewSnippets/CSharp/Window1.xaml.cs#commandingoverviewkeygestureoncmd)]
 [!code-vb[CommandingOverviewSnippets#CommandingOverviewKeyGestureOnCmd](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CommandingOverviewSnippets/visualbasic/window1.xaml.vb#commandingoverviewkeygestureoncmd)]  
  
<a name="Command_Binding"></a>
### <a name="commandbinding"></a>CommandBinding  
 Uma <xref:System.Windows.Input.CommandBinding> associa um comando aos manipuladores de eventos que implementam o comando.  
  
 A classe <xref:System.Windows.Input.CommandBinding> contém uma propriedade <xref:System.Windows.Input.CommandBinding.Command%2A> e <xref:System.Windows.Input.CommandBinding.PreviewExecuted>, <xref:System.Windows.Input.CommandBinding.Executed>, <xref:System.Windows.Input.CommandBinding.PreviewCanExecute> e <xref:System.Windows.Input.CommandBinding.CanExecute> eventos.  
  
 <xref:System.Windows.Input.CommandBinding.Command%2A> é o comando ao qual a <xref:System.Windows.Input.CommandBinding> está sendo associada.  Os manipuladores de eventos anexados aos eventos <xref:System.Windows.Input.CommandBinding.PreviewExecuted> e <xref:System.Windows.Input.CommandBinding.Executed> implementam a lógica de comando.  Os manipuladores de eventos anexados aos eventos <xref:System.Windows.Input.CommandBinding.PreviewCanExecute> e <xref:System.Windows.Input.CommandBinding.CanExecute> determinam se o comando pode ser executado no destino de comando atual.  
  
 O exemplo a seguir mostra como criar uma <xref:System.Windows.Input.CommandBinding> na <xref:System.Windows.Window> raiz de um aplicativo.  A <xref:System.Windows.Input.CommandBinding> associa o comando <xref:System.Windows.Input.ApplicationCommands.Open%2A> aos manipuladores <xref:System.Windows.Input.CommandManager.Executed> e <xref:System.Windows.Input.CommandBinding.CanExecute>.  
  
 [!code-xaml[commandwithhandler#CommandHandlerCommandBinding](~/samples/snippets/csharp/VS_Snippets_Wpf/commandWithHandler/CSharp/Window1.xaml#commandhandlercommandbinding)]  
  
 [!code-csharp[CommandHandlerProcedural#CommandHandlerBindingInit](~/samples/snippets/csharp/VS_Snippets_Wpf/CommandHandlerProcedural/CSharp/Window1.xaml.cs#commandhandlerbindinginit)]
 [!code-vb[CommandHandlerProcedural#CommandHandlerBindingInit](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CommandHandlerProcedural/visualbasic/window1.xaml.vb#commandhandlerbindinginit)]  
  
 Em seguida, o <xref:System.Windows.Input.ExecutedRoutedEventHandler> e um <xref:System.Windows.Input.CanExecuteRoutedEventHandler> são criados.  O <xref:System.Windows.Input.ExecutedRoutedEventHandler> abre uma <xref:System.Windows.MessageBox> que exibe uma cadeia de caracteres indicando que o comando foi executado.  O <xref:System.Windows.Input.CanExecuteRoutedEventHandler> define a propriedade <xref:System.Windows.Input.CanExecuteRoutedEventArgs.CanExecute%2A> como `true`.  
  
 [!code-csharp[commandwithhandler#CommandHandlerExecutedHandler](~/samples/snippets/csharp/VS_Snippets_Wpf/commandWithHandler/CSharp/Window1.xaml.cs#commandhandlerexecutedhandler)]
 [!code-vb[commandwithhandler#CommandHandlerExecutedHandler](~/samples/snippets/visualbasic/VS_Snippets_Wpf/commandWithHandler/VisualBasic/Window1.xaml.vb#commandhandlerexecutedhandler)]  
  
 [!code-csharp[commandwithhandler#CommandHandlerCanExecuteHandler](~/samples/snippets/csharp/VS_Snippets_Wpf/commandWithHandler/CSharp/Window1.xaml.cs#commandhandlercanexecutehandler)]
 [!code-vb[commandwithhandler#CommandHandlerCanExecuteHandler](~/samples/snippets/visualbasic/VS_Snippets_Wpf/commandWithHandler/VisualBasic/Window1.xaml.vb#commandhandlercanexecutehandler)]  
  
 Uma <xref:System.Windows.Input.CommandBinding> está anexada a um objeto específico, como a <xref:System.Windows.Window> raiz do aplicativo ou de um controle.  O objeto ao qual a <xref:System.Windows.Input.CommandBinding> está anexada define o escopo da associação.  Por exemplo, uma <xref:System.Windows.Input.CommandBinding> anexada a um ancestral do destino de comando pode ser acessada pelo evento <xref:System.Windows.Input.CommandBinding.Executed>, mas uma <xref:System.Windows.Input.CommandBinding> anexada a um descendente do destino de comando não pode ser acessada.  Essa é uma consequência direta da forma como um <xref:System.Windows.RoutedEvent> passa por um túnel e se propaga do objeto que aciona o evento.  
  
 Em algumas situações, a <xref:System.Windows.Input.CommandBinding> está anexada ao próprio destino de comando, como ocorre com a classe <xref:System.Windows.Controls.TextBox> e os comandos <xref:System.Windows.Input.ApplicationCommands.Cut%2A>, <xref:System.Windows.Input.ApplicationCommands.Copy%2A> e <xref:System.Windows.Input.ApplicationCommands.Paste%2A>. No entanto, muitas vezes, é mais conveniente anexar a <xref:System.Windows.Input.CommandBinding> a um ancestral do destino de comando, como a <xref:System.Windows.Window> principal ou o objeto Application, especialmente se a mesma <xref:System.Windows.Input.CommandBinding> pode ser usada para vários destinos de comando.  São decisões de design que você deverá considerar no momento de criar sua infraestrutura de comando.  
  
<a name="Commane_Target"></a>
### <a name="command-target"></a>Destino do comando  
 O destino do comando é o elemento no qual o comando é executado.  Com relação a um <xref:System.Windows.Input.RoutedCommand>, o destino de comando é o elemento no qual o roteamento do <xref:System.Windows.Input.CommandManager.Executed> e do <xref:System.Windows.Input.CommandManager.CanExecute> é iniciado.  Conforme observado anteriormente, no [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)], a propriedade <xref:System.Windows.Input.ICommandSource.CommandTarget%2A> em <xref:System.Windows.Input.ICommandSource> só é aplicável quando o <xref:System.Windows.Input.ICommand> é um <xref:System.Windows.Input.RoutedCommand>.  Se o <xref:System.Windows.Input.ICommandSource.CommandTarget%2A> estiver definido em uma <xref:System.Windows.Input.ICommandSource> e o comando correspondente não for um <xref:System.Windows.Input.RoutedCommand>, o destino de comando será ignorado.  
  
 A fonte do comando pode definir explicitamente o destino do comando.  Se o destino do comando não for definido, o elemento com o foco do teclado será usado como destino do comando.  Uma das vantagens de usar o elemento com o foco do teclado como destino do comando é que ele permite que o desenvolvedor do aplicativo use a mesma fonte de comando para invocar um comando em vários destinos, sem a necessidade de controlar o destino do comando.  Por exemplo, se um <xref:System.Windows.Controls.MenuItem> invoca o comando **Paste** em um aplicativo que tem um controle <xref:System.Windows.Controls.TextBox> e um controle <xref:System.Windows.Controls.PasswordBox>, o destino pode ser a <xref:System.Windows.Controls.TextBox> ou a <xref:System.Windows.Controls.PasswordBox>, dependendo de qual controle tem o foco do teclado.  
  
 O exemplo a seguir mostra como definir explicitamente o destino do comando na marcação e no code-behind.  
  
 [!code-xaml[CommandingOverviewSnippets#CommandingOverviewXAMLCommandTarget](~/samples/snippets/csharp/VS_Snippets_Wpf/CommandingOverviewSnippets/CSharp/Window1.xaml#commandingoverviewxamlcommandtarget)]  
  
 [!code-csharp[CommandingOverviewSnippets#CommandingOverviewCommandTargetCodeBehind](~/samples/snippets/csharp/VS_Snippets_Wpf/CommandingOverviewSnippets/CSharp/Window1.xaml.cs#commandingoverviewcommandtargetcodebehind)]
 [!code-vb[CommandingOverviewSnippets#CommandingOverviewCommandTargetCodeBehind](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CommandingOverviewSnippets/visualbasic/window1.xaml.vb#commandingoverviewcommandtargetcodebehind)]  
  
<a name="Command_Manager"></a>
### <a name="the-commandmanager"></a>O CommandManager  
 O <xref:System.Windows.Input.CommandManager> fornece várias funções relacionadas a um comando.  Ele fornece um conjunto de métodos estáticos para adicionar os manipuladores de eventos <xref:System.Windows.Input.CommandManager.PreviewExecuted>, <xref:System.Windows.Input.CommandManager.Executed>, <xref:System.Windows.Input.CommandManager.PreviewCanExecute> e <xref:System.Windows.Input.CommandManager.CanExecute> a um elemento específico e removê-los de um deles.  Ele fornece um meio para registrar objetos <xref:System.Windows.Input.CommandBinding> e <xref:System.Windows.Input.InputBinding> em uma classe específica.  O <xref:System.Windows.Input.CommandManager> também fornece um meio, por meio do evento <xref:System.Windows.Input.CommandManager.RequerySuggested>, para notificar um comando quando ele deve acionar o evento <xref:System.Windows.Input.ICommand.CanExecuteChanged>.  
  
 O método <xref:System.Windows.Input.CommandManager.InvalidateRequerySuggested%2A> força o <xref:System.Windows.Input.CommandManager> a acionar o evento <xref:System.Windows.Input.CommandManager.RequerySuggested>.  Isso é útil em condições que devem desabilitar/habilitar um comando, mas que não são condições reconhecidas pelo <xref:System.Windows.Input.CommandManager>.  
  
<a name="Command_Library"></a>
## <a name="command-library"></a>Biblioteca de comandos  
 O [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] oferece um conjunto de comandos predefinidos.  A biblioteca de comandos consiste nas seguintes classes: <xref:System.Windows.Input.ApplicationCommands>, <xref:System.Windows.Input.NavigationCommands>, <xref:System.Windows.Input.MediaCommands>, <xref:System.Windows.Documents.EditingCommands> e o <xref:System.Windows.Input.ComponentCommands>.  Essas classes fornecem comandos como <xref:System.Windows.Input.ApplicationCommands.Cut%2A>, <xref:System.Windows.Input.NavigationCommands.BrowseBack%2A> e <xref:System.Windows.Input.NavigationCommands.BrowseForward%2A>, <xref:System.Windows.Input.MediaCommands.Play%2A>, <xref:System.Windows.Input.MediaCommands.Stop%2A> e <xref:System.Windows.Input.MediaCommands.Pause%2A>.  
  
 Muitos desses comandos incluem um conjunto de associações de entrada padrão.  Por exemplo, se você especificar que seu aplicativo manipula o comando de cópia, você obtém automaticamente a ligação de teclado "CTRL + C". você também obtém associações para outros dispositivos de entrada, como gestos de caneta do Tablet PC e informações de fala.  
  
 Quando você faz referência a comandos nas várias bibliotecas de comandos usando [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)], geralmente é possível omitir o nome de classe da classe da biblioteca que expõe a propriedade de comando estático. De modo geral, os nomes dos comandos são cadeias de caracteres não ambíguas; os tipos de propriedade existem para fornecer um agrupamento lógico de comandos, mas não são necessários para eliminar a ambiguidade. Por exemplo, é possível especificar o `Command="Cut"` em vez do `Command="ApplicationCommands.Cut"`, mais detalhado. Esse é um mecanismo de conveniência interno ao [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] processador para comandos (mais precisamente, é um comportamento de conversor de tipo <xref:System.Windows.Input.ICommand> , que o [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] processador faz referência no tempo de carregamento).  
  
<a name="creating_commands"></a>
## <a name="creating-custom-commands"></a>Criando comandos personalizados  
 Se os comandos nas classes de biblioteca de comandos não satisfizerem suas necessidades, você poderá criar seus próprios comandos.  Há duas maneiras de criar um comando personalizado.  A primeira é começar do início e implementar a interface <xref:System.Windows.Input.ICommand>.  A outra forma, e a abordagem mais comum, é criar um <xref:System.Windows.Input.RoutedCommand> ou <xref:System.Windows.Input.RoutedUICommand>.  
  
 Para obter um exemplo de como criar um <xref:System.Windows.Input.RoutedCommand> personalizado, confira [Criar uma amostra de um RoutedCommand personalizado](https://github.com/Microsoft/WPF-Samples/tree/master/Input%20and%20Commands/CustomRoutedCommand).  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.Input.RoutedCommand>
- <xref:System.Windows.Input.CommandBinding>
- <xref:System.Windows.Input.InputBinding>
- <xref:System.Windows.Input.CommandManager>
- [Visão geral da entrada](input-overview.md)
- [Visão geral de eventos roteados](routed-events-overview.md)
- [Implementar ICommandSource](how-to-implement-icommandsource.md)
- [Como adicionar um comando a um MenuItem](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms741839(v=vs.90))
- [Criar uma amostra personalizada do RoutedCommand](https://github.com/Microsoft/WPF-Samples/tree/master/Input%20and%20Commands/CustomRoutedCommand)
