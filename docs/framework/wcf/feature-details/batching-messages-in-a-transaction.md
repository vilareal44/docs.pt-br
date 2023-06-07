---
title: Mensagens de lote em uma transação
ms.date: 03/30/2017
helpviewer_keywords:
- batching messages [WCF]
ms.assetid: 53305392-e82e-4e89-aedc-3efb6ebcd28c
ms.openlocfilehash: 3b35d1de76587ce750bf73189eb37658c3d87a90
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84593614"
---
# <a name="batching-messages-in-a-transaction"></a>Mensagens de lote em uma transação
Os aplicativos em fila usam transações para garantir a exatidão e a entrega confiável de mensagens. As transações, no entanto, são operações caras e podem reduzir drasticamente a taxa de transferência da mensagem. Uma maneira de melhorar a taxa de transferência da mensagem é fazer com que um aplicativo Leia e processe várias mensagens em uma única transação. A compensação é entre o desempenho e a recuperação: à medida que o número de mensagens em um lote aumenta, a quantidade de trabalho de recuperação necessária se as transações são revertidas. É importante observar a diferença entre o envio em lote de mensagens em uma transação e sessões. Uma *sessão* é um agrupamento de mensagens relacionadas que são processadas por um único aplicativo e confirmadas como uma única unidade. As sessões geralmente são usadas quando um grupo de mensagens relacionadas deve ser processado em conjunto. Um exemplo disso é um site de compras online. Os *lotes* são usados para processar várias mensagens não relacionadas de forma a aumentar a taxa de transferência da mensagem. Para obter mais informações sobre sessões, consulte [agrupando mensagens em fila em uma sessão](grouping-queued-messages-in-a-session.md). As mensagens em um lote também são processadas por um único aplicativo e confirmadas como uma única unidade, mas pode não haver nenhuma relação entre as mensagens no lote. O envio em lote de mensagens em uma transação é uma otimização que não altera a forma como o aplicativo é executado.  
  
## <a name="entering-batching-mode"></a>Inserindo modo de lote  
 O <xref:System.ServiceModel.Description.TransactedBatchingBehavior> comportamento do ponto de extremidade controla o envio em lote. Adicionar esse comportamento de ponto de extremidade a um ponto de extremidade de serviço informa Windows Communication Foundation (WCF) para mensagens em lote em uma transação. Nem todas as mensagens exigem uma transação, portanto, somente as mensagens que exigem uma transação são colocadas em um lote e somente as mensagens enviadas de operações marcadas com `TransactionScopeRequired`  =  `true` e `TransactionAutoComplete`  =  `true` são consideradas para um lote. Se todas as operações no contrato de serviço forem marcadas com `TransactionScopeRequired`  =  `false` e `TransactionAutoComplete`  =  `false` , o modo de envio em lote nunca será inserido.  
  
## <a name="committing-a-transaction"></a>Confirmando uma transação  
 Uma transação em lote é confirmada com base no seguinte:  
  
- `MaxBatchSize`. Uma propriedade do <xref:System.ServiceModel.Description.TransactedBatchingBehavior> comportamento. Essa propriedade determina o número máximo de mensagens que são colocadas em um lote. Quando esse número é atingido, o lote é confirmado. Esse valor não é um limite estrito, é possível confirmar um lote antes de receber esse número de mensagens.  
  
- `Transaction Timeout`. Após 80% do tempo limite da transação expirou, o lote é confirmado e um novo lote é criado. Isso significa que se 20 por cento ou menos do tempo fornecido para a conclusão de uma transação permanecer, o lote será confirmado.  
  
- `TransactionScopeRequired`. Ao processar um lote de mensagens, se o WCF encontrar um que tenha `TransactionScopeRequired`  =  `false` , ele confirmará o lote e reabrirá um novo lote no recebimento da primeira mensagem com `TransactionScopeRequired`  =  `true` e `TransactionAutoComplete`  =  `true` .  
  
- Se não existirem mais mensagens na fila, o lote atual será confirmado, mesmo que `MaxBatchSize` não tenha sido atingido ou 80% do tempo limite da transação não tiver decorrido.  
  
## <a name="leaving-batching-mode"></a>Saindo do modo de envio em lote  
 Se uma mensagem em um lote fizer com que a transação seja anulada, as seguintes etapas ocorrerão:  
  
1. Todo o lote de mensagens é revertido.  
  
