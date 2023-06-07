---
title: Ativação de UDP
ms.date: 03/30/2017
ms.assetid: 4b0ccd10-0dfb-4603-93f9-f0857c581cb7
ms.openlocfilehash: 13d20524693b234a14b2b31061c6259f75b1c0b8
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84591096"
---
# <a name="udp-activation"></a>Ativação de UDP
Este exemplo é baseado na amostra [Transport: UDP](transport-udp.md) . Ele estende a amostra [Transport: UDP](transport-udp.md) para dar suporte à ativação do processo usando o WAS (serviço de ativação de processos do Windows).  
  
 O exemplo consiste em três partes principais:  
  
- Um UDP Protocol Activator, um processo autônomo que recebe mensagens UDP em nome dos aplicativos que devem ser ativados.  
  
- Um cliente que usa o transporte personalizado UDP para enviar mensagens.  
  
- Um serviço (hospedado em um processo de trabalho ativado pelo WAS) que recebe mensagens sobre o transporte personalizado UDP.  
  
## <a name="udp-protocol-activator"></a>Ativador de protocolo UDP  
 O UDP Protocol Activator é uma ponte entre o cliente WCF e o serviço WCF. Ele fornece comunicação de dados por meio do protocolo UDP na camada de transporte. Ele tem duas funções principais:  
  
- ERA o adaptador de escuta (LA), que colabora com o WAS para ativar processos em resposta a mensagens recebidas.  
  
- Ouvinte de protocolo UDP, que aceita mensagens UDP em nome dos aplicativos que devem ser ativados.  
  
 O ativador deve estar sendo executado como um programa autônomo no computador do servidor. Normalmente, os adaptadores de escuta eram (como o NetTcpActivator e o NetPipeActivator) são implementados em serviços do Windows de longa execução. No entanto, para simplificar e clareza, este exemplo implementa o ativador de protocolo como um aplicativo autônomo.  
  
### <a name="was-listener-adapter"></a>FOI o adaptador de escuta  
 O adaptador de escuta WAS para UDP é implementado na `UdpListenerAdapter` classe. É o módulo que interage com o WAS para executar a ativação do aplicativo para o protocolo UDP. Isso é feito chamando as seguintes APIs Webhost:  
  
- `WebhostRegisterProtocol`  
  
- `WebhostUnregisterProtocol`  
  
- `WebhostOpenListenerChannelInstance`  
  
- `WebhostCloseAllListenerChannelInstances`  
  
 Depois de chamar inicialmente `WebhostRegisterProtocol` , o adaptador de escuta recebe o retorno de chamada `ApplicationCreated` do was para todos os aplicativos registrados em ApplicationHost. config (localizado em%windir%\system32\inetsrv.). Neste exemplo, tratamos apenas dos aplicativos com o protocolo UDP (com a ID de protocolo como "net. UDP") habilitado. Outras implementações podem lidar com isso de forma diferente se essas implementações responderem às alterações de configuração dinâmicas para o aplicativo (por exemplo, uma transição de aplicativo de desabilitado para habilitado).  
  
 Quando o retorno de chamada `ConfigManagerInitializationCompleted` é recebido, ele indica que o foi concluído todas as notificações para a inicialização do protocolo. Neste momento, o adaptador do ouvinte está pronto para processar solicitações de ativação.  
  
 Quando uma nova solicitação chega na primeira vez para um aplicativo, o adaptador do ouvinte chama o `WebhostOpenListenerChannelInstance` was, que inicia o processo de trabalho se ele ainda não foi iniciado. Em seguida, os manipuladores de protocolo são carregados e a comunicação entre o adaptador de escuta e o aplicativo virtual pode ser iniciada.  
  
 O adaptador de escuta é registrado no%SystemRoot%\System32\inetsrv\ApplicationHost.config na seção < `listenerAdapters` > da seguinte maneira:  
  
```xml  
<add name="net.udp" identity="S-1-5-21-2127521184-1604012920-1887927527-387045" />  
```  
  
### <a name="protocol-listener"></a>Ouvinte de protocolo  
 O ouvinte de protocolo UDP é um módulo dentro do ativador de protocolo que escuta em um ponto de extremidade UDP em nome do aplicativo virtual. Ele é implementado na classe `UdpSocketListener` . O ponto de extremidade é representado como `IPEndpoint` para o qual o número da porta é extraído da associação do protocolo para o site.  
  
### <a name="control-service"></a>Serviço de controle  
 Neste exemplo, usamos o WCF para se comunicar entre o ativador e o WAS processo de trabalho. O serviço que reside no ativador é chamado de serviço de controle.  
  
## <a name="protocol-handlers"></a>Manipuladores de protocolo  
 Depois que o adaptador de escuta chamar `WebhostOpenListenerChannelInstance` , o Gerenciador de processos do was iniciará o processo de trabalho se ele não for iniciado. O Gerenciador de aplicativos dentro do processo de trabalho então carrega o PPH (manipulador de protocolo de processo) do UDP com a solicitação para isso `ListenerChannelId` . O PPH em ativa chamadas `IAdphManager` .`StartAppDomainProtocolListenerChannel` para iniciar o manipulador de protocolo de AppDomain de UDP (ADPH).  
  
