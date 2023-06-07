---
title: Introdução a aplicativos do Serviço Windows
description: Leia uma introdução aos aplicativos de serviço do Windows. Os serviços permitem que você crie aplicativos executáveis de longa execução que são executados em suas próprias sessões do Windows.
ms.date: 03/30/2017
f1_keywords:
- ServiceController
helpviewer_keywords:
- Windows Service applications, deploying
- OnStop method
- OnPause method
- services, about services
- Service class, Windows Service applications
- framework services, creating services
- ServiceController components, about Windows services
- Win32OwnProcess service type
- services, lifetime
- OnContinue method
- Windows Service applications, about Windows Service applications
- services, states
- service states
- WaitForStatus method
- Win32ShareProcess service type
- Windows Service applications, lifetime
ms.assetid: 1b1b5e67-3ff3-40c0-8154-322cfd6ef0ae
author: ghogen
ms.openlocfilehash: 13bd1f42776ac76a43a83667465ac0ca277e3452
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86925664"
---
# <a name="introduction-to-windows-service-applications"></a>Introdução a aplicativos do Serviço Windows
Os serviços do Microsoft Windows, anteriormente conhecidos como serviços do NT, permitem que você crie aplicativos executáveis de longa execução que são executados em suas próprias sessões do Windows. Esses serviços podem ser iniciados automaticamente quando o computador é inicializado, podem ser colocados em pausa e reiniciados e não exibem nenhuma interface do usuário. Esses recursos fazem com que os serviços sejam ideais para serem usados em um servidor ou sempre que você precisar de uma funcionalidade de longa execução que não interfira em outros usuários que trabalham no mesmo computador. Você também pode executar serviços no contexto de segurança de uma conta de usuário específica diferente do usuário conectado ou da conta padrão do computador. Para obter mais informações sobre serviços e sessões do Windows, confira a documentação do SDK do Windows.  
  
 Você pode criar serviços facilmente, criando um aplicativo que é instalado como um serviço. Por exemplo, considere que você queira monitorar dados do contador de desempenho e reagir a valores de limite. Você pode escrever um aplicativo de Serviço Windows que escute os dados do contador de desempenho, implante o aplicativo e comece a coleta e a análise de dados.  
  
 Você cria o serviço como um projeto do Microsoft Visual Studio, definindo um código que controla os comandos que podem ser enviados ao serviço e quais ações devem ser tomadas quando esses comandos são recebidos. Os comandos que podem ser enviados a um serviço incluem iniciar, pausar, retomar e parar o serviço. Você também pode executar comandos personalizados.  
  
 Depois de criar e compilar o aplicativo, você pode instalá-lo executando o utilitário de linha de comando InstallUtil.exe e passando o caminho para o arquivo executável do serviço. Você pode usar o **Gerenciador de Controle de Serviços** para iniciar, parar, pausar, retomar e configurar o serviço. Você também pode realizar muitas das mesmas tarefas no nó **Serviços** no **Gerenciador de Servidores** ou usando a classe <xref:System.ServiceProcess.ServiceController>.  
  
## <a name="service-applications-vs-other-visual-studio-applications"></a>Aplicativos de serviço versus outros aplicativos do Visual Studio  
 Os aplicativos de serviço funcionam de forma diferente de vários outros tipos de projeto por vários motivos:  
  
- O arquivo executável compilado que um projeto de aplicativo de serviço cria precisa ser instalado no servidor antes que o projeto possa funcionar de forma significativa. Você não pode depurar ou executar um aplicativo de serviço pressionando F5 ou F11. Você não pode executar um serviço ou intervir no código imediatamente. Nesse caso, você precisa instalar e iniciar o serviço e, em seguida, anexar um depurador ao processo do serviço. Para obter mais informações, confira [Como depurar aplicativos de Serviço Windows](how-to-debug-windows-service-applications.md).  
  
- Ao contrário de alguns tipos de projetos, você precisa criar componentes de instalação para aplicativos de serviço. Os componentes de instalação instalam e registram o serviço no servidor e criam uma entrada para o serviço com o **Gerenciador de Controle de Serviços** do Windows. Para obter mais informações, confira [Como adicionar instaladores no seu aplicativo de serviço](how-to-add-installers-to-your-service-application.md).  
  
