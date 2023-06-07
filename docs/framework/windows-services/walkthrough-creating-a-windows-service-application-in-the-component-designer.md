---
title: 'Tutorial: criar um aplicativo de serviço do Windows'
description: Neste tutorial, crie um aplicativo de serviço do Windows no Visual Studio que grava mensagens em um log de eventos. Adicionar recursos, definir status, adicionar instaladores e muito mais.
ms.date: 03/27/2019
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Windows service applications, walkthroughs
- Windows service applications, creating
ms.assetid: e24d8a3d-edc6-485c-b6e0-5672d91fb607
author: ghogen
ms.openlocfilehash: 487a974af2280a02b83fe685324c9464df705585
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86925625"
---
# <a name="tutorial-create-a-windows-service-app"></a>Tutorial: criar um aplicativo de serviço do Windows

Este artigo demonstra como criar um aplicativo de serviço Windows no Visual Studio que grava mensagens em um log de eventos.

## <a name="create-a-service"></a>Criar um serviço

Para começar, crie o projeto e defina os valores necessários para o serviço funcionar corretamente.

1. No menu **Arquivo** do Visual Studio, selecione **Novo** > **Projeto** (ou pressione **Ctrl**+**Shift**+**N**) para abrir a janela **Novo Projeto**.

2. Navegue para o modelo de projeto **Serviço Windows (.NET Framework)** e selecione-o. Para encontrá-lo, expanda **Instalados** e **Visual C#** ou **Visual Basic** e, em seguida, selecione **Windows Desktop**. Se preferir, insira *Serviço Windows* na caixa de pesquisa no canto superior direito e pressione **Enter**.

   ![Modelo de Serviço Windows na caixa de diálogo Novo Projeto no Visual Studio](./media/new-project-dialog.png)

   > [!NOTE]
   > Se você não vir o modelo de **serviço do Windows** , talvez seja necessário instalar a carga de trabalho de **desenvolvimento do .net desktop** :
   >
   > Na caixa de diálogo **Novo Projeto**, selecione **Abrir Instalador do Visual Studio** na parte inferior esquerda. Selecione a carga de trabalho **Desenvolvimento para desktop com .NET** e, em seguida, selecione **Modificar**.

3. Em **Nome**, insira *MyNewService* e, em seguida, selecione **OK**.

   A guia **Design** será exibida (**Service1.cs [Design]** ou **Service1.vb [Design]**).

   O modelo de projeto inclui uma classe de componente denominada `Service1` herdada de <xref:System.ServiceProcess.ServiceBase?displayProperty=nameWithType>. Isso inclui grande parte do código de serviço básico, como o código para iniciar o serviço.

## <a name="rename-the-service"></a>Renomear o serviço

Renomeie o serviço de **Service1** para **MyNewService**.

1. No **Gerenciador de Soluções**, selecione **Service1.cs** ou **Service1.vb** e escolha **Renomear** no menu de atalho. Renomeie o arquivo como **MyNewService.cs** ou **MyNewService.vb** e, em seguida, pressione **Enter**

    Uma janela pop-up será exibida perguntando se você deseja renomear todas as referências ao elemento de código *Service1*.

2. Na janela pop-up, selecione **Sim**.

    ![Renomear prompt](./media/windows-service-rename.png "Prompt de renomeação de serviço do Windows")

3. Na guia **Design**, selecione **Propriedades** no menu de atalho. Na janela **Propriedades**, altere o valor **ServiceName** para *MyNewService*.

    ![Propriedades do serviço](./media/windows-service-properties.png "Propriedades do serviço Windows")

4. Selecione **Salvar Tudo** no menu **Arquivo**.

## <a name="add-features-to-the-service"></a>Adicionar recursos ao serviços

Na próxima seção, adicione um log de eventos personalizado ao serviço Windows. O componente <xref:System.Diagnostics.EventLog> é um exemplo do tipo de componente que pode ser adicionado a um serviço Windows.

### <a name="add-custom-event-log-functionality"></a>Adicionar a funcionalidade de log de eventos personalizado

1. No **Gerenciador de Soluções**, no menu de atalho de **MyNewService.cs** ou **MyNewService.vb**, escolha **Designer de Exibição**.

