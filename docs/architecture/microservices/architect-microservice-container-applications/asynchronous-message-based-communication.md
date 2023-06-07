---
title: Comunicação assíncrona baseada em mensagens
description: Arquitetura de Microsserviços do .NET para Aplicativos .NET em Contêineres | A comunicação assíncrona baseada em mensagens é um conceito essencial na arquitetura de microsserviços, porque ela é a melhor maneira de manter microsserviços independentes uns dos outros, estando também simultaneamente sincronizados no fim das contas.
ms.date: 09/20/2018
ms.openlocfilehash: a8af94540a7906c474b9b784c28aa60ebae0a6e3
ms.sourcegitcommit: 5280b2aef60a1ed99002dba44e4b9e7f6c830604
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84306962"
---
# <a name="asynchronous-message-based-communication"></a>Comunicação assíncrona baseada em mensagens

Mensagens assíncronas e comunicação controlada por evento são críticos ao propagar alterações entre vários microsserviços e seus modelos de domínio relacionados. Conforme mencionado anteriormente na discussão, microsserviços e BCs (Contextos Limitados), modelos (Usuário, Cliente, Produto, Conta etc.) podem ter diferentes significados para diferentes microsserviços ou BCs. Isso significa que, quando ocorrem alterações, você precisa de algum modo de reconciliar essas alterações entre diferentes modelos. Uma solução é consistência eventual e comunicação controlada por evento com base em mensagens assíncronas.

Ao usar mensagens, processos se comunicam trocando mensagens de maneira assíncrona. Um cliente faz um comando ou uma solicitação a um serviço ao enviar uma mensagem a ele. Se o serviço precisar responder, ele enviará uma mensagem diferente de volta ao cliente. Como é uma comunicação baseada em mensagens, o cliente presume que a resposta não será recebida imediatamente e que poderá não haver resposta alguma.

Uma mensagem é composta por um cabeçalho (metadados como informações de identificação ou de segurança) e um corpo. As mensagens geralmente são enviadas por meio de protocolos assíncronos, como AMQP.

A infraestrutura preferencial para esse tipo de comunicação na comunidade de microsserviços é um agente de mensagem leve, que é diferente de agentes e orquestradores grandes usados em SOA. Em um agente de mensagem leve, a infraestrutura é normalmente "inútil," atuando somente como um agente de mensagens, com implantações simples como RabbitMQ ou um barramento de serviço escalonável na nuvem como o Barramento de Serviço do Azure. Nesse cenário, a maioria do pensamento "inteligente" ainda está nos pontos de extremidade que estão produzindo e consumindo mensagens; ou seja, nos microsserviços.

Outra regra que você deve tentar seguir o máximo possível é usar apenas mensagens assíncronas entre os serviços internos e usar comunicação síncrona (como HTTP) apenas dos aplicativos cliente para os serviços de front-end (gateways de API mais o primeiro nível de microsserviços).

Há dois tipos de comunicação assíncrona de mensagens: comunicação baseada em mensagens do receptor único e comunicação baseada em mensagens de vários receptores. As seções a seguir fornecem detalhes sobre eles.

## <a name="single-receiver-message-based-communication"></a>Comunicação baseada em mensagem de destinatário único

Comunicação assíncrona baseada em mensagem com um único destinatário significa que há comunicação ponto a ponto que entrega uma mensagem a exatamente um dos consumidores que está lendo do canal e que essa mensagem é processada apenas uma vez. No entanto, existem situações especiais. Por exemplo, em um sistema de nuvem que tenta se recuperar automaticamente de falhas, a mesma mensagem pode ser enviada várias vezes. Devido à rede ou outras falhas, o cliente deve ser capaz de tentar novamente enviar as mensagens, e o servidor precisa implementar uma operação para ser idempotente para processar uma mensagem específica apenas uma vez.

A comunicação baseada em mensagem destinatário único é especialmente adequada para enviar comandos assíncronos de um microsserviço para outro, conforme mostrado na Figura 4-18 que ilustra essa abordagem.

Depois de começar a enviar comunicação baseada em mensagens (seja com comandos ou eventos), você deve evitar misturar comunicação baseada em mensagem com comunicação HTTP síncrona.

![Um único microsserviço recebendo uma mensagem assíncrona](./media/asynchronous-message-based-communication/single-receiver-message-based-communication.png)

**Figura 4-18**. Um único microsserviço recebendo uma mensagem assíncrona

