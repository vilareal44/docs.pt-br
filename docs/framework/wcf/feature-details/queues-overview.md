---
title: Visão geral de filas
ms.date: 03/30/2017
helpviewer_keywords:
- queues [WCF], MSMQ integration
ms.assetid: b8757992-ffce-40ad-9e9b-3243f6d0fce1
ms.openlocfilehash: 3e75b6d5926b65a93204241eb7c71ca23a5694af
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596715"
---
# <a name="queues-overview"></a>Visão geral das filas

Esta seção apresenta os conceitos gerais e principais por trás da comunicação em fila. As seções subsequentes entram em detalhes sobre como os conceitos de enfileiramento descritos aqui são manifestados no Windows Communication Foundation (WCF).  
  
## <a name="basic-queuing-concepts"></a>Conceitos básicos de enfileiramento  
 Ao criar um aplicativo distribuído, é importante escolher o transporte correto para comunicação entre serviços e clientes. Vários fatores afetam o tipo de transporte a ser usado. Um fator importante: o isolamento entre o serviço, o cliente e o transporte – determina o uso de um transporte em fila ou de um transporte direto, como TCP ou HTTP. Devido à natureza dos transportes diretos, como TCP e HTTP, a comunicação será totalmente interrompida se o serviço ou o cliente parar de funcionar ou se a rede falhar. O serviço, o cliente e a rede devem estar em execução ao mesmo tempo para que o aplicativo funcione. Os transportes em fila fornecem isolamento, o que significa que, se o serviço ou cliente falhar ou se os links de comunicação entre eles falharem, o cliente e o serviço poderão continuar a funcionar.  
  
 As filas do fornecem comunicação confiável, mesmo com falhas nas partes de comunicação ou na rede. As filas capturam e entregam mensagens trocadas entre as partes de comunicação. Em geral, as filas são apoiadas por algum tipo de armazenamento, que pode ser volátil ou durável. As filas armazenam mensagens de um cliente em nome de um serviço e encaminham essas mensagens posteriormente para o serviço. As filas de indireção fornecem isolamento garantido de falha por qualquer uma das partes, tornando-o o mecanismo de comunicação preferencial para sistemas de alta disponibilidade e serviços desconectados. A indireção vem com o custo de alta latência. *Latência* é o tempo de atraso entre a hora em que o cliente envia uma mensagem e a hora em que o serviço a recebe. Isso significa que depois que uma mensagem é enviada, você não sabe quando essa mensagem pode ser processada. A maioria dos aplicativos na fila lida com alta latência. A ilustração a seguir mostra um modelo conceitual de comunicação em fila.  
  
 ![Modelo de comunicação enfileirada](media/qconceptual-figure1c.gif "QConceptual-Figure1c")  
  
 Modelo conceitual de comunicação na fila  
  
 Na realidade, a fila é um conceito distribuído. Como tal, eles podem ser locais de qualquer parte ou remoto para ambas as partes. Normalmente, a fila é local para o serviço. Nessa configuração, o cliente não pode depender da conectividade com a fila remota disponível constantemente. Da mesma forma, a fila deve estar disponível independentemente da disponibilidade do serviço de leitura da fila. Um Gerenciador de filas gerencia uma coleção de filas. Ele é responsável por aceitar mensagens enviadas para suas filas de outros gerenciadores de filas. Ele também é responsável por gerenciar a conectividade com filas remotas e transferir mensagens para essas filas remotas. Para garantir a disponibilidade das filas apesar das falhas de aplicativos de cliente ou de serviço, o Gerenciador de filas normalmente é executado como um serviço externo.  
  
 Quando um cliente envia uma mensagem para uma fila, ele resolve a mensagem para a fila de destino, que é a fila gerenciada pelo Gerenciador de filas do serviço. O Gerenciador de filas no cliente envia a mensagem para uma fila de transmissão (ou saída). A fila de transmissão é uma fila no Gerenciador de fila de cliente que armazena mensagens para transmissão para a fila de destino. Em seguida, o Gerenciador de filas localiza um caminho para o Gerenciador de filas que possui a fila de destino e transfere a mensagem para ele. Para garantir uma comunicação confiável, os gerenciadores de filas implementam um protocolo de transferência confiável para evitar a perda de dados. O Gerenciador de filas de destino aceita mensagens endereçadas às filas de destino que possui e armazena as mensagens. O serviço faz solicitações para ler da fila de destino, quando o Gerenciador de filas, em seguida, entrega a mensagem para o aplicativo de destino. A ilustração a seguir mostra a comunicação entre as quatro partes.  
  
 ![Diagrama de aplicativos enfileirados](media/distributed-queue-figure.jpg "Distributed-Queue-figura")  
  
 Comunicação em fila em um cenário de implantação típico  
  
 Portanto, o Gerenciador de filas fornece o isolamento necessário para que o remetente e o destinatário possam falhar de forma independente sem afetar a comunicação real. O benefício do indireção extra que as filas fornecem também permite que várias instâncias do aplicativo sejam lidas da mesma fila, de modo que o pecuária funcione entre os nós atinge uma taxa de transferência mais alta. Portanto, não é incomum ver as filas que estão sendo usadas para obter requisitos de escala e taxa de transferência mais altos.  
  
## <a name="queues-and-transactions"></a>Filas e transações  
 As transações permitem que você agrupe um conjunto de operações para que, se uma operação falhar, todas as operações falhem. Um exemplo de como usar transações é quando uma pessoa usa um ATM para transferir $1000 de sua conta de economia para sua conta de verificação. Isso envolve as seguintes operações:  
  
- Retirando $1000 da conta de poupança.  
  
