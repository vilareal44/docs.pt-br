---
title: 'Como: Depurar aplicativos do serviço Windows'
description: Entenda como depurar aplicativos de serviço do Windows, que não são tão simples de depurar quanto outros tipos de aplicativos do Visual Studio.
ms.date: 03/30/2017
helpviewer_keywords:
- debugging Windows Service applications
- debugging [Visual Studio], Windows services
- Windows NT services, debugging
- Windows Service applications, debugging
- services, debugging
ms.assetid: 63ab0800-0f05-4f1e-88e6-94c73fd920a2
author: ghogen
ms.openlocfilehash: fb58f2ff4f480347f0f233ecd9a619cf287cfdfd
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86925755"
---
# <a name="how-to-debug-windows-service-applications"></a>Como: Depurar aplicativos do serviço Windows
Um serviço deve ser executado de dentro do contexto do Gerenciador de Controle de Serviços, em vez de dentro do Visual Studio. Por esse motivo, depurar um serviço não é tão simples como depurar outros tipos de aplicativos do Visual Studio. Para depurar um serviço, você deve iniciar o serviço e, em seguida, anexar um depurador ao processo no qual ele está sendo executado. Em seguida, você pode depurar seu aplicativo usando todas as funcionalidades de depuração padrão do Visual Studio.  
  
> [!CAUTION]
> Você não deve anexá-lo a um processo, a menos que você saiba qual é o processo e entenda as consequências da anexação, possivelmente eliminando esse processo. Por exemplo, se você anexá-lo ao processo WinLogon e interromper a depuração, o sistema será interrompido porque ele não pode operar sem o WinLogon.  
  
 Você pode anexar o depurador somente a um serviço em execução. O processo de anexação interrompe o funcionamento atual do seu serviço. Na verdade, ele não para ou pausa o processamento do serviço. Ou seja, se o serviço está sendo executado quando você inicia a depuração, ele ainda está tecnicamente num estado inicial conforme você depura, mas seu processamento foi suspenso.  
  
 Depois de anexá-lo ao processo, você pode definir pontos de interrupção e usá-los para depurar seu código. Depois que sair da caixa de diálogo usada para anexá-lo ao processo, você estará efetivamente em modo de depuração. Você pode usar o Gerenciador de Controle de Serviços para iniciar, parar, pausar e continuar seu serviço, alcançando, assim, os pontos de interrupção que você definiu. Posteriormente, você pode remover esse serviço fictício depois de depuração ser concluída com êxito.  
  
 Este artigo aborda a depuração de um serviço que está em execução no computador local, mas você também pode depurar serviços Windows que executados em um computador remoto. Confira [Depuração remota](/visualstudio/debugger/debug-installed-app-package).  
  
> [!NOTE]
> Depurar o método <xref:System.ServiceProcess.ServiceBase.OnStart%2A> pode ser difícil porque o Gerenciador de controle de Serviços impõe um limite de 30 segundos em todas as tentativas de iniciar um serviço. Para obter mais informações, confira [Solução de problemas: depuração de Serviços Windows](troubleshooting-debugging-windows-services.md).  
  
> [!WARNING]
> Para obter informações significativas para depuração, o depurador do Visual Studio deve localizar os arquivos de símbolo para os binários que estão sendo depurados. Se você estiver depurando um serviço que você criou no Visual Studio, os arquivos de símbolo (arquivos .pdb) estarão na mesma pasta que o executável ou biblioteca, e o depurador os carregará automaticamente. Se você estiver depurando um serviço que não criou, deve primeiro localizar símbolos para o serviço e certificar-se de que eles podem ser encontrados pelo depurador. Confira [Especificar arquivos de símbolo (.pdb) e de origem no Depurador do Visual Studio](/visualstudio/debugger/specify-symbol-dot-pdb-and-source-files-in-the-visual-studio-debugger). Se você estiver depurando um processo do sistema ou se desejar ter símbolos para chamadas do sistema em seus serviços, você deve adicionar os Servidores de Símbolo Microsoft. Confira [Depurando símbolos](/windows/desktop/DxTechArts/debugging-with-symbols).  
  
### <a name="to-debug-a-service"></a>Para depurar um serviço  
  
1. Crie seu serviço na configuração de depuração.  
  
2. Instalar o serviço. Para obter mais informações, confira [Como instalar e desinstalar serviços](how-to-install-and-uninstall-services.md).  
  
3. Inicie o serviço, usando o **Gerenciador de Controle de Serviços**, o **Gerenciador de Servidores** ou o código. Para obter mais informações, confira [Como iniciar serviços](how-to-start-services.md).  
  
4. Inicie o Visual Studio com credenciais administrativas para que você possa se conectar aos processos do sistema.  
  
