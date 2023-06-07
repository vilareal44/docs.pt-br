---
title: Transmissão de transferência de mensagem
ms.date: 03/30/2017
ms.assetid: 72a47a51-e5e7-4b76-b24a-299d51e0ae5a
ms.openlocfilehash: 462144856750a1b8726b574fdc82746da2d72ff7
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594784"
---
# <a name="streaming-message-transfer"></a>Transmissão de transferência de mensagem
Os transportes do Windows Communication Foundation (WCF) dão suporte a dois modos de transferência de mensagens:  
  
- As transferências em buffer mantêm toda a mensagem em um buffer de memória até que a transferência seja concluída. Uma mensagem em buffer deve ser totalmente entregue antes que um receptor possa lê-la.  
  
- As transferências transmitidas expõem a mensagem como um fluxo. O receptor começa a processar a mensagem antes de ser totalmente entregue.  
  
- Transferências em fluxo podem melhorar a escalabilidade de um serviço eliminando o requisito para grandes buffers de memória. Se a alteração do modo de transferência melhorar a escalabilidade depende do tamanho das mensagens que estão sendo transferidas. Tamanhos de mensagens grandes favorecem o uso de transferências em fluxo.  
  
 Por padrão, os transportes HTTP, TCP/IP e pipe nomeado usam transferências em buffer. Este documento descreve como mudar esses transportes de um modo de transferência em buffer para o em fluxo e as consequências de fazer isso.  
  
## <a name="enabling-streamed-transfers"></a>Habilitando transferências em fluxo  
 A seleção entre os modos de transferência em buffer e em fluxo é feita no elemento de associação do transporte. O elemento Binding tem uma <xref:System.ServiceModel.TransferMode> propriedade que pode ser definida como `Buffered` , `Streamed` , `StreamedRequest` ou `StreamedResponse` . Definir o modo de transferência para `Streamed` habilita a comunicação de streaming em ambas as direções. Definir o modo de transferência como `StreamedRequest` ou `StreamedResponse` habilita a comunicação de streaming somente na direção indicada.  
  
 As <xref:System.ServiceModel.BasicHttpBinding> <xref:System.ServiceModel.NetTcpBinding> associações, e <xref:System.ServiceModel.NetNamedPipeBinding> expõem a <xref:System.ServiceModel.TransferMode> propriedade. Para outros transportes, você deve criar uma associação personalizada para definir o modo de transferência.  
  
 A decisão de usar transferência em buffer ou em streaming é uma decisão local do ponto de extremidade. Para transportes HTTP, o modo de transferência não se propaga por uma conexão ou para servidores e outros intermediários. A definição do modo de transferência não é refletida na descrição da interface de serviço. Depois de gerar uma classe de cliente para um serviço, você deve editar o arquivo de configuração para serviços destinados a serem usados com transferências transmitidas para definir o modo. Para transportes TCP e pipe nomeado, o modo de transferência é propagado como uma declaração de política.  
  
 Para obter exemplos de código, consulte [como habilitar streaming](how-to-enable-streaming.md).  
  
## <a name="enabling-asynchronous-streaming"></a>Habilitando o streaming assíncrono  
 Para habilitar o streaming assíncrono, adicione o comportamento do ponto de extremidade <xref:System.ServiceModel.Description.DispatcherSynchronizationBehavior> ao host de serviço e defina sua propriedade <xref:System.ServiceModel.Description.DispatcherSynchronizationBehavior.AsynchronousSendEnabled%2A> como `true`.  
  
 Essa versão do WCF também Adde o recurso de streaming assíncrono verdadeiro no lado de envio. Isso melhora a escalabilidade do serviço em cenários em que ele está transmitindo mensagens para vários clientes, alguns dos quais estão lentos na leitura; possivelmente devido ao congestionamento da rede ou não estar lendo. Nesses cenários, o WCF não bloqueia mais threads individuais no serviço por cliente. Isso garante que o serviço possa processar muito mais clientes, melhorando dessa forma a escalabilidade do serviço.  
  
## <a name="restrictions-on-streamed-transfers"></a>Restrições em transferências em fluxo  
 O uso do modo de transferência transmitido causa o tempo de execução para impor restrições adicionais.  
  
 As operações que ocorrem em um transporte transmitido podem ter um contrato com, no máximo, um parâmetro de entrada ou saída. Esse parâmetro corresponde a todo o corpo da mensagem e deve ser um <xref:System.ServiceModel.Channels.Message> , um tipo derivado <xref:System.IO.Stream> ou uma <xref:System.Xml.Serialization.IXmlSerializable> implementação. Ter um valor de retorno para uma operação é equivalente a ter um parâmetro de saída.  
  
 Alguns recursos do WCF, como mensagens confiáveis, transações e segurança em nível de mensagem SOAP, dependem do armazenamento em buffer de mensagens para transmissões. O uso desses recursos pode reduzir ou eliminar os benefícios de desempenho obtidos usando o streaming. Para proteger um transporte em fluxo, use somente segurança de nível de transporte ou use segurança de nível de transporte mais segurança de mensagem de autenticação.  
  
 Os cabeçalhos SOAP sempre são armazenados em buffer, mesmo quando o modo de transferência é definido como transmitido. Os cabeçalhos de uma mensagem não devem exceder o tamanho da `MaxBufferSize` cota de transporte. Para obter mais informações sobre essa configuração, consulte [cotas de transporte](transport-quotas.md).  
  
## <a name="differences-between-buffered-and-streamed-transfers"></a>Diferenças entre transferências em buffer e em fluxo  
 Alterar o modo de transferência do buffer para transmitido também altera a forma de canal nativo dos transportes TCP e pipe nomeado. Para transferências em buffer, a forma de canal nativo é <xref:System.ServiceModel.Channels.IDuplexSessionChannel> . Para transferências em fluxo, os canais nativos são <xref:System.ServiceModel.Channels.IRequestChannel> e <xref:System.ServiceModel.Channels.IReplyChannel> . Alterar o modo de transferência em um aplicativo existente que usa esses transportes diretamente (ou seja, não através de um contrato de serviço) requer a alteração da forma de canal esperada para fábricas de canal e ouvintes.  
  
## <a name="see-also"></a>Consulte também

- [Como habilitar transmissão](how-to-enable-streaming.md)
