---
title: Como habilitar transmissão
description: Saiba como habilitar as mensagens transmitidas no WCF em vez das transferências em buffer padrão, que devem ser completamente recebidas antes de serem processadas.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 6ca2cf4b-c7a1-49d8-a79b-843a90556ba4
ms.openlocfilehash: 538fd8634094aa6fbf097ddb94469d7bca749a63
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247020"
---
# <a name="how-to-enable-streaming"></a>Como habilitar transmissão
O Windows Communication Foundation (WCF) pode enviar mensagens usando transferências em buffer ou em fluxo. No modo de transferência em buffer padrão, uma mensagem deve ser totalmente entregue antes que um receptor possa lê-la. No modo de transferência de streaming, o receptor pode começar a processar a mensagem antes de ser totalmente entregue. O modo de streaming é útil quando as informações que são passadas são demoradas e podem ser processadas em série. O modo de streaming também é útil quando a mensagem é muito grande para ser totalmente armazenada em buffer.  
  
 Para habilitar o streaming, defina o de forma `OperationContract` apropriada e habilite o streaming no nível de transporte.  
  
### <a name="to-stream-data"></a>Para transmitir dados  
  
1. Para transmitir dados, o `OperationContract` para o serviço deve atender a dois requisitos:  
  
    1. O parâmetro que mantém os dados a serem transmitidos deve ser o único parâmetro no método. Por exemplo, se a mensagem de entrada for aquela a ser transmitida, a operação deverá ter exatamente um parâmetro de entrada. Da mesma forma, se a mensagem de saída for para ser transmitida, a operação deverá ter exatamente um parâmetro de saída ou um valor de retorno.  
  
    2. Pelo menos um dos tipos do parâmetro e do valor de retorno deve ser <xref:System.IO.Stream> , <xref:System.ServiceModel.Channels.Message> ou <xref:System.Xml.Serialization.IXmlSerializable> .  
  
     Veja a seguir um exemplo de um contrato para dados transmitidos.  
  
     [!code-csharp[c_HowTo_EnableStreaming#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_enablestreaming/cs/service.cs#1)]
     [!code-vb[c_HowTo_EnableStreaming#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_enablestreaming/vb/service.vb#1)]  
  
     A `GetStream` operação recebe alguns dados de entrada em buffer como um `string` , que é armazenado em buffer e retorna um `Stream` , que é transmitido. Por outro lado `UploadStream` , o leva em um `Stream` (transmitido) e retorna um `bool` (em buffer). `EchoStream`Pega e retorna `Stream` e é um exemplo de uma operação cujas mensagens de entrada e saída são transmitidas. Por fim, `GetReversedStream` o não usa entradas e retorna um `Stream` (transmitido).  
  
2. O streaming deve ser habilitado na associação. Você define uma `TransferMode` propriedade, que pode usar um dos seguintes valores:  
  
    1. `Buffered`,  
  
    2. `Streamed`, que permite a comunicação de streaming em ambas as direções.  
  
    3. `StreamedRequest`, que permite transmitir apenas a solicitação.  
  
    4. `StreamedResponse`, que permite transmitir apenas a resposta.  
  
     O `BasicHttpBinding` expõe a `TransferMode` Propriedade na associação, como faz `NetTcpBinding` e `NetNamedPipeBinding` . A `TransferMode` propriedade também pode ser definida no elemento de associação de transporte e usada em uma associação personalizada.  
  
     Os exemplos a seguir mostram como definir `TransferMode` pelo código e alterando o arquivo de configuração. Os exemplos também definem a `maxReceivedMessageSize` propriedade como 64 MB, que coloca um limite no tamanho máximo permitido de mensagens em recebimento. O padrão `maxReceivedMessageSize` é 64 KB, que geralmente é muito baixo para cenários de streaming. Defina essa configuração de cota conforme apropriado, dependendo do tamanho máximo das mensagens que seu aplicativo espera receber. Observe também que `maxBufferSize` o controla o tamanho máximo que é armazenado em buffer e o define adequadamente.  
  
    1. O trecho de configuração a seguir do exemplo mostra a configuração da `TransferMode` propriedade como streaming no `basicHttpBinding` e uma associação http personalizada.  
  
         [!code-xml[c_HowTo_EnableStreaming#103](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_enablestreaming/common/app.config#103)]
  
    2. O trecho de código a seguir mostra a configuração da `TransferMode` propriedade como streaming no `basicHttpBinding` e uma associação http personalizada.  
  
         [!code-csharp[c_HowTo_EnableStreaming_code#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_enablestreaming_code/cs/c_howto_enablestreaming_code.cs#2)]
         [!code-vb[c_HowTo_EnableStreaming_code#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_enablestreaming_code/vb/c_howto_enablestreaming_code.vb#2)]  
  
    3. O trecho de código a seguir mostra a configuração da `TransferMode` propriedade como streaming em uma associação TCP personalizada.  
  
         [!code-csharp[c_HowTo_EnableStreaming_code#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_enablestreaming_code/cs/c_howto_enablestreaming_code.cs#3)]
         [!code-vb[c_HowTo_EnableStreaming_code#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_enablestreaming_code/vb/c_howto_enablestreaming_code.vb#3)]  
  
3. As operações `GetStream` , `UploadStream` e `EchoStream` todas lidam com o envio de dados diretamente de um arquivo ou salvam dados recebidos diretamente em um arquivo. O código a seguir é para `GetStream` .  
  
     [!code-csharp[c_HowTo_EnableStreaming#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_enablestreaming/cs/service.cs#4)]
     [!code-vb[c_HowTo_EnableStreaming#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_enablestreaming/vb/service.vb#4)]  
  
### <a name="writing-a-custom-stream"></a>Escrevendo um fluxo personalizado  
  
1. Para fazer um processamento especial em cada parte de um fluxo de dados como ele está sendo enviado ou recebido, derive uma classe de fluxo personalizada de <xref:System.IO.Stream> . Como um exemplo de um fluxo personalizado, o código a seguir contém um `GetReversedStream` método e uma `ReverseStream` classe-.  
  
     `GetReversedStream`Cria e retorna uma nova instância do `ReverseStream` . O processamento real ocorre à medida que o sistema lê do `ReverseStream` objeto. O `ReverseStream.Read` método lê um bloco de bytes do arquivo subjacente, reverte-os e retorna os bytes invertidos. Esse método não reverte todo o conteúdo do arquivo; Ele reverte uma parte de bytes de cada vez. Este exemplo mostra como você pode executar o processamento de fluxo, pois o conteúdo está sendo lido ou gravado no fluxo.  
  
     [!code-csharp[c_HowTo_EnableStreaming#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_enablestreaming/cs/service.cs#2)]
     [!code-vb[c_HowTo_EnableStreaming#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_enablestreaming/vb/service.vb#2)]  
  
## <a name="see-also"></a>Veja também

- [Dados grandes e streaming](large-data-and-streaming.md)
- [STREAM](../samples/stream.md)