2. As mensagens são lidas uma de cada vez até que o número de mensagens lidas exceda duas vezes o tamanho máximo do lote.  
  
3. O modo de lote é inserido novamente.  
  
## <a name="choosing-the-batch-size"></a>Escolhendo o tamanho do lote  
 O tamanho de um lote depende do aplicativo. O método empírica é a melhor maneira de chegar a um tamanho de lote ideal para o aplicativo. É importante lembrar-se de escolher um tamanho de lote para escolher o tamanho de acordo com o modelo de implantação real de seu aplicativo. Por exemplo, ao implantar o aplicativo, se você precisar de um SQL Server em um computador remoto e uma transação que abranja a fila e o SQL Server, o tamanho do lote será melhor determinado executando essa configuração exata.  
  
## <a name="concurrency-and-batching"></a>Simultaneidade e envio em lote  
 Para aumentar a taxa de transferência, você também pode ter muitos lotes executados simultaneamente. Ao configurar o `ConcurrencyMode.Multiple` no `ServiceBehaviorAttribute` , você habilita o envio em lote simultâneo.  
  
 A *limitação de serviço* é um comportamento de serviço que é usado para indicar quantas chamadas simultâneas máximas podem ser feitas no serviço. Quando usado com o envio em lote, isso é interpretado como quantos lotes simultâneos podem ser executados. Se a limitação de serviço não for definida, o WCF padronizará o máximo de chamadas simultâneas para 16. Portanto, se o comportamento de envio em lote fosse adicionado por padrão, um máximo de 16 lotes pode estar ativo ao mesmo tempo. É melhor ajustar a limitação de serviço e o envio em lote com base em sua capacidade. Por exemplo, se a fila tiver 100 mensagens e um lote de 20 for desejado, ter o máximo de chamadas simultâneas definido como 16 não será útil porque, dependendo da taxa de transferência, 16 transações poderão estar ativas, semelhante a não ter ativado o envio em lote. Portanto, ao ajustar o desempenho, não haverá envio em lote simultâneo ou ter envio em lote simultâneo com o tamanho correto do acelerador de serviço.  
  
## <a name="batching-and-multiple-endpoints"></a>Envio em lote e vários pontos de extremidade  
 Um ponto de extremidade é composto por um endereço e um contrato. Pode haver vários pontos de extremidade que compartilham a mesma ligação. É possível que dois pontos de extremidade compartilhem a mesma ligação e escutem Uniform Resource Identifier (URI) ou endereço de fila. Se dois pontos de extremidade estiverem sendo lidos da mesma fila e o comportamento de envio em lote transacionado for adicionado a ambos os pontos de extremidade, poderá ocorrer um conflito nos tamanhos de lote especificados. Isso é resolvido implementando o envio em lote usando o tamanho mínimo de lote especificado entre os dois comportamentos de envio em lote transacionados. Nesse cenário, se um dos pontos de extremidade não especificar o envio em lote transacionado, os dois pontos de extremidade não usarão o envio em lote.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como especificar o `TransactedBatchingBehavior` em um arquivo de configuração.  
  
```xml  
<behaviors>
  <endpointBehaviors>
    <behavior name="TransactedBatchingBehavior"
              maxBatchSize="100" />
  </endpointBehaviors>
</behaviors>
```  
  
 O exemplo a seguir mostra como especificar o <xref:System.ServiceModel.Description.TransactedBatchingBehavior> no código.  
  
```csharp
using (ServiceHost serviceHost = new ServiceHost(typeof(OrderProcessorService)))
{
     ServiceEndpoint sep = ServiceHost.AddServiceEndpoint(typeof(IOrderProcessor), new NetMsmqBinding(), "net.msmq://localhost/private/ServiceModelSamplesTransacted");
     sep.Behaviors.Add(new TransactedBatchingBehavior(100));

     // Open the ServiceHost to create listeners and start listening for messages.
    serviceHost.Open();
  
    // The service can now be accessed.
    Console.WriteLine("The service is ready.");
    Console.WriteLine("Press <ENTER> to terminate service.");
    Console.WriteLine();
    Console.ReadLine();
  
    // Close the ServiceHostB to shut down the service.
    serviceHost.Close();
}  
```  
  
## <a name="see-also"></a>Consulte também

- [Visão geral de filas](queues-overview.md)
- [Enfileiramento no WCF](queuing-in-wcf.md)
