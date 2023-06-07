---
title: Limitando a distribuição de mensagens
ms.date: 03/30/2017
ms.assetid: 8b5ec4b8-1ce9-45ef-bb90-2c840456bcc1
ms.openlocfilehash: 188d7bd365caad7d4cd438744c78ae8e7cd95e7e
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84586306"
---
# <a name="limiting-message-distribution"></a>Limitando a distribuição de mensagens

O canal par é por meio do design de uma malha de difusão. Seu modelo de inundação básico envolve a distribuição de cada mensagem enviada por qualquer membro de uma malha para todos os outros membros dessa malha. Isso é ideal em situações em que cada mensagem gerada por um membro é relevante e útil para todos os outros membros (por exemplo, uma sala de chat). No entanto, muitos aplicativos têm uma necessidade ocasional de limitar a distribuição de mensagens. Por exemplo, se um novo membro ingressar em uma malha e quiser recuperar a última mensagem enviada por meio da malha, essa solicitação não precisará ser inundada para todos os membros da malha. A solicitação pode ser limitada a vizinhos próximos ou as mensagens geradas localmente podem ser filtradas. As mensagens também podem ser enviadas a um nó individual na malha. Este tópico discute o uso da contagem de saltos, um filtro de propagação de mensagens, um filtro local ou uma conexão direta para controlar como as mensagens são encaminhadas em toda a malha e fornece diretrizes gerais para escolher uma abordagem.

## <a name="hop-counts"></a>Contagens de saltos

O conceito de `PeerHopCount` é semelhante ao TTL (vida útil) usado no protocolo IP. O valor de `PeerHopCount` é vinculado a uma instância de mensagem e especifica quantas vezes uma mensagem deve ser encaminhada antes de ser descartada. Cada vez que uma mensagem é recebida por um cliente de canal par, o cliente examina a mensagem para ver se `PeerHopCount` é especificado. Se for especificado, o cliente decrementará o valor da contagem de saltos em um antes de encaminhar a mensagem para os nós vizinhos. Quando um cliente recebe uma mensagem com um valor de contagem de salto igual a zero, o cliente processa a mensagem, mas não encaminha a mensagem aos vizinhos.

A contagem de saltos pode ser adicionada a uma mensagem adicionando-se `PeerHopCount` como um atributo à propriedade ou ao campo aplicável na implementação da classe de mensagem. Você pode definir isso com um valor específico antes de enviar a mensagem para a malha. Dessa maneira, você pode usar a contagem de saltos para limitar a distribuição de mensagens em toda a malha quando necessário, evitando potencialmente a duplicação de mensagens desnecessárias. Isso é útil em casos em que a malha contém uma grande quantidade de dados redundantes, ou para enviar uma mensagem para vizinhos imediatos ou vizinhos dentro de alguns saltos.

- Para obter trechos de código e informações relacionadas, consulte o [atributo PeerHopCount: controle](https://docs.microsoft.com/archive/blogs/peerchan/the-peerhopcount-attribute-controlling-message-distribution) post de distribuição de mensagens no blog do canal par.

## <a name="message-propagation-filter"></a>Filtro de propagação de mensagem

`MessagePropagationFilter`pode ser usado para o controle personalizado de inundação de mensagem, especialmente quando o conteúdo da mensagem ou outros cenários específicos determinam a propagação. O filtro toma decisões de propagação para cada mensagem que passa pelo nó. Isso é verdadeiro para mensagens originadas em outro lugar na malha que seu nó recebeu, bem como mensagens criadas pelo seu aplicativo. O filtro tem acesso à mensagem e à sua origem, portanto, as decisões sobre o encaminhamento ou descarte da mensagem podem se basear nas informações completas disponíveis.

<xref:System.ServiceModel.PeerMessagePropagationFilter>é uma classe abstrata de base com uma única função, <xref:System.ServiceModel.PeerMessagePropagationFilter.ShouldMessagePropagate%2A> . O primeiro argumento da chamada do método passa em uma cópia completa da mensagem. As alterações feitas na mensagem não afetam a mensagem real. O último argumento da chamada de método identifica a origem da mensagem ( `PeerMessageOrigination.Local` ou `PeerMessageOrigination.Remote` ). Implementações concretas desse método devem retornar uma constante da <xref:System.ServiceModel.PeerMessagePropagation> enumeração que indica que a mensagem deve ser encaminhada para o aplicativo local ( `Local` ), encaminhada para clientes remotos ( `Remote` ), ambos ( `LocalAndRemote` ) ou nenhum () `None` . Esse filtro pode ser aplicado acessando o `PeerNode` objeto correspondente e especificando uma instância da classe de filtro de propagação derivada na `PeerNode.MessagePropagationFilter` propriedade. Verifique se o filtro de propagação está anexado antes de abrir o canal par.

- Para obter trechos de código e informações relacionadas, consulte o [canal par e o MessagePropagationFilter](https://docs.microsoft.com/archive/blogs/peerchan/peer-channel-and-messagepropagationfilter) post no blog do canal par.

## <a name="contacting-an-individual-node-in-the-mesh"></a>Contatando um nó individual na malha

Um nó individual em uma malha pode ser contatado Configurando um filtro local ou configurando uma conexão direta.

Se os nós em uma malha tiverem uma ID individual, uma ID de destino poderá ser especificada na implementação de sua mensagem. Um filtro local pode ser configurado escrevendo uma função em seu contrato de mensagem que só exibirá a mensagem para o nó atual se sua ID corresponder à ID de destino especificada. A malha transporta a mensagem, portanto, a sobrecarga de configurar uma nova conexão não precisa ser incorrida. No entanto, há uma perda de eficiência, pois a mensagem é enviada muitas vezes em toda a malha. Isso funciona bem para enviar mensagens a membros individuais de uma malha, contanto que as mensagens não sejam muito grandes nem muito frequentes.

Para conexões de longa largura de banda larga, as conexões diretas são preferíveis. Você pode enviar informações de conexão pela malha e, em seguida, configurar uma conexão direta de sua escolha para enviar/receber mensagens.

## <a name="choosing-an-approach-for-limiting-message-distribution"></a>Escolhendo uma abordagem para limitar a distribuição de mensagens

Ao descobrir um cenário no qual você precisa limitar a distribuição de mensagens, faça as seguintes perguntas:

- **Quem** precisa receber a mensagem? Apenas um nó vizinho? Um nó em algum outro lugar na malha? Metade da malha?

- **Com que frequência** esta mensagem será enviada?

- Que tipo de **largura de banda** essa mensagem usará?

As respostas para essas perguntas podem ajudá-lo a determinar se deve usar a contagem de saltos, um filtro de propagação de mensagens, um filtro local ou uma conexão direta. Considere as seguintes diretrizes gerais:

- **Usuários**

  - *Nó individual*: filtro local ou conexão direta.

  - *Vizinhos em uma determinada vizinhança*: PeerHopCount.

  - *Subconjunto complexo da malha*: MessagePropagationFilter.

- **Com que frequência**

  - *Muito frequente*: conexão direta, PeerHopCount, MessagePropagationFilter.

  - *Ocasional*: filtro local.

- **Uso de largura de banda**

  - *Alta*: conexão direta, menos aconselhada para usar o MessagePropagationFilter ou o filtro local.

  - *Baixa*: any, a conexão direta provavelmente não é necessária.

## <a name="see-also"></a>Consulte também

- [Compilando um aplicativo de canal par](building-a-peer-channel-application.md)
