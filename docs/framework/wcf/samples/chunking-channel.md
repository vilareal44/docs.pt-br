---
title: Canal de agrupamento
ms.date: 03/30/2017
ms.assetid: e4d53379-b37c-4b19-8726-9cc914d5d39f
ms.openlocfilehash: 7a5e5292bcb37e83de21458716e34887a0557d91
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84585539"
---
# <a name="chunking-channel"></a>Canal de agrupamento

Ao enviar mensagens grandes usando Windows Communication Foundation (WCF), geralmente é desejável limitar a quantidade de memória usada para armazenar em buffer essas mensagens. Uma solução possível é transmitir o corpo da mensagem (supondo que a massa dos dados esteja no corpo). No entanto, alguns protocolos exigem o armazenamento em buffer de toda a mensagem. As mensagens e a segurança confiáveis são dois exemplos. Outra solução possível é dividir a mensagem grande em mensagens menores chamadas de partes, enviar as partes uma parte por vez e reconstituir a mensagem grande no lado de recebimento. O próprio aplicativo poderia fazer esse agrupamento e desagrupamento ou usar um canal personalizado para fazer isso. O exemplo de canal de agrupamento mostra como um protocolo personalizado ou canal em camadas pode ser usado para fazer agrupamentos e desagrupamento de mensagens arbitrariamente grandes.

O agrupamento deve ser sempre empregado somente depois que a mensagem inteira a ser enviada tiver sido construída. Um canal de agrupamento deve estar sempre em camadas abaixo de um canal de segurança e um canal de sessão confiável.

> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.

> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Channels\ChunkingChannel`

## <a name="chunking-channel-assumptions-and-limitations"></a>Trechos de considerações e limitações de canal

### <a name="message-structure"></a>Estrutura da mensagem

O canal de agrupamento assume a seguinte estrutura de mensagem para que as mensagens sejam fragmentadas:

```xml
<soap:Envelope>
  <!-- headers -->
  <soap:Body>
    <operationElement>
      <paramElement>data to be chunked</paramElement>
    </operationElement>
  </soap:Body>
</soap:Envelope>
```

Ao usar o ServiceModel, as operações de contrato que têm um parâmetro de entrada são compatíveis com essa forma de mensagem para sua mensagem de entrada. Da mesma forma, as operações de contrato que têm 1 parâmetro de saída ou valor de retorno estão em conformidade com essa forma de mensagem para sua mensagem de saída. Veja a seguir exemplos de tais operações:

```csharp
[ServiceContract]
interface ITestService
{
    [OperationContract]
    Stream EchoStream(Stream stream);

    [OperationContract]
    Stream DownloadStream();

    [OperationContract(IsOneWay = true)]
    void UploadStream(Stream stream);
}
```

### <a name="sessions"></a>Sessões

O canal de agrupamento exige que as mensagens sejam entregues exatamente uma vez, na entrega ordenada de mensagens (partes). Isso significa que a pilha de canais subjacente deve ser sessão. As sessões podem ser fornecidas pelo transporte (por exemplo, transporte TCP) ou por um canal de protocolo de sessão (por exemplo, canal ReliableSession).

### <a name="asynchronous-send-and-receive"></a>Envio e recebimento assíncronos

Os métodos de envio e recebimento assíncronos não são implementados nesta versão do exemplo de canal de agrupamento.

## <a name="chunking-protocol"></a>Protocolo de agrupamento

O canal de agrupamento define um protocolo que indica o início e o término de uma série de partes, bem como o número de sequência de cada parte. As três mensagens de exemplo a seguir demonstram as mensagens de início, bloco e fim com comentários que descrevem os principais aspectos de cada um.

### <a name="start-message"></a>Mensagem de início

```xml
<s:Envelope xmlns:a="http://www.w3.org/2005/08/addressing"
            xmlns:s="http://www.w3.org/2003/05/soap-envelope">
  <s:Header>
<!--Original message action is replaced with a chunking-specific action. -->
    <a:Action s:mustUnderstand="1">http://samples.microsoft.com/chunkingAction</a:Action>
