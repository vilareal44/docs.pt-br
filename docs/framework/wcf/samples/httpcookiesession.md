---
title: HttpCookieSession
ms.date: 03/30/2017
ms.assetid: 101cb624-8303-448a-a3af-933247c1e109
ms.openlocfilehash: 8dba147ace7ada221b5d274cd233e4b9618835d9
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84585124"
---
# <a name="httpcookiesession"></a>HttpCookieSession
Este exemplo demonstra como criar um canal de protocolo personalizado para usar cookies HTTP para gerenciamento de sessão. Esse canal permite a comunicação entre os serviços de Windows Communication Foundation (WCF) e os clientes ASMX ou entre clientes WCF e serviços ASMX.  
  
 Quando um cliente chama um método Web em um serviço Web ASMX baseado em sessão, o mecanismo ASP.NET faz o seguinte:  
  
- Gera uma ID exclusiva (ID de sessão).  
  
- Gera o objeto de sessão e o associa com a ID exclusiva.  
  
- Adiciona a ID exclusiva a um cabeçalho de resposta HTTP Set-cookie e a envia ao cliente.  
  
- Identifica o cliente em chamadas subsequentes com base na ID de sessão que ele envia a ele.  
  
 O cliente inclui essa ID de sessão em suas solicitações subsequentes para o servidor. O servidor usa a ID de sessão do cliente para carregar o objeto de sessão apropriado para o contexto HTTP atual.  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Channels\HttpCookieSession`  
  
## <a name="httpcookiesession-channel-message-exchange-pattern"></a>Padrão de troca de mensagens do canal HttpCookieSession  
 Este exemplo habilita sessões para cenários semelhantes a ASMX. Na parte inferior da pilha do canal, temos o transporte HTTP que dá suporte a <xref:System.ServiceModel.Channels.IRequestChannel> e <xref:System.ServiceModel.Channels.IReplyChannel> . É o trabalho do canal para fornecer sessões para os níveis mais altos da pilha de canais. O exemplo implementa dois canais ( <xref:System.ServiceModel.Channels.IRequestSessionChannel> e <xref:System.ServiceModel.Channels.IReplySessionChannel> ) que dão suporte a sessões.  
  
## <a name="service-channel"></a>Canal de serviço  
 O exemplo fornece um canal de serviço na `HttpCookieReplySessionChannelListener` classe. Essa classe implementa a <xref:System.ServiceModel.Channels.IChannelListener> interface e converte o <xref:System.ServiceModel.Channels.IReplyChannel> canal de baixo na pilha de canais para um <xref:System.ServiceModel.Channels.IReplySessionChannel> . Esse processo pode ser dividido nas seguintes partes:  
  
- Quando o ouvinte de canal é aberto, ele aceita um canal interno de seu ouvinte interno. Como o ouvinte interno é um ouvinte de datagrama e o tempo de vida de um canal aceito é dissociado do tempo de vida do ouvinte, podemos fechar o ouvinte interno e manter apenas o canal interno  
  
    ```csharp  
                this.innerChannelListener.Open(timeoutHelper.RemainingTime());  
    this.innerChannel = this.innerChannelListener.AcceptChannel(timeoutHelper.RemainingTime());  
    this.innerChannel.Open(timeoutHelper.RemainingTime());  
    this.innerChannelListener.Close(timeoutHelper.RemainingTime());  
    ```  
  
- Quando o processo aberto é concluído, configuramos um loop de mensagem para receber mensagens do canal interno.  
  
    ```csharp  
    IAsyncResult result = BeginInnerReceiveRequest();  
    if (result != null && result.CompletedSynchronously)  
    {  
       // do not block the user thread  
       this.completeReceiveCallback ??= new WaitCallback(CompleteReceiveCallback);
       ThreadPool.QueueUserWorkItem(this.completeReceiveCallback, result);  
    }  
    ```  
  
- Quando uma mensagem chega, o canal de serviço examina o identificador de sessão e os demultiplexs para o canal de sessão apropriado. O ouvinte de canal mantém um dicionário que mapeia os identificadores de sessão para as instâncias de canal de sessão.  
  
    ```csharp  
    Dictionary<string, IReplySessionChannel> channelMapping;  
    ```  
  
 A `HttpCookieReplySessionChannel` classe implementa <xref:System.ServiceModel.Channels.IReplySessionChannel> . Níveis mais altos da pilha de canal chamam o <xref:System.ServiceModel.Channels.IReplyChannel.ReceiveRequest%2A> método para ler solicitações para esta sessão. Cada canal de sessão tem uma fila de mensagens privada que é populada pelo canal de serviço.  
  
```csharp  
InputQueue<RequestContext> requestQueue;  
```  
  
 No caso em que alguém chama o <xref:System.ServiceModel.Channels.IReplyChannel.ReceiveRequest%2A> método e não há mensagens na fila de mensagens, o canal aguarda um período de tempo especificado antes de desligá-lo. Isso limpa os canais de sessão criados para clientes não WCF.  
  
 Usamos o `channelMapping` para acompanhar o `ReplySessionChannels` e não fechamos nosso subjacente `innerChannel` até que todos os canais aceitos tenham sido fechados. Dessa forma, `HttpCookieReplySessionChannel` pode existir além do tempo de vida de `HttpCookieReplySessionChannelListener` . Também não precisamos nos preocupar com o ouvinte que está recebendo o lixo para baixo, pois os canais aceitos mantêm uma referência ao ouvinte por meio do `OnClosed` retorno de chamada.  
  
## <a name="client-channel"></a>Canal de cliente  
 O canal do cliente correspondente está na `HttpCookieSessionChannelFactory` classe. Durante a criação do canal, a fábrica de canais encapsula o canal de solicitação interno com um `HttpCookieRequestSessionChannel` . A `HttpCookieRequestSessionChannel` classe encaminha as chamadas para o canal de solicitação subjacente. Quando o cliente fecha o proxy, o `HttpCookieRequestSessionChannel` envia uma mensagem para o serviço que indica que o canal está sendo fechado. Assim, a pilha de canais de serviço pode desligar normalmente o canal de sessão que está em uso.  
  
## <a name="binding-and-binding-element"></a>Elemento Binding e Binding  
 Depois de criar os canais de serviço e cliente, a próxima etapa é integrá-los ao tempo de execução do WCF. Os canais são expostos ao WCF por meio de associações e elementos de associação. Uma associação consiste em um ou vários elementos de associação. O WCF oferece várias associações definidas pelo sistema; por exemplo, BasicHttpBinding ou WSHttpBinding. A `HttpCookieSessionBindingElement` classe contém a implementação para o elemento de associação. Ele substitui o ouvinte do canal e os métodos de criação de fábrica de canal para fazer as instanciações necessárias do canal ou do ouvinte de canal.  
  
 O exemplo usa declarações de política para a descrição do serviço. Isso permite que o exemplo publique seus requisitos de canal para outros clientes que podem consumir o serviço. Por exemplo, esse elemento de associação publica declarações de política para permitir que clientes potenciais saibam que ele dá suporte a sessões. Como o exemplo habilita a `ExchangeTerminateMessage` Propriedade na configuração do elemento de associação, ele adiciona as declarações necessárias para mostrar que o serviço dá suporte a uma ação de troca de mensagens extra para encerrar a conversa da sessão. Os clientes podem usar essa ação. O código WSDL a seguir mostra as declarações de política criadas a partir do `HttpCookieSessionBindingElement` .  
  
```xml  
<wsp:Policy wsu:Id="HttpCookieSessionBinding_IWcfCookieSessionService_policy" xmlns:wsp="http://schemas.xmlsoap.org/ws/2004/09/policy" xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">  
<wsp:ExactlyOne>  
<wsp:All>  
<wspe:Utf816FFFECharacterEncoding xmlns:wspe="http://schemas.xmlsoap.org/ws/2004/09/policy/encoding"/>  
<mhsc:httpSessionCookie xmlns:mhsc="http://samples.microsoft.com/wcf/mhsc/policy"/>  
</wsp:All>  
</wsp:ExactlyOne>  
</wsp:Policy>  
```  
  
 A `HttpCookieSessionBinding` classe é uma associação fornecida pelo sistema que usa o elemento de associação descrito anteriormente.  
  
## <a name="adding-the-channel-to-the-configuration-system"></a>Adicionando o canal ao sistema de configuração  
 O exemplo fornece duas classes que expõem o canal de exemplo por meio da configuração. O primeiro é um <xref:System.ServiceModel.Configuration.BindingElementExtensionElement> para o `HttpCookieSessionBindingElement` . A maior parte da implementação é delegada para o `HttpCookieSessionBindingConfigurationElement` , que deriva de <xref:System.ServiceModel.Configuration.StandardBindingElement> . O `HttpCookieSessionBindingConfigurationElement` tem propriedades que correspondem às propriedades em `HttpCookieSessionBindingElement` .  
  
### <a name="binding-element-extension-section"></a>Seção de extensão do elemento de associação  
 A seção `HttpCookieSessionBindingElementSection` é um <xref:System.ServiceModel.Configuration.BindingElementExtensionElement> que expõe `HttpCookieSessionBindingElement` para o sistema de configuração. Com algumas substituições do nome da seção de configuração, o tipo do elemento de associação e como criar o elemento de associação são definidos. Em seguida, podemos registrar a seção de extensão em um arquivo de configuração da seguinte maneira:  
  
```xml  
<configuration>
    <system.serviceModel>
      <extensions>
        <bindingElementExtensions>
          <add name="httpCookieSession"
               type=  
