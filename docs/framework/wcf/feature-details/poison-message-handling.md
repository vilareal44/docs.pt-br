---
title: Manuseio de mensagem suspeita
ms.date: 03/30/2017
ms.assetid: 8d1c5e5a-7928-4a80-95ed-d8da211b8595
ms.openlocfilehash: d219c18bb072684deb6cc1d8a2d17b1989762151
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84590560"
---
# <a name="poison-message-handling"></a>Manuseio de mensagem suspeita
Uma *mensagem suspeita* é uma mensagem que excedeu o número máximo de tentativas de entrega para o aplicativo. Essa situação pode surgir quando um aplicativo baseado em fila não pode processar uma mensagem devido a erros. Para atender às demandas de confiabilidade, um aplicativo em fila recebe mensagens em uma transação. Anular a transação na qual uma mensagem em fila foi recebida deixa a mensagem na fila para que a mensagem seja repetida em uma nova transação. Se o problema que causou a anulação da transação não for corrigido, o aplicativo receptor poderá ficar preso em um loop recebendo e anulando a mesma mensagem até que o número máximo de tentativas de entrega tenha sido excedido e uma mensagem suspeita resulte.  
  
 Uma mensagem pode se tornar uma mensagem suspeita por vários motivos. Os motivos mais comuns são específicos do aplicativo. Por exemplo, se um aplicativo lê uma mensagem de uma fila e executa algum processamento de banco de dados, o aplicativo pode falhar ao obter um bloqueio no banco de dados, fazendo com que ele anule a transação. Como a transação do banco de dados foi anulada, a mensagem permanece na fila, o que faz com que o aplicativo leia novamente a mensagem uma segunda vez e faça outra tentativa de adquirir um bloqueio no banco de dados. As mensagens também podem se tornar suspeitas se contiverem informações inválidas. Por exemplo, uma ordem de compra pode conter um número de cliente inválido. Nesses casos, o aplicativo pode, voluntariamente, anular a transação e forçar a mensagem a se tornar uma mensagem suspeita.  
  
 Em raras ocasiões, as mensagens podem falhar ao serem expedidas para o aplicativo. A camada Windows Communication Foundation (WCF) pode encontrar um problema com a mensagem, como se a mensagem tem o quadro incorreto, credenciais de mensagem inválidas anexadas a ela ou um cabeçalho de ação inválido. Nesses casos, o aplicativo nunca recebe a mensagem; no entanto, a mensagem ainda pode se tornar uma mensagem suspeita e ser processada manualmente.  
  
## <a name="handling-poison-messages"></a>Tratando de mensagens suspeitas  
 No WCF, a manipulação de mensagens suspeitas fornece um mecanismo para um aplicativo de recebimento lidar com mensagens que não podem ser expedidas para o aplicativo ou mensagens que são expedidas para o aplicativo, mas que não são processadas devido a motivos específicos do aplicativo. Configure a manipulação de mensagens suspeitas com as seguintes propriedades em cada uma das associações em fila disponíveis:  
  
- `ReceiveRetryCount`. Um valor inteiro que indica o número máximo de vezes para repetir a entrega de uma mensagem da fila do aplicativo para o aplicativo. O valor padrão é 5. Isso é suficiente nos casos em que uma repetição imediata corrige o problema, como com um deadlock temporário em um banco de dados.  
  
- `MaxRetryCycles`. Um valor inteiro que indica o número máximo de ciclos de repetição. Um ciclo de repetição consiste em transferir uma mensagem da fila do aplicativo para a subfila de repetição e, após um atraso configurável, da subfila de repetição para a fila do aplicativo para tentar a entrega novamente. O valor padrão é 2. No Windows Vista, a mensagem é tentada no máximo ( `ReceiveRetryCount` + 1) * ( `MaxRetryCycles` + 1) vezes. `MaxRetryCycles`é ignorado no Windows Server 2003 e no Windows XP.  
  
- `RetryCycleDelay`. O intervalo de tempo entre os ciclos de repetição. O valor padrão é 30 minutos. `MaxRetryCycles`e `RetryCycleDelay` juntos fornecem um mecanismo para resolver o problema em que uma nova tentativa após um atraso periódico corrige o problema. Por exemplo, isso manipula um conjunto de linhas bloqueado em SQL Server confirmação de transação pendente.  
  
