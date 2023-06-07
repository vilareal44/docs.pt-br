---
title: Diferenças de recursos em fila no Windows Vista, Windows Server 2003, e no Windows XP
ms.date: 03/30/2017
helpviewer_keywords:
- queues [WCF], differences in operating systems
ms.assetid: aa809d93-d0a3-4ae6-a726-d015cca37c04
ms.openlocfilehash: abd81b5e7bf611fc6b4f446a82628b83130f2d54
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599198"
---
# <a name="differences-in-queuing-features-in-windows-vista-windows-server-2003-and-windows-xp"></a>Diferenças de recursos em fila no Windows Vista, Windows Server 2003, e no Windows XP
Este tópico resume as diferenças no recurso de filas do Windows Communication Foundation (WCF) entre o Windows Vista, o Windows Server 2003 e o Windows XP.  
  
## <a name="application-specific-dead-letter-queue"></a>Fila de mensagens mortas específica do aplicativo  
 As mensagens em fila podem permanecer na fila indefinidamente se o aplicativo de recebimento não lê-las em tempo hábil. Esse comportamento não é aconselhável se as mensagens forem sensíveis ao tempo. As mensagens com detecção de hora têm uma `TimeToLive` propriedade definida na associação em fila. Essa propriedade indica por quanto tempo as mensagens podem estar na fila antes de expirarem. As mensagens expiradas são enviadas para uma fila especial chamada fila de mensagens mortas. Uma mensagem também pode acabar em uma fila de mensagens mortas por outros motivos, como exceder uma cota de fila ou apresentar uma falha de autenticação.  
  
 Normalmente, uma única fila de mensagens mortas de todo o sistema existe para todos os aplicativos em fila que compartilham um Gerenciador de filas. Uma fila de mensagens mortas para cada aplicativo permite um melhor isolamento entre os aplicativos em fila que compartilham um Gerenciador de filas, permitindo que esses aplicativos especifiquem sua própria fila de mensagens mortas específica do aplicativo. Um aplicativo que compartilha uma fila de mensagens mortas com outros aplicativos tem que procurar na fila para localizar as mensagem que se aplicam a ela. Com uma fila de mensagens mortas específica do aplicativo, o aplicativo pode ter certeza de que todas as mensagens em sua fila de mensagens mortas são aplicáveis a ela.  
  
 O Windows Vista fornece filas de mensagens mortas específicas do aplicativo. As filas de mensagens mortas específicas do aplicativo não estão disponíveis no Windows Server 2003 e no Windows XP, e os aplicativos devem usar a fila de mensagens mortas de todo o sistema.  
  
## <a name="poison-message-handling"></a>Manipulação de mensagens suspeitas  
 Uma mensagem suspeita é uma mensagem que excedeu o número máximo de tentativas de entrega para o aplicativo de recebimento. Essa situação pode surgir quando um aplicativo que lê uma mensagem de uma fila transacional não pode processar a mensagem imediatamente devido a erros. Se o aplicativo abortar a transação na qual a mensagem em fila foi recebida, ela retornará a mensagem para a fila. Em seguida, o aplicativo tenta recuperar a mensagem novamente em uma nova transação. Se o problema que causa o erro não for corrigido, o aplicativo receptor poderá ficar preso em um loop recebendo e anulando a mesma mensagem até que ela exceda o número máximo de tentativas de entrega e um resultado de mensagem suspeita.  
  
 As principais diferenças entre o MSMQ (enfileiramento de mensagens) no Windows Vista, no Windows Server 2003 e no Windows XP que são relevantes para o tratamento de suspeitas incluem o seguinte:  
  
- O MSMQ no Windows Vista dá suporte a subfilas, enquanto o Windows Server 2003 e o Windows XP não dão suporte a subfilas. As subfilas são usadas na manipulação de mensagens suspeitas. As filas de repetição e a fila de suspeitas são subfilas para a fila de aplicativo que é criada com base nas configurações de tratamento de mensagens suspeitas. O `MaxRetryCycles` determina quantas subfilas de repetição criar. Portanto, quando executado no Windows Server 2003 ou no Windows XP, `MaxRetryCycles` é ignorado e `ReceiveErrorHandling.Move` não é permitido.  
  
- O MSMQ no Windows Vista dá suporte à confirmação negativa, enquanto o Windows Server 2003 e o Windows XP não fazem isso. Uma confirmação negativa do Gerenciador de filas de recebimento faz com que o Gerenciador de filas de envio Coloque a mensagem rejeitada na fila de mensagens mortas. Como tal, o `ReceiveErrorHandling.Reject` não é permitido com o Windows Server 2003 e o Windows XP.  
  
- O MSMQ no Windows Vista oferece suporte a uma propriedade de mensagem que mantém a contagem do número de vezes que a entrega de mensagem é tentada. Essa propriedade de anulação de contagem não está disponível no Windows Server 2003 e no Windows XP. O WCF mantém a contagem de anulação na memória, portanto, é possível que essa propriedade não contenha um valor preciso quando a mesma mensagem é lida por mais de um serviço WCF em um Web farm.  
  
## <a name="remote-transactional-read"></a>Leitura transacional remota  
 O MSMQ no Windows Vista dá suporte a leituras transacionais remotas. Isso permite que um aplicativo que está lendo de uma fila seja hospedado em um computador diferente do computador no qual a fila está hospedada. Isso garante a capacidade de ter um farm de serviços de leitura de uma fila central, o que aumenta a taxa de transferência geral do sistema. Ele também garante que, se ocorrer uma falha durante a leitura e o processamento da mensagem, a transação será revertida e a mensagem permanecerá na fila para processamento posterior.  
  
## <a name="see-also"></a>Consulte também

- [Usando filas de mensagens mortas para lidar com falhas de transferência de mensagem](using-dead-letter-queues-to-handle-message-transfer-failures.md)
- [Manipulação de mensagens suspeitas](poison-message-handling.md)