<!--
Original message is assigned a unique id that is transmitted
in a MessageId header. Note that this is different from the WS-Addressing MessageId header.
-->
    <MessageId s:mustUnderstand="1" xmlns="http://samples.microsoft.com/chunking">
53f183ee-04aa-44a0-b8d3-e45224563109
</MessageId>
<!--
ChunkingStart header signals the start of a chunked message.
-->
    <ChunkingStart s:mustUnderstand="1" i:nil="true" xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://samples.microsoft.com/chunking" />
<!--
Original message action is transmitted in OriginalAction.
This is required to re-create the original message on the other side.
-->
    <OriginalAction xmlns="http://samples.microsoft.com/chunking">
http://tempuri.org/ITestService/EchoStream
    </OriginalAction>
   <!--
    All original message headers are included here.
   -->
  </s:Header>
  <s:Body>
<!--
Chunking assumes this structure of Body content:
<element>
  <childelement>large data to be chunked<childelement>
</element>
The start message contains just <element> and <childelement> without
the data to be chunked.
-->
    <EchoStream xmlns="http://tempuri.org/">
      <stream />
    </EchoStream>
  </s:Body>
</s:Envelope>
```

### <a name="chunk-message"></a>Mensagem de bloco

```xml
<s:Envelope
  xmlns:a="http://www.w3.org/2005/08/addressing"
  xmlns:s="http://www.w3.org/2003/05/soap-envelope">
  <s:Header>
   <!--
    All chunking protocol messages have this action.
   -->
    <a:Action s:mustUnderstand="1">
      http://samples.microsoft.com/chunkingAction
    </a:Action>
<!--
Same as MessageId in the start message. The GUID indicates which original message this chunk belongs to.
-->
    <MessageId s:mustUnderstand="1"
               xmlns="http://samples.microsoft.com/chunking">
      53f183ee-04aa-44a0-b8d3-e45224563109
    </MessageId>
<!--
The sequence number of the chunk.
This number restarts at 1 with each new sequence of chunks.
-->
    <ChunkNumber s:mustUnderstand="1"
                 xmlns="http://samples.microsoft.com/chunking">
      1096
    </ChunkNumber>
  </s:Header>
  <s:Body>
<!--
The chunked data is wrapped in a chunk element.
The encoding of this data (and the entire message)
depends on the encoder used. The chunking channel does not mandate an encoding.
-->
    <chunk xmlns="http://samples.microsoft.com/chunking">
kfSr2QcBlkHTvQ==
    </chunk>
  </s:Body>
</s:Envelope>
```

### <a name="end-message"></a>Mensagem final

```xml
<s:Envelope xmlns:a="http://www.w3.org/2005/08/addressing"
            xmlns:s="http://www.w3.org/2003/05/soap-envelope">
  <s:Header>
    <a:Action s:mustUnderstand="1">
      http://samples.microsoft.com/chunkingAction
    </a:Action>
<!--
Same as MessageId in the start message. The GUID indicates which original message this chunk belongs to.
-->
    <MessageId s:mustUnderstand="1"
               xmlns="http://samples.microsoft.com/chunking">
      53f183ee-04aa-44a0-b8d3-e45224563109
    </MessageId>
<!--
ChunkingEnd header signals the end of a chunk sequence.
-->
    <ChunkingEnd s:mustUnderstand="1" i:nil="true"
                 xmlns:i="http://www.w3.org/2001/XMLSchema-instance"
                 xmlns="http://samples.microsoft.com/chunking" />
<!--
ChunkingEnd messages have a sequence number.
-->
    <ChunkNumber s:mustUnderstand="1"
                 xmlns="http://samples.microsoft.com/chunking">
      79
    </ChunkNumber>
  </s:Header>
  <s:Body>
<!--
The ChunkingEnd message has the same <element><childelement> structure
as the ChunkingStart message.
-->
    <EchoStream xmlns="http://tempuri.org/">
      <stream />
    </EchoStream>
  </s:Body>