- `ReceiveErrorHandling`. Uma enumeração que indica a ação a ser tomada para uma mensagem com falha na entrega após o número máximo de repetições ter sido tentada. Os valores podem ser falha, soltar, rejeitar e mover. A opção padrão é Fault.  
  
- Tolerância. Essa opção envia uma falha ao ouvinte que causou a `ServiceHost` falha. A mensagem deve ser removida da fila de aplicativos por algum mecanismo externo antes que o aplicativo possa continuar a processar mensagens da fila.  
  
- Drop. Essa opção descarta a mensagem suspeita e a mensagem nunca é entregue ao aplicativo. Se a propriedade da mensagem `TimeToLive` tiver expirado neste ponto, a mensagem poderá aparecer na fila de mensagens mortas do remetente. Caso contrário, a mensagem não aparecerá em lugar nenhum. Essa opção indica que o usuário não especificou o que fazer se a mensagem for perdida.  
  
- Rejeitar. Essa opção só está disponível no Windows Vista. Isso instrui o MSMQ (enfileiramento de mensagens) a enviar uma confirmação negativa de volta ao Gerenciador de filas de envio que o aplicativo não pode receber a mensagem. A mensagem é colocada na fila de mensagens mortas do Gerenciador de filas de envio.  
  
- Prosseguir. Essa opção só está disponível no Windows Vista. Isso move a mensagem suspeita para uma fila de mensagens suspeitas para processamento posterior por um aplicativo de manipulação de mensagens suspeitas. A fila de mensagens suspeitas é uma subfila da fila de aplicativos. Um aplicativo de manipulação de mensagens suspeitas pode ser um serviço WCF que lê mensagens da fila de suspeitas. A fila de suspeitas é uma subfila da fila de aplicativos e pode ser tratada como net. MSMQ:// \<*machine-name*> / *applicationQueue*;p Oison, em que *Machine-Name* é o nome do computador no qual a fila reside e o *applicationQueue* é o nome da fila específica do aplicativo.  
  
Veja a seguir o número máximo de tentativas de entrega feitas para uma mensagem:  
  
- ((ReceiveRetryCount + 1) * (MaxRetryCycles + 1)) no Windows Vista.  
  
- (ReceiveRetryCount + 1) no Windows Server 2003 e no Windows XP.  
  