## <a name="hostedudptransportconfiguration"></a>HostedUDPTransportConfiguration  
 As informações são registradas no Web. config da seguinte maneira:  
  
```xml  
<serviceHostingEnvironment>  
<add name="net.udp" transportConfigurationType="Microsoft.ServiceModel.Samples.Hosting.HostedUdpTransportConfiguration, UdpActivation, Version=1.0.0.0, Culture=neutral, PublicKeyToken=6fa904d2da1848d6" />  
</serviceHostingEnvironment>  
```  
  
## <a name="special-setup-for-this-sample"></a>Configuração especial para este exemplo  
 Este exemplo só pode ser criado e executado no Windows Vista, no Windows Server 2008 ou no Windows 7. Para executar o exemplo, você deve primeiro obter todos os componentes configurados corretamente. Use as etapas a seguir para instalar o exemplo.  
  
#### <a name="to-set-up-this-sample"></a>Para configurar este exemplo  
  
1. Instale o ASP.NET 4,0 usando o comando a seguir.  
  
    ```console  
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable  
    ```  
  
2. Compile o projeto no Windows Vista. Após a compilação, ele também executa as seguintes operações na fase pós-compilação:  
  
    - Instala a associação UDP ao site "Default Web site".  
  
    - Cria o aplicativo virtual "ServiceModelSamples" para apontar para o caminho físico: "%SystemDrive%\inetpub\wwwroot\servicemodelsamples".  
  
    - Ele também habilita o protocolo "net. UDP" para este aplicativo virtual.  
  
3. Inicie o aplicativo de interface do usuário "WasNetActivator. exe". Clique na guia **instalação** , marque as seguintes caixas de seleção e clique em **instalar** para instalá-las:  
  
    - Adaptador de escuta UDP  
  
    - Manipuladores de protocolo UDP  
  
4. Clique na guia **ativação** do aplicativo de interface do usuário "WasNetActivator. exe". Clique no botão **Iniciar** para iniciar o adaptador do ouvinte. Agora você está pronto para executar o programa.  
  
    > [!NOTE]
    > Ao concluir este exemplo, você deve executar o Cleanup. bat para remover a associação net. UDP do "site padrão".  
  
## <a name="sample-usage"></a>Exemplo de uso  
 Após a compilação, há quatro binários diferentes gerados:  
  
- Client. exe: o código do cliente. O app. config é compilado no arquivo de configuração do cliente Client. exe. config.  
  
- UDPActivation. dll: a biblioteca que contém todas as principais implementações de UDP.  
  
- Service. dll: o código do serviço. Isso é copiado para o diretório \bin do aplicativo virtual ServiceModelSamples. O arquivo de serviço é Service. svc e o arquivo de configuração é Web. config. Após a compilação, eles são copiados para o seguinte local:%SystemDrive%\Inetpub\wwwroot\ServiceModelSamples.  
  
- WasNetActivator: o programa UDP Activator.  
  
- Verifique se todas as partes necessárias estão instaladas corretamente. As etapas a seguir mostram como executar o exemplo:  
  
1. Verifique se os seguintes serviços do Windows foram iniciados:  
  
    - WAS (serviço de ativação de processos do Windows).  
  
    - Serviços de Informações da Internet (IIS): W3SVC.  
  
2. Em seguida, inicie o ativador, WasNetActivator. exe. Na guia **ativação** , o único protocolo, **UDP**, é selecionado na lista suspensa. Clique no botão **Iniciar** para iniciar o ativador.  
  
3. Depois que o ativador for iniciado, você poderá executar o código do cliente executando o Client. exe em uma janela de comando. A seguir está a saída de exemplo:  
  
    ```console  
    Testing Udp Activation.  
    Start the status service.  
    Sending UDP datagrams.  
    Type a word that you want to say to the server: Hello, world!  
        Sending datagram: Hello, world![0]  
        Sending datagram: Hello, world![1]  
        Sending datagram: Hello, world![2]  
        Sending datagram: Hello, world![3]  
        Sending datagram: Hello, world![4]  
    Calling UDP duplex contract (ICalculatorContract).  
        0 + 0 = 0  
        1 + 2 = 3  
        2 + 4 = 6  
        3 + 6 = 9  
        4 + 8 = 12  
    Getting status and dump server traces:  
        Operation 'Hello' is called: Hello, world![0]  
        Operation 'Hello' is called: Hello, world![1]  
        Operation 'Hello' is called: Hello, world![2]  
        Operation 'Hello' is called: Hello, world![3]  
        Operation 'Hello' is called: Hello, world![4]  
        Operation 'Add' is called: 0 + 0  
        Operation 'Add' is called: 1 + 2  
        Operation 'Add' is called: 2 + 4  
        Operation 'Add' is called: 3 + 6  
        Operation 'Add' is called: 4 + 8  
    Press <ENTER> to complete test.  
    ```  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Transport\UdpActivation`  
