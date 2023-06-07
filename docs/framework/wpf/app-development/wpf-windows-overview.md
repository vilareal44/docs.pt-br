---
title: Visão geral do Windows
description: Familiarize-se com as noções básicas de criação e gerenciamento do Windows para aplicativos autônomos no Windows Presentation Foundation (WPF).
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- XAML [WPF], displaying content via
- XAML pages [WPF], displaying
- content [WPF], displaying via XAML
- window objects [WPF]
- hosting [WPF], applications
- managing windows [WPF]
- dialog boxes [WPF]
- Page object [WPF]
- NavigationWindow objects [WPF]
- applications [WPF], hosting
- content [WPF], displaying
- events [WPF]
- content [WPF], displaying via procedural code
- modal windows [WPF]
- procedural code [WPF], displaying content via
- displaying content via procedural code [WPF]
- window management [WPF]
- displaying content [WPF]
- window events [WPF]
- windows [WPF]
- modal dialog boxes [WPF]
- displaying XAML pages [WPF]
ms.assetid: 737d04ec-8861-46c3-8d44-fa11d3528d23
ms.openlocfilehash: e1ad3c118fbaea8f1e23d012721f0cf3c2c50015
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85622881"
---
# <a name="wpf-windows-overview"></a>Visão geral do WPF do Windows
Os usuários interagem com aplicativos autônomos do Windows Presentation Foundation (WPF) por meio do Windows. O objetivo principal de uma janela é hospedar conteúdo que visualiza dados e permite aos usuários interagir com os dados. Os aplicativos autônomos do WPF fornecem suas próprias janelas usando a <xref:System.Windows.Window> classe. Este tópico apresenta <xref:System.Windows.Window> antes de abranger os conceitos básicos da criação e gerenciamento de janelas em aplicativos autônomos.  
  
> [!NOTE]
> Aplicativos WPF hospedados em navegador, incluindo aplicativos de navegador XAML (XBAPs) e [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] páginas flexíveis, não fornecem suas próprias janelas. Em vez disso, eles são hospedados no Windows fornecido pelo Windows Internet Explorer. Consulte [visão geral de aplicativos de navegador XAML WPF](wpf-xaml-browser-applications-overview.md).  

<a name="TheWindowClass"></a>
## <a name="the-window-class"></a>A classe Window  
 A figura a seguir ilustra as partes constituintes de uma janela:  
  
 ![Captura de tela que mostra os elementos da janela.](./media/wpf-windows-overview/window-constituent-elements.png)  
  
 Uma janela é dividida em duas áreas: a área de não cliente e a área de cliente.  
  
 A *área de não-cliente* de uma janela é implementada pelo WPF e inclui as partes de uma janela que são comuns à maioria das janelas, incluindo as seguintes:  
  
- Uma borda.  
  
- Uma barra de título.  
  
- Um ícone.  
  
- Botões Minimizar, Maximizar e Restaurar.  
  
- Um botão Fechar.  
  
- Um menu Sistema com itens de menu que permitem aos usuários minimizar, maximizar, restaurar, mover, redimensionar e fechar uma janela.  
  
 A *área de cliente* de uma janela é a área dentro de uma área não-cliente da janela e é usada por desenvolvedores para adicionar conteúdo específico do aplicativo, como barras de menu, barras de ferramentas e controles.  
  
 No WPF, uma janela é encapsulada pela <xref:System.Windows.Window> classe que você usa para fazer o seguinte:  
  
- Exibir uma janela.  
  
- Configurar o tamanho, posição e aparência de uma janela.  
  
- Conteúdo específico do aplicativo host.  
  
- Gerenciar o tempo de vida de uma janela.  
  