- O método `Main` para o aplicativo de serviço precisa emitir o comando Run para os serviços que o projeto contém. O método `Run` carrega os serviços no **Gerenciador de Controle de Serviços** no servidor apropriado. Se você usar o modelo de projeto de **Serviços Windows**, esse método será escrito automaticamente. Observe carregar um serviço não é a mesma coisa que iniciar o serviço. Confira "Tempo de vida do serviço" abaixo para obter mais informações.  
  
- Os aplicativos de Serviço Windows são executados em uma estação de janela diferente da estação interativa do usuário conectado. Uma estação de janela é um objeto seguro que contém uma área de transferência, um conjunto de átomos globais e um grupo de objetos de área de trabalho. Como a estação de Serviço Windows não é uma estação interativa, as caixas de diálogo geradas de um aplicativo de Serviço Windows não serão vistas e poderão fazer com que o programa pare de responder. Da mesma forma, as mensagens de erro devem ser registradas no log de eventos do Windows e não geradas na interface do usuário.  
  
     As classes de Serviço Windows com suporte do .NET Framework não permitem a interação com estações interativas, ou seja, com o usuário conectado. O .NET Framework também não inclui classes que representam estações e áreas de trabalho. Se o Serviço Windows precisar interagir com outras estações, será necessário acessar a API do Windows não gerenciada. Para obter mais informações, confira a documentação do SDK do Windows.  
  
     A interação do serviço Windows com o usuário ou com outras estações precisa ser projetada cuidadosamente para incluir cenários como, quando não houver nenhum usuário conectado ou quando o usuário tiver um conjunto inesperado de objetos de área de trabalho. Em alguns casos, pode ser mais apropriado escrever um aplicativo do Windows que seja executado sob o controle do usuário.  
  
- Os aplicativos de serviço Windows são executados em seus próprios contextos de segurança e são iniciados antes do usuário fazer logon no computador Windows no qual estão instalados. Você deve planejar cuidadosamente em qual conta de usuário o serviço será executado. Um serviço executado na conta do sistema tem mais permissões e privilégios do que em uma conta de usuário.  
  
## <a name="service-lifetime"></a>Tempo de vida do serviço  
 Um serviço passa por vários estados internos durante seu tempo de vida. Primeiro, o serviço está instalado no sistema no qual será executado. Esse processo executa os instaladores do projeto de serviço e carrega o serviço no **Gerenciador de Controle de Serviços** desse computador. O **Gerenciador de Controle de Serviços** é o utilitário central fornecido pelo Windows para administrar serviços.  
  
 Depois que o serviço é carregado, ele precisa ser iniciado. Iniciar o serviço permite que ele comece a funcionar. Você pode iniciar um serviço usando o **Gerenciador de Controle de Serviços**, o **Gerenciador de Servidores** ou o código chamando o método <xref:System.ServiceProcess.ServiceController.Start%2A>. O método <xref:System.ServiceProcess.ServiceController.Start%2A> passa o processamento para o método <xref:System.ServiceProcess.ServiceBase.OnStart%2A> do aplicativo e processa qualquer código que você tenha definido ali.  
  
 Um serviço em execução pode existir nesse estado indefinidamente até que ele seja interrompido ou colocado em pausa, ou até que o computador seja desligado. Um serviço pode existir em um de três estados básico: <xref:System.ServiceProcess.ServiceControllerStatus.Running>, <xref:System.ServiceProcess.ServiceControllerStatus.Paused> ou <xref:System.ServiceProcess.ServiceControllerStatus.Stopped>. O serviço também pode relatar o estado de um comando pendente: <xref:System.ServiceProcess.ServiceControllerStatus.ContinuePending>, <xref:System.ServiceProcess.ServiceControllerStatus.PausePending>, <xref:System.ServiceProcess.ServiceControllerStatus.StartPending> ou <xref:System.ServiceProcess.ServiceControllerStatus.StopPending>. Esses status indicam que um comando foi emitido, como um comando para pausar um serviço em execução, mas ainda não foi executado. Você pode consultar o <xref:System.ServiceProcess.ServiceController.Status%2A> para determinar em qual estado o serviço está ou usar o <xref:System.ServiceProcess.ServiceController.WaitForStatus%2A> para executar uma ação quando algum desses estados ocorrer.  
  
 Você pode pausar, parar ou retomar um serviço usando o **Gerenciador de Controle de Serviço**, do **Gerenciador de Servidores** ou chamando os métodos no código. Cada uma dessas ações pode chamar um procedimento associado no serviço (<xref:System.ServiceProcess.ServiceBase.OnStop%2A>, <xref:System.ServiceProcess.ServiceBase.OnPause%2A> ou <xref:System.ServiceProcess.ServiceBase.OnContinue%2A>), no qual você pode definir o processamento adicional a ser executado quando o estado do serviço for alterado.  
  
