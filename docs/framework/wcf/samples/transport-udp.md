---
title: 'Transporte: UDP'
ms.date: 03/30/2017
ms.assetid: 738705de-ad3e-40e0-b363-90305bddb140
ms.openlocfilehash: 44e47dd2d291ffc27d1777a04b645d57984919cd
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84591430"
---
# <a name="transport-udp"></a>Transporte: UDP
O exemplo de transporte UDP demonstra como implementar UDP unicast e multicast como um transporte de Windows Communication Foundation (WCF) personalizado. O exemplo descreve o procedimento recomendado para criar um transporte personalizado no WCF, usando a estrutura de canal e as práticas recomendadas do WCF a seguir. As etapas para criar um transporte personalizado são as seguintes:  
  
1. Decida qual dos padrões de [troca de mensagens](#MessageExchangePatterns) do canal (IOutputChannel, IInputChannel, IDuplexChannel, IRequestChannel ou IReplyChannel) a ChannelFactory e a ChannelListener terão suporte. Em seguida, decida se você dará suporte às variações de sessão dessas interfaces.  
  
2. Crie uma fábrica de canais e um ouvinte que ofereçam suporte ao seu padrão de troca de mensagens.  
  
3. Certifique-se de que qualquer exceção específica de rede seja normalizada para a classe derivada apropriada do <xref:System.ServiceModel.CommunicationException> .  
  
4. Adicione um [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) elemento que adiciona o transporte personalizado a uma pilha de canais. Para obter mais informações, consulte [adicionando um elemento de associação](#AddingABindingElement).  
  
5. Adicione uma seção de extensão de elemento de associação para expor o novo elemento de associação ao sistema de configuração.  
  
6. Adicione extensões de metadados para comunicar recursos a outros pontos de extremidade.  
  
7. Adicione uma associação que predefine uma pilha de elementos de associação de acordo com um perfil bem definido. Para obter mais informações, consulte [adicionando uma associação padrão](#AddingAStandardBinding).  
  
8. Adicione uma seção de associação e um elemento de configuração de associação para expor a associação ao sistema de configuração. Para obter mais informações, consulte [adicionando suporte à configuração](#AddingConfigurationSupport).  
  
<a name="MessageExchangePatterns"></a>
## <a name="message-exchange-patterns"></a>Padrões de troca de mensagens  
 A primeira etapa na gravação de um transporte personalizado é decidir quais padrões de troca de mensagens (MEPs) são necessários para o transporte. Há três MEPs de escolha:  
  
- Datagrama (IInputChannel/IOutputChannel)  
  
     Ao usar um dataMEP de datagrama, um cliente envia uma mensagem usando uma troca "disparar e esquecer". Um incêndio e esquecido o Exchange é um que requer a confirmação fora de banda da entrega bem-sucedida. A mensagem pode ser perdida em trânsito e nunca alcançar o serviço. Se a operação de envio for concluída com êxito na extremidade do cliente, ela não garante que o ponto de extremidade remoto tenha recebido a mensagem. O datagrama é um bloco de construção fundamental para mensagens, pois você pode criar seus próprios protocolos sobre ele, incluindo protocolos confiáveis e protocolos seguros. Canais de datagrama de cliente implementam a <xref:System.ServiceModel.Channels.IOutputChannel> interface e os canais de datagrama de serviço implementam a <xref:System.ServiceModel.Channels.IInputChannel> interface.  
  
- Solicitação-resposta (IRequestChannel/IReplyChannel)  
  
     Neste MEP, uma mensagem é enviada e uma resposta é recebida. O padrão consiste em pares de solicitação-resposta. Exemplos de chamadas de solicitação-resposta são RPC (chamadas de procedimento remoto) e o navegador obtém. Esse padrão também é conhecido como Half-duplex. Neste MEP, os canais de cliente implementam o <xref:System.ServiceModel.Channels.IRequestChannel> e os canais de serviço implementam <xref:System.ServiceModel.Channels.IReplyChannel> .  
  
- Duplex (IDuplexChannel)  
  
     O MEP duplex permite que um número arbitrário de mensagens seja enviado por um cliente e recebido em qualquer ordem. O duplex MEP é como uma conversa telefônica, onde cada palavra que está sendo falada é uma mensagem. Como ambos os lados podem enviar e receber neste MEP, a interface implementada pelos canais de cliente e de serviço é <xref:System.ServiceModel.Channels.IDuplexChannel> .  
  
 Cada um desses MEPs também pode dar suporte a sessões. A funcionalidade adicionada fornecida por um canal com reconhecimento de sessão é que ela correlaciona todas as mensagens enviadas e recebidas em um canal. O padrão de solicitação-resposta é uma sessão autônoma de duas mensagens, pois a solicitação e a resposta são correlacionadas. Por outro lado, o padrão de solicitação-resposta que dá suporte a sessões implica que todos os pares de solicitação/resposta nesse canal estão correlacionados entre si. Isso oferece um total de seis MEPs – datagrama, solicitação-resposta, duplex, datagrama com sessões, solicitação-resposta com sessões e duplex com sessões – para escolher.  
  
> [!NOTE]
> Para o transporte UDP, o único MEP com suporte é datagrama, porque o UDP é inerentemente a um protocolo "Fire and esqueça".  
  
### <a name="the-icommunicationobject-and-the-wcf-object-lifecycle"></a>O ICommunicationObject e o ciclo de vida do objeto do WCF  
 O WCF tem um computador de Estado comum que é usado para gerenciar o ciclo de vida de objetos como <xref:System.ServiceModel.Channels.IChannel> , <xref:System.ServiceModel.Channels.IChannelFactory> e <xref:System.ServiceModel.Channels.IChannelListener> que são usados para comunicação. Há cinco Estados em que esses objetos de comunicação podem existir. Esses Estados são representados pela <xref:System.ServiceModel.CommunicationState> enumeração e são os seguintes:  
  
- Criado: esse é o estado de um <xref:System.ServiceModel.ICommunicationObject> quando ele é instanciado pela primeira vez. Nenhuma e/s (entrada/saída) ocorre nesse estado.  
  
- Abrindo: a transição de objetos para esse estado quando <xref:System.ServiceModel.ICommunicationObject.Open%2A> é chamado. Neste ponto, as propriedades se tornam imutáveis e a entrada/saída pode começar. Essa transição é válida somente a partir do estado criado.  
  
- Aberto: os objetos são transferidos para esse estado quando o processo aberto é concluído. Essa transição é válida somente a partir do estado de abertura. Neste ponto, o objeto é totalmente utilizável para transferência.  
  
- Fechamento: a transição de objetos para esse estado quando <xref:System.ServiceModel.ICommunicationObject.Close%2A> é chamado para um desligamento normal. Essa transição é válida somente a partir do estado aberto.  
  
- Fechado: nos objetos de estado fechado não são mais utilizáveis. Em geral, a maioria das configurações ainda é acessível para inspeção, mas nenhuma comunicação pode ocorrer. Esse estado é equivalente a ser Descartado.  
  
- Com falha: no estado com falha, os objetos são acessíveis para inspeção, mas não podem mais ser usados. Quando ocorre um erro não recuperável, o objeto faz a transição para esse estado. A única transição válida desse Estado está no `Closed` estado.  
  
 Há eventos que são acionados para cada transição de estado. O <xref:System.ServiceModel.ICommunicationObject.Abort%2A> método pode ser chamado a qualquer momento e faz com que o objeto faça a transição imediatamente do seu estado atual para o estado fechado. Chamar <xref:System.ServiceModel.ICommunicationObject.Abort%2A> encerra qualquer trabalho não concluído.  
  
<a name="ChannelAndChannelListener"></a>
## <a name="channel-factory-and-channel-listener"></a>Fábrica de canais e ouvinte de canal  
 A próxima etapa na gravação de um transporte personalizado é criar uma implementação do <xref:System.ServiceModel.Channels.IChannelFactory> para canais de cliente e do <xref:System.ServiceModel.Channels.IChannelListener> para canais de serviço. A camada de canal usa um padrão de fábrica para construir canais. O WCF fornece auxiliares de classe base para esse processo.  
  
- A <xref:System.ServiceModel.Channels.CommunicationObject> classe implementa <xref:System.ServiceModel.ICommunicationObject> e impõe a máquina de estado descrita anteriormente na etapa 2.

- A <xref:System.ServiceModel.Channels.ChannelManagerBase> classe implementa <xref:System.ServiceModel.Channels.CommunicationObject> e fornece uma classe base unificada para o <xref:System.ServiceModel.Channels.ChannelFactoryBase> e o <xref:System.ServiceModel.Channels.ChannelListenerBase> . A <xref:System.ServiceModel.Channels.ChannelManagerBase> classe funciona em conjunto com <xref:System.ServiceModel.Channels.ChannelBase> , que é uma classe base que implementa <xref:System.ServiceModel.Channels.IChannel> .  
  
- A <xref:System.ServiceModel.Channels.ChannelFactoryBase> classe implementa <xref:System.ServiceModel.Channels.ChannelManagerBase> e <xref:System.ServiceModel.Channels.IChannelFactory> consolida as `CreateChannel` sobrecargas em um `OnCreateChannel` método abstrato.  
  
- A <xref:System.ServiceModel.Channels.ChannelListenerBase> classe implementa <xref:System.ServiceModel.Channels.IChannelListener> . Ele cuida do gerenciamento de estado básico.  
  
 Neste exemplo, a implementação de fábrica está contida em UdpChannelFactory.cs e a implementação do ouvinte está contida em UdpChannelListener.cs. As <xref:System.ServiceModel.Channels.IChannel> implementações estão em UdpOutputChannel.cs e UdpInputChannel.cs.  
  
### <a name="the-udp-channel-factory"></a>A fábrica de canais UDP  
 O `UdpChannelFactory` deriva de <xref:System.ServiceModel.Channels.ChannelFactoryBase> . As substituições de exemplo <xref:System.ServiceModel.Channels.ChannelFactoryBase.GetProperty%2A> para fornecer acesso à versão de mensagem do codificador de mensagem. O exemplo também substitui <xref:System.ServiceModel.Channels.ChannelFactoryBase.OnClose%2A> para que possamos subdividir nossa instância de <xref:System.ServiceModel.Channels.BufferManager> quando a máquina de estado faz a transição.  
  
#### <a name="the-udp-output-channel"></a>O canal de saída UDP  
 O `UdpOutputChannel` implementa <xref:System.ServiceModel.Channels.IOutputChannel> . O Construtor valida os argumentos e constrói um objeto de destino <xref:System.Net.EndPoint> com base no <xref:System.ServiceModel.EndpointAddress> que é passado.  
  
```csharp
this.socket = new Socket(this.remoteEndPoint.AddressFamily, SocketType.Dgram, ProtocolType.Udp);  
```  
  
 O canal pode ser fechado de forma normal ou inadequada. Se o canal for fechado normalmente, o Soquete será fechado e uma chamada será feita ao método da classe base `OnClose` . Se isso lançar uma exceção, a infraestrutura chamará `Abort` para garantir que o canal seja limpo.  
  
```csharp
this.socket.Close(0);  
```  
  
 Em seguida, implementamos `Send()` e `BeginSend()` / `EndSend()` . Isso é dividido em duas seções principais. Primeiro, nós serializamos a mensagem em uma matriz de bytes.  
  
```csharp
ArraySegment<byte> messageBuffer = EncodeMessage(message);  
```  
  
 Em seguida, enviamos os dados resultantes na conexão.  
  
```csharp
this.socket.SendTo(messageBuffer.Array, messageBuffer.Offset, messageBuffer.Count, SocketFlags.None, this.remoteEndPoint);  
```  
  
### <a name="the-udpchannellistener"></a>O UdpChannelListener  
 O `UdpChannelListener` que o exemplo implementa deriva da <xref:System.ServiceModel.Channels.ChannelListenerBase> classe. Ele usa um único soquete UDP para receber datagramas. O `OnOpen` método recebe dados usando o soquete UDP em um loop assíncrono. Os dados são então convertidos em mensagens usando a estrutura de codificação de mensagens.  
  
```csharp
message = MessageEncoderFactory.Encoder.ReadMessage(new ArraySegment<byte>(buffer, 0, count), bufferManager);  
```  
  
 Como o mesmo canal de datagrama representa mensagens que chegam de um número de fontes, o `UdpChannelListener` é um ouvinte singleton. Há, no máximo, um ativo <xref:System.ServiceModel.Channels.IChannel> associado a esse ouvinte por vez. O exemplo gera outro somente se um canal retornado pelo `AcceptChannel` método for descartado posteriormente. Quando uma mensagem é recebida, ela é enfileirada nesse canal singleton.  
  
#### <a name="udpinputchannel"></a>UdpInputChannel  
 A `UdpInputChannel` classe implementa `IInputChannel` . Ele consiste em uma fila de mensagens de entrada que é preenchida pelo `UdpChannelListener` soquete do. Essas mensagens são removidas da fila pelo `IInputChannel.Receive` método.  
  
<a name="AddingABindingElement"></a>
## <a name="adding-a-binding-element"></a>Adicionando um elemento de associação  
 Agora que as fábricas e os canais são criados, devemos expô-los ao tempo de execução de ServiceModel por meio de uma associação. Uma associação é uma coleção de elementos de associação que representa a pilha de comunicação associada a um endereço de serviço. Cada elemento na pilha é representado por um [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) elemento.  
  
 No exemplo, o elemento de associação é `UdpTransportBindingElement` , que deriva de <xref:System.ServiceModel.Channels.TransportBindingElement> . Ele substitui os métodos a seguir para criar as fábricas associadas à nossa associação.  
  
```csharp
public IChannelFactory<TChannel> BuildChannelFactory<TChannel>(BindingContext context)  
{  
    return (IChannelFactory<TChannel>)(object)new UdpChannelFactory(this, context);  
}  
  
public IChannelListener<TChannel> BuildChannelListener<TChannel>(BindingContext context)  
{  
    return (IChannelListener<TChannel>)(object)new UdpChannelListener(this, context);  
}  
```  
  
 Ele também contém membros para clonar `BindingElement` e retornar nosso esquema (SOAP. UDP).  
  
## <a name="adding-metadata-support-for-a-transport-binding-element"></a>Adicionando suporte de metadados para um elemento de associação de transporte  
 Para integrar nosso transporte ao sistema de metadados, devemos dar suporte à importação e à exportação da política. Isso nos permite gerar clientes de nossa ligação por meio da [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md).  
  
### <a name="adding-wsdl-support"></a>Adicionando suporte a WSDL  
 O elemento de associação de transporte em uma associação é responsável por exportar e importar informações de endereçamento nos metadados. Ao usar uma associação SOAP, o elemento de associação de transporte também deve exportar um URI de transporte correto nos metadados.  
  
#### <a name="wsdl-export"></a>Exportação de WSDL  
 Para exportar informações de endereçamento, o `UdpTransportBindingElement` implementa a `IWsdlExportExtension` interface. O `ExportEndpoint` método adiciona as informações de endereçamento corretas à porta WSDL.  
  
```csharp
if (context.WsdlPort != null)  
{  
    AddAddressToWsdlPort(context.WsdlPort, context.Endpoint.Address, encodingBindingElement.MessageVersion.Addressing);  
}  
```  
  
 A `UdpTransportBindingElement` implementação do `ExportEndpoint` método também exporta um URI de transporte quando o ponto de extremidade usa uma associação SOAP.  
  
```csharp
WsdlNS.SoapBinding soapBinding = GetSoapBinding(context, exporter);  
if (soapBinding != null)  
{  
    soapBinding.Transport = UdpPolicyStrings.UdpNamespace;  
}  
```  
  
#### <a name="wsdl-import"></a>Importação de WSDL  
 Para estender o sistema de importação WSDL para lidar com a importação dos endereços, devemos adicionar a configuração a seguir ao arquivo de configuração para SvcUtil. exe, conforme mostrado no arquivo svcutil. exe. config.  
  
```xml
<configuration>  
  <system.serviceModel>  
    <client>  
      <metadata>  
        <policyImporters>  
          <extension type=" Microsoft.ServiceModel.Samples.UdpBindingElementImporter, UdpTransport" />  
        </policyImporters>  
      </metadata>  
    </client>  
  </system.serviceModel>  
</configuration>  
```  
  
 Ao executar svcutil. exe, há duas opções para obter svcutil. exe para carregar as extensões de importação WSDL:  
  
1. Aponte para o SvcUtil. exe para nosso arquivo de configuração usando o/SvcutilConfig: \<file> .  
  
2. Adicione a seção de configuração a svcutil. exe. config no mesmo diretório que svcutil. exe.  
  
 O `UdpBindingElementImporter` tipo implementa a `IWsdlImportExtension` interface. O `ImportEndpoint` método importa o endereço da porta WSDL.  
  
```csharp
BindingElementCollection bindingElements = context.Endpoint.Binding.CreateBindingElements();  
TransportBindingElement transportBindingElement = bindingElements.Find<TransportBindingElement>();  
if (transportBindingElement is UdpTransportBindingElement)  
{  
    ImportAddress(context);  
}  
```  
  
### <a name="adding-policy-support"></a>Adicionando suporte à política  
 O elemento de associação personalizado pode exportar declarações de política na associação WSDL para um ponto de extremidade de serviço para expressar os recursos desse elemento de associação.  
  
#### <a name="policy-export"></a>Exportação de política  
 O `UdpTransportBindingElement` tipo implementa `IPolicyExportExtension` para adicionar suporte para exportar a política. Como resultado, `System.ServiceModel.MetadataExporter` o inclui `UdpTransportBindingElement` na geração de política para qualquer associação que a inclua.  
  
 No `IPolicyExportExtension.ExportPolicy` , adicionamos uma asserção para UDP e outra asserção se estivermos no modo multicast. Isso ocorre porque o modo multicast afeta a forma como a pilha de comunicação é construída e, portanto, deve ser coordenado entre ambos os lados.  
  
```csharp
ICollection<XmlElement> bindingAssertions = context.GetBindingAssertions();  
XmlDocument xmlDocument = new XmlDocument();  
bindingAssertions.Add(xmlDocument.CreateElement(  
UdpPolicyStrings.Prefix, UdpPolicyStrings.TransportAssertion, UdpPolicyStrings.UdpNamespace));  
if (Multicast)  
{  
    bindingAssertions.Add(xmlDocument.CreateElement(
        UdpPolicyStrings.Prefix,
        UdpPolicyStrings.MulticastAssertion,
        UdpPolicyStrings.UdpNamespace));  
}  
```  
  
 Como os elementos de associação de transporte personalizados são responsáveis pelo tratamento de endereçamento, a `IPolicyExportExtension` implementação no `UdpTransportBindingElement` também deve lidar com a exportação das asserções de política apropriadas do WS-Addressing para indicar a versão do WS-Addressing que está sendo usada.  
  
```csharp
AddWSAddressingAssertion(context, encodingBindingElement.MessageVersion.Addressing);  
```  
  
#### <a name="policy-import"></a>Importação de política  
 Para estender o sistema de importação de política, devemos adicionar a configuração a seguir ao arquivo de configuração para SvcUtil. exe, conforme mostrado no arquivo svcutil. exe. config.  
  
```xml
<configuration>  
  <system.serviceModel>  
    <client>  
      <metadata>  
        <policyImporters>  
          <extension type=" Microsoft.ServiceModel.Samples.UdpBindingElementImporter, UdpTransport" />  
        </policyImporters>  
      </metadata>  
    </client>  
  </system.serviceModel>  
</configuration>  
```  
  
 Em seguida, implementamos a `IPolicyImporterExtension` partir de nossa classe registrada ( `UdpBindingElementImporter` ). No `ImportPolicy()` , examinamos as asserções em nosso namespace e processamos aquelas para gerar o transporte e verificar se ele é multicast. Também devemos remover as asserções que manipulamos da lista de asserções de associação. Novamente, ao executar svcutil. exe, há duas opções de integração:  
  
1. Aponte para o SvcUtil. exe para nosso arquivo de configuração usando o/SvcutilConfig: \<file> .  
  
2. Adicione a seção de configuração a svcutil. exe. config no mesmo diretório que svcutil. exe.  
  
<a name="AddingAStandardBinding"></a>
## <a name="adding-a-standard-binding"></a>Adicionando uma associação padrão  
 Nosso elemento Binding pode ser usado das duas maneiras a seguir:  
  
- Por meio de uma associação personalizada: uma associação personalizada permite que o usuário crie sua própria associação com base em um conjunto arbitrário de elementos de ligação.  
  
- Usando uma associação fornecida pelo sistema que inclui nosso elemento de ligação. O WCF fornece várias dessas associações definidas pelo sistema, como `BasicHttpBinding` , `NetTcpBinding` e `WsHttpBinding` . Cada uma dessas associações está associada a um perfil bem definido.  
  
 O exemplo implementa a associação de perfil no `SampleProfileUdpBinding` , que deriva de <xref:System.ServiceModel.Channels.Binding> . O `SampleProfileUdpBinding` contém até quatro elementos de ligação dentro dele: `UdpTransportBindingElement` , `TextMessageEncodingBindingElement CompositeDuplexBindingElement` e `ReliableSessionBindingElement` .  
  
```csharp
public override BindingElementCollection CreateBindingElements()  
{
    BindingElementCollection bindingElements = new BindingElementCollection();  
    if (ReliableSessionEnabled)  
    {  
        bindingElements.Add(session);  
        bindingElements.Add(compositeDuplex);  
    }  
    bindingElements.Add(encoding);  
    bindingElements.Add(transport);  
    return bindingElements.Clone();  
}  
```  
  
### <a name="adding-a-custom-standard-binding-importer"></a>Adicionando um importador de associação padrão personalizado  
 O SvcUtil. exe e o `WsdlImporter` tipo, por padrão, reconhecem e importam associações definidas pelo sistema. Caso contrário, a associação será importada como uma `CustomBinding` instância. Para habilitar svcutil. exe e o `WsdlImporter` para importar o `SampleProfileUdpBinding` `UdpBindingElementImporter` também atua como um importador de associação padrão personalizado.  
  
 Um importador de associação padrão personalizado implementa o `ImportEndpoint` método na `IWsdlImportExtension` interface para examinar a `CustomBinding` instância importada de metadados para ver se ela pode ter sido gerada por uma associação padrão específica.  
  
```csharp
if (context.Endpoint.Binding is CustomBinding)  
{  
    Binding binding;  
    if (transportBindingElement is UdpTransportBindingElement)  
    {  
        //if TryCreate is true, the CustomBinding will be replace by a SampleProfileUdpBinding in the  
        //generated config file for better typed generation.  
        if (SampleProfileUdpBinding.TryCreate(bindingElements, out binding))  
        {  
            binding.Name = context.Endpoint.Binding.Name;  
            binding.Namespace = context.Endpoint.Binding.Namespace;  
            context.Endpoint.Binding = binding;  
        }  
    }  
}  
```  
  
 Em geral, a implementação de um importador de associação padrão personalizado envolve a verificação das propriedades dos elementos de associação importados para verificar se apenas as propriedades que poderiam ter sido definidas pela associação padrão foram alteradas e todas as outras propriedades são seus padrões. Uma estratégia básica para implementar um importador de associação padrão é criar uma instância da associação padrão, propagar as propriedades dos elementos de associação para a instância de associação padrão à qual a associação padrão dá suporte e comparar os elementos de associação da associação padrão com os elementos de associação importados.  
  
<a name="AddingConfigurationSupport"></a>
## <a name="adding-configuration-support"></a>Adicionando suporte à configuração  
 Para expor nosso transporte por meio da configuração, devemos implementar duas seções de configuração. O primeiro é um `BindingElementExtensionElement` para `UdpTransportBindingElement` . Isso é para que as `CustomBinding` implementações possam referenciar nosso elemento de ligação. A segunda é uma `Configuration` para nossa `SampleProfileUdpBinding` .  
  
### <a name="binding-element-extension-element"></a>Elemento de extensão do elemento de associação  
 A seção `UdpTransportElement` é um `BindingElementExtensionElement` que expõe `UdpTransportBindingElement` para o sistema de configuração. Com algumas substituições básicas, definimos nosso nome de seção de configuração, o tipo de nosso elemento de ligação e como criar nosso elemento de ligação. Em seguida, podemos registrar nossa seção de extensão em um arquivo de configuração, conforme mostrado no código a seguir.  
  
```xml
<configuration>  
  <system.serviceModel>  
    <extensions>  
      <bindingElementExtensions>  
        <add name="udpTransport" type="Microsoft.ServiceModel.Samples.UdpTransportElement, UdpTransport" />  
      </bindingElementExtensions>  
    </extensions>  
  </system.serviceModel>  
</configuration>  
```  
  
 A extensão pode ser referenciada de associações personalizadas para usar UDP como o transporte.  
  
```xml
<configuration>  
  <system.serviceModel>  
    <bindings>  
      <customBinding>  
       <binding configurationName="UdpCustomBinding">  
         <udpTransport/>  
       </binding>  
      </customBinding>  
    </bindings>  
  </system.serviceModel>  
</configuration>  
```  
  
### <a name="binding-section"></a>Seção de associação  
 A seção `SampleProfileUdpBindingCollectionElement` é um `StandardBindingCollectionElement` que expõe `SampleProfileUdpBinding` para o sistema de configuração. A maior parte da implementação é delegada para o `SampleProfileUdpBindingConfigurationElement` , que deriva de `StandardBindingElement` . O `SampleProfileUdpBindingConfigurationElement` tem propriedades que correspondem às propriedades em `SampleProfileUdpBinding` e às funções a serem mapeadas da `ConfigurationElement` associação. Por fim, substitua o `OnApplyConfiguration` método em nosso `SampleProfileUdpBinding` , conforme mostrado no código de exemplo a seguir.  
  
```csharp
protected override void OnApplyConfiguration(string configurationName)  
{  
    if (binding == null)
        throw new ArgumentNullException("binding");

    if (binding.GetType() != typeof(SampleProfileUdpBinding))
    {
        throw new ArgumentException(string.Format(CultureInfo.CurrentCulture,
            "Invalid type for binding. Expected type: {0}. Type passed in: {1}.",
            typeof(SampleProfileUdpBinding).AssemblyQualifiedName,
            binding.GetType().AssemblyQualifiedName));
    }
    SampleProfileUdpBinding udpBinding = (SampleProfileUdpBinding)binding;

    udpBinding.OrderedSession = this.OrderedSession;
    udpBinding.ReliableSessionEnabled = this.ReliableSessionEnabled;
    udpBinding.SessionInactivityTimeout = this.SessionInactivityTimeout;
    if (this.ClientBaseAddress != null)
        udpBinding.ClientBaseAddress = ClientBaseAddress;
}
```  
  
 Para registrar esse manipulador com o sistema de configuração, adicionamos a seção a seguir ao arquivo de configuração relevante.  
  
```xml
<configuration>  
  <configSections>  
     <sectionGroup name="system.serviceModel">  
        <sectionGroup name="bindings">  
          <section name="sampleProfileUdpBinding" type="Microsoft.ServiceModel.Samples.SampleProfileUdpBindingCollectionElement, UdpTransport" />  
        </sectionGroup>  
     </sectionGroup>  
  </configSections>  
</configuration>  
```  
  
 Em seguida, ele pode ser referenciado na seção de configuração de serviceModel.  
  
```xml
<configuration>  
  <system.serviceModel>  
    <client>  
      <endpoint configurationName="calculator"  
                address="soap.udp://localhost:8001/"
                bindingConfiguration="CalculatorServer"  
                binding="sampleProfileUdpBinding"
                contract= "Microsoft.ServiceModel.Samples.ICalculatorContract">  
      </endpoint>  
    </client>  
  </system.serviceModel>  
</configuration>  
```  
  
## <a name="the-udp-test-service-and-client"></a>O cliente e o serviço de teste UDP  
 O código de teste para usar esse transporte de exemplo está disponível nos diretórios UdpTestService e UdpTestClient. O código de serviço consiste em dois testes – um teste define associações e pontos de extremidade do código e o outro faz isso por meio da configuração. Ambos os testes usam dois pontos de extremidade. Um ponto de extremidade usa o `SampleUdpProfileBinding` com [\<reliableSession>](https://docs.microsoft.com/previous-versions/ms731375(v=vs.90)) definido como `true` . O outro ponto de extremidade usa uma associação personalizada com `UdpTransportBindingElement` . Isso é equivalente a usar `SampleUdpProfileBinding` with [\<reliableSession>](https://docs.microsoft.com/previous-versions/ms731375(v=vs.90)) definido como `false` . Ambos os testes criam um serviço, adicionam um ponto de extremidade para cada associação, abrem o serviço e aguardam que o usuário pressione ENTER antes de fechar o serviço.  
  
 Ao iniciar o aplicativo de teste de serviço, você deverá ver a saída a seguir.  
  
```console
Testing Udp From Code.  
Service is started from code...  
Press <ENTER> to terminate the service and start service from config...  
```  
  
 Em seguida, você pode executar o aplicativo cliente de teste nos pontos de extremidade publicados. O aplicativo de teste do cliente cria um cliente para cada ponto de extremidade e envia cinco mensagens para cada ponto de extremidade. A saída a seguir está no cliente.  
  
```console
Testing Udp From Imported Files Generated By SvcUtil.  
0  
3  
6  
9  
12  
Press <ENTER> to complete test.  
```  
  
 A seguir está a saída completa do serviço.  
  
```console
Service is started from code...  
Press <ENTER> to terminate the service and start service from config...  
Hello, world!  
Hello, world!  
Hello, world!  
Hello, world!  
Hello, world!  
   adding 0 + 0  
   adding 1 + 2  
   adding 2 + 4  
   adding 3 + 6  
   adding 4 + 8  
```  
  
 Para executar o aplicativo cliente em pontos de extremidade publicados usando a configuração, pressione ENTER no serviço e execute o cliente de teste novamente. Você deverá ver a seguinte saída no serviço.  
  
```console
Testing Udp From Config.  
Service is started from config...  
Press <ENTER> to terminate the service and exit...  
```  
  
 Executar o cliente novamente produz o mesmo resultado dos resultados anteriores.  
  
 Para regenerar o código do cliente e a configuração usando svcutil. exe, inicie o aplicativo de serviço e execute o SvcUtil. exe a seguir do diretório raiz do exemplo.  
  
```console
svcutil http://localhost:8000/udpsample/ /reference:UdpTransport\bin\UdpTransport.dll /svcutilConfig:svcutil.exe.config  
```  
  
 Observe que svcutil. exe não gera a configuração de extensão de associação para o `SampleProfileUdpBinding` , portanto, você deve adicioná-lo manualmente.  
  
```xml
<configuration>  
  <system.serviceModel>
    <extensions>  
      <!-- This was added manually because svcutil.exe does not add this extension to the file -->  
      <bindingExtensions>  
        <add name="sampleProfileUdpBinding" type="Microsoft.ServiceModel.Samples.SampleProfileUdpBindingCollectionElement, UdpTransport" />  
      </bindingExtensions>  
    </extensions>  
  </system.serviceModel>  
</configuration>  
```  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Para compilar a solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
2. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
  
3. Consulte a seção "serviço de teste e cliente de UDP" anterior.  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Transport\Udp`