<a name="DefiningAWindow"></a>
## <a name="implementing-a-window"></a>Implementar uma janela  
 A implementação de uma janela típica compreende a aparência e o comportamento, em que a *aparência* define como uma janela se parece com os usuários e o *comportamento* define a maneira como uma janela funciona quando os usuários interagem com ela. No WPF, você pode implementar a aparência e o comportamento de uma janela usando o código ou a [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] marcação.  
  
 Em geral, no entanto, a aparência de uma janela é implementada usando [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] marcação, e seu comportamento é implementado usando code-behind, conforme mostrado no exemplo a seguir.  
  
 [!code-xaml[WindowsOverviewSnippets#MarkupAndCodeBehindWindowMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/MarkupAndCodeBehindWindow.xaml#markupandcodebehindwindowmarkup)]  
  
 [!code-csharp[WindowsOverviewSnippets#MarkupAndCodeBehindWindowCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/MarkupAndCodeBehindWindow.xaml.cs#markupandcodebehindwindowcodebehind)]
 [!code-vb[WindowsOverviewSnippets#MarkupAndCodeBehindWindowCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WindowsOverviewSnippets/VisualBasic/MarkupAndCodeBehindWindow.xaml.vb#markupandcodebehindwindowcodebehind)]  
  
 Para permitir que um [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] arquivo de marcação e um arquivo code-behind funcionem juntos, são necessários os seguintes:  
  
- Na marcação, o `Window` elemento deve incluir o `x:Class` atributo. Quando o aplicativo é compilado, a existência do `x:Class` no arquivo de marcação faz com que o Microsoft Build Engine (MSBuild) crie uma `partial` classe derivada de <xref:System.Windows.Window> e que tenha o nome especificado pelo `x:Class` atributo. Isso requer a adição de uma declaração de namespace XML para o [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] esquema ( `xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"` ). A `partial` classe gerada implementa o `InitializeComponent` método, que é chamado para registrar os eventos e definir as propriedades que são implementadas na marcação.  
  
- No code-behind, a classe deve ser uma `partial` classe com o mesmo nome que é especificado pelo `x:Class` atributo na marcação e deve derivar de <xref:System.Windows.Window> . Isso permite que o arquivo code-behind seja associado à `partial` classe gerada para o arquivo de marcação quando o aplicativo é compilado (consulte [criando um aplicativo WPF](building-a-wpf-application-wpf.md)).  
  
- No code-behind, a <xref:System.Windows.Window> classe deve implementar um construtor que chama o `InitializeComponent` método. `InitializeComponent`é implementado pela classe gerada do arquivo de marcação `partial` para registrar eventos e definir propriedades que são definidas na marcação.  
  
> [!NOTE]
> Quando você adiciona um novo <xref:System.Windows.Window> ao seu projeto usando o Visual Studio, o <xref:System.Windows.Window> é implementado usando marcação e code-behind e inclui a configuração necessária para criar a associação entre os arquivos de marcação e code-behind, conforme descrito aqui.  
  
 Com essa configuração em vigor, você pode se concentrar na definição da aparência da janela na [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] marcação e na implementação de seu comportamento no code-behind. O exemplo a seguir mostra uma janela com um botão, implementado em [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] marcação e um manipulador de eventos para o evento do botão <xref:System.Windows.Controls.Primitives.ButtonBase.Click> , implementado no code-behind.  
  
 [!code-xaml[WindowsOverviewWindowWithButtonSnippets#MarkupAndCodeBehindWindowMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewWindowWithButtonSnippets/CSharp/MarkupAndCodeBehindWindow.xaml#markupandcodebehindwindowmarkup)]  
  
 [!code-csharp[WindowsOverviewWindowWithButtonSnippets#MarkupAndCodeBehindWindowCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewWindowWithButtonSnippets/CSharp/MarkupAndCodeBehindWindow.xaml.cs#markupandcodebehindwindowcodebehind)]
 [!code-vb[WindowsOverviewWindowWithButtonSnippets#MarkupAndCodeBehindWindowCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WindowsOverviewWindowWithButtonSnippets/VisualBasic/MarkupAndCodeBehindWindow.xaml.vb#markupandcodebehindwindowcodebehind)]  
  
<a name="ConfiguringWindowForMSBuild"></a>
## <a name="configuring-a-window-definition-for-msbuild"></a>Configurar uma definição de janela para MSBuild  
 A forma como você implementa sua janela determina como ela é configurada para o MSBuild. Para uma janela que é definida usando [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] marcação e code-behind:  
  
- Os arquivos de marcação XAML são configurados como itens do MSBuild `Page` .  
  
- Os arquivos code-behind são configurados como itens do MSBuild `Compile` .  
  
 Isso é mostrado no seguinte arquivo de projeto do MSBuild.  
  
```xml  
<Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003" ... >  
    ...  
    <Page Include="MarkupAndCodeBehindWindow.xaml" />  
    <Compile Include=" MarkupAndCodeBehindWindow.xaml.cs" />  
    ...  
</Project>  
```  
  
 Para obter informações sobre a criação de aplicativos do WPF, consulte [criando um aplicativo WPF](building-a-wpf-application-wpf.md).  
  
<a name="WindowLifetime"></a>
## <a name="window-lifetime"></a>Tempo de vida de janela  
 Assim como com qualquer classe, uma janela tem um tempo de vida que começa quando ela é instanciada pela primeira vez, fato após o qual ela é aberta, ativada e desativada e, eventualmente, fechada.  

<a name="Opening_a_Window"></a>
### <a name="opening-a-window"></a>Abrir uma janela  
 Para abrir uma janela, você primeiro cria uma instância dela, o que é demonstrado no exemplo a seguir.  
  
 [!code-xaml[WindowsOverviewStartupEventSnippets#AppMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewStartupEventSnippets/CSharp/App.xaml#appmarkup)]  
  
 [!code-csharp[WindowsOverviewStartupEventSnippets#AppCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewStartupEventSnippets/CSharp/App.xaml.cs#appcodebehind)]  
  
 Neste exemplo, o `MarkupAndCodeBehindWindow` é instanciado quando o aplicativo é iniciado, o que ocorre quando o <xref:System.Windows.Application.Startup> evento é gerado.  
  
 Quando uma janela é instanciada, uma referência a ela é adicionada automaticamente a uma lista de janelas gerenciadas pelo <xref:System.Windows.Application> objeto (consulte <xref:System.Windows.Application.Windows%2A?displayProperty=nameWithType> ). Além disso, a primeira janela a ser instanciada é, por padrão, definida <xref:System.Windows.Application> como a janela principal do aplicativo (consulte <xref:System.Windows.Application.MainWindow%2A?displayProperty=nameWithType> ).  
  
 A janela é finalmente aberta chamando o <xref:System.Windows.Window.Show%2A> método; o resultado é mostrado na figura a seguir.  
  
 ![Uma janela aberta chamando janela. show](./media/wpf-windows-overview//window-opened-show-method.png)  
  
 Uma janela que é aberta chamando <xref:System.Windows.Window.Show%2A> é uma janela sem-modo, o que significa que o aplicativo opera em um modo que permite que os usuários ativem outras janelas no mesmo aplicativo.  
  
> [!NOTE]
> <xref:System.Windows.Window.ShowDialog%2A>é chamado para abrir janelas como caixas de diálogo modalmente. Consulte [visão geral das caixas de diálogo](dialog-boxes-overview.md) para obter mais informações.  
  
 Quando <xref:System.Windows.Window.Show%2A> é chamado, uma janela executa o trabalho de inicialização antes de ser mostrada para estabelecer a infraestrutura que permite receber entrada do usuário. Quando a janela é inicializada, o <xref:System.Windows.Window.SourceInitialized> evento é gerado e a janela é mostrada.  
  
 Como um atalho, <xref:System.Windows.Application.StartupUri%2A> pode ser definido para especificar a primeira janela que é aberta automaticamente quando um aplicativo é iniciado.  
  
 [!code-xaml[WindowsOverviewSnippets#ApplicationStartupUriMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/App.xaml#applicationstartupurimarkup)]  
  
 Quando o aplicativo é iniciado, a janela especificada pelo valor de <xref:System.Windows.Application.StartupUri%2A> é aberta modelessly; internamente, a janela é aberta chamando seu <xref:System.Windows.Window.Show%2A> método.  
  
<a name="Ownership"></a>
#### <a name="window-ownership"></a>Posse de janela  
 Uma janela que é aberta usando o <xref:System.Windows.Window.Show%2A> método não tem uma relação implícita com a janela que a criou; os usuários podem interagir com qualquer uma das janelas independentemente da outra, o que significa que qualquer uma das janelas pode fazer o seguinte:  
  
- Cubra o outro (a menos que uma das janelas tenha sua <xref:System.Windows.Window.Topmost%2A> propriedade definida como `true` ).  
  
- Ser minimizada, maximizada e restaurada sem afetar a outra.  
  
 Algumas janelas requerem uma relação com a janela que as abre. Por exemplo, um aplicativo IDE (ambiente de desenvolvimento integrado) pode abrir janelas de propriedades e janelas de ferramentas cujo comportamento típico é abordar a janela que as cria. Além disso, janelas desse tipo devem sempre fechar, minimizar, maximizar e restaurar em conjunto com a janela que as criou. Tal relação pode ser estabelecida fazendo uma janela *própria* e é obtida definindo a <xref:System.Windows.Window.Owner%2A> propriedade da *janela de propriedade* com uma referência à *janela do proprietário*. Isso é mostrado no exemplo a seguir.  
  
 [!code-csharp[WindowOwnerOwnedWindowsSnippets#SetWindowOwnerCODE](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowOwnerOwnedWindowsSnippets/CSharp/MainWindow.xaml.cs#setwindowownercode)]
 [!code-vb[WindowOwnerOwnedWindowsSnippets#SetWindowOwnerCODE](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WindowOwnerOwnedWindowsSnippets/visualbasic/mainwindow.xaml.vb#setwindowownercode)]  
  
 Depois que a posse é estabelecida:  
  
- A janela de propriedade pode referenciar sua janela do proprietário inspecionando o valor de sua <xref:System.Windows.Window.Owner%2A> propriedade.  
  
- A janela do proprietário pode descobrir todas as janelas que ela possui inspecionando o valor de sua <xref:System.Windows.Window.OwnedWindows%2A> propriedade.  
  
<a name="Preventing"></a>
#### <a name="preventing-window-activation"></a>Impedindo a ativação de janela  
 Há cenários em que o Windows não deve ser ativado quando mostrado, como janelas de conversa de um aplicativo no estilo Internet Messenger ou janelas de notificação de um aplicativo de email.  
  
 Se seu aplicativo tiver uma janela que não deve ser ativada quando for mostrado, você poderá definir sua <xref:System.Windows.Window.ShowActivated%2A> propriedade como `false` antes de chamar o <xref:System.Windows.Window.Show%2A> método pela primeira vez. Como consequência disso:  
  
- A janela não é ativada.  
  
- O evento da janela <xref:System.Windows.Window.Activated> não é gerado.  
  
- A janela ativada no momento permanece ativada.  
  
 A janela se tornará ativada, no entanto, assim que o usuário ativá-la clicando na área de cliente ou então na área não cliente. Nesse caso:  
  
- A janela é ativada.  
  
- O evento da janela <xref:System.Windows.Window.Activated> é gerado.  
  
- A janela ativada anteriormente é desativada.  
  
- <xref:System.Windows.Window.Deactivated> <xref:System.Windows.Window.Activated> Em seguida, os eventos da janela são gerados conforme o esperado em resposta às ações do usuário.  
  
<a name="Window_Activation"></a>
### <a name="window-activation"></a>Ativação de janela  
 Quando uma janela é aberta pela primeira vez, ela se torna a janela ativa (a menos que ela seja mostrada com <xref:System.Windows.Window.ShowActivated%2A> definido como `false` ). A *janela ativa* é a janela que está atualmente capturando a entrada do usuário, como traços de tecla e cliques do mouse. Quando uma janela se torna ativa, ela gera o <xref:System.Windows.Window.Activated> evento.  
  
> [!NOTE]
> Quando uma janela é aberta pela primeira vez, os <xref:System.Windows.FrameworkElement.Loaded> <xref:System.Windows.Window.ContentRendered> eventos e são gerados somente depois que o <xref:System.Windows.Window.Activated> evento é gerado. Com isso em mente, uma janela pode ser efetivamente considerada aberta quando <xref:System.Windows.Window.ContentRendered> é gerada.  
  
 Depois que uma janela fica ativa, um usuário pode ativar outra janela no mesmo aplicativo ou então ativar outro aplicativo. Quando isso acontece, a janela ativa no momento torna-se desativada e gera o <xref:System.Windows.Window.Deactivated> evento. Da mesma forma, quando o usuário seleciona uma janela desativada no momento, a janela torna-se ativa novamente e <xref:System.Windows.Window.Activated> é gerada.  
  
 Um motivo comum para manipular <xref:System.Windows.Window.Activated> e <xref:System.Windows.Window.Deactivated> é habilitar e desabilitar a funcionalidade que só pode ser executada quando uma janela está ativa. Por exemplo, algumas janelas exibem conteúdo interativo que requer constante atenção ou entrada do usuário, incluindo players de vídeo e jogos. O exemplo a seguir é um player de vídeo simplificado que demonstra como tratar <xref:System.Windows.Window.Activated> e <xref:System.Windows.Window.Deactivated> implementar esse comportamento.  
  
 [!code-xaml[WindowsOverviewSnippets#ActivationDeactivationMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/CustomMediaPlayerWindow.xaml#activationdeactivationmarkup)]  
  
 [!code-csharp[WindowsOverviewSnippets#ActivationDeactivationCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/CustomMediaPlayerWindow.xaml.cs#activationdeactivationcodebehind)]
 [!code-vb[WindowsOverviewSnippets#ActivationDeactivationCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WindowsOverviewSnippets/VisualBasic/CustomMediaPlayerWindow.xaml.vb#activationdeactivationcodebehind)]  
  
 Outros tipos de aplicativos podem ainda executar códigos em segundo plano quando uma janela é desativada. Por exemplo, um cliente de email pode continuar a sondagem do servidor de email enquanto o usuário está usando outros aplicativos. Aplicativos como esses geralmente apresentam comportamento diferente ou adicional enquanto a janela principal está desativada. Em relação ao programa de email, isso pode significar tanto adicionar o novo item de email à caixa de entrada quanto adicionar um ícone de notificação à bandeja do sistema. Um ícone de notificação só precisa ser exibido quando a janela de email não está ativa, o que pode ser determinado inspecionando a <xref:System.Windows.Window.IsActive%2A> propriedade.  
  
 Se uma tarefa em segundo plano for concluída, uma janela pode querer notificar o usuário de forma mais urgente chamando o <xref:System.Windows.Window.Activate%2A> método. Se o usuário estiver interagindo com outro aplicativo ativado quando <xref:System.Windows.Window.Activate%2A> for chamado, o botão da barra de tarefas da janela piscará. Se um usuário estiver interagindo com o aplicativo atual, a chamada levará <xref:System.Windows.Window.Activate%2A> a janela para o primeiro plano.  
  
> [!NOTE]
> Você pode manipular a ativação de escopo de aplicativo usando os <xref:System.Windows.Application.Activated?displayProperty=nameWithType> <xref:System.Windows.Application.Deactivated?displayProperty=nameWithType> eventos e.  
  
<a name="Closing_a_Window"></a>
### <a name="closing-a-window"></a>Fechar uma janela  
 O fim da vida de uma janela se começa a se aproximar quando um usuário a fecha. Uma janela pode ser fechada pelo uso de elementos na área de não cliente, incluindo o seguinte:  
  
- O item **fechar** do menu do **sistema** .  
  
- Pressionar ALT + F4.  
  
- Pressionando o botão **fechar** .  
  
 Você pode fornecer mecanismos adicionais à área de cliente para fechar uma janela, sendo que os mais comuns deles incluem os seguintes:  
  
- Um item de **saída** no menu **arquivo** , normalmente para janelas de aplicativo principal.  
  
- Um item de **fechamento** no menu **arquivo** , normalmente em uma janela de aplicativo secundário.  
  
- Um botão de **cancelamento** , normalmente em uma caixa de diálogo modal.  
  
- Um botão **fechar** , normalmente em uma caixa de diálogo sem janela restrita.  
  
 Para fechar uma janela em resposta a um desses mecanismos personalizados, você precisa chamar o <xref:System.Windows.Window.Close%2A> método. O exemplo a seguir implementa a capacidade de fechar uma janela escolhendo **sair** no menu **arquivo** .  
  
 [!code-xaml[WindowsOverviewSnippets#WindowWithFileExitMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/WindowWithFileExit.xaml#windowwithfileexitmarkup)]  
  
 [!code-csharp[WindowsOverviewSnippets#WindowWithFileExitCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/WindowWithFileExit.xaml.cs#windowwithfileexitcodebehind)]
 [!code-vb[WindowsOverviewSnippets#WindowWithFileExitCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WindowsOverviewSnippets/VisualBasic/WindowWithFileExit.xaml.vb#windowwithfileexitcodebehind)]  
  
 Quando uma janela é fechada, ela gera dois eventos: <xref:System.Windows.Window.Closing> e <xref:System.Windows.Window.Closed> .  
  
 <xref:System.Windows.Window.Closing>é gerado antes da janela ser fechada e fornece um mecanismo pelo qual o fechamento da janela pode ser impedido. Uma razão comum para evitar o fechamento da janela é caso o conteúdo da janela contenha dados modificados. Nessa situação, o <xref:System.Windows.Window.Closing> evento pode ser tratado para determinar se os dados estão sujos e, em caso afirmativo, perguntar ao usuário se deseja continuar fechando a janela sem salvar os dados ou cancelar o fechamento da janela. O exemplo a seguir mostra os principais aspectos de manipulação <xref:System.Windows.Window.Closing> .  
  
 [!code-csharp[WindowClosingSnippets](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowClosingSnippets/CSharp/DataWindow.xaml.cs)]
 [!code-vb[WindowClosingSnippets](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WindowClosingSnippets/visualbasic/datawindow.xaml.vb)]  

 O <xref:System.Windows.Window.Closing> manipulador de eventos é passado a <xref:System.ComponentModel.CancelEventArgs> , que implementa a `Boolean` <xref:System.ComponentModel.CancelEventArgs.Cancel%2A> propriedade que você definiu como `true` para impedir que uma janela seja fechada.  
  
 Se <xref:System.Windows.Window.Closing> não for tratado ou for manipulado, mas não cancelado, a janela será fechada. Logo antes de uma janela ser fechada, <xref:System.Windows.Window.Closed> é gerado. Neste ponto, uma janela não pode ser impedida de fechar.  
  
> [!NOTE]
> Um aplicativo pode ser configurado para ser desligado automaticamente quando a janela principal do aplicativo é fechada (consulte <xref:System.Windows.Application.MainWindow%2A> ) ou a última janela é fechada. Para obter detalhes, consulte <xref:System.Windows.Application.ShutdownMode%2A>.  
  
 Embora uma janela possa ser fechada explicitamente por meio de mecanismos fornecidos nas áreas não cliente e cliente, uma janela também pode ser fechada implicitamente como resultado de um comportamento em outras partes do aplicativo ou do Windows, incluindo o seguinte:  
  
- Um usuário faz logoff ou desliga o Windows.  
  
- O proprietário de uma janela fecha (consulte <xref:System.Windows.Window.Owner%2A> ).  
  
- A janela principal do aplicativo está fechada e <xref:System.Windows.Application.ShutdownMode%2A> é <xref:System.Windows.ShutdownMode.OnMainWindowClose> .  
  
- <xref:System.Windows.Application.Shutdown%2A> é chamado.  
  
> [!NOTE]
> Uma janela não pode ser reaberta depois que é fechada.  
  
<a name="Window_Lifetime_Events"></a>
### <a name="window-lifetime-events"></a>Eventos de tempo de vida da janela  
 A ilustração a seguir mostra a sequência dos eventos principais no tempo de vida de uma janela:  
  
 ![Diagrama que mostra eventos no tempo de vida de uma janela.](./media/wpf-windows-overview/window-lifetime-events.png)  
  
 A ilustração a seguir mostra a sequência dos eventos principais no tempo de vida de uma janela que é mostrada sem ativação ( <xref:System.Windows.Window.ShowActivated%2A> é definida como `false` antes de a janela ser mostrada):  
  
 ![Diagrama que mostra eventos no tempo de vida de uma janela sem ativação.](./media/wpf-windows-overview/window-lifetime-no-activation.png)  
  
<a name="WindowLocation"></a>
## <a name="window-location"></a>Localização da janela  
 Enquanto uma janela estiver aberta, ela terá uma localização nas dimensões x e y em relação à área de trabalho. Esse local pode ser determinado inspecionando as <xref:System.Windows.Window.Left%2A> Propriedades e <xref:System.Windows.Window.Top%2A> , respectivamente. Você pode definir essas propriedades para alterar o local da janela.  
  
 Você também pode especificar o local inicial de um <xref:System.Windows.Window> quando ele é exibido pela primeira vez, definindo a <xref:System.Windows.Window.WindowStartupLocation%2A> propriedade com um dos seguintes <xref:System.Windows.WindowStartupLocation> valores de enumeração:  
  
- <xref:System.Windows.WindowStartupLocation.CenterOwner> (padrão)  
  
- <xref:System.Windows.WindowStartupLocation.CenterScreen>  
  
- <xref:System.Windows.WindowStartupLocation.Manual>  
  
 Se o local de inicialização for especificado como <xref:System.Windows.WindowStartupLocation.Manual> , e <xref:System.Windows.Window.Left%2A> as <xref:System.Windows.Window.Top%2A> Propriedades e não tiverem sido definidas, o <xref:System.Windows.Window> Windows pedirá que um local apareça.  
  
<a name="Topmost_Windows_and_Z_Order"></a>
### <a name="topmost-windows-and-z-order"></a>Janela superior e ordem Z  
 Além de ter uma localização x e y, uma janela também tem uma localização na dimensão z, que determina a posição vertical em relação a outras janelas. Isso é conhecido como a ordem z da janela, e há dois tipos: ordem z normal e ordem z superior. O local de uma janela na *ordem z normal* é determinado pelo fato de estar atualmente ativo ou não. Por padrão, uma janela está localizada na ordem z normal. O local de uma janela na *ordem z superior* também é determinado pelo fato de estar atualmente ativo ou não. Além disso, janelas na ordem z superior sempre estão localizadas acima das janelas na ordem z normal. Uma janela está localizada na ordem z superior definindo sua <xref:System.Windows.Window.Topmost%2A> propriedade como `true` .  
  
 [!code-xaml[WindowsOverviewSnippets#TopmostWindowMARKUP1](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/TopmostWindow.xaml#topmostwindowmarkup1)]  
  
 Dentro de cada ordem z, a janela ativa no momento aparece acima de todas as outras janelas na mesma ordem z.  
  
<a name="WindowSize"></a>
## <a name="window-size"></a>Tamanho da janela  
 Além de ter um local de área de trabalho, uma janela tem um tamanho que é determinado por várias propriedades, incluindo as várias propriedades width e Height e <xref:System.Windows.Window.SizeToContent%2A> .  
  
 <xref:System.Windows.FrameworkElement.MinWidth%2A>, <xref:System.Windows.FrameworkElement.Width%2A> e <xref:System.Windows.FrameworkElement.MaxWidth%2A> são usados para gerenciar o intervalo de larguras que uma janela pode ter durante seu tempo de vida e são configurados conforme mostrado no exemplo a seguir.  
  
 [!code-xaml[WindowsOverviewSnippets#WidthWindowMARKUP1](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/WidthWindow.xaml#widthwindowmarkup1)]  
  
 A altura da janela é gerenciada pelo, e e é <xref:System.Windows.FrameworkElement.MinHeight%2A> <xref:System.Windows.FrameworkElement.Height%2A> <xref:System.Windows.FrameworkElement.MaxHeight%2A> configurada conforme mostrado no exemplo a seguir.  
  
 [!code-xaml[WindowsOverviewSnippets#HeightWindowMARKUP1](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/HeightWindow.xaml#heightwindowmarkup1)]  
  
 Já que os diversos valores de largura e de altura especificam um intervalo, é possível que a largura e altura de uma janela redimensionável estejam em qualquer lugar dentro do intervalo especificado para a respectiva dimensão. Para detectar sua largura e altura atuais, inspecione <xref:System.Windows.FrameworkElement.ActualWidth%2A> e <xref:System.Windows.FrameworkElement.ActualHeight%2A> , respectivamente.  
  
 Se você quiser que a largura e a altura da janela tenham um tamanho que se ajuste ao tamanho do conteúdo da janela, você poderá usar a <xref:System.Windows.Window.SizeToContent%2A> propriedade, que tem os seguintes valores:  
  
- <xref:System.Windows.SizeToContent.Manual>. Sem efeito (padrão).  
  
- <xref:System.Windows.SizeToContent.Width>. Ajustar à largura do conteúdo, que tem o mesmo efeito que definir <xref:System.Windows.FrameworkElement.MinWidth%2A> e <xref:System.Windows.FrameworkElement.MaxWidth%2A> com a largura do conteúdo.  
  
- <xref:System.Windows.SizeToContent.Height>. Ajustar à altura do conteúdo, que tem o mesmo efeito que definir <xref:System.Windows.FrameworkElement.MinHeight%2A> e <xref:System.Windows.FrameworkElement.MaxHeight%2A> com a altura do conteúdo.  
  
- <xref:System.Windows.SizeToContent.WidthAndHeight>. Ajustar à largura e à altura do conteúdo, que tem o mesmo efeito que definir <xref:System.Windows.FrameworkElement.MinHeight%2A> e <xref:System.Windows.FrameworkElement.MaxHeight%2A> com a altura do conteúdo, e definir <xref:System.Windows.FrameworkElement.MinWidth%2A> e <xref:System.Windows.FrameworkElement.MaxWidth%2A> com a largura do conteúdo.  
  
 O exemplo a seguir mostra uma janela que se dimensiona automaticamente para ajustar-se ao próprio conteúdo, verticalmente e horizontalmente, quando mostrada pela primeira vez.  
  
 [!code-xaml[WindowsOverviewSnippets#SizeToContentWindowMARKUP1](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/SizeToContentWindow.xaml#sizetocontentwindowmarkup1)]  
  
 O exemplo a seguir mostra como definir a <xref:System.Windows.Window.SizeToContent%2A> propriedade no código para especificar como uma janela é redimensionada para se ajustar ao seu conteúdo.
  
 [!code-csharp[HOWTOWindowManagementSnippets#SetWindowSizeToContentPropertyCODE](~/samples/snippets/csharp/VS_Snippets_Wpf/HOWTOWindowManagementSnippets/CSharp/MainWindow.xaml.cs#setwindowsizetocontentpropertycode)]
 [!code-vb[HOWTOWindowManagementSnippets#SetWindowSizeToContentPropertyCODE](~/samples/snippets/visualbasic/VS_Snippets_Wpf/HOWTOWindowManagementSnippets/visualbasic/mainwindow.xaml.vb#setwindowsizetocontentpropertycode)]  
  
<a name="OrderOfPrecedence"></a>
## <a name="order-of-precedence-for-sizing-properties"></a>Ordem de precedência para propriedades de dimensionamento  
 Essencialmente, as diversas propriedades de tamanhos de uma janela são combinadas para definir o intervalo de largura e altura de uma janela redimensionável. Para garantir que um intervalo válido seja mantido, <xref:System.Windows.Window> o avalia os valores das propriedades de tamanho usando as seguintes ordens de precedência.  
  
 **Para propriedades de altura:**  
  
1. <xref:System.Windows.FrameworkElement.MinHeight%2A?displayProperty=nameWithType>
  
2. <xref:System.Windows.FrameworkElement.MaxHeight%2A?displayProperty=nameWithType>
  
3. <xref:System.Windows.SizeToContent.Height?displayProperty=nameWithType>/<xref:System.Windows.SizeToContent.WidthAndHeight?displayProperty=nameWithType>
  
4. <xref:System.Windows.FrameworkElement.Height%2A?displayProperty=nameWithType>  
  
 **Para propriedades de largura:**  
  
1. <xref:System.Windows.FrameworkElement.MinWidth%2A?displayProperty=nameWithType>
  
2. <xref:System.Windows.FrameworkElement.MaxWidth%2A?displayProperty=nameWithType>
  
3. <xref:System.Windows.SizeToContent.Width?displayProperty=nameWithType>/<xref:System.Windows.SizeToContent.WidthAndHeight?displayProperty=nameWithType>
  
4. <xref:System.Windows.FrameworkElement.Width%2A?displayProperty=nameWithType>  
  
 A ordem de precedência também pode determinar o tamanho de uma janela quando ela é maximizada, que é gerenciada com a <xref:System.Windows.Window.WindowState%2A> propriedade.  
  
<a name="WindowState"></a>
## <a name="window-state"></a>Estado da janela  
 Durante o tempo de vida de uma janela redimensionável, ela pode ter três estados: normal, minimizada e maximizada. Uma janela com um estado *normal* é o estado padrão de uma janela. Uma janela com esse estado permitirá que um usuário a mova e a redimensione usando uma alça de redimensionamento ou a borda, se ela for redimensionável.  
  
 Uma janela com um estado *minimizado* será recolhida ao botão da barra de tarefas se <xref:System.Windows.Window.ShowInTaskbar%2A> for definido como `true` ; caso contrário, ele será recolhido para o menor tamanho possível que pode ser e realocado para o canto inferior esquerdo da área de trabalho. Nenhum tipo de janela minimizada pode ser redimensionado usando uma borda ou alça redimensionável, embora uma janela minimizada não mostrada na barra de tarefas possa ser arrastada pela área de trabalho.  
  
 Uma janela com um estado *maximizado* se expande para o tamanho máximo que pode ser, que será tão grande quanto suas <xref:System.Windows.FrameworkElement.MaxWidth%2A> Propriedades, <xref:System.Windows.FrameworkElement.MaxHeight%2A> e <xref:System.Windows.Window.SizeToContent%2A> ditadas. Assim como ocorre com uma janela minimizada, uma janela maximizada não pode ser redimensionada usando uma alça de redimensionamento ou arrastando a borda.  
  
> [!NOTE]
> Os valores das <xref:System.Windows.Window.Top%2A> Propriedades, <xref:System.Windows.Window.Left%2A> , e <xref:System.Windows.FrameworkElement.Width%2A> <xref:System.Windows.FrameworkElement.Height%2A> de uma janela sempre representam os valores para o estado normal, mesmo quando a janela está atualmente maximizada ou minimizada.  
  
 O estado de uma janela pode ser configurado definindo sua <xref:System.Windows.Window.WindowState%2A> propriedade, que pode ter um dos seguintes valores de <xref:System.Windows.WindowState> enumeração:  
  
- <xref:System.Windows.WindowState.Normal> (padrão)  
  
- <xref:System.Windows.WindowState.Maximized>  
  
- <xref:System.Windows.WindowState.Minimized>  
  
 O exemplo a seguir mostra como criar uma janela que é mostrada como maximizada quando é aberta.  
  
 [!code-xaml[WindowsOverviewSnippets#WindowStateWindowMARKUP1](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/WindowStateWindow.xaml#windowstatewindowmarkup1)]  
  
 Em geral, você deve definir <xref:System.Windows.Window.WindowState%2A> para configurar o estado inicial de uma janela. Assim que uma janela redimensionável é exibida, os usuários podem pressionar os botões minimizar, maximizar e restaurar na barra de título da janela para alterar o estado desta.  
  
<a name="WindowAppearance"></a>
## <a name="window-appearance"></a>Aparência da janela  
 Você pode alterar a aparência da área de cliente de uma janela adicionando conteúdo específico de janela a ela, por exemplo, botões, rótulos e caixas de texto. Para configurar a área não cliente, o <xref:System.Windows.Window> fornece várias propriedades, que incluem <xref:System.Windows.Window.Icon%2A> para definir o ícone de uma janela e <xref:System.Windows.Window.Title%2A> definir seu título.  
  
 Você também pode alterar a aparência e o comportamento da borda da área de não cliente configurando o modo de redimensionamento da janela, o estilo da janela e definindo se essa janela aparece ou não como um botão na barra de tarefas da área de trabalho.  

<a name="Resize_Mode"></a>
### <a name="resize-mode"></a>Modo de redimensionamento  
 Dependendo da <xref:System.Windows.Window.WindowStyle%2A> propriedade, você pode controlar como (e se) os usuários podem redimensionar a janela. A escolha do estilo de janela afeta se um usuário pode redimensionar a janela arrastando sua borda com o mouse, se os botões **minimizar**, **maximizar**e **redimensionar** aparecem na área não cliente e, se forem exibidos, se estiverem habilitados.  
  
 Você pode configurar como uma janela é redimensionada definindo sua <xref:System.Windows.Window.ResizeMode%2A> propriedade, que pode ser um dos seguintes <xref:System.Windows.ResizeMode> valores de enumeração:  
  
- <xref:System.Windows.ResizeMode.NoResize>  
  
- <xref:System.Windows.ResizeMode.CanMinimize>  
  
- <xref:System.Windows.ResizeMode.CanResize> (padrão)  
  
- <xref:System.Windows.ResizeMode.CanResizeWithGrip>  
  
 Assim como acontece com <xref:System.Windows.Window.WindowStyle%2A> o, o modo de redimensionamento de uma janela é improvável de ser alterado durante seu tempo de vida, o que significa que você provavelmente o definirá da [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] marcação.  
  
 [!code-xaml[WindowsOverviewSnippets#ResizeModeWindowMARKUP1](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/ResizeModeWindow.xaml#resizemodewindowmarkup1)]  
  
 Observe que você pode detectar se uma janela é maximizada, minimizada ou restaurada inspecionando a <xref:System.Windows.Window.WindowState%2A> propriedade.  
  
<a name="Window_Style"></a>
### <a name="window-style"></a>Estilo de Janela  
 A borda que é exposta da área de não cliente de uma janela é adequada para a maioria dos aplicativos. No entanto, existem circunstâncias em que diferentes tipos de bordas são necessários ou em que nenhuma borda é necessária, dependendo do tipo de janela.  
  
 Para controlar o tipo de borda que uma janela Obtém, defina sua <xref:System.Windows.Window.WindowStyle%2A> propriedade com um dos seguintes valores da <xref:System.Windows.WindowStyle> enumeração:  
  
- <xref:System.Windows.WindowStyle.None>  
  
- <xref:System.Windows.WindowStyle.SingleBorderWindow> (padrão)  
  
- <xref:System.Windows.WindowStyle.ThreeDBorderWindow>  
  
- <xref:System.Windows.WindowStyle.ToolWindow>  
  
 O efeito desses estilos de janela é ilustrado na figura a seguir:  
  
 ![Ilustração de estilos de borda de janela.](./media/wpf-windows-overview/window-border-styles.png)  
  
 Você pode definir <xref:System.Windows.Window.WindowStyle%2A> usando a [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] marcação ou o código; como é improvável que seja alterado durante o tempo de vida de uma janela, você provavelmente a configurará usando [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] marcação.  
  
 [!code-xaml[WindowsOverviewSnippets#WindowStyleWindowMARKUP1](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/WindowStyleWindow.xaml#windowstylewindowmarkup1)]  
  
#### <a name="non-rectangular-window-style"></a>Estilo de janela não retangular  
 Há também situações em que os estilos de borda que <xref:System.Windows.Window.WindowStyle%2A> permitem que você tenha não sejam suficientes. Por exemplo, você pode querer criar um aplicativo com uma borda não retangular, como o Microsoft Windows Media Player usa.  
  
 Por exemplo, considere a janela de bolha de fala mostrada na figura a seguir:  
  
 ![Uma janela de bolhas de fala que diz arrastar para mim.](./media/wpf-windows-overview/non-rectangular-window-figure.png)  
  
 Esse tipo de janela pode ser criado definindo a <xref:System.Windows.Window.WindowStyle%2A> propriedade como <xref:System.Windows.WindowStyle.None> e usando o suporte especial que <xref:System.Windows.Window> tem para transparência.  
  
 [!code-xaml[WindowsOverviewSnippets#TransparentWindowMARKUP1](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/TransparentWindow.xaml#transparentwindowmarkup1)]  
  
 Essa combinação de valores instrui a janela a ser renderizada completamente transparente. Nesse estado, os adornos da área de não cliente da janela (o menu Fechar, botões Minimizar, Maximizar e Restaurar e assim por diante) não podem ser usados. Consequentemente, você precisa fornecer os seus próprios.  
  
<a name="Task_Bar_Presence"></a>
### <a name="task-bar-presence"></a>Presença da barra de tarefas  

A aparência padrão de uma janela inclui um botão da barra de tarefas, como aquele mostrado na figura a seguir:

 ![Captura de tela que mostra uma janela com um botão da barra de tarefas.](./media/wpf-windows-overview/window-taskbar-button.png)  
  
 Alguns tipos de janelas não têm um botão da barra de tarefas, como caixas de mensagens e caixas de diálogo (consulte [visão geral das caixas de diálogo](dialog-boxes-overview.md)). Você pode controlar se o botão da barra de tarefas de uma janela é mostrado definindo a <xref:System.Windows.Window.ShowInTaskbar%2A> Propriedade ( `true` por padrão).  
  
 [!code-xaml[WindowsOverviewSnippets#ShowInTaskbarWindowMARKUP1](~/samples/snippets/csharp/VS_Snippets_Wpf/WindowsOverviewSnippets/CSharp/ShowInTaskbarWindow.xaml#showintaskbarwindowmarkup1)]  
  
<a name="SecurityConsiderations"></a>
## <a name="security-considerations"></a>Considerações de segurança  
 <xref:System.Windows.Window>requer `UnmanagedCode` que a permissão de segurança seja instanciada. Para aplicativos instalados e iniciados no computador local, isso se encaixa no conjunto de permissões concedidas ao aplicativo.  
  
 No entanto, isso fica fora do conjunto de permissões concedidos a aplicativos iniciados pela Internet ou pela zona da intranet local usando o ClickOnce. Consequentemente, os usuários receberão um aviso de segurança do ClickOnce e precisarão elevar o conjunto de permissões do aplicativo para confiança total.  
  
 Além disso, XBAPs não podem mostrar janelas ou caixas de diálogo por padrão. Para obter uma discussão sobre considerações de segurança de aplicativo autônomo, consulte [estratégia de segurança do WPF – segurança da plataforma](../wpf-security-strategy-platform-security.md).  
  
<a name="Other_Types_of_Windows"></a>
## <a name="other-types-of-windows"></a>Outros tipos de janelas  
 <xref:System.Windows.Navigation.NavigationWindow>é uma janela que é projetada para hospedar conteúdo navegável. Para obter mais informações, consulte [visão geral de navegação](navigation-overview.md)).  
  
 Caixas de diálogo são janelas que geralmente são usadas para coletar informações de um usuário para concluir uma função. Por exemplo, quando um usuário deseja abrir um arquivo, a caixa de diálogo **Abrir arquivo** geralmente é exibida por um aplicativo para obter o nome do arquivo do usuário. Para obter mais informações, consulte [Visão geral das caixas de diálogo](dialog-boxes-overview.md).  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Windows.Window>
- <xref:System.Windows.MessageBox>
- <xref:System.Windows.Navigation.NavigationWindow>
- <xref:System.Windows.Application>
- [Visão geral das caixas de diálogo](dialog-boxes-overview.md)
- [Compilando um aplicativo WPF](building-a-wpf-application-wpf.md)