5. (Opcional) Na barra de menus do Visual Studio, escolha **Ferramentas**, **Opções**. Na caixa de diálogo **Opções**, escolha **Depuração**, **Símbolos**, marque a caixa de seleção **Servidores de Símbolos da Microsoft** e, em seguida, clique no botão **OK**.  
  
6. Na barra de menus, escolha **Anexar ao Processo** no menu **Depurar** ou **Ferramentas**. (Teclado: Ctrl+Alt+P)  
  
     A caixa de diálogo **Processos** é exibida.  
  
7. Marque a caixa de seleção **Mostrar processos de todos os usuários**.  
  
8. Na seção **Processos Disponíveis**, escolha o processo para o serviço e, em seguida, escolha **Anexar**.  
  
    > [!TIP]
    > O processo terá o mesmo nome que o arquivo executável do serviço.  
  
     A caixa de diálogo **Anexar ao Processo** é exibida.  
  
9. Escolha as opções apropriadas e, em seguida, clique em **OK** para fechar a caixa de diálogo.  
  
    > [!NOTE]
    > Agora você está no modo de depuração.  
  
10. Defina os pontos de interrupção que você deseja usar em seu código.  
  
11. Acesse o Gerenciador de Controle de Serviços e manuseie seu serviço, enviando comando de parar, pausar e continuar para atingir seus pontos de interrupção. Para obter mais informações de como executar o Gerenciador de Controle de Serviços, confira [Como iniciar serviços](how-to-start-services.md). Além disso, confira [Solução de problemas: depurando Serviços Windows](troubleshooting-debugging-windows-services.md).  
  
## <a name="debugging-tips-for-windows-services"></a>Dicas de depuração para serviços Windows  
 Anexar ao processo do serviço permite depurar a maior parte, mas não todo, do código para esse serviço. Por exemplo, como o serviço já foi iniciado, você não pode depurar o código no serviço do método <xref:System.ServiceProcess.ServiceBase.OnStart%2A> ou o código no método `Main` que é usado para carregar o serviço dessa maneira. Uma maneira de contornar essa limitação é criar um segundo serviço temporário no seu aplicativo de serviço que existe somente para ajudar na depuração. Você pode instalar os dois serviços e, em seguida, iniciar o serviço fictício para carregar o processo do serviço. Depois que o serviço temporário começar o processo, você poderá usar o menu **Depurar** do Visual Studio para anexar ao processo do serviço.  
  
 Tente adicionar chamadas para o método <xref:System.Threading.Thread.Sleep%2A> para atrasar a ação até que você possa anexar o processo.  
  
 Tente alterar o programa para um aplicativo de console regular. Para fazer isso, reescreva o método `Main` da seguinte maneira para que ele possa ser executado como um serviço Windows e como um aplicativo de console, dependendo de como for iniciado.  
  
#### <a name="how-to-run-a-windows-service-as-a-console-application"></a>Como executar um serviço Windows como um aplicativo do console  
  
1. Adicione um método para o serviço que executa os métodos <xref:System.ServiceProcess.ServiceBase.OnStart%2A> e <xref:System.ServiceProcess.ServiceBase.OnStop%2A>:  
  
    ```csharp  
    internal void TestStartupAndStop(string[] args)  
    {  
        this.OnStart(args);  
        Console.ReadLine();  
        this.OnStop();  
    }  
    ```  
  
2. Reescreva o método `Main` da seguinte maneira:  
  
    ```csharp  
    static void Main(string[] args)  
    {  
        if (Environment.UserInteractive)  
        {  
            MyNewService service1 = new MyNewService(args);  
            service1.TestStartupAndStop(args);  
        }  
        else  
        {  
            // Put the body of your old Main method here.  
        }  
    }
    ```  
  
3. Na guia **Aplicativo** das propriedades do projeto, defina o **Tipo de saída** para **Aplicativo de Console**.  
  
4. Escolha **Iniciar Depuração** (F5).  
  
5. Para executar novamente o programa como um serviço Windows, instale-o e inicie-o como de costume para um serviço Windows. Não é necessário reverter essas alterações.  
  
 Em alguns casos, como quando você deseja depurar um problema ocorre apenas na inicialização do sistema, é necessário usar o depurador do Windows. [Baixe o Windows Driver Kit (WDK)](/windows-hardware/drivers/download-the-wdk) e consulte [Como depurar serviços Windows](https://support.microsoft.com/kb/824344).  
  
## <a name="see-also"></a>Veja também

- [Introdução a aplicativos do Serviço Windows](introduction-to-windows-service-applications.md)
- [Como: instalar e desinstalar serviços](how-to-install-and-uninstall-services.md)
- [Como: Iniciar serviços](how-to-start-services.md)
- [Depurar um serviço](/windows/desktop/Services/debugging-a-service)