"Microsoft.ServiceModel.Samples.HttpCookieSessionBindingElementElement,
                    HttpCookieSessionExtension, Version=1.0.0.0,
                    Culture=neutral, PublicKeyToken=null"/>  
        </bindingElementExtensions >  
      </extensions>  
  
      <bindings>  
      <customBinding>  
        <binding name="allowCookiesBinding">  
          <textMessageEncoding messageVersion="Soap11WSAddressing10" />  
          <httpCookieSession sessionTimeout="10" exchangeTerminateMessage="true" />  
          <httpTransport allowCookies="true" />  
        </binding>  
      </customBinding>  
      </bindings>
    </system.serviceModel>
</configuration>  
```  
  
## <a name="test-code"></a>Código de teste  
 O código de teste para usar esse transporte de exemplo está disponível nos diretórios de cliente e serviço. Ele consiste em dois testes: um teste usa uma associação com `allowCookies` definido como `true` no cliente. O segundo teste habilita o desligamento explícito (usando a troca de mensagens de encerramento) na associação.  
  
 Ao executar o exemplo, você deverá ver a seguinte saída:  
  
```console  
Simple binding:  
AddItem(10000,2): ItemCount=2  
AddItem(10550,5): ItemCount=7  
RemoveItem(10550,2): ItemCount=5  
Items  
10000, 2  
10550, 3  
Smart binding:  
AddItem(10000,2): ItemCount=2  
AddItem(10550,5): ItemCount=7  
RemoveItem(10550,2): ItemCount=5  
Items  
10000, 2  
10550, 3  
  
Press <ENTER> to terminate client.  
```  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Instale o ASP.NET 4,0 usando o comando a seguir.  
  
    ```console  
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable  
    ```  
  
2. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
3. Para compilar a solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
4. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
