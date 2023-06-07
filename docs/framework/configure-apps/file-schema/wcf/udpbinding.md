---
title: <udpBinding>
ms.date: 03/30/2017
ms.assetid: fa291901-8340-45c6-9c44-5d9281c70bc3
ms.openlocfilehash: 7fa72d233d6489ab6a2c534f69c66a55a22d0f59
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "74429836"
---
# \<udpBinding>
Um elemento de configuração usado para configurar a <xref:System.ServiceModel.UdpBinding> associação.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<udpBinding>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<udpBinding>
  <binding closeTimeout="TimeSpan"
           duplicateMessageHistoryLength="Integer"
           maxBufferPoolSize="Integer"
           maxBufferSize="Integer"
           maxPendingMessagesTotalSize="Integer"
           maxReceivedMessageSize="Integer"
           maxRetransmitCount="Integer"
           multicastInterfaceId="Integer"
           name="String"
           openTimeout="TimeSpan"
           receiveTimeout="TimeSpan"
           sendTimeout="TimeSpan"
           textEncoding="UnicodeFffeTextEncoding/Utf16TextEncoding/Utf8TextEncoding"
           timeToLive="TimeSpan">
    <readerQuotas maxArrayLength="Integer"
                  maxBytesPerRead="Integer"
                  maxDepth="Integer"
                  maxNameTableCharCount="Integer"
                  maxStringContentLength="Integer" />
  </binding>
</basicHttpBinding>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`closeTimeout`|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de fechamento. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|  
|`duplicateMessageHistoryLength`|Um valor inteiro que especifica o comprimento do histórico de mensagens duplicado.|  
|`maxBufferPoolSize`|Um valor inteiro que especifica a quantidade máxima de memória alocada para uso pelo Gerenciador dos buffers de mensagens que recebem mensagens do canal. O valor padrão é 524288 (0x80000) bytes.|  
|`maxBufferSize`|Um valor inteiro que especifica o tamanho máximo, em bytes, de um buffer que armazena mensagens enquanto elas são processadas para um ponto de extremidade configurado com essa associação. O valor padrão é 65.536 bytes.|  
|`maxPendingMessagesTotalSize`|Um valor inteiro que especifica o número máximo de mensagens que são recebidas, mas ainda não foram removidas da fila de entrada para uma instância de canal individual.|  
|`maxReceivedMessageSize`|Um inteiro positivo que define o tamanho máximo da mensagem, em bytes, incluindo cabeçalhos, para uma mensagem que pode ser recebida em um canal configurado com essa associação. O remetente receberá uma falha de SOAP se a mensagem for muito grande para o destinatário. O receptor remove a mensagem e cria uma entrada do evento no log de rastreamento. O padrão é de 65.536 bytes.|  
|`maxRetransmitCount`|Um valor inteiro que especifica o número máximo de mensagens de retransmissão.|  
|`multicastInterfaceId`|Um valor inteiro que especifica a ID da interface multicast.|  
|`name`|Uma cadeia de caracteres que contém o nome da configuração da associação. Esse valor deve ser exclusivo porque é usado como uma identificação para a associação. A partir do .NET Framework 4, associações e comportamentos não precisam ter um nome. Para obter mais informações sobre configurações padrão e associações e comportamentos do sem nome, consulte [configuração simplificada](../../../wcf/simplified-configuration.md) e [configuração simplificada para serviços WCF](../../../wcf/samples/simplified-configuration-for-wcf-services.md).|  
|`openTimeout`|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de abertura. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|  
|`receiveTimeout`|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de recebimento. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:10:00.|  
|`sendTimeout`|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de envio. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|  
|`textEncoding`|Define a codificação do conjunto de caracteres a ser usada para emitir mensagens na associação. Os valores válidos incluem os seguintes:<br /><br /> -BigEndianUnicode: codificação BigEndian Unicode.<br />-Unicode: codificação de 16 bits.<br />-UTF8: codificação de 8 bits<br /><br /> O padrão é UTF8. Esse atributo é do tipo <xref:System.Text.Encoding> .|  
|`timeToLive`|Um valor TimeSpan que especifica a vida útil para a associação.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<readerQuotas>](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms731325(v=vs.100))|Define as restrições sobre a complexidade de mensagens SOAP que podem ser processadas por pontos de extremidade configurados com essa associação. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.XmlDictionaryReaderQuotasElement> .|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<bindings>](bindings.md)|Esse elemento contém uma coleção de associações padrão e personalizadas.|  
  
## <a name="remarks"></a>Comentários  
 O UdpBinding permite que os serviços WCF se comuniquem por meio do transporte UDP. Ele permite trocas de mensagens de "incêndio e esquecido" onde um cliente envia uma mensagem a um serviço e não espera resposta.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como configurar o <xref:System.ServiceModel.UdpBinding> usando o `udpBinding` elemento <>.  
  
```xml  
<udpBinding>
  <binding  closeTimeout="00:10:00"
            duplicateMessageHistoryLength="100"
            maxBufferPoolSize="100"
            maxPendingMessagesTotalSize="100000"
            maxReceivedMessageSize="65536"
            maxRetransmitCount="10"
            multicastInterfaceId="00000"
            name="myUdpBinding"
            openTimeout="00:10:00"
            receiveTimeout="00:10:00"
            sendTimeout="00:10:00"
            textEncoding="utf-8"
            timeToLive="00:10:00">
    <readerQuotas />
  </binding>
</udpBinding>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Channels.Binding>
- <xref:System.ServiceModel.Channels.BindingElement>
- <xref:System.ServiceModel.BasicHttpBinding>
- <xref:System.ServiceModel.Configuration.BasicHttpBindingElement>
- [Associações](../../../wcf/bindings.md)
- [Configurando associações fornecidas pelo sistema](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [Usando associações para configurar serviços e clientes](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