> [!NOTE]
> Nenhuma nova tentativa é feita para uma mensagem entregue com êxito.  
  
 Para controlar o número de vezes que uma mensagem é tentada, o Windows Vista mantém uma propriedade de mensagem durável que conta o número de anulações e uma propriedade de contagem de movimento que conta o número de vezes que a mensagem se move entre a fila do aplicativo e as subfilas. O canal do WCF usa esses para calcular a contagem de repetição de recebimento e a contagem de ciclos de repetição. No Windows Server 2003 e no Windows XP, a contagem de anulações é mantida na memória pelo canal do WCF e é redefinida se o aplicativo falhar. Além disso, o canal do WCF pode conter as contagens de anulação de até 256 mensagens na memória a qualquer momento. Se uma mensagem 257th for lida, a contagem de anulação da mensagem mais antiga será redefinida.  
  
 As propriedades contagem de anulação e contagem de movimentação estão disponíveis para a operação de serviço por meio do contexto de operação. O exemplo de código a seguir mostra como acessá-los.  
  
 [!code-csharp[S_UE_MSMQ_Poison#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_ue_msmq_poison/cs/service.cs#1)]  
  
 O WCF fornece duas ligações padrão em fila:  
  
- <xref:System.ServiceModel.NetMsmqBinding>. Uma associação .NET Framework adequada para executar a comunicação baseada em fila com outros pontos de extremidade do WCF.  
  
- <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding>. Uma associação adequada para comunicação com aplicativos de enfileiramento de mensagens existentes.  
  
> [!NOTE]
> Você pode alterar as propriedades dessas associações com base nos requisitos de seu serviço WCF. Todo o mecanismo de manipulação de mensagens suspeitas é local para o aplicativo de recebimento. O processo é invisível para o aplicativo de envio, a menos que o aplicativo de recebimento interrompa e envie uma confirmação negativa de volta ao remetente. Nesse caso, a mensagem é movida para a fila de mensagens mortas do remetente.  
  
## <a name="best-practice-handling-msmqpoisonmessageexception"></a>Prática recomendada: manipulação de MsmqPoisonMessageException  
 Quando o serviço determina que uma mensagem é suspeita, o transporte em fila gera um <xref:System.ServiceModel.MsmqPoisonMessageException> que contém a `LookupId` da mensagem suspeita.  
  
 Um aplicativo receptor pode implementar a <xref:System.ServiceModel.Dispatcher.IErrorHandler> interface para lidar com quaisquer erros que o aplicativo exigir. Para obter mais informações, consulte [ampliando o controle sobre tratamento de erros e relatórios](../samples/extending-control-over-error-handling-and-reporting.md).  
  
 O aplicativo pode exigir algum tipo de manipulação automatizada de mensagens suspeitas que move as mensagens suspeitas para uma fila de mensagens suspeitas para que o serviço possa acessar o restante das mensagens na fila. O único cenário para usar o mecanismo do manipulador de erros para escutar exceções de mensagens suspeitas é quando a <xref:System.ServiceModel.Configuration.MsmqBindingElementBase.ReceiveErrorHandling%2A> configuração é definida como <xref:System.ServiceModel.ReceiveErrorHandling.Fault> . O exemplo de mensagem suspeita para o enfileiramento de mensagens 3,0 demonstra esse comportamento. O seguinte descreve as etapas a serem seguidas para lidar com mensagens suspeitas, incluindo as práticas recomendadas:  
  
1. Certifique-se de que suas configurações suspeitas reflitam os requisitos do seu aplicativo. Ao trabalhar com as configurações, certifique-se de entender as diferenças entre os recursos do enfileiramento de mensagens no Windows Vista, no Windows Server 2003 e no Windows XP.  
  
2. Se necessário, implemente o `IErrorHandler` para tratar erros de mensagens suspeitas. Como a configuração `ReceiveErrorHandling` para `Fault` requer um mecanismo manual para mover a mensagem suspeita para fora da fila ou para corrigir um problema dependente externo, o uso típico é implementar `IErrorHandler` quando `ReceiveErrorHandling` é definido como `Fault` , conforme mostrado no código a seguir.  
  
     [!code-csharp[S_UE_MSMQ_Poison#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_ue_msmq_poison/cs/poisonerrorhandler.cs#2)]  
  
3. Crie um `PoisonBehaviorAttribute` que o comportamento do serviço possa usar. O comportamento instala o `IErrorHandler` no Dispatcher. Consulte o exemplo de código a seguir.  
  
     [!code-csharp[S_UE_MSMQ_Poison#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_ue_msmq_poison/cs/poisonbehaviorattribute.cs#3)]  
  
4. Verifique se o serviço está anotado com o atributo de comportamento suspeita.  

 Além disso, se o `ReceiveErrorHandling` for definido como `Fault` , as `ServiceHost` falhas ao encontrar a mensagem suspeita. Você pode conectar-se ao evento com falha e desligar o serviço, executar ações corretivas e reiniciar. Por exemplo, o `LookupId` no <xref:System.ServiceModel.MsmqPoisonMessageException> propagado para o `IErrorHandler` pode ser anotado e quando o host falha, você pode usar a `System.Messaging` API para receber a mensagem da fila usando o `LookupId` para remover a mensagem da fila e armazenar a mensagem em algum repositório externo ou em outra fila. Em seguida, você pode reiniciar `ServiceHost` para retomar o processamento normal. O [tratamento de mensagens suspeitas no MSMQ 4,0](../samples/poison-message-handling-in-msmq-4-0.md) demonstra esse comportamento.  
  
## <a name="transaction-time-out-and-poison-messages"></a>Tempo limite da transação e mensagens suspeitas  
 Uma classe de erros pode ocorrer entre o canal de transporte enfileirado e o código do usuário. Esses erros podem ser detectados por camadas, entre elas, como a camada de segurança da mensagem ou a lógica de expedição do serviço. Por exemplo, um certificado X. 509 ausente detectado na camada de segurança SOAP e uma ação ausente são casos em que a mensagem é expedida para o aplicativo. Quando isso acontece, o modelo de serviço descarta a mensagem. Como a mensagem é lida em uma transação e um resultado para essa transação não pode ser fornecido, a transação eventualmente expira, anula e a mensagem é colocada de volta na fila. Em outras palavras, para uma determinada classe de erros, a transação não é anulada imediatamente, mas aguarda até que a transação expire. Você pode modificar o tempo limite da transação para um serviço usando <xref:System.ServiceModel.ServiceBehaviorAttribute> .  
  
 Para alterar o tempo limite da transação em todo o computador, modifique o arquivo Machine. config e defina o tempo limite da transação apropriado. É importante observar que, dependendo do tempo limite definido na transação, a transação eventualmente aborta e volta para a fila e sua contagem de anulação é incrementada. Eventualmente, a mensagem se torna suspeita e a disposição certa é feita de acordo com as configurações do usuário.  
  
## <a name="sessions-and-poison-messages"></a>Sessões e mensagens suspeitas  
 Uma sessão passa pelos mesmos procedimentos de nova tentativa e tratamento de mensagens suspeitas como uma única mensagem. As propriedades listadas anteriormente para mensagens suspeitas se aplicam à sessão inteira. Isso significa que toda a sessão é repetida e vai para uma fila de mensagens suspeitas final ou a fila de mensagem mortas do remetente se a mensagem for rejeitada.  
  
## <a name="batching-and-poison-messages"></a>Mensagens suspeitas e em lote  
 Se uma mensagem se tornar uma mensagem suspeita e fizer parte de um lote, todo o lote será revertido e o canal retornará para ler uma mensagem por vez. Para obter mais informações sobre o envio em lote, consulte [envio em lote de mensagens em uma transação](batching-messages-in-a-transaction.md)  
  
## <a name="poison-message-handling-for-messages-in-a-poison-queue"></a>Manipulação de mensagens suspeitas para mensagens em uma fila suspeita  
 A manipulação de mensagens suspeitas não termina quando uma mensagem é colocada na fila de mensagens suspeitas. As mensagens na fila de mensagens suspeitas ainda devem ser lidas e tratadas. Você pode usar um subconjunto das configurações de tratamento de mensagens suspeitas ao ler mensagens da subfila de suspeitas final. As configurações aplicáveis são `ReceiveRetryCount` e `ReceiveErrorHandling` . Você pode definir `ReceiveErrorHandling` como remover, rejeitar ou falhar. `MaxRetryCycles`será ignorado e uma exceção será gerada se `ReceiveErrorHandling` for definida como move.  
  
## <a name="windows-vista-windows-server-2003-and-windows-xp-differences"></a>Diferenças do Windows Vista, do Windows Server 2003 e do Windows XP  
 Conforme observado anteriormente, nem todas as configurações de manipulação de mensagens suspeitas se aplicam ao Windows Server 2003 e ao Windows XP. As seguintes diferenças principais entre o enfileiramento de mensagens no Windows Server 2003, no Windows XP e no Windows Vista são relevantes para a manipulação de mensagens suspeitas:  
  
- O enfileiramento de mensagens no Windows Vista dá suporte a subfilas, enquanto o Windows Server 2003 e o Windows XP não dão suporte a subfilas. As subfilas são usadas na manipulação de mensagens suspeitas. As filas de repetição e a fila de suspeitas são subfilas para a fila de aplicativo que é criada com base nas configurações de tratamento de mensagens suspeitas. O `MaxRetryCycles` determina quantas subfilas de repetição criar. Portanto, quando executado no Windows Server 2003 ou no Windows XP, `MaxRetryCycles` é ignorado e `ReceiveErrorHandling.Move` não é permitido.  
  
- O enfileiramento de mensagens no Windows Vista dá suporte à confirmação negativa, enquanto o Windows Server 2003 e o Windows XP não fazem isso. Uma confirmação negativa do Gerenciador de filas de recebimento faz com que o Gerenciador de filas de envio Coloque a mensagem rejeitada na fila de mensagens mortas. Como tal, o `ReceiveErrorHandling.Reject` não é permitido com o Windows Server 2003 e o Windows XP.  
  
- O enfileiramento de mensagens no Windows Vista dá suporte a uma propriedade Message que mantém a contagem do número de vezes que a entrega de mensagens é tentada. Essa propriedade de anulação de contagem não está disponível no Windows Server 2003 e no Windows XP. O WCF mantém a contagem de anulação na memória, portanto, é possível que essa propriedade não contenha um valor preciso quando a mesma mensagem é lida por mais de um serviço WCF em um farm.  
  
## <a name="see-also"></a>Consulte também

- [Visão geral de filas](queues-overview.md)
- [Diferenças de recursos em fila no Windows Vista, Windows Server 2003, e no Windows XP](diff-in-queue-in-vista-server-2003-windows-xp.md)
- [Especificando e lidando com falhas em contratos e serviços](../specifying-and-handling-faults-in-contracts-and-services.md)