Quando os comandos são provenientes de aplicativos cliente, eles podem ser implementados como comandos síncronos HTTP. Use comandos baseados em mensagem quando precisar de maior escalabilidade ou quando já estiver em um processo de negócios baseado em mensagem.

## <a name="multiple-receivers-message-based-communication"></a>Comunicação baseada em mensagens de vários destinatários

Como uma abordagem mais flexível, você também poderá usar um mecanismo de publicação/assinatura para que a sua comunicação do remetente esteja disponível a microsserviços de assinante adicionais ou para aplicativos externos. Portanto, ele o ajuda a seguir o [princípio de abrir/fechar](https://en.wikipedia.org/wiki/Open/closed_principle) no serviço de envio. Dessa forma, mais assinantes podem ser adicionados no futuro sem necessidade de modificar o serviço do remetente.

Ao usar uma comunicação de publicação/assinatura, talvez você esteja usando uma interface de barramento de eventos para publicar eventos a qualquer assinante.

## <a name="asynchronous-event-driven-communication"></a>Comunicação controlada por evento assíncrono

Ao usar comunicação controlada por evento assíncrono, um microsserviço publica um evento de integração quando acontece algo em seu domínio e outro microsserviço precisa estar ciente disso, como uma alteração de preço em um microsserviço do catálogo de produtos. Microsserviços adicionais assinam os eventos para que possam recebê-los de maneira assíncrona. Quando isso acontece, os receptores podem atualizar as próprias entidades de domínio, o que podem causar a publicação de mais eventos de integração. Este sistema de publicação/assinatura normalmente é executado por meio de uma implementação de um barramento de evento. O barramento de evento pode ser projetado como uma abstração ou interface, com a API que é necessária para assinar ou cancelar a assinatura de eventos e para publicar eventos. O barramento de evento também pode ter uma ou mais implementações com base em qualquer agente de mensagens e entre processos, como uma fila de mensagens ou barramento de serviço que seja compatível com a comunicação assíncrona e um modelo de publicação/assinatura.

Se um sistema usa consistência eventual orientada por eventos de integração, é recomendável que essa abordagem fique completamente clara para o usuário final. O sistema não deve usar uma abordagem que simule eventos de integração, como sistemas de sondagem ou SignalR do cliente. O usuário final e o proprietário da empresa precisam adotar explicitamente consistência eventual no sistema e observar que, em muitos casos, os negócios não têm nenhum problema com essa abordagem, desde que ela seja explícita. Isso é importante porque os usuários podem esperar ver alguns resultados imediatamente e isso pode não acontecer com a consistência eventual.

Conforme observado anteriormente na seção [Desafios e soluções para o gerenciamento de dados distribuídos](distributed-data-management.md), você pode usar eventos de integração para implementar as tarefas de negócios que abrangem vários microsserviços. Assim, você terá consistência eventual entre esses serviços. Uma transação eventualmente consistente é composta por uma coleção de ações distribuídas. Em cada ação, o microsserviço relacionado atualiza uma entidade de domínio e publica outro evento de integração que gera a próxima ação dentro da mesma tarefa comercial de ponta a ponta.

Um ponto importante é que você pode querer comunicar-se com vários microsserviços inscritos para o mesmo evento. Para fazer isso, você pode usar mensagens de publicação/assinatura com base em comunicação controlada por evento, conforme mostra a Figura 4-19. Esse mecanismo de publicação/assinatura não é exclusivo da arquitetura de microsserviço. Ele é semelhante à maneira como [Contextos Limitados](https://martinfowler.com/bliki/BoundedContext.html) no DDD devem se comunicar ou à maneira como você propaga atualizações do banco de dados de gravação para o banco de dados de leitura no padrão de arquitetura de [CQRS (Segregação de Responsabilidade de Comando e Consulta)](https://martinfowler.com/bliki/CQRS.html). A meta é ter consistência eventual entre várias fontes de dados em seu sistema distribuído.

![Diagrama mostrando comunicações assíncronas controladas por eventos.](./media/asynchronous-message-based-communication/asynchronous-event-driven-communication.png)

**Figura 4-19**. Comunicação de mensagem controlada por evento assíncrono

Na comunicação assíncrona orientada por evento, um microserviço publica eventos em um barramento de evento e muitos microserviços podem assiná-lo, para ser notificado e agir sobre ele. Sua implementação determinará o protocolo a ser usado para comunicações controladas por eventos e baseadas em mensagem. O [AMQP](https://en.wikipedia.org/wiki/Advanced_Message_Queuing_Protocol) pode ajudar a obter uma comunicação confiável na fila.

Ao usar um barramento de evento, talvez você queira usar um nível de abstração (como uma interface de barramento de evento) com base em uma implementação relacionada em classes com o código usando a API de um agente de mensagens como [RabbitMQ](https://www.rabbitmq.com/) ou um barramento de serviço como [Barramento de Serviço do Azure com Tópicos](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions). Como alternativa, você talvez queira usar um barramento de serviço de nível superior, como NServiceBus, MassTransit ou Brighter para articular seu barramento de evento e sistema de publicação/assinatura.

## <a name="a-note-about-messaging-technologies-for-production-systems"></a>Uma observação sobre tecnologias para mensagens para sistemas de produção

As tecnologias de mensagens disponíveis para implementar seu barramento do evento abstrato estão em diferentes níveis. Por exemplo, produtos, como RabbitMQ (um transporte do agente de mensagens) e o Barramento de Serviço do Azure ficam em um nível inferior a outros produtos, como NServiceBus, MassTransit ou Brighter, que podem funcionar sobre RabbitMQ e o Barramento de Serviço do Azure. Sua escolha depende de quantos recursos avançados no nível do aplicativo e escalabilidade imediata você precisa para seu aplicativo. Para implementar apenas um barramento de evento de prova de conceito para seu ambiente de desenvolvimento, como foi feito no exemplo de eShopOnContainers, pode ser suficiente uma implementação simples sobre RabbitMQ em execução em um contêiner do Docker.

No entanto, para sistemas críticos e de produção que precisam de hiperescalabilidade, talvez você queira avaliar o Barramento de Serviço do Azure. Para abstrações de alto nível e recursos que tornam o desenvolvimento de aplicativos distribuídos mais fácil, recomendamos que você avalie outros barramentos de serviço de software livre e comerciais, como NServiceBus, MassTransit e Brighter. É claro que você pode criar seus próprios recursos de barramento de serviço sobre tecnologias de nível inferior, como RabbitMQ e Docker. Mas esse trabalho pode custar muito caro para um aplicativo empresarial personalizado.

## <a name="resiliently-publishing-to-the-event-bus"></a>Publicação resiliente para o barramento de evento

Um desafio ao implementar uma arquitetura orientada a eventos em vários microsserviços é como atualizar atomicamente o estado no microsserviço original enquanto publica de maneira resiliente o evento de integração relacionado no barramento de evento, de alguma maneira baseado em transações. A seguir estão algumas maneiras de fazer isso, embora haja outras abordagens também.

- Usando uma fila transacional (baseada em DTC) como MSMQ. (No entanto, essa é uma abordagem herdada.)

- Usando [mineração do log de transações](https://www.scoop.it/t/sql-server-transaction-log-mining).

- Usando o padrão [Event Sourcing](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing) completo.

- Usando o [Padrão de caixa de saída](https://www.kamilgrzybek.com/design/the-outbox-pattern/): uma tabela de banco de dados transacional como uma fila de mensagens que será a base de um componente do criador do evento que criará e publicará o evento.

Tópicos adicionais a serem considerados ao usar comunicação assíncrona são idempotência de mensagem e eliminação de duplicação de mensagem. Esses tópicos são abordados na seção [Implementar a comunicação baseada em eventos entre microsserviços (eventos de integração)](../multi-container-microservice-net-applications/integration-event-based-microservice-communications.md) mais adiante neste guia.

## <a name="additional-resources"></a>Recursos adicionais

- **Mensagens controladas por evento** \
  <https://soapatterns.org/design_patterns/event_driven_messaging>

- **Canal de publicação/assinatura** \
  <https://www.enterpriseintegrationpatterns.com/patterns/messaging/PublishSubscribeChannel.html>

- **Udi Dahan. CQRS esclarecido** \
  <https://udidahan.com/2009/12/09/clarified-cqrs/>

- **Separação das Operações de Comando e de Consulta (CQRS)** \
  <https://docs.microsoft.com/azure/architecture/patterns/cqrs>

- **Comunicação entre contextos limitados** \
  <https://docs.microsoft.com/previous-versions/msp-n-p/jj591572(v=pandp.10)>

- **Consistência eventual** \
  <https://en.wikipedia.org/wiki/Eventual_consistency>

- **Jimmy Bogard. Refatoração em relação à resiliência: avaliando o acoplamento** \
  <https://jimmybogard.com/refactoring-towards-resilience-evaluating-coupling/>

> [!div class="step-by-step"]
> [Anterior](communication-in-microservice-architecture.md) 
>  [Avançar](maintain-microservice-apis.md)