- Depositando $1000 na conta de verificação.  
  
 Se a primeira operação for bem-sucedida e $1000 for retirado da conta de poupança, mas a segunda operação falhar, o $1000 será perdido porque já foi retirado da conta de poupança. Para manter as contas em um estado válido, se uma operação falhar, ambas as operações deverão falhar.  
  
 Em mensagens transacionais, as mensagens podem ser enviadas para a fila e recebidas da fila em uma transação. Portanto, se uma mensagem for enviada em uma transação e a transação for revertida, o resultado será como se a mensagem nunca tivesse sido enviada para a fila. Da mesma forma, se uma mensagem for recebida em uma transação e a transação for revertida, o resultado será como se a mensagem nunca tivesse sido recebida. A mensagem permanece na fila a ser lida.  
  
 Devido à alta latência, ao enviar uma mensagem, você não tem como saber quanto tempo leva para alcançar sua fila de destino, nem sabe quanto tempo leva para o serviço processar a mensagem. Por isso, você não deseja usar uma única transação para enviar a mensagem, receber a mensagem e, em seguida, processar a mensagem. Isso cria uma transação que não é confirmada por um período indeterminado de tempo. Quando um cliente e um serviço se comunicam por meio de uma fila usando uma transação, duas transações são envolvidas: uma no cliente e outra no serviço. A ilustração a seguir mostra os limites de transação na comunicação em fila típica.  
  
 ![Fila com transações](media/qwithtransactions-figure3.gif "QWithTransactions-Figure3")  
  
 Comunicação em fila mostrando transações separadas para captura e entrega  
  
 A transação do cliente processa e envia a mensagem. Quando a transação é confirmada, a mensagem está na fila de transmissão. No serviço, a transação lê a mensagem da fila de destino, processa a mensagem e, em seguida, confirma a transação. Se ocorrer um erro durante o processamento, a mensagem será revertida e colocada na fila de destino.  
  
## <a name="asynchronous-communication-using-queues"></a>Comunicação assíncrona usando filas  
 As filas do fornecem um meio assíncrono de comunicação. Os aplicativos que enviam mensagens usando filas não podem aguardar a mensagem ser recebida e processada pelo receptor devido à alta latência introduzida pelo Gerenciador de filas. As mensagens podem permanecer na fila por um tempo muito maior do que o esperado pelo aplicativo. Para evitar isso, o aplicativo pode especificar um valor de vida útil na mensagem. Esse valor especifica quanto tempo a mensagem deve permanecer na fila de transmissão. Se esse valor de tempo for excedido e a mensagem ainda não tiver sido enviada para a fila de destino, a mensagem poderá ser transferida para uma fila de mensagens mortas.  
  
 Quando o remetente envia uma mensagem, o retorno da operação de envio implica que a mensagem o fez apenas na fila de transmissão no remetente. Assim, se houver uma falha ao obter a mensagem para a fila de destino, o aplicativo de envio não poderá conhecê-la imediatamente. Para anotar essas falhas, a mensagem com falha é transferida para uma fila de mensagens mortas.  
  
 Qualquer erro, como uma mensagem que não consegue acessar a fila de destino ou a expiração de tempo de vida, deve ser processado separadamente. Não é incomum, portanto, para que os aplicativos em fila escrevam dois conjuntos de lógica:  
  
- O cliente normal e a lógica de serviço do envio e recebimento de mensagens.  
  
- Lógica de compensação para tratar mensagens da transmissão ou entrega com falha.  
  
 As seções a seguir discutem esses conceitos.  
  
## <a name="dead-letter-queue-programming"></a>Programação de fila de mensagens mortas  
 As filas de mensagens mortas contêm mensagens que falharam ao alcançar a fila de destino por vários motivos. Os motivos podem variar de mensagens expiradas a problemas de conectividade, impedindo a transferência da mensagem para a fila de destino.  
  
 Normalmente, um aplicativo pode ler mensagens de uma fila de mensagens mortas de todo o sistema, determinar o que deu errado e tomar as devidas medidas, como corrigir os erros e reenviar a mensagem ou anotar isso.  
  
## <a name="poison-message-queue-programming"></a>Programação de fila de mensagens suspeitas  
 Depois que uma mensagem é transformada na fila de destino, o serviço pode falhar repetidamente ao processar a mensagem. Por exemplo, um aplicativo que lê uma mensagem da fila em uma transação e a atualização de um banco de dados pode encontrar o banco de dados temporariamente desconectado. Nesse caso, a transação é revertida, uma nova transação é criada e a mensagem é relida da fila. Uma segunda tentativa pode ser bem-sucedida ou falhar. Em alguns casos, dependendo da causa do erro, a mensagem pode falhar repetidamente com a entrega para o aplicativo. Nesse caso, a mensagem é considerada "suspeita". Essas mensagens são movidas para uma fila suspeita que pode ser lida por um aplicativo de tratamento de envenenamento.  
  
## <a name="see-also"></a>Consulte também

- [Enfileiramento no WCF](queuing-in-wcf.md)
- [Sessões e filas](../samples/sessions-and-queues.md)
- [Filas de mensagens de inatividade](../samples/dead-letter-queues.md)
- [Comunicação em fila volátil](../samples/volatile-queued-communication.md)
- [Windows Communication Foundation para enfileiramento de mensagens](../samples/wcf-to-message-queuing.md)
- [Instalando o Enfileiramento de Mensagens (MSMQ)](../samples/installing-message-queuing-msmq.md)
- [Enfileiramento de mensagens para o Windows Communication Foundation](../samples/message-queuing-to-wcf.md)
- [Segurança de mensagem através do enfileiramento de mensagem](../samples/message-security-over-message-queuing.md)