</s:Envelope>
```

## <a name="chunking-channel-architecture"></a>Arquitetura de canal de agrupamento

O canal de agrupamento é um `IDuplexSessionChannel` que, em um alto nível, segue a arquitetura de canal típica. Há um `ChunkingBindingElement` que pode criar um `ChunkingChannelFactory` e um `ChunkingChannelListener` . O `ChunkingChannelFactory` cria instâncias de `ChunkingChannel` quando é solicitado. O `ChunkingChannelListener` cria instâncias do `ChunkingChannel` quando um novo canal interno é aceito. O `ChunkingChannel` próprio é responsável por enviar e receber mensagens.

No próximo nível abaixo, o `ChunkingChannel` depende de vários componentes para implementar o protocolo de agrupamento. No lado do envio, o canal usa um <xref:System.Xml.XmlDictionaryWriter> chamado personalizado `ChunkingWriter` que faz o agrupamento real. `ChunkingWriter`usa o canal interno diretamente para enviar partes. O uso de um personalizado `XmlDictionaryWriter` nos permite enviar partes, já que o corpo grande da mensagem original está sendo gravado. Isso significa que não armazenamos em buffer toda a mensagem original.

![Diagrama que mostra a arquitetura de envio do canal de agrupamento.](./media/chunking-channel/chunking-channel-send.gif)

No lado do recebimento, `ChunkingChannel` o recebe as mensagens do canal interno e as passa para uma <xref:System.Xml.XmlDictionaryReader> chamada personalizada `ChunkingReader` , que reconstitui a mensagem original das partes de entrada. `ChunkingChannel`encapsula isso `ChunkingReader` em uma implementação personalizada `Message` chamada `ChunkingMessage` e retorna essa mensagem para a camada acima. Essa combinação de `ChunkingReader` e `ChunkingMessage` nos permite retirar a parte do corpo da mensagem original, pois ele está sendo lido pela camada acima, em vez de ter que armazenar em buffer todo o corpo da mensagem original. `ChunkingReader`tem uma fila na qual ele armazena em buffer as partes de entrada até um número máximo configurável de partes em buffer. Quando esse limite máximo é atingido, o leitor aguarda que as mensagens sejam descarregadas da fila pela camada acima (ou seja, apenas lendo do corpo da mensagem original) ou até que o tempo limite máximo de recebimento seja atingido.

![Diagrama que mostra a arquitetura de recebimento do canal de agrupamento.](./media/chunking-channel/chunking-channel-receive.gif)

## <a name="chunking-programming-model"></a>Modelo de programação de agrupamento

Os desenvolvedores de serviço podem especificar quais mensagens devem ser fragmentadas aplicando o `ChunkingBehavior` atributo às operações no contrato. O atributo expõe uma `AppliesTo` propriedade que permite ao desenvolvedor especificar se o agrupamento se aplica à mensagem de entrada, à mensagem de saída ou a ambas. O exemplo a seguir mostra o uso do `ChunkingBehavior` atributo:

```csharp
[ServiceContract]
interface ITestService
{
    [OperationContract]
    [ChunkingBehavior(ChunkingAppliesTo.Both)]
    Stream EchoStream(Stream stream);

    [OperationContract]
    [ChunkingBehavior(ChunkingAppliesTo.OutMessage)]
    Stream DownloadStream();

