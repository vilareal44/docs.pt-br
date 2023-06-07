---
title: Estendendo o modelo de aplicativo do Visual Basic
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic Application Model, extending
ms.assetid: e91d3bed-4c27-40e3-871d-2be17467c72c
ms.openlocfilehash: e707f034f05aababdc70d5d6e1f9e1da0ed558bc
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410225"
---
# <a name="extending-the-visual-basic-application-model"></a>Estendendo o modelo de aplicativo do Visual Basic

Você pode adicionar funcionalidade ao modelo de aplicativo substituindo os `Overridable` membros da <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase> classe. Essa técnica permite que você personalize o comportamento do modelo de aplicativo e adicione chamadas para seus próprios métodos à medida que o aplicativo é iniciado e desligado.

## <a name="visual-overview-of-the-application-model"></a>Visão geral visual do modelo de aplicativo

Esta seção apresenta visualmente a sequência de chamadas de função no modelo de aplicativo Visual Basic. A próxima seção descreve a finalidade de cada função em detalhes.

O gráfico a seguir mostra a sequência de chamada de modelo de aplicativo em um aplicativo Visual Basic normal Windows Forms. A sequência é iniciada quando o `Sub Main` procedimento chama o <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Run%2A> método.

![Diagrama mostrando a sequência de chamada de modelo de aplicativo.](./media/extending-the-visual-basic-application-model/application-model-call-sequence.gif)

O modelo de aplicativo Visual Basic também fornece <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.StartupNextInstance> os <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.UnhandledException> eventos e. Os gráficos a seguir mostram o mecanismo para gerar esses eventos.

![Diagrama que mostra o método OnStartupNextInstance que gera o evento StartupNextInstance.](./media/extending-the-visual-basic-application-model/raise-startupnextinstance-event.gif)

![Diagrama que mostra o método OnUnhandledException que gera o evento UnhandledException.](./media/extending-the-visual-basic-application-model/raise-unhandledexception-event.gif)

## <a name="overriding-the-base-methods"></a>Substituindo os métodos base

O <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Run%2A> método define a ordem na qual os `Application` métodos são executados. Por padrão, o `Sub Main` procedimento para um aplicativo Windows Forms chama o <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Run%2A> método.

Se o aplicativo for um aplicativo normal (aplicativo de várias instâncias) ou a primeira instância de um aplicativo de instância única, o <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Run%2A> método executará os `Overridable` métodos na seguinte ordem:

1. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnInitialize%2A>. Por padrão, esse método define os estilos visuais, os estilos de exibição de texto e a entidade de segurança atual do thread do aplicativo principal (se o aplicativo usa a autenticação do Windows) e chama `ShowSplashScreen` se nem `/nosplash` nem `-nosplash` é usado como um argumento de linha de comando.

     A sequência de inicialização do aplicativo será cancelada se essa função retornar `False` . Isso pode ser útil se houver circunstâncias em que o aplicativo não deve ser executado.

     O <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnInitialize%2A> método chama os seguintes métodos:

    1. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.ShowSplashScreen%2A>. Determina se o aplicativo tem uma tela inicial definida e, se tiver, exibe a tela inicial em um thread separado.

         O <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.ShowSplashScreen%2A> método contém o código que exibe a tela inicial de pelo menos o número de milissegundos especificado pela <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.MinimumSplashScreenDisplayTime%2A> propriedade. Para usar essa funcionalidade, você deve adicionar a tela inicial ao seu aplicativo usando o **Designer de projeto** (que define a `My.Application.MinimumSplashScreenDisplayTime` propriedade como dois segundos) ou definir a `My.Application.MinimumSplashScreenDisplayTime` propriedade em um método que substitui o <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnInitialize%2A> <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnCreateSplashScreen%2A> método ou. Para obter mais informações, consulte <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.MinimumSplashScreenDisplayTime%2A>.

    2. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnCreateSplashScreen%2A>. Permite que um designer Emita código que inicializa a tela inicial.

         Por padrão, esse método não faz nada. Se você selecionar uma tela inicial para seu aplicativo na Visual Basic **Designer de projeto**, o designer substituirá o <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnCreateSplashScreen%2A> método por um método que define a <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.SplashScreen%2A> propriedade como uma nova instância do formulário da tela inicial.

2. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnStartup%2A>. Fornece um ponto de extensibilidade para gerar o `Startup` evento. A sequência de inicialização do aplicativo será interrompida se essa função retornar `False` .

     Por padrão, esse método gera o <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Startup> evento. Se o manipulador de eventos definir a <xref:System.ComponentModel.CancelEventArgs.Cancel> Propriedade do argumento de evento como `True` , o método retornará `False` para cancelar a inicialização do aplicativo.

3. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnRun%2A>. Fornece o ponto de partida para quando o aplicativo principal está pronto para começar a ser executado, após a inicialização ser feita.

     Por padrão, antes de entrar no loop de Windows Forms mensagem, esse método chama os `OnCreateMainForm` métodos (para criar o formulário principal do aplicativo) e `HideSplashScreen` (para fechar a tela inicial):

    1. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnCreateMainForm%2A>. Fornece uma maneira para um designer emitir código que inicializa o formulário principal.

         Por padrão, esse método não faz nada. No entanto, quando você seleciona um formulário principal para seu aplicativo na Visual Basic **Designer de projeto**, o designer substitui o <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnCreateMainForm%2A> método por um método que define a <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.MainForm%2A> propriedade como uma nova instância do formulário principal.

    2. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.HideSplashScreen%2A>. Se o aplicativo tiver uma tela inicial definida e estiver aberto, esse método fechará a tela inicial.

         Por padrão, esse método fecha a tela inicial.

4. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnStartupNextInstance%2A>. Fornece uma maneira de personalizar como um aplicativo de instância única se comporta quando outra instância do aplicativo é iniciada.

     Por padrão, esse método gera o <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.StartupNextInstance> evento.

5. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnShutdown%2A>. Fornece um ponto de extensibilidade para gerar o `Shutdown` evento. Esse método não será executado se ocorrer uma exceção sem tratamento no aplicativo principal.

     Por padrão, esse método gera o <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Shutdown> evento.

6. <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnUnhandledException%2A>. Executado se uma exceção sem tratamento ocorrer em qualquer um dos métodos listados acima.

     Por padrão, esse método gera o <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.UnhandledException> evento, desde que um depurador não esteja anexado e o aplicativo esteja manipulando o `UnhandledException` evento.

 Se o aplicativo for um aplicativo de instância única e o aplicativo já estiver em execução, a instância subsequente do aplicativo chamará o <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnStartupNextInstance%2A> método na instância original do aplicativo e, em seguida, será encerrada.

 O <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OnStartupNextInstance(Microsoft.VisualBasic.ApplicationServices.StartupNextInstanceEventArgs)> construtor chama a <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.UseCompatibleTextRendering%2A> propriedade para determinar qual mecanismo de renderização de texto a ser usado para os formulários do aplicativo. Por padrão, a <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.UseCompatibleTextRendering%2A> propriedade retorna `False` , indicando que o mecanismo de renderização de texto GDI será usado, que é o padrão no Visual Basic 2005 e versões posteriores. Você pode substituir a <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.UseCompatibleTextRendering%2A> Propriedade a ser retornada `True` , o que indica que o mecanismo de renderização de texto de GDI+ deve ser usado, que é o padrão em Visual Basic .NET 2002 e Visual Basic .NET 2003.

## <a name="configuring-the-application"></a>Configurando o aplicativo

 Como parte do modelo de aplicativo Visual Basic, a <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase> classe fornece propriedades protegidas que configuram o aplicativo. Essas propriedades devem ser definidas no construtor da classe de implementação.

 Em um projeto de Windows Forms padrão, o **Designer de projeto** cria o código para definir as propriedades com as configurações de designer. As propriedades são usadas somente quando o aplicativo está sendo iniciado; configurá-los depois que o aplicativo é iniciado não tem nenhum efeito.

|Propriedade|Verifica|Configuração no painel de aplicativo do designer de projeto|
|---|---|---|
|<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.IsSingleInstance%2A>|Se o aplicativo é executado como um aplicativo de instância única ou de várias instâncias.|Caixa de seleção **criar aplicativo de instância única**|
|<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.EnableVisualStyles%2A>|Se o aplicativo usar estilos visuais que correspondam ao Windows XP.|Caixa de seleção **Habilitar estilos visuais do XP**|
|<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.SaveMySettingsOnExit%2A>|Se o aplicativo salvar automaticamente as alterações de configurações de usuário do aplicativo quando o aplicativo for encerrado.|Caixa **de seleção Salvar My. Settings na desligamento**|
|<xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.ShutdownStyle%2A>|O que faz com que o aplicativo seja encerrado, como quando o formulário de inicialização é fechado ou quando o último formulário é fechado.|Lista de **modo de desligamento**|

## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Startup>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.StartupNextInstance>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.UnhandledException>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Shutdown>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.NetworkAvailabilityChanged>
- [Visão geral do modelo de aplicativo do Visual Basic](../development-with-my/overview-of-the-visual-basic-application-model.md)
- [Página de Aplicativo, Designer de Projeto (Visual Basic)](/visualstudio/ide/reference/application-page-project-designer-visual-basic)