## <a name="types-of-services"></a>Tipos de serviços  
 Há dois tipos de serviços que você pode criar no Visual Studio usando o .NET Framework. Os serviços que são o único serviço em um processo recebem o tipo <xref:System.ServiceProcess.ServiceType.Win32OwnProcess>. Os serviços que compartilham um processo com outro serviço recebem o tipo <xref:System.ServiceProcess.ServiceType.Win32ShareProcess>. Você pode recuperar o tipo do serviço consultando a propriedade <xref:System.ServiceProcess.ServiceController.ServiceType%2A>.  
  
 Ocasionalmente, você poderá ver outros tipos de serviço ao consultar os serviços existentes que não foram criados no Visual Studio. Para obter mais informações sobre eles, confira o <xref:System.ServiceProcess.ServiceType>.  
  
## <a name="services-and-the-servicecontroller-component"></a>Serviços e o componente ServiceController  
 O componente <xref:System.ServiceProcess.ServiceController> é usado para se conectar a um serviço instalado e manipular o seu estado. Usando um componente <xref:System.ServiceProcess.ServiceController>, você pode iniciar e parar um serviço, pausar e continuar seu funcionamento e enviar comandos personalizados a um serviço. No entanto, você não precisa usar um componente <xref:System.ServiceProcess.ServiceController> ao criar um aplicativo de serviço. Na verdade, na maioria dos casos o componente <xref:System.ServiceProcess.ServiceController> deve existir em um aplicativo separado do aplicativo de serviço Windows que define seu serviço.  
  
 Para obter mais informações, consulte <xref:System.ServiceProcess.ServiceController>.  
  
## <a name="requirements"></a>Requisitos  
  
- Os serviços precisam ser criados em um projeto de aplicativo de **Serviço Windows** ou em outro projeto habilitado pelo .NET Framework que crie um arquivo .exe ao ser compilado e seja herdado da classe <xref:System.ServiceProcess.ServiceBase>.  
  
- Os projetos que contêm serviços Windows precisam ter os componentes de instalação para o projeto e seus serviços. Isso pode ser conseguido facilmente pela janela **Propriedades**. Para obter mais informações, confira [Como adicionar instaladores no seu aplicativo de serviço](how-to-add-installers-to-your-service-application.md).  
  
## <a name="see-also"></a>Veja também

- [Aplicativos do serviço Windows](index.md)
- [Arquitetura de programação do aplicativo de serviço](service-application-programming-architecture.md)
- [Como: Criar serviços Windows](how-to-create-windows-services.md)
- [Como: instalar e desinstalar serviços](how-to-install-and-uninstall-services.md)
- [Como: Iniciar serviços](how-to-start-services.md)
- [Como: Depurar aplicativos do serviço Windows](how-to-debug-windows-service-applications.md)
- [Passo a passo: criando um aplicativo de Serviço Windows no Designer de componentes](walkthrough-creating-a-windows-service-application-in-the-component-designer.md)
- [Como: Adicionar instaladores ao aplicativo de serviço](how-to-add-installers-to-your-service-application.md)
