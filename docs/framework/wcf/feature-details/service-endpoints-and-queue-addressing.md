---
title: Pontos de extremidade de serviço e endereçamento de fila
ms.date: 03/30/2017
ms.assetid: 7d2d59d7-f08b-44ed-bd31-913908b83d97
ms.openlocfilehash: a17e680732cd257fbdfd95eb09df8c53f5894400
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600381"
---
# <a name="service-endpoints-and-queue-addressing"></a>Pontos de extremidade de serviço e endereçamento de fila
Este tópico discute como os clientes atendem aos serviços que lêem de filas e como os pontos de extremidade de serviço são mapeados para filas. Como lembrete, a ilustração a seguir mostra a implantação do aplicativo em fila do WCF (Windows Communication Foundation clássico).  
  
 ![Diagrama de aplicativos enfileirados](media/distributed-queue-figure.jpg "Distributed-Queue-figura")  
  
 Para que o cliente envie a mensagem para o serviço, o cliente endereça a mensagem para a fila de destino. Para que o serviço leia mensagens da fila, ele define seu endereço de escuta para a fila de destino. O endereçamento no WCF é Uniform Resource Identifier (URI) com base nos nomes de fila do MSMQ (serviço de enfileiramento de mensagens) não são baseados em URI. Portanto, é essencial entender como endereçar filas criadas no MSMQ usando o WCF.  
  