    [OperationContract(IsOneWay=true)]
    [ChunkingBehavior(ChunkingAppliesTo.InMessage)]
    void UploadStream(Stream stream);

}
```

A partir desse modelo de programação, o `ChunkingBindingElement` compila uma lista de URIs de ação que identificam as mensagens a serem fragmentadas. A ação de cada mensagem de saída é comparada com essa lista para determinar se a mensagem deve ser colocada em bloco ou enviada diretamente.

## <a name="implementing-the-send-operation"></a>Implementando a operação de envio

Em um nível alto, a operação enviar primeiro verifica se a mensagem de saída deve ser colocada em bloco e, caso contrário, envia a mensagem diretamente usando o canal interno.

Se a mensagem precisar ser enfileirada, Send criará uma nova `ChunkingWriter` e chamará `WriteBodyContents` a mensagem de saída que a transmite `ChunkingWriter` . `ChunkingWriter`Em seguida, o faz a fragmentação da mensagem (incluindo copiar os cabeçalhos da mensagem original para a mensagem de início da parte) e envia partes usando o canal interno.

Alguns detalhes vale A pena observar:

- Envie as primeiras chamadas `ThrowIfDisposedOrNotOpened` para garantir que o `CommunicationState` esteja aberto.

- O envio é sincronizado para que apenas uma mensagem possa ser enviada por vez para cada sessão. Há um `ManualResetEvent` nome `sendingDone` que é redefinido quando uma mensagem em partes é enviada. Depois que a mensagem final da parte é enviada, esse evento é definido. O método Send aguarda esse evento ser definido antes de tentar enviar a mensagem de saída.

- Enviar bloqueia o `CommunicationObject.ThisLock` para evitar alterações de estado sincronizado ao enviar. Consulte a <xref:System.ServiceModel.Channels.CommunicationObject> documentação para obter mais informações sobre <xref:System.ServiceModel.Channels.CommunicationObject> Estados e computador de estado.

- O tempo limite passado para Send é usado como o tempo limite para toda a operação de envio, que inclui o envio de todas as partes.

- O <xref:System.Xml.XmlDictionaryWriter> design personalizado foi escolhido para evitar o buffer de todo o corpo da mensagem original. Se tivéssemos que obter um <xref:System.Xml.XmlDictionaryReader> no corpo usando `message.GetReaderAtBodyContents` o corpo inteiro seria armazenado em buffer. Em vez disso, temos um personalizado <xref:System.Xml.XmlDictionaryWriter> que é passado para `message.WriteBodyContents` . À medida que a mensagem chama WriteBase64 no gravador, o gravador empacota partes em mensagens e as envia usando o canal interno. WriteBase64 blocos de bloqueio até que a parte seja enviada.

## <a name="implementing-the-receive-operation"></a>Implementando a operação de recebimento

Em um alto nível, a operação de recebimento verifica primeiro se a mensagem de entrada não é `null` e se sua ação é `ChunkingAction` . Se ele não atender aos dois critérios, a mensagem será retornada inalterada de Receive. Caso contrário, Receive criará um novo `ChunkingReader` e um novo `ChunkingMessage` encapsulado em torno dele (chamando `GetNewChunkingMessage` ). Antes de retornar essa nova `ChunkingMessage` , Receive usa um thread de ThreadPool a ser executado `ReceiveChunkLoop` , que chama `innerChannel.Receive` em um loop e transfere partes para o `ChunkingReader` até que a mensagem de bloco final seja recebida ou o tempo limite de recebimento seja atingido.

Alguns detalhes vale A pena observar:

- Como enviar, receba as primeiras chamadas `ThrowIfDisposedOrNotOepned` para garantir que o `CommunicationState` esteja aberto.

- Receive também é sincronizado para que apenas uma mensagem possa ser recebida por vez da sessão. Isso é especialmente importante porque depois que uma mensagem de início de bloco é recebida, todas as mensagens subsequentes recebidas devem ser partes nessa nova sequência de bloco até que uma mensagem de bloco final seja recebida. O recebimento não pode efetuar pull de mensagens do canal interno até que todas as partes que pertencem à mensagem que está sendo desfragmentada no momento sejam recebidas. Para fazer isso, Receive usa um `ManualResetEvent` nome `currentMessageCompleted` , que é definido quando a mensagem de parte final é recebida e redefinida quando uma nova mensagem de início de bloco é recebida.

- Ao contrário de enviar, Receive não impede transições de estado sincronizadas durante o recebimento. Por exemplo, Close pode ser chamado durante o recebimento e aguarda até que o recebimento pendente da mensagem original seja concluído ou o valor de tempo limite especificado seja atingido.

- O tempo limite passado para Receive é usado como o tempo limite para a operação de recebimento inteira, que inclui o recebimento de todas as partes.

- Se a camada que consome a mensagem estiver consumindo o corpo da mensagem em uma taxa menor do que a taxa de mensagens de bloco de entrada, o `ChunkingReader` armazenará em buffer essas partes de entrada até o limite especificado por `ChunkingBindingElement.MaxBufferedChunks` . Depois que esse limite for atingido, nenhuma parte mais será extraída da camada inferior até que uma parte armazenada em buffer seja consumida ou o tempo limite de recebimento seja atingido.

## <a name="communicationobject-overrides"></a>Substituições de CommunicationObject

### <a name="onopen"></a>OnOpen

`OnOpen`chamadas `innerChannel.Open` para abrir o canal interno.

### <a name="onclose"></a>Fechamento

`OnClose`o primeiro define `stopReceive` como `true` para sinalizar o pendente `ReceiveChunkLoop` para parar. Em seguida, ele aguarda o `receiveStopped` <xref:System.Threading.ManualResetEvent> , que é definido quando é `ReceiveChunkLoop` interrompido. Supondo que as `ReceiveChunkLoop` interrupções dentro do tempo limite especificado sejam `OnClose` chamadas `innerChannel.Close` com o tempo limite restante.

### <a name="onabort"></a>OnAbort

`OnAbort`chamadas `innerChannel.Abort` para anular o canal interno. Se houver um, `ReceiveChunkLoop` ele receberá uma exceção da chamada pendente `innerChannel.Receive` .

### <a name="onfaulted"></a>Com falha

O `ChunkingChannel` não requer comportamento especial quando o canal tem falha, portanto `OnFaulted` não é substituído.

## <a name="implementing-channel-factory"></a>Implementando a fábrica de canais

O `ChunkingChannelFactory` é responsável por criar instâncias do `ChunkingDuplexSessionChannel` e para transições de estado em cascata para a fábrica de canais interna.

`OnCreateChannel`usa a fábrica de canais interna para criar um `IDuplexSessionChannel` canal interno. Em seguida, ele cria um novo `ChunkingDuplexSessionChannel` passando esse canal interno junto com a lista de ações de mensagens a serem enfileiradas e o número máximo de partes para armazenar em buffer após o recebimento. A lista de ações de mensagens a serem enfileiradas e o número máximo de partes para o buffer são dois parâmetros passados para `ChunkingChannelFactory` em seu construtor. A seção sobre `ChunkingBindingElement` descreve de onde vêm esses valores.

O `OnOpen` , `OnClose` , `OnAbort` e seus equivalentes assíncronos chamam o método de transição de estado correspondente na fábrica de canais interna.

## <a name="implementing-channel-listener"></a>Implementando ouvinte de canal

O `ChunkingChannelListener` é um wrapper em um ouvinte de canal interno. Sua função principal, além de chamadas delegadas para esse ouvinte de canal interno, é encapsular o novo `ChunkingDuplexSessionChannels` em volta dos canais aceitos do ouvinte de canal interno. Isso é feito no `OnAcceptChannel` e no `OnEndAcceptChannel` . O recém-criado `ChunkingDuplexSessionChannel` é passado para o canal interno junto com os outros parâmetros descritos anteriormente.

## <a name="implementing-binding-element-and-binding"></a>Implementando elemento de associação e Associação

`ChunkingBindingElement`é responsável por criar o `ChunkingChannelFactory` e o `ChunkingChannelListener` . O `ChunkingBindingElement` verifica se T no `CanBuildChannelFactory` \<T> e `CanBuildChannelListener` \<T> é do tipo `IDuplexSessionChannel` (o único canal com suporte no canal de agrupamento) e se os outros elementos de ligação na associação dão suporte a esse tipo de canal.

`BuildChannelFactory`\<T>primeiro verifica se o tipo de canal solicitado pode ser compilado e, em seguida, obtém uma lista de ações de mensagens a serem fragmentadas. Para saber mais, veja a seção a seguir. Em seguida, ele cria um novo `ChunkingChannelFactory` passando-o para o alocador de canal interno (como retornado de `context.BuildInnerChannelFactory<IDuplexSessionChannel>` ), a lista de ações de mensagem e o número máximo de partes para armazenar em buffer. O número máximo de partes vem de uma propriedade chamada `MaxBufferedChunks` exposta pelo `ChunkingBindingElement` .

`BuildChannelListener<T>`o tem uma implementação semelhante para criar `ChunkingChannelListener` e passar o ouvinte de canal interno.

Há uma associação de exemplo incluída neste exemplo denominada `TcpChunkingBinding` . Essa associação consiste em dois elementos de ligação: `TcpTransportBindingElement` e `ChunkingBindingElement` . Além de expor a `MaxBufferedChunks` propriedade, a associação também define algumas das `TcpTransportBindingElement` Propriedades como `MaxReceivedMessageSize` (define para `ChunkingUtils.ChunkSize` + 100 KB bytes para cabeçalhos).

`TcpChunkingBinding`também implementa `IBindingRuntimePreferences` e retorna true do `ReceiveSynchronously` método que indica que apenas as chamadas de recebimento síncronas são implementadas.

### <a name="determining-which-messages-to-chunk"></a>Determinando quais mensagens partes

O canal de agrupamento fragmenta apenas as mensagens identificadas por meio do `ChunkingBehavior` atributo. A `ChunkingBehavior` classe implementa `IOperationBehavior` e é implementada chamando o `AddBindingParameter` método. Nesse método, o `ChunkingBehavior` examina o valor de sua `AppliesTo` Propriedade ( `InMessage` `OutMessage` ou ambos) para determinar quais mensagens devem ser fragmentadas. Em seguida, ele obtém a ação de cada uma dessas mensagens (da coleção messages em `OperationDescription` ) e a adiciona a uma coleção de cadeia de caracteres contida em uma instância do `ChunkingBindingParameter` . Em seguida, ele adiciona isso `ChunkingBindingParameter` ao fornecido `BindingParameterCollection` .

Isso `BindingParameterCollection` é passado dentro de `BindingContext` cada elemento de ligação na associação quando esse elemento de ligação cria a fábrica de canais ou o ouvinte de canal. A `ChunkingBindingElement` implementação do `BuildChannelFactory<T>` e `BuildChannelListener<T>` Retire isso `ChunkingBindingParameter` dos `BindingContext’` s `BindingParameterCollection` . A coleção de ações contidas no `ChunkingBindingParameter` é passada para o `ChunkingChannelFactory` ou `ChunkingChannelListener` , que, por sua vez, passa para o `ChunkingDuplexSessionChannel` .

## <a name="running-the-sample"></a>Executando o exemplo

#### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo

1. Instale o ASP.NET 4,0 usando o comando a seguir.

    ```console
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable
    ```

2. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

3. Para compilar a solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

4. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).

5. Execute o Service. exe primeiro e execute Client. exe e observe as janelas do console para saída.

Ao executar o exemplo, a saída a seguir é esperada.

Cliente:

```console
Press enter when service is available

 > Sent chunk 1 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 > Sent chunk 2 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 > Sent chunk 3 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 > Sent chunk 4 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 > Sent chunk 5 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 > Sent chunk 6 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 > Sent chunk 7 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 > Sent chunk 8 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 > Sent chunk 9 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 > Sent chunk 10 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 < Received chunk 1 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 < Received chunk 2 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 < Received chunk 3 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 < Received chunk 4 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 < Received chunk 5 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 < Received chunk 6 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 < Received chunk 7 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 < Received chunk 8 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 < Received chunk 9 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 < Received chunk 10 of message 5b226ad5-c088-4988-b737-6a565e0563dd
```

Servidor: 

```console
Service started, press enter to exit
 < Received chunk 1 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 < Received chunk 2 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 < Received chunk 3 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 < Received chunk 4 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 < Received chunk 5 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 < Received chunk 6 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 < Received chunk 7 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 < Received chunk 8 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 < Received chunk 9 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 < Received chunk 10 of message 867c1fd1-d39e-4be1-bc7b-32066d7ced10
 > Sent chunk 1 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 > Sent chunk 2 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 > Sent chunk 3 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 > Sent chunk 4 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 > Sent chunk 5 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 > Sent chunk 6 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 > Sent chunk 7 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 > Sent chunk 8 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 > Sent chunk 9 of message 5b226ad5-c088-4988-b737-6a565e0563dd
 > Sent chunk 10 of message 5b226ad5-c088-4988-b737-6a565e0563dd
```