2. Na **Caixa de Ferramentas**, expanda **Componentes** e, em seguida, arraste o componente **EventLog** para a guia **Service1.cs [Design]** ou **Service1.vb [Design]**.

3. No **Gerenciador de Soluções**, no menu de atalho de **MyNewService.cs** ou **MyNewService.vb**, escolha **Exibir Código**.

4. Defina um log de eventos personalizado. Para o C#, edite o construtor `MyNewService()` existente; para o Visual Basic, adicione o construtor `New()`:

   [!code-csharp[VbRadconService#2](../../../samples/snippets/csharp/VS_Snippets_VBCSharp/VbRadconService/CS/MyNewService.cs#2)]
   [!code-vb[VbRadconService#2](../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbRadconService/VB/MyNewService.vb#2)]

5. Adicione uma instrução `using` a **MyNewService.cs** (caso ela ainda não exista) ou uma instrução `Imports` a **MyNewService.vb** para o namespace <xref:System.Diagnostics?displayProperty=nameWithType>:

    ```csharp
    using System.Diagnostics;
    ```

    ```vb
    Imports System.Diagnostics
    ```

6. Selecione **Salvar Tudo** no menu **Arquivo**.

### <a name="define-what-occurs-when-the-service-starts"></a>Definir o que ocorre quando o serviço é iniciado

No editor de códigos de **MyNewService.cs** ou **MyNewService.vb**, localize o método <xref:System.ServiceProcess.ServiceBase.OnStart%2A>; o Visual Studio criou automaticamente uma definição de método vazia quando você criou o projeto. Adicione um código que grava uma entrada no log de eventos quando o serviço é iniciado:

[!code-csharp[VbRadconService#3](../../../samples/snippets/csharp/VS_Snippets_VBCSharp/VbRadconService/CS/MyNewService.cs#3)]
[!code-vb[VbRadconService#3](../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbRadconService/VB/MyNewService.vb#3)]

#### <a name="polling"></a>Sondagem

Como um aplicativo de serviço foi projetado para ser de execução longa, ele geralmente sonda ou monitora o sistema, que você configurou no método <xref:System.ServiceProcess.ServiceBase.OnStart%2A>. O método `OnStart` precisa ser retornado ao sistema operacional depois que a operação do serviço é iniciada, de modo que o sistema não seja bloqueado.

Para configurar um mecanismo simples de sondagem, use o componente <xref:System.Timers.Timer?displayProperty=nameWithType>. O temporizador aciona um evento <xref:System.Timers.Timer.Elapsed> em intervalos regulares, momento em que o serviço pode fazer o monitoramento. Use o componente <xref:System.Timers.Timer> da seguinte maneira:

- Defina as propriedades do componente <xref:System.Timers.Timer> no método `MyNewService.OnStart`.
- Inicie o temporizador chamando o método <xref:System.Timers.Timer.Start%2A>.

##### <a name="set-up-the-polling-mechanism"></a>Configure o mecanismo de sondagem.

1. Adicione o seguinte código ao evento `MyNewService.OnStart` para configurar o mecanismo de sondagem:

   ```csharp
   // Set up a timer that triggers every minute.
   Timer timer = new Timer();
   timer.Interval = 60000; // 60 seconds
   timer.Elapsed += new ElapsedEventHandler(this.OnTimer);
   timer.Start();
   ```

   ```vb
   ' Set up a timer that triggers every minute.
   Dim timer As Timer = New Timer()
   timer.Interval = 60000 ' 60 seconds
   AddHandler timer.Elapsed, AddressOf Me.OnTimer
   timer.Start()
   ```

2. Adicione uma instrução `using` a **MyNewService.cs** ou uma instrução `Imports` a **MyNewService.vb** para o namespace <xref:System.Timers?displayProperty=nameWithType>:

   ```csharp
   using System.Timers;
   ```

   ```vb
   Imports System.Timers
   ```

3. Na classe `MyNewService`, adicione o método `OnTimer` para manipular o evento <xref:System.Timers.Timer.Elapsed?displayProperty=nameWithType>:

   ```csharp
   public void OnTimer(object sender, ElapsedEventArgs args)
   {
       // TODO: Insert monitoring activities here.
       eventLog1.WriteEntry("Monitoring the System", EventLogEntryType.Information, eventId++);
   }
   ```

   ```vb
   Private Sub OnTimer(sender As Object, e As Timers.ElapsedEventArgs)
      ' TODO: Insert monitoring activities here.
      eventLog1.WriteEntry("Monitoring the System", EventLogEntryType.Information, eventId)
      eventId = eventId + 1
   End Sub
   ```

4. Na classe `MyNewService`, adicione uma variável de membro. Ela contém o identificador do próximo evento a ser gravado no log de eventos:

   ```csharp
   private int eventId = 1;
   ```

   ```vb
   Private eventId As Integer = 1
   ```

Em vez de executar todo o trabalho no thread principal, você pode executar tarefas usando threads de trabalho em segundo plano. Para obter mais informações, consulte <xref:System.ComponentModel.BackgroundWorker?displayProperty=fullName>.

### <a name="define-what-occurs-when-the-service-is-stopped"></a>Definir o que ocorre quando o serviço é interrompido

Insira uma linha de código no método <xref:System.ServiceProcess.ServiceBase.OnStop%2A> que adiciona uma entrada ao log de eventos quando o serviço é interrompido:

[!code-csharp[VbRadconService#2](../../../samples/snippets/csharp/VS_Snippets_VBCSharp/VbRadconService/CS/MyNewService.cs#4)]
[!code-vb[VbRadconService#4](../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbRadconService/VB/MyNewService.vb#4)]

### <a name="define-other-actions-for-the-service"></a>Definir outras ações para o serviço

Também é possível substituir os métodos <xref:System.ServiceProcess.ServiceBase.OnPause%2A>, <xref:System.ServiceProcess.ServiceBase.OnContinue%2A> e <xref:System.ServiceProcess.ServiceBase.OnShutdown%2A> para definir o processamento adicional para seu componente.

O seguinte código mostra como substituir o método <xref:System.ServiceProcess.ServiceBase.OnContinue%2A> na classe `MyNewService`:

[!code-csharp[VbRadconService#5](../../../samples/snippets/csharp/VS_Snippets_VBCSharp/VbRadconService/CS/MyNewService.cs#5)]
[!code-vb[VbRadconService#5](../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbRadconService/VB/MyNewService.vb#5)]

## <a name="set-service-status"></a>Definir status do serviço

Os serviços relatam seu status para o [Gerenciador de Controle de Serviço](/windows/desktop/Services/service-control-manager), de modo que um usuário possa determinar se um serviço está funcionando corretamente. Por padrão, um serviço que herda de <xref:System.ServiceProcess.ServiceBase> relata um conjunto limitado de configurações de status, que incluem SERVICE_STOPPED, SERVICE_PAUSED e SERVICE_RUNNING. Se um serviço leva um tempo para ser inicializado, é útil relatar um status SERVICE_START_PENDING.

Você pode implementar as configurações de status SERVICE_START_PENDING e SERVICE_STOP_PENDING adicionando o código que chama a função [SetServiceStatus](/windows/desktop/api/winsvc/nf-winsvc-setservicestatus) do Windows.

### <a name="implement-service-pending-status"></a>Implementar o status pendente do serviço

1. Adicione uma instrução `using` a **MyNewService.cs** ou uma instrução `Imports` a **MyNewService.vb** para o namespace <xref:System.Runtime.InteropServices?displayProperty=nameWithType>:

    ```csharp
    using System.Runtime.InteropServices;
    ```

    ```vb
    Imports System.Runtime.InteropServices
    ```

2. Adicione o seguinte código a **MyNewService.cs** ou **MyNewService.vb** para declarar os valores `ServiceState` e adicionar uma estrutura ao status, que você usará em uma chamada de invocação de plataforma:

    ```csharp
    public enum ServiceState
    {
        SERVICE_STOPPED = 0x00000001,
        SERVICE_START_PENDING = 0x00000002,
        SERVICE_STOP_PENDING = 0x00000003,
        SERVICE_RUNNING = 0x00000004,
        SERVICE_CONTINUE_PENDING = 0x00000005,
        SERVICE_PAUSE_PENDING = 0x00000006,
        SERVICE_PAUSED = 0x00000007,
    }

    [StructLayout(LayoutKind.Sequential)]
    public struct ServiceStatus
    {
        public int dwServiceType;
        public ServiceState dwCurrentState;
        public int dwControlsAccepted;
        public int dwWin32ExitCode;
        public int dwServiceSpecificExitCode;
        public int dwCheckPoint;
        public int dwWaitHint;
    };
    ```

    ```vb
    Public Enum ServiceState
        SERVICE_STOPPED = 1
        SERVICE_START_PENDING = 2
        SERVICE_STOP_PENDING = 3
        SERVICE_RUNNING = 4
        SERVICE_CONTINUE_PENDING = 5
        SERVICE_PAUSE_PENDING = 6
        SERVICE_PAUSED = 7
    End Enum

    <StructLayout(LayoutKind.Sequential)>
    Public Structure ServiceStatus
        Public dwServiceType As Long
        Public dwCurrentState As ServiceState
        Public dwControlsAccepted As Long
        Public dwWin32ExitCode As Long
        Public dwServiceSpecificExitCode As Long
        Public dwCheckPoint As Long
        Public dwWaitHint As Long
    End Structure
    ```

    > [!NOTE]
    > O Gerenciador de Controle de Serviço usa os membros `dwWaitHint` e `dwCheckpoint` da [estrutura SERVICE_STATUS](/windows/win32/api/winsvc/ns-winsvc-service_status) para determinar quanto tempo esperar para que um serviço Windows seja iniciado ou desligado. Se os métodos `OnStart` e `OnStop` tiverem uma execução longa, o serviço poderá solicitar mais tempo chamando `SetServiceStatus` novamente com um valor `dwCheckPoint` incrementado.

3. Na classe `MyNewService`, declare a função [SetServiceStatus](/windows/desktop/api/winsvc/nf-winsvc-setservicestatus) usando a [inovação de plataforma](../interop/consuming-unmanaged-dll-functions.md):

    ```csharp
    [DllImport("advapi32.dll", SetLastError = true)]
    private static extern bool SetServiceStatus(System.IntPtr handle, ref ServiceStatus serviceStatus);
    ```

    ```vb
    Declare Auto Function SetServiceStatus Lib "advapi32.dll" (ByVal handle As IntPtr, ByRef serviceStatus As ServiceStatus) As Boolean
    ```

4. Para implementar o status SERVICE_START_PENDING, adicione o seguinte código ao início do método <xref:System.ServiceProcess.ServiceBase.OnStart%2A>:

    ```csharp
    // Update the service state to Start Pending.
    ServiceStatus serviceStatus = new ServiceStatus();
    serviceStatus.dwCurrentState = ServiceState.SERVICE_START_PENDING;
    serviceStatus.dwWaitHint = 100000;
    SetServiceStatus(this.ServiceHandle, ref serviceStatus);
    ```

    ```vb
    ' Update the service state to Start Pending.
    Dim serviceStatus As ServiceStatus = New ServiceStatus()
    serviceStatus.dwCurrentState = ServiceState.SERVICE_START_PENDING
    serviceStatus.dwWaitHint = 100000
    SetServiceStatus(Me.ServiceHandle, serviceStatus)
    ```

5. Adicione o código ao final do método `OnStart` para definir o status como SERVICE_RUNNING:

    ```csharp
    // Update the service state to Running.
    serviceStatus.dwCurrentState = ServiceState.SERVICE_RUNNING;
    SetServiceStatus(this.ServiceHandle, ref serviceStatus);
    ```

    ```vb
    ' Update the service state to Running.
    serviceStatus.dwCurrentState = ServiceState.SERVICE_RUNNING
    SetServiceStatus(Me.ServiceHandle, serviceStatus)
    ```

6. (Opcional) Se <xref:System.ServiceProcess.ServiceBase.OnStop%2A> for um método de execução longa, repita esse procedimento no método `OnStop`. Implemente o status SERVICE_STOP_PENDING e retorne o status SERVICE_STOPPED antes da saída do método `OnStop`.

   Por exemplo:

    ```csharp
    // Update the service state to Stop Pending.
    ServiceStatus serviceStatus = new ServiceStatus();
    serviceStatus.dwCurrentState = ServiceState.SERVICE_STOP_PENDING;
    serviceStatus.dwWaitHint = 100000;
    SetServiceStatus(this.ServiceHandle, ref serviceStatus);

    // Update the service state to Stopped.
    serviceStatus.dwCurrentState = ServiceState.SERVICE_STOPPED;
    SetServiceStatus(this.ServiceHandle, ref serviceStatus);
    ```

    ```vb
    ' Update the service state to Stop Pending.
    Dim serviceStatus As ServiceStatus = New ServiceStatus()
    serviceStatus.dwCurrentState = ServiceState.SERVICE_STOP_PENDING
    serviceStatus.dwWaitHint = 100000
    SetServiceStatus(Me.ServiceHandle, serviceStatus)

    ' Update the service state to Stopped.
    serviceStatus.dwCurrentState = ServiceState.SERVICE_STOPPED
    SetServiceStatus(Me.ServiceHandle, serviceStatus)
    ```

## <a name="add-installers-to-the-service"></a>Adicionar instaladores ao serviço

Antes de executar um serviço Windows, é necessário instalá-lo, o que o registra no Gerenciador de Controle de Serviço. Adicione instaladores ao projeto para lidar com os detalhes de registro.

1. No **Gerenciador de Soluções**, no menu de atalho de **MyNewService.cs** ou **MyNewService.vb**, escolha **Designer de Exibição**.

2. Na exibição **Design**, selecione a área em segundo plano e, em seguida, escolha **Adicionar Instalador** no menu de atalho.

     Por padrão, o Visual Studio adiciona uma classe de componente chamada `ProjectInstaller`, que contém dois instaladores, ao projeto. Esses instaladores destinam-se ao serviço e ao processo associado do serviço.

3. Na exibição **Design** de **ProjectInstaller**, selecione **serviceInstaller1** para um projeto do Visual C# ou **ServiceInstaller1** para um projeto do Visual Basic e, em seguida, escolha **Propriedades** no menu de atalho.

4. Na janela **Propriedades**, verifique se a propriedade <xref:System.ServiceProcess.ServiceInstaller.ServiceName%2A> está definida como **MyNewService**.

5. Adicione um texto à propriedade <xref:System.ServiceProcess.ServiceInstaller.Description%2A>, como *Um serviço de exemplo*.

     Esse texto é exibido na coluna **Descrição** da janela **Serviços** e descreve o serviço para o usuário.

    ![Descrição do serviço na janela serviços.](./media/windows-service-description.png "Descrição do serviço")

6. Adicione o texto à propriedade <xref:System.ServiceProcess.ServiceInstaller.DisplayName%2A>. Por exemplo, *Nome de Exibição do MyNewService*.

     Esse texto é exibido na coluna **Nome de Exibição** da janela **Serviços**. Esse nome pode ser diferente da propriedade <xref:System.ServiceProcess.ServiceInstaller.ServiceName%2A>, que é o nome usado pelo sistema (por exemplo, o nome usado para o comando `net start` para iniciar o serviço).

7. Defina a propriedade <xref:System.ServiceProcess.ServiceInstaller.StartType%2A> como <xref:System.ServiceProcess.ServiceStartMode.Automatic> na lista suspensa.

8. Quando terminar, as janelas **Propriedades** deverão ser semelhantes à seguinte figura:

     ![Propriedades do instalador para um serviço do Windows](./media/windows-service-installer-properties.png "Propriedades do instalador de serviço do Windows")

9. Na exibição **Design** de **ProjectInstaller**, escolha **serviceProcessInstaller1** para um projeto do Visual C# ou **ServiceProcessInstaller1** para um projeto do Visual Basic e, em seguida, escolha **Propriedades** no menu de atalho. Defina a propriedade <xref:System.ServiceProcess.ServiceProcessInstaller.Account%2A> como <xref:System.ServiceProcess.ServiceAccount.LocalSystem> na lista suspensa.

     Essa configuração instala o serviço e o executa usando a conta Sistema Local.

    > [!IMPORTANT]
    > A conta <xref:System.ServiceProcess.ServiceAccount.LocalSystem> tem amplas permissões, incluindo a capacidade de gravar no log de eventos. Use essa conta com cuidado, pois ela pode aumentar o risco de ataques de software mal-intencionado. Para outras tarefas, pense em usar a conta <xref:System.ServiceProcess.ServiceAccount.LocalService>, que atua como um usuário não privilegiado no computador local e apresenta credenciais anônimas a qualquer servidor remoto. Este exemplo falhará se você tentar usar a conta <xref:System.ServiceProcess.ServiceAccount.LocalService>, pois ele precisa de permissão para gravar no log de eventos.

Para obter mais informações sobre instaladores, consulte [como: Adicionar instaladores ao seu aplicativo de serviço](how-to-add-installers-to-your-service-application.md).

## <a name="optional-set-startup-parameters"></a>(Opcional) Defina os parâmetros de inicialização

> [!NOTE]
> Antes de decidir adicionar parâmetros de inicialização, considere se esta é a melhor maneira de passar informações para o serviço. Embora elas sejam fáceis de serem usadas e analisadas e um usuário possa substituí-las com facilidade, elas podem ser mais difíceis para um usuário descobrir e usá-las sem a documentação. Em geral, se o serviço exigir mais do que apenas alguns parâmetros de inicialização, você deverá usar o Registro ou um arquivo de configuração.

Um serviço Windows pode aceitar argumentos de linha de comando ou parâmetros de inicialização. Quando você adiciona um código aos parâmetros de inicialização do processo, um usuário pode iniciar o serviço com seus próprios parâmetros de inicialização personalizados na janela Propriedades do serviço. No entanto, esses parâmetros de inicialização não são persistentes na próxima vez que o serviço é iniciado. Para definir parâmetros de inicialização permanentemente, defina-os no Registro.

Cada serviço Windows tem uma entrada do Registro na subchave **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services**. Na subchave de cada serviço, use a subchave **Parâmetros** para armazenar informações que o serviço pode acessar. É possível usar arquivos de configuração de aplicativo para um serviço Windows da mesma forma que faz para outros tipos de programas. Para obter um código de exemplo, confira <xref:System.Configuration.ConfigurationManager.AppSettings?displayProperty=nameWithType>.

### <a name="to-add-startup-parameters"></a>Para adicionar parâmetros de inicialização

1. Selecione **Program.cs** ou **MyNewService.Designer.vb** e, em seguida, escolha **Exibir Código** no menu de atalho. No método `Main`, altere o código para adicionar um parâmetro de entrada e passá-lo para o construtor do serviço:

   [!code-csharp[VbRadconService](../../../samples/snippets/csharp/VS_Snippets_VBCSharp/VbRadconService/CS/Program-add-parameter.cs?highlight=1,6)]
   [!code-vb[VbRadconService](../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbRadconService/VB/MyNewService.Designer-add-parameter.vb?highlight=1-2)]

2. Em **MyNewService.cs** ou **MyNewService.vb**, altere o construtor `MyNewService` para processar o parâmetro de entrada da seguinte maneira:

   ```csharp
   using System.Diagnostics;

   public MyNewService(string[] args)
   {
       InitializeComponent();

       string eventSourceName = "MySource";
       string logName = "MyNewLog";

       if (args.Length > 0)
       {
          eventSourceName = args[0];
       }

       if (args.Length > 1)
       {
           logName = args[1];
       }

       eventLog1 = new EventLog();

       if (!EventLog.SourceExists(eventSourceName))
       {
           EventLog.CreateEventSource(eventSourceName, logName);
       }

       eventLog1.Source = eventSourceName;
       eventLog1.Log = logName;
   }
   ```

   ```vb
   Imports System.Diagnostics

   Public Sub New(ByVal cmdArgs() As String)
       InitializeComponent()
       Dim eventSourceName As String = "MySource"
       Dim logName As String = "MyNewLog"
       If (cmdArgs.Count() > 0) Then
           eventSourceName = cmdArgs(0)
       End If
       If (cmdArgs.Count() > 1) Then
           logName = cmdArgs(1)
       End If
       eventLog1 = New EventLog()
       If (Not EventLog.SourceExists(eventSourceName)) Then
           EventLog.CreateEventSource(eventSourceName, logName)
       End If
       eventLog1.Source = eventSourceName
       eventLog1.Log = logName
   End Sub
   ```

   Esse código define o nome do log e da origem do evento, de acordo com os parâmetros de inicialização fornecidos pelo usuário. Se nenhum argumento for fornecido, ele usará valores padrão.

3. Para especificar os argumentos de linha de comando, adicione o seguinte código à classe `ProjectInstaller` em **ProjectInstaller.cs** ou **ProjectInstaller.vb**:

   ```csharp
   protected override void OnBeforeInstall(IDictionary savedState)
   {
       string parameter = "MySource1\" \"MyLogFile1";
       Context.Parameters["assemblypath"] = "\"" + Context.Parameters["assemblypath"] + "\" \"" + parameter + "\"";
       base.OnBeforeInstall(savedState);
   }
   ```

   ```vb
   Protected Overrides Sub OnBeforeInstall(ByVal savedState As IDictionary)
       Dim parameter As String = "MySource1"" ""MyLogFile1"
       Context.Parameters("assemblypath") = """" + Context.Parameters("assemblypath") + """ """ + parameter + """"
       MyBase.OnBeforeInstall(savedState)
   End Sub
   ```

   Normalmente, esse valor contém o caminho completo para o executável do serviço Windows. Para que o serviço seja iniciado corretamente, o usuário precisará inserir aspas no caminho e em cada parâmetro individual. Um usuário pode alterar os parâmetros na entrada do Registro **ImagePath** para alterar os parâmetros de inicialização do serviço Windows. No entanto, uma maneira melhor é alterar o valor de forma programática e expor a funcionalidade de forma amigável, como por meio de um utilitário de configuração ou gerenciamento.

## <a name="build-the-service"></a>Compilar o serviço

1. No **Gerenciador de Soluções**, escolha **Propriedades** no menu de atalho do projeto **MyNewService**.

   As páginas de propriedades do seu projeto aparecem.

2. Na guia **Aplicativo**, na lista **Objeto de inicialização**, escolha **MyNewService.Program** ou **Sub Main** para projetos do Visual Basic.

3. Para compilar o projeto, no **Gerenciador de Soluções**, escolha **Compilar** no menu de atalho do projeto (ou pressione **Ctrl**+**Shift**+**B**).

## <a name="install-the-service"></a>Instalar o serviço

Agora que você criou o serviço Windows, poderá instalá-lo. Para instalar um serviço Windows, é necessário ter credenciais de administrador no computador no qual ele é instalado.

1. Abra o [Prompt de Comando do Desenvolvedor para Visual Studio](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs) com credenciais administrativas. No menu **Iniciar** do Windows, selecione **Prompt de Comando do Desenvolvedor para VS 2017** na pasta Visual Studio e, em seguida, selecione **Mais** > **Executar como Administrador** no menu de atalho.

2. Na janela **Prompt de Comando do Desenvolvedor para Visual Studio**, navegue para a pasta que contém a saída do projeto (por padrão, o subdiretório *\bin\Debug* do projeto).

3. Insira o seguinte comando:

    ```shell
    installutil MyNewService.exe
    ```

    Se o serviço for instalado com êxito, o comando relatará o êxito.

    Se o sistema não puder localizar *installutil.exe*, verifique se ele existe no computador. Essa ferramenta é instalada com o .NET Framework para a pasta *%windir%\Microsoft.NET\Framework [64] \\ &lt; versão &gt; da estrutura*. Por exemplo, o caminho padrão da versão de 64 bits é *%windir%\Microsoft.NET\Framework64\v4.0.30319\InstallUtil.exe*.

    Se o processo **installutil.exe** falhar, verifique o log de instalação para saber o motivo. Por padrão, o log está localizado na mesma pasta do executável do serviço. A instalação poderá falhar se:
    - A classe <xref:System.ComponentModel.RunInstallerAttribute> não estiver presente na classe `ProjectInstaller`.
    - O atributo não estiver definido como `true`.
    - A classe `ProjectInstaller` não estiver definida como `public`.

Para obter mais informações, consulte [como: instalar e desinstalar serviços](how-to-install-and-uninstall-services.md).

## <a name="start-and-run-the-service"></a>Iniciar e executar o serviço

1. No Windows, abra o aplicativo da área de trabalho **Serviços**. Pressione **Windows** + **R** para abrir a caixa **executar** , digite *Services. msc*e pressione **Enter** ou selecione **OK**.

     Você deve ver o serviço listado em **Serviços**, exibido em ordem alfabética pelo nome de exibição definido para ele.

     ![MyNewService na janela Serviços.](./media/windowsservices-serviceswindow.PNG)

2. Para iniciar o serviço, escolha **Iniciar** no menu de atalho do serviço.

3. Para interromper o serviço, escolha **Parar** no menu de atalho do serviço.

4. (Opcional) Na linha de comando, use os comandos **net start &lt;nome do serviço&gt;** e **net stop &lt;nome do serviço&gt;** para iniciar e parar o serviço.

### <a name="verify-the-event-log-output-of-your-service"></a>Verificar a saída do log de eventos do seu serviço

1. No Windows, abra o aplicativo da área de trabalho **Visualizador de Eventos**. Insira *Visualizador de Eventos* na barra de pesquisa do Windows e, em seguida, selecione **Visualizador de Eventos** nos resultados da pesquisa.

   > [!TIP]
   > No Visual Studio, é possível acessar logs de eventos abrindo **Gerenciador de Servidores** do menu **Exibir** (ou pressione **Ctrl**+**Alt**+**S**) e expandindo o nó **Logs de Eventos** no computador local.

2. No **Visualizador de Eventos**, expanda **Logs de Aplicativos e Serviços**.

3. Localize a listagem de **MyNewLog** (ou **MyLogFile1**, se você seguiu o procedimento para adicionar argumentos de linha de comando) e expanda-a. Você deverá ver as entradas para as duas ações (iniciar e parar) executadas pelo serviço.

     ![Use o Visualizador de Eventos para ver as entradas do log de eventos](./media/windows-service-event-viewer.png)

## <a name="clean-up-resources"></a>Limpar os recursos

Caso não precise mais do aplicativo serviço Windows, remova-o.

1. Abra o **Prompt de Comando do Desenvolvedor para Visual Studio** com credenciais administrativas.

2. Na janela **Prompt de Comando do Desenvolvedor para Visual Studio**, navegue para a pasta que contém a saída do projeto.

3. Insira o seguinte comando:

    ```shell
    installutil.exe /u MyNewService.exe
    ```

   Se o serviço for desinstalado com êxito, o comando relatará que o serviço foi removido com sucesso. Para obter mais informações, consulte [como: instalar e desinstalar serviços](how-to-install-and-uninstall-services.md).

## <a name="next-steps"></a>Próximas etapas

Agora que você criou o serviço, você poderá:

- Criar um programa de instalação autônoma para que outros possam usá-lo para instalar o serviço Windows. Usar o [Conjunto de ferramentas do WiX](https://wixtoolset.org/) para criar um instalador para um serviço Windows. Para ver outras ideias, confira [Create an installer package](/visualstudio/deployment/deploying-applications-services-and-components#create-an-installer-package-windows-desktop) (Criar um pacote do instalador).

- Explorar o componente <xref:System.ServiceProcess.ServiceController>, que permite enviar comandos ao serviço instalado.

- Em vez de criar o log de eventos quando o aplicativo é executado, use um instalador para criar um log de eventos ao instalar o aplicativo. O log de eventos é excluído pelo instalador quando você desinstala o aplicativo. Para obter mais informações, consulte <xref:System.Diagnostics.EventLogInstaller>.

## <a name="see-also"></a>Veja também

- [Aplicativos de serviço do Windows](index.md)
- [Introdução aos aplicativos de serviço do Windows](introduction-to-windows-service-applications.md)
- [Como: depurar aplicativos de serviço do Windows](how-to-debug-windows-service-applications.md)
- [Serviços (Windows)](/windows/desktop/Services/services)