## <a name="msmq-addressing"></a>Endereçamento MSMQ  
 O MSMQ usa caminhos e nomes de formato para identificar uma fila. Caminhos especificam um nome de host e um `QueueName` . Opcionalmente, pode haver um `Private$` entre o nome do host e o `QueueName` para indicar uma fila privada que não está publicada no serviço de diretório Active Directory.  
  
 Os nomes de caminho são mapeados para "Formatnames" para determinar aspectos adicionais do endereço, incluindo o roteamento e o protocolo de transferência do Gerenciador de filas. O Gerenciador de filas dá suporte a dois protocolos de transferência: protocolo MSMQ nativo e SRMP (protocolo de mensagens confiáveis SOAP).  
  
 Para obter mais informações sobre nomes de formato e caminho do MSMQ, consulte [sobre o enfileiramento de mensagens](https://docs.microsoft.com/previous-versions/windows/desktop/legacy/ms706032(v=vs.85)).  
  
## <a name="netmsmqbinding-and-service-addressing"></a>NetMsmqBinding e endereçamento de serviço  
 Ao endereçar uma mensagem a um serviço, o esquema no URI é escolhido com base no transporte usado para comunicação. Cada transporte no WCF tem um esquema exclusivo. O esquema deve refletir a natureza do transporte usado para comunicação. Por exemplo, net. TCP, net. pipe, HTTP e assim por diante.  
  
 O transporte em fila do MSMQ no WCF expõe um esquema net. MSMQ. Qualquer mensagem endereçada usando o esquema net. MSMQ é enviada usando o no `NetMsmqBinding` canal de transporte enfileirado do MSMQ.  
  
 O endereçamento de uma fila no WCF é baseado no seguinte padrão:  
  
 NET. MSMQ:// \<*host-name*> /[privado/]\<*queue-name*>  
  
 onde:  
  
- \<*host-name*>é o nome do computador que hospeda a fila de destino.  
  
- [Private] é opcional. Ele é usado ao endereçar uma fila de destino que é uma fila privada. Para endereçar uma fila pública, você não deve especificar Private. Observe que, ao contrário dos caminhos do MSMQ, não há "$" no formato do URI do WCF.  
  
- \<*queue-name*>é o nome da fila. O nome da fila também pode se referir a uma subfila. Portanto, \<*queue-name*>  =  \<*name-of-queue*> [;* sub-Queue-Name*].  
  
 Example1: para tratar de uma fila privada PurchaseOrders hospedada no computador ABC atadatum.com, o URI seria net. MSMQ://abc.adatum.com/private/PurchaseOrders.  
  
 Example2: para tratar de uma fila pública AccountsPayable hospedada no computador def atadatum.com, o URI seria net. MSMQ://def.adatum.com/AccountsPayable.  
  
 O endereço da fila é usado como o URI de escuta pelo ouvinte para ler mensagens. Em outras palavras, o endereço da fila é equivalente à porta de escuta do soquete TCP.  
  
 Um ponto de extremidade que lê de uma fila deve especificar o endereço da fila usando o mesmo esquema especificado anteriormente ao abrir o ServiceHost. Para obter exemplos, consulte [Associação net MSMQ](../samples/net-msmq-binding.md).  
  
### <a name="multiple-contracts-in-a-queue"></a>Vários contratos em uma fila  
 As mensagens em uma fila podem implementar contratos diferentes. Nesse caso, é essencial que uma das seguintes opções seja verdadeira para ler e processar todas as mensagens com êxito:  
  
- Especifique um ponto de extremidade para um serviço que implementa todos os contratos. Essa é a abordagem recomendada.  
  
- Especifique vários pontos de extremidade com contratos diferentes, mas verifique se todos os pontos de extremidade usam o mesmo `NetMsmqBinding` objeto. A lógica de expedição no ServiceModel usa uma bomba de mensagem que lê mensagens do canal de transporte para expedição, o que eventualmente desmultiplexa mensagens com base no contrato para diferentes pontos de extremidade. Uma bomba de mensagem é criada para um par de associação/URI de escuta. O endereço da fila é usado como o URI de escuta pelo ouvinte em fila. Fazer com que todos os pontos de extremidade usem o mesmo objeto de ligação garante que uma única bomba de mensagem seja usada para ler a mensagem e Desmultiplexar para pontos de extremidade relevantes com base no contrato.  
  
### <a name="srmp-messaging"></a>Mensagens SRMP  
 Conforme discutido anteriormente, você pode usar o protocolo SRMP para transferências de fila para fila. Isso é comumente usado quando um transporte HTTP transmite mensagens entre a fila de transmissão e a fila de destino.  
  
 Para usar o protocolo de transferência SRMP, endereçar mensagens usando o esquema de URI net. MSMQ, conforme mencionado anteriormente, e especifique a opção de SRMP ou de SRMP protegido na `QueueTransferProtocol` Propriedade do `NetMsmqBinding` .  
  
 A especificação da `QueueTransferProtocol` propriedade é um recurso somente de envio. Essa é uma indicação pelo cliente que o tipo de protocolo de transferência de fila usar.  
  
### <a name="using-active-directory"></a>Como usar o Active Directory  
 O MSMQ é fornecido com suporte para integração de Active Directory. Quando o MSMQ é instalado com a integração do Active Directory, o computador deve fazer parte de um domínio do Windows. Active Directory é usado para publicar filas para descoberta; Essas filas são chamadas de *filas públicas*. Ao endereçar uma fila, a fila pode ser resolvida usando Active Directory. Isso é semelhante a como o DNS (sistema de nomes de domínio) é usado para resolver o endereço IP de um nome de rede. A `UseActiveDirectory` propriedade em `NetMsmqBinding` é um booliano que indica se o canal em fila deve usar Active Directory para resolver o URI da fila. Por padrão, ele é definido como `false` . Se a `UseActiveDirectory` propriedade for definida como `true` , o canal em fila usará Active Directory para converter net. MSMQ://URI em nome de formato.  
  
 A `UseActiveDirectory` propriedade só é significativa para o cliente que está enviando a mensagem porque ela é usada para resolver o endereço da fila ao enviar mensagens.  
  
### <a name="mapping-netmsmq-uri-to-message-queuing-format-names"></a>Mapeando URI net. MSMQ para nomes de formato de enfileiramento de mensagens  
 O canal enfileirado lida com o mapeamento do nome do URI net. MSMQ fornecido ao canal para nomes de formato MSMQ. A tabela a seguir resume as regras usadas para mapear entre elas.  
  
|Endereço de fila baseado em URI do WCF|Usar a propriedade Active Directory|Propriedade do protocolo de transferência de fila|Nomes de formato MSMQ resultantes|  
|----------------------------------|-----------------------------------|--------------------------------------|---------------------------------|  
|`Net.msmq://<machine-name>/private/abc`|False (padrão)|Nativo (padrão)|`DIRECT=OS:machine-name\private$\abc`|  
|`Net.msmq://<machine-name>/private/abc`|Falso|SRMP|`DIRECT=http://machine/msmq/private$/abc`|  
|`Net.msmq://<machine-name>/private/abc`|True|Nativo|`PUBLIC=some-guid`(o GUID da fila)|  
  
### <a name="reading-messages-from-the-dead-letter-queue-or-the-poison-message-queue"></a>Lendo mensagens da fila de mensagens mortas ou da fila de mensagem suspeita  
 Para ler mensagens de uma fila de mensagens suspeitas que é uma subfila da fila de destino, abra o `ServiceHost` com o endereço da subfila.  
  
 Exemplo: um serviço que lê a fila de mensagens suspeitas da fila particular PurchaseOrders da máquina local resolveria net. MSMQ://localhost/private/PurchaseOrders; suspeita.  
  
 Para ler mensagens de uma fila de mensagens mortas de sistema transacional, o URI deve estar no formato: net. MSMQ://localhost/System $;D eadXact.  
  
 Para ler mensagens de uma fila de mensagens mortas não transacionais do sistema, o URI deve estar no formato: net. MSMQ://localhost/System $;D eadLetter.  
  
 Ao usar uma fila de mensagens mortas personalizada, observe que a fila de mensagens mortas deve residir no computador local. Como tal, o URI para a fila de mensagens mortas é restrito ao formulário:  
  
 NET. MSMQ://localhost/[Private/] \<*custom-dead-letter-queue-name*> .  
  
 Um serviço WCF verifica se todas as mensagens recebidas foram endereçadas para a fila específica em que está escutando. Se a fila de destino da mensagem não corresponder à fila em que foi encontrada, o serviço não processará a mensagem. Esse é um problema que os serviços que escutam em uma fila de mensagens mortas devem resolver porque qualquer mensagem na fila de mensagens mortas foi destinada a ser entregue em outro lugar. Para ler mensagens de uma fila de mensagem mortas ou de uma fila suspeita, um `ServiceBehavior` com o <xref:System.ServiceModel.AddressFilterMode.Any> parâmetro deve ser usado. Para obter um exemplo, consulte [filas de mensagens mortas](../samples/dead-letter-queues.md).  
  
## <a name="msmqintegrationbinding-and-service-addressing"></a>MsmqIntegrationBinding e endereçamento de serviço  
 O `MsmqIntegrationBinding` é usado para comunicação com aplicativos MSMQ tradicionais. Para facilitar a interoperação com um aplicativo MSMQ existente, o WCF dá suporte apenas ao endereçamento de nome de formato. Portanto, as mensagens enviadas usando essa associação devem estar de acordo com o esquema de URI:  
  
 MSMQ. FormatName:\<*MSMQ-format-name*>>  
  
 O nome de formato MSMQ é o formato especificado pelo MSMQ no [enfileiramento de mensagens](https://docs.microsoft.com/previous-versions/windows/desktop/legacy/ms706032(v=vs.85)).  
  
 Observe que você só pode usar nomes de formato diretos e nomes de formato público e privado (requer integração de Active Directory) ao receber mensagens de uma fila usando `MsmqIntegrationBinding` . No entanto, é recomendável que você use nomes de formato diretos. Por exemplo, no Windows Vista, o uso de qualquer outro nome de formato causa um erro porque o sistema tenta abrir uma subfila, que só pode ser aberta com nomes de formato diretos.  
  
 Ao tratar de SRMP usando, não há `MsmqIntegrationBinding` nenhum requisito para adicionar/MSMQ/no nome de formato direto para ajudar a serviços de informações da Internet (IIS) com expedição. Por exemplo: ao endereçar uma fila ABC usando o protocolo SRMP, em vez de `DIRECT=http://adatum.com/msmq/private$/abc` , você deve usar `DIRECT=http://adatum.com/private$/abc` .  
  
 Observe que você não pode usar net. MSMQ://endereçamento com `MsmqIntegrationBinding` . Como o `MsmqIntegrationBinding` dá suporte ao endereçamento de nome de formato MSMQ de forma livre, você pode usar um serviço WCF que usa essa associação para usar recursos de lista de distribuição e multicast no MSMQ. Uma exceção é especificar `CustomDeadLetterQueue` ao usar o `MsmqIntegrationBinding` . Ele deve estar no formato net. MSMQ://, semelhante a como ele é especificado usando o `NetMsmqBinding` .  
  
## <a name="see-also"></a>Consulte também

- [Hospedagem na Web de um aplicativo na fila](web-hosting-a-queued-application.md)
