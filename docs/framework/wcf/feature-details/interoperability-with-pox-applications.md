---
title: Interoperabilidade com aplicativos POX
ms.date: 03/30/2017
ms.assetid: 449276b8-4633-46f0-85c9-81f01d127636
ms.openlocfilehash: 64a6d850a32b14bc60cd43466e04b53a7a39be81
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84579261"
---
# <a name="interoperability-with-pox-applications"></a>Interoperabilidade com aplicativos POX

Os aplicativos de "POX (XML antigo") se comunicam por meio da troca de mensagens HTTP brutas que contêm apenas dados de aplicativos XML que não estão incluídos em um envelope SOAP. O Windows Communication Foundation (WCF) pode fornecer serviços e clientes que usam mensagens POX. No serviço, o WCF pode ser usado para implementar serviços que exponham pontos de extremidade para clientes como navegadores da Web e linguagens de script que enviam e recebem mensagens POX. No cliente, o modelo de programação do WCF pode ser usado para implementar clientes que se comunicam com serviços baseados em POX.  
  
> [!NOTE]
> Este documento foi escrito originalmente para o .NET Framework 3,0.  .NET Framework 3,5 tem suporte interno para trabalhar com aplicativos POX. Para obter mais informações sobre como consultar o [modelo de programação do WCF Web http](wcf-web-http-programming-model.md).
  
## <a name="pox-programming-with-wcf"></a>Programação POX com WCF

Os serviços WCF que se comunicam por HTTP usando mensagens POX usam um [\<customBinding>](../../configure-apps/file-schema/wcf/custombinding.md) .

```xml
<customBinding>
   <binding name="poxServerBinding">
       <textMessageEncoding messageVersion="None" />
       <httpTransport />
   </binding>
</customBinding>
```

Essa associação personalizada contém dois elementos:

- [\<httpTransport>](../../configure-apps/file-schema/wcf/httptransport.md)

- [\<textMessageEncoding>](../../configure-apps/file-schema/wcf/textmessageencoding.md)

O codificador de mensagem de texto do WCF padrão é especialmente configurado para usar o <xref:System.ServiceModel.Channels.MessageVersion.None%2A> valor, que permite processar cargas de mensagens XML que não chegam encapsuladas em um envelope SOAP.

Os clientes WCF que se comunicam por HTTP usando mensagens POX usam uma associação semelhante (mostrada no código imperativo a seguir).

```csharp
private static Binding CreatePoxBinding()
{
    TextMessageEncodingBindingElement encoder =
        new TextMessageEncodingBindingElement( MessageVersion.None, Encoding.UTF8 );
    HttpTransportBindingElement transport = new HttpTransportBindingElement();
    transport.ManualAddressing = true;
    return new CustomBinding( new BindingElement[] { encoder, transport } );
}
```

Como os clientes POX devem especificar explicitamente os URIs para os quais eles enviam mensagens, eles geralmente devem configurar o <xref:System.ServiceModel.Channels.HttpTransportBindingElement> para um modo de endereçamento manual, definindo a <xref:System.ServiceModel.Channels.TransportBindingElement.ManualAddressing%2A> propriedade como `true` no elemento. Isso permite que as mensagens sejam resolvidas explicitamente pelo código do aplicativo e não é necessário criar uma nova <xref:System.ServiceModel.ChannelFactory> toda vez que um aplicativo envia uma mensagem para um URI http diferente.

Como as mensagens POX não usam cabeçalhos SOAP para transmitir informações importantes de protocolo, os clientes POX e os serviços geralmente devem manipular partes da solicitação HTTP subjacente usada para enviar ou receber uma mensagem. As informações de protocolo específicas de HTTP, como os cabeçalhos HTTP e os códigos de status, são apresentadas no modelo de programação do WCF por meio de duas classes:

- <xref:System.ServiceModel.Channels.HttpRequestMessageProperty>, que contém informações sobre a solicitação HTTP, como o método HTTP e cabeçalhos de solicitação.

- <xref:System.ServiceModel.Channels.HttpResponseMessageProperty>, que contém informações sobre a resposta HTTP, como o código de status HTTP e a descrição do status, bem como quaisquer cabeçalhos de resposta HTTP.
  
O exemplo de código a seguir mostra como criar uma mensagem de solicitação HTTP GET que é endereçada ao `http://localhost:8100/customers` .

```csharp
Message request = Message.CreateMessage( MessageVersion.None, String.Empty );
request.Headers.To = "http://localhost:8100/customers";

HttpRequestMessageProperty property = new HttpRequestMessageProperty();
property.Method = "GET";
property.SuppressEntityBody = true;
request.Properties.Add( HttpRequestMessageProperty.Name, property );
```

Primeiro, uma solicitação vazia <xref:System.ServiceModel.Channels.Message> é criada chamando <xref:System.ServiceModel.Channels.Message.CreateMessage%28System.ServiceModel.Channels.MessageVersion%2CSystem.String%29> . O <xref:System.ServiceModel.Channels.MessageVersion.None%2A> parâmetro é usado para indicar que um envelope SOAP não é necessário e <xref:System.String.Empty> o parâmetro é passado como a ação. Em seguida, a mensagem de solicitação é resolvida definindo <xref:System.ServiceModel.Channels.MessageHeaders.To%2A> o cabeçalho como o URI desejado. Em seguida, um <xref:System.ServiceModel.Channels.HttpRequestMessageProperty> é criado e o <xref:System.ServiceModel.Channels.HttpRequestMessageProperty.Method%2A> é definido como o método Get do verbo HTTP e o <xref:System.ServiceModel.Channels.HttpRequestMessageProperty.SuppressEntityBody%2A> é definido como `true` para indicar que nenhum dado deve ser enviado no corpo da mensagem de solicitação HTTP de saída. Por fim, a propriedade Request é adicionada à <xref:System.ServiceModel.Channels.Message.Properties%2A> coleção da mensagem de solicitação para que ela possa influenciar como o transporte http envia a solicitação. A mensagem está pronta para ser enviada por uma instância apropriada do <xref:System.ServiceModel.Channels.IRequestChannel> .

Técnicas semelhantes podem ser usadas no serviço para extrair o <xref:System.ServiceModel.Channels.HttpRequestMessageProperty> de uma mensagem de entrada e construir uma resposta.
