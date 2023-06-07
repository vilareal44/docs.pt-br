---
title: Hospedando um aplicativo de serviço do Windows
ms.date: 03/30/2017
ms.assetid: f4199998-27f3-4dd9-aee4-0a4addfa9f24
ms.openlocfilehash: ba49d123508ceb8da677d1e9c67721e4f86aa7c3
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597326"
---
# <a name="hosting-in-a-windows-service-application"></a>Hospedando um aplicativo de serviço do Windows
Os serviços do Windows (anteriormente conhecidos como serviços do Windows NT) fornecem um modelo de processo especialmente adequado para aplicativos que devem residir em um executável de execução demorada e não exibir nenhuma forma de interface do usuário. O tempo de vida do processo de um aplicativo de serviço do Windows é gerenciado pelo SCM (Gerenciador de controle de serviço), que permite iniciar, parar e pausar aplicativos de serviço do Windows. Você pode configurar um processo de serviço do Windows para iniciar automaticamente quando o computador for iniciado, tornando-o um ambiente de hospedagem adequado para aplicativos "Always on". Para obter mais informações sobre aplicativos de serviço do Windows, consulte [aplicativos de serviço do Windows](https://go.microsoft.com/fwlink/?LinkId=89450).  
  
 Os aplicativos que hospedam serviços de Windows Communication Foundation de longa execução (WCF) compartilham muitas características com os serviços do Windows. Em particular, os serviços WCF são executáveis de servidor de longa execução que não interagem diretamente com o usuário e, portanto, não implementam nenhuma forma de interface do usuário. Dessa forma, hospedar serviços WCF dentro de um aplicativo de serviço do Windows é uma opção para criar aplicativos WCF robustos e de longa execução.  
  
 Geralmente, os desenvolvedores do WCF devem decidir se desejam hospedar seu aplicativo WCF dentro de um aplicativo de serviço do Windows ou dentro do ambiente de hospedagem Serviços de Informações da Internet (IIS) ou serviço de ativação de processos do Windows (WAS). Você deve considerar o uso de aplicativos de serviço do Windows sob as seguintes condições:  
  
- Seu aplicativo requer ativação explícita. Por exemplo, você deve usar os serviços do Windows quando seu aplicativo deve ser iniciado automaticamente quando o servidor é iniciado em vez de ser iniciado dinamicamente em resposta à primeira mensagem de entrada.  
  
- O processo que hospeda seu aplicativo deve permanecer em execução uma vez iniciado. Depois de iniciado, um processo de serviço do Windows permanece em execução, a menos que seja desligado explicitamente por um administrador de servidor usando o Gerenciador de controle de serviço. Os aplicativos hospedados no IIS ou WAS podem ser iniciados e interrompidos dinamicamente para fazer uso ideal dos recursos do sistema. Os aplicativos que exigem controle explícito durante o tempo de vida de seu processo de hospedagem devem usar os serviços do Windows em vez do IIS ou WAS.  
  
- Seu serviço WCF deve ser executado no Windows Server 2003 e usar transportes diferentes de HTTP. No Windows Server 2003, o ambiente de hospedagem do IIS 6,0 é restrito apenas à comunicação HTTP. Os aplicativos de serviço do Windows não estão sujeitos a essa restrição e podem usar qualquer suporte ao WCF de transporte, incluindo net. TCP, net. pipe e net. MSMQ.  
  
### <a name="to-host-wcf-inside-of-a-windows-service-application"></a>Para hospedar o WCF dentro de um aplicativo de serviço do Windows  
  
1. Crie um aplicativo de serviço do Windows. Você pode escrever aplicativos de serviço do Windows em código gerenciado usando as classes no <xref:System.ServiceProcess> namespace. Esse aplicativo deve incluir uma classe que herda de <xref:System.ServiceProcess.ServiceBase> .  
  
2. Vincule o tempo de vida dos serviços WCF ao tempo de vida do aplicativo de serviço do Windows. Normalmente, você deseja que os serviços WCF hospedados em um aplicativo de serviço do Windows se tornem ativos quando o serviço de hospedagem é iniciado, interrompa a escuta de mensagens quando o serviço de hospedagem é interrompido e desligue o processo de hospedagem quando o serviço WCF encontra um erro. Isso pode ser feito da seguinte maneira:  
  
    - Substitua <xref:System.ServiceProcess.ServiceBase.OnStart%28System.String%5B%5D%29> para abrir uma ou mais instâncias do <xref:System.ServiceModel.ServiceHost> . Um único aplicativo de serviço do Windows pode hospedar vários serviços WCF que iniciam e param como um grupo.  
  
    - Substitua <xref:System.ServiceProcess.ServiceBase.OnStop%2A> para chamar <xref:System.ServiceModel.Channels.CommunicationObject.Closed> em <xref:System.ServiceModel.ServiceHost> todos os serviços WCF em execução que foram iniciados durante <xref:System.ServiceProcess.ServiceBase.OnStart%28System.String%5B%5D%29> .  
  
    - Assine o <xref:System.ServiceModel.Channels.CommunicationObject.Faulted> evento de <xref:System.ServiceModel.ServiceHost> e use a <xref:System.ServiceProcess.ServiceController> classe para desligar o aplicativo de serviço do Windows em caso de erro.  
  
     Os aplicativos de serviço do Windows que hospedam serviços WCF são implantados e gerenciados da mesma maneira que os aplicativos de serviço do Windows que não usam o WCF.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.ServiceProcess>
- [Passo a passo: criando um aplicativo de Serviço Windows no Designer de componentes](https://go.microsoft.com/fwlink/?LinkId=94875)
- [Como hospedar um serviço WCF em um serviço Windows gerenciado](how-to-host-a-wcf-service-in-a-managed-windows-service.md)
- [Host de serviço do Windows](../samples/windows-service-host.md)
- [Arquitetura de programação do aplicativo de serviço](https://go.microsoft.com/fwlink/?LinkId=94876)
- [Recursos de hospedagem do Windows Server AppFabric](https://docs.microsoft.com/previous-versions/appfabric/ee677189(v=azure.10))
