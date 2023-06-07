---
title: Eventos de domínio. design e implementação
description: Arquitetura de Microsserviços .NET para aplicativos .NET em contêineres | Obtenha uma visão detalhada dos eventos de domínio, um conceito fundamental para estabelecer a comunicação entre agregações.
ms.date: 10/08/2018
ms.openlocfilehash: 630bd0a0b060431e565df98faa77f452e2045fa2
ms.sourcegitcommit: ee5b798427f81237a3c23d1fd81fff7fdc21e8d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84144299"
---
# <a name="domain-events-design-and-implementation"></a>Eventos de domínio: design e implementação

Use eventos de domínio para implementar explicitamente os efeitos colaterais de alterações em seu domínio. Em outras palavras, e usando terminologia DDD, use eventos de domínio para implementar explicitamente efeitos colaterais entre várias agregações. Opcionalmente, para melhor escalabilidade e menor impacto em bloqueios de banco de dados, use consistência eventual entre agregações dentro do mesmo domínio.

## <a name="what-is-a-domain-event"></a>O que é um evento de domínio?

Um evento é algo que ocorreu no passado. Um evento de domínio é algo que ocorreu no domínio que você deseja que outras partes do mesmo domínio (em processo) tenham conhecimento. As partes notificadas geralmente reagem de alguma forma aos eventos.

Um benefício importante dos eventos de domínio é que os efeitos colaterais podem ser expressos explicitamente.

Por exemplo, se você estivesse usando apenas o Entity Framework e precisasse haver uma reação a um evento, provavelmente você codificaria tudo o que precisa perto do que dispara o evento. Portanto, a regra fica acoplada, implicitamente, ao código e você precisa examinar o código para, com sorte, perceber que a regra está implementada lá.

Por outro lado, usar eventos de domínio torna o conceito explícito, porque há um `DomainEvent` e pelo menos um `DomainEventHandler` envolvidos.

Por exemplo, no aplicativo eShopOnContainers, quando um pedido é criado, o usuário se torna um comprador, portanto, um `OrderStartedDomainEvent` será disparado e tratado no `ValidateOrAddBuyerAggregateWhenOrderStartedDomainEventHandler`, de forma que o conceito subjacente é evidente.

Em resumo, eventos de domínio ajudam você a expressar, explicitamente, as regras de domínio, com base na linguagem ubíqua fornecida pelos especialistas do domínio. Os eventos de domínio também permitem uma melhor separação de interesses entre classes dentro do mesmo domínio.

É importante garantir que, assim como uma transação de banco de dados, todas as operações relacionadas a um evento de domínio sejam concluídas com êxito ou nenhuma delas seja.

Os eventos de domínio são parecidos com eventos do estilo de mensagens, com uma diferença importante. Com mensagens reais, enfileiramento de mensagens, agentes de mensagens ou um barramento de serviço que usa o AMQP, uma mensagem é sempre enviada de forma assíncrona e comunicada entre processos e computadores. Isso é útil para a integração de vários contextos delimitados, microsserviços ou até mesmo aplicativos diferentes. No entanto, com os eventos de domínio, ao acionar um evento na operação de domínio em execução no momento, você deseja que os efeitos colaterais ocorram dentro do mesmo domínio.

Os eventos de domínio e seus efeitos colaterais (as ações disparadas depois que são gerenciadas por manipuladores de eventos) devem ocorrer quase imediatamente, geralmente em processo, e dentro do mesmo domínio. Assim, os eventos de domínio podem ser síncronos ou assíncronos. Os eventos de integração, no entanto, devem sempre ser assíncronos.

## <a name="domain-events-versus-integration-events"></a>Eventos de domínio versus eventos de integração

Semanticamente, os eventos de integração e de domínio são a mesma coisa: notificações sobre algo que acabou de ocorrer. No entanto, a implementação deles deve ser diferente. Os eventos de domínio são apenas mensagens enviadas por push para um dispatcher de evento de domínio, que pode ser implementado como um mediador na memória, com base em um contêiner de IoC ou qualquer outro método.

Por outro lado, a finalidade dos eventos de integração é a propagação de transações e atualizações confirmadas para outros subsistemas, independentemente de serem outros microsserviços, contextos delimitados ou, até mesmo, aplicativos externos. Assim, eles deverão ocorrer somente se a entidade for persistida com êxito, caso contrário, será como se toda a operação nunca tivesse acontecido.

Conforme o que foi mencionado antes, os eventos de integração devem ser baseados em comunicação assíncrona entre vários microsserviços (outros contextos delimitados) ou mesmo aplicativos/sistemas externos.

Assim, a interface do barramento de eventos precisa de alguma infraestrutura que permita a comunicação entre processos e distribuída entre serviços potencialmente remotos. Ela pode ser baseada em um barramento de serviço comercial, em filas, em um banco de dados compartilhado usado como uma caixa de correio ou em qualquer outro sistema de mensagens distribuídas e, idealmente, baseado em push.

## <a name="domain-events-as-a-preferred-way-to-trigger-side-effects-across-multiple-aggregates-within-the-same-domain"></a>Eventos de domínio como uma maneira preferencial para disparar efeitos colaterais entre várias agregações dentro do mesmo domínio

Se a execução de um comando relacionado a uma instância de agregação exigir regras de domínio adicionais para ser executado em uma ou mais agregações, você deverá projetar e implementar esses efeitos colaterais para que sejam disparados por eventos de domínio. Conforme mostrado na Figura 7-14 e como um dos mais importantes casos de uso, um evento de domínio deve ser usado para propagar alterações de estado entre várias agregações dentro do mesmo modelo de domínio.

![Diagrama mostrando um evento de domínio que controla dados para um comprador agregado.](./media/domain-events-design-implementation/domain-model-ordering-microservice.png)

**Figura 7-14**. Eventos de domínio para impor consistência entre várias agregações dentro do mesmo domínio

A Figura 7-14 mostra como a consistência entre agregações é obtida por eventos de domínio. Quando o usuário inicia uma ordem, a agregação de ordem envia um `OrderStarted` evento de domínio. O evento de domínio OrderStarted é tratado pelo comprador agregado para criar um objeto de comprador no microserviço de ordenação, com base nas informações do usuário original do microserviço de identidade (com as informações fornecidas no comando CreateOrder).

Como alternativa, você pode fazer com que a raiz da agregação assine eventos acionado pelos membros de suas respectivas agregações (entidades filho). Por exemplo, cada entidade filho OrderItem poderá acionar um evento quando o preço do item for maior que um valor específico, ou quando a quantidade de itens do produto for muito alta. Assim, a raiz de agregação poderá receber esses eventos e executar um cálculo global ou uma agregação.

É importante entender que essa comunicação baseada em eventos não é implementada diretamente nas agregações; você precisa implementar manipuladores de eventos de domínio.

A manipulação de eventos de domínio é um interesse do aplicativo. A camada do modelo de domínio deve se concentrar apenas na lógica do domínio, algo que um especialista em domínio entende, e não na infraestrutura do aplicativo, como manipuladores e ações de persistência de efeito colateral com o uso de repositórios. Portanto, o nível de camada de aplicativo é o local em que você deve ter manipuladores de eventos de domínio disparando ações quando um evento de domínio é acionado.

Os eventos de domínio também podem ser usados para disparar um grande número de ações de aplicativo e, o mais importante, devem estar abertos para aumentar esse número no futuro de maneira separada. Por exemplo, quando o pedido é iniciado, você publica um evento de domínio para propagar essas informações para outras agregações ou, até mesmo, para gerar ações de aplicativo, como notificações.

O ponto-chave é o número indefinido de ações a serem executadas quando ocorre um evento de domínio. As ações e regras do domínio e do aplicativo vão, eventualmente, aumentar. A complexidade ou o número de ações de efeito colateral quando algo acontecer aumentará, mas se o seu código estivesse acoplado a "cola" (ou seja, criando objetos específicos com `new` ), sempre que for necessário adicionar uma nova ação, você também precisaria alterar o código de trabalho e testado.

Essa alteração pode resultar em novos bugs e essa abordagem também vai contra o [Princípio Aberto/Fechado](https://en.wikipedia.org/wiki/Open/closed_principle) de [SOLID](https://en.wikipedia.org/wiki/SOLID). E não se trata apenas disso, pois a classe original que estava orquestrando as operações cresceria sem parar, o que vai contra o [SRP (princípio de responsabilidade única)](https://en.wikipedia.org/wiki/Single_responsibility_principle).

Por outro lado, se você usa eventos de domínio, você pode criar uma implementação refinada e desacoplada por meio da segregação de responsabilidades, usando essa abordagem:

1. Enviar um comando (por exemplo, CreateOrder).
2. Receber o comando em um manipulador de comandos.
   - Executar uma única transação de agregação.
   - (Opcional) Acionar eventos de domínio para efeitos colaterais (por exemplo, OrderStartedDomainEvent).
3. Manipular eventos de domínio (dentro do processo atual) que executarão um número indefinido de efeitos colaterais em várias agregações ou ações de aplicativo. Por exemplo:
   - Verificar ou criar o comprador e a forma de pagamento.
   - Criar e enviar um evento de integração relacionado ao barramento de eventos a fim de propagar estados entre microsserviços ou disparar ações externas, como o envio de um email para o comprador.
   - Manipular outros efeitos colaterais.

Conforme mostrado na Figura 7-15, começando pelo mesmo evento de domínio, você pode manipular várias ações relacionadas a outras agregações do domínio ou ações de aplicativos adicionais que você precisa realizar entre microsserviços que se conectam com eventos de integração e o barramento de eventos.

![Diagrama que mostra um evento de domínio passando dados para vários manipuladores de eventos.](./media/domain-events-design-implementation/aggregate-domain-event-handlers.png)

**Figura 7-15**. Manipulando várias ações por domínio

Pode haver vários manipuladores para o mesmo evento de domínio na camada de aplicativo, um manipulador pode resolver a consistência entre agregações e outro manipulador pode publicar um evento de integração, para que outros microsserviços possam fazer algo com ele. Os manipuladores de eventos normalmente estão na camada de aplicativo, pois você usará objetos de infraestrutura como repositórios ou uma API de aplicativo para o comportamento do microserviço. Nesse sentido, os manipuladores de eventos são semelhantes aos manipuladores de comandos, portanto, ambos fazem parte da camada de aplicativo. A diferença importante é que um comando deve ser processado apenas uma vez. Um evento de domínio pode ser processado zero ou *n* vezes, porque ele pode ser recebido por vários destinatários ou manipuladores de eventos, com uma finalidade diferente para cada manipulador.

Ter um número aberto de manipuladores por evento de domínio permite que você adicione quantas regras de domínio forem necessárias, sem afetar o código atual. Por exemplo, a implementação da seguinte regra de negócios poderá ser tão fácil quanto adicionar alguns manipuladores de eventos (ou apenas um):

> Quando o valor total comprado por um cliente na loja, em qualquer número de pedidos, excede US$ 6.000, aplicar 10% de desconto para cada novo pedido e notificar o cliente com um email, informando sobre esse desconto para pedidos futuros.

## <a name="implement-domain-events"></a>Implementar eventos de domínio

No C#, um evento de domínio é simplesmente uma classe ou estrutura de retenção de dados, como um DTO, com todas as informações relacionadas ao que acabou de ocorrer no domínio, conforme mostrado no exemplo a seguir:

```csharp
public class OrderStartedDomainEvent : INotification
{
    public string UserId { get; }
    public int CardTypeId { get; }
    public string CardNumber { get; }
    public string CardSecurityNumber { get; }
    public string CardHolderName { get; }
    public DateTime CardExpiration { get; }
    public Order Order { get; }

    public OrderStartedDomainEvent(Order order,
                                   int cardTypeId, string cardNumber,
                                   string cardSecurityNumber, string cardHolderName,
                                   DateTime cardExpiration)
    {
        Order = order;
        CardTypeId = cardTypeId;
        CardNumber = cardNumber;
        CardSecurityNumber = cardSecurityNumber;
        CardHolderName = cardHolderName;
        CardExpiration = cardExpiration;
    }
}
```

Essa é essencialmente uma classe que retém todos os dados relacionados ao evento OrderStarted.

Nos termos da linguagem ubíqua do domínio, como um evento é algo que ocorreu no passado, o nome de classe do evento deverá ser representado como um verbo no passado, como OrderStartedDomainEvent ou OrderShippedDomainEvent. É assim que o evento de domínio é implementado no microsserviço de pedidos no eShopOnContainers.

Conforme observado anteriormente, uma característica importante de eventos é que, como um evento é algo que ocorreu no passado, ele não deve ser alterado. Portanto, ele deve ser uma classe imutável. Observe no código anterior que as propriedades são somente leitura. Não é possível atualizar o objeto, você pode definir os valores apenas quando ele é criado.

É importante destacar aqui que, se os eventos de domínio fossem tratados de forma assíncrona, usando uma fila que exigia a serialização e desserialização dos objetos de evento, as propriedades teriam que ser "conjunto particular" em vez de somente leitura, de modo que o desserializador seria capaz de atribuir os valores na retirada do enfileiramento. Isso não é um problema no microsserviço de pedidos, pois o evento de domínio pub/sub é implementado de forma síncrona usando o MediatR.

### <a name="raise-domain-events"></a>Acionar eventos de domínio

A próxima pergunta é: como acionar um evento de domínio para que ele alcance os respectivos manipuladores de eventos? Você pode usar várias abordagens.

Udi Dahan originalmente propôs (em várias postagens relacionadas, como, [Domain Events – Take 2 (Eventos de domínio – tomada 2)](http://udidahan.com/2008/08/25/domain-events-take-2/)) o uso de uma classe estática para gerenciar e acionar eventos. Isso incluiria uma classe estática chamada DomainEvents, que geraria eventos de domínio assim que fosse chamada, usando uma sintaxe como: `DomainEvents.Raise(Event myEvent)`. Jimmy Bogard escreveu uma postagem no blog ([Strengthening your domain: Domain Events (Fortalecendo seu domínio: eventos de domínio)](https://lostechies.com/jimmybogard/2010/04/08/strengthening-your-domain-domain-events/)) que recomenda uma abordagem semelhante.

No entanto, quando a classe dos eventos de domínio é estática, ela também faz a expedição imediata aos manipuladores. Isso torna o teste e a depuração mais difíceis, pois os manipuladores de eventos com a lógica de efeitos colaterais são executados imediatamente após o evento ser acionado. Ao testar e depurar, você quer se concentrar somente no que está acontecendo nas classes de agregação atuais; você não deseja ser redirecionado repentinamente para outros manipuladores de eventos de efeitos colaterais relacionados a outras agregações ou lógica de aplicativo. É por isso as outras abordagens evoluíram, conforme explicado na próxima seção.

#### <a name="the-deferred-approach-to-raise-and-dispatch-events"></a>A abordagem adiada para acionar e despacho de eventos

Em vez de expedir imediatamente para um manipulador de eventos de domínio, uma abordagem melhor é adicionar os eventos de domínio a uma coleção e, em seguida, expedir esses eventos de domínio *logo antes* ou *logo* *depois* da confirmação da transação (como acontece com SaveChanges no EF). (Essa abordagem foi descrita por Jimmy Bogard nesta postagem [A better domain events pattern (Um padrão de eventos de domínio melhor)](https://lostechies.com/jimmybogard/2014/05/13/a-better-domain-events-pattern/)).

A decisão de enviar os eventos de domínio logo antes ou logo após a confirmação da transação é importante, pois determinará se você vai incluir os efeitos colaterais como parte da mesma transação ou em transações diferentes. No último caso, você precisará lidar com a consistência eventual entre várias agregações. Este tópico será abordado na próxima seção.

A abordagem adiada é que o eShopOnContainers usa. Primeiro, você adiciona os eventos que estão ocorrendo nas suas entidades a uma coleção ou uma lista de eventos por entidade. Essa lista deve fazer parte do objeto de entidade, ou, melhor ainda, parte de sua classe de entidade base, conforme mostrado no seguinte exemplo da classe base Entity:

```csharp
public abstract class Entity
{
     //...
     private List<INotification> _domainEvents;
     public List<INotification> DomainEvents => _domainEvents;

     public void AddDomainEvent(INotification eventItem)
     {
         _domainEvents = _domainEvents ?? new List<INotification>();
         _domainEvents.Add(eventItem);
     }

     public void RemoveDomainEvent(INotification eventItem)
     {
         _domainEvents?.Remove(eventItem);
     }
     //... Additional code
}
```

Quando você deseja acionar um evento, basta adicioná-lo à coleção de eventos através do código em qualquer método de entidade raiz de agregação.

O seguinte código, parte da [Raiz de agregação de ordem no eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.Domain/AggregatesModel/OrderAggregate/Order.cs), mostra um exemplo:

```csharp
var orderStartedDomainEvent = new OrderStartedDomainEvent(this, //Order object
                                                          cardTypeId, cardNumber,
                                                          cardSecurityNumber,
                                                          cardHolderName,
                                                          cardExpiration);
this.AddDomainEvent(orderStartedDomainEvent);
```

Observe que a única coisa que o método AddDomainEvent está fazendo é a adição de um evento à lista. Nenhum evento foi expedido, e nenhum manipulador de eventos foi invocado ainda.

Na verdade, você deseja expedir os eventos mais tarde, ao confirmar a transação no banco de dados. Se você estiver usando o Entity Framework Core, isso será realizado no método SaveChanges do DbContext do EF, como no código a seguir:

```csharp
// EF Core DbContext
public class OrderingContext : DbContext, IUnitOfWork
{
    // ...
    public async Task<bool> SaveEntitiesAsync(CancellationToken cancellationToken = default(CancellationToken))
    {
        // Dispatch Domain Events collection.
        // Choices:
        // A) Right BEFORE committing data (EF SaveChanges) into the DB. This makes
        // a single transaction including side effects from the domain event
        // handlers that are using the same DbContext with Scope lifetime
        // B) Right AFTER committing data (EF SaveChanges) into the DB. This makes
        // multiple transactions. You will need to handle eventual consistency and
        // compensatory actions in case of failures.
        await _mediator.DispatchDomainEventsAsync(this);

        // After this line runs, all the changes (from the Command Handler and Domain
        // event handlers) performed through the DbContext will be committed
        var result = await base.SaveChangesAsync();
    }
}
```

Com esse código, você expede os eventos de entidade aos respectivos manipuladores de eventos.

O resultado geral é que você desacoplou o acionamento de um evento de domínio (uma simples adição a uma lista na memória) da expedição dele para um manipulador de eventos. Além disso, dependendo do tipo de dispatcher que você está usando, é possível expedir os eventos de forma síncrona ou assíncrona.

Lembre-se que os limites transacionais desempenham funções significativas aqui. Se for possível sua unidade de trabalho e transação alcançar mais de uma agregação (como ao usar o EF Core e um banco de dados relacional), isso poderá funcionar bem. Mas se a transação não puder alcançar agregações, como ao usar um banco de dados NoSQL, como o Azure CosmosDB, você precisará implementar etapas adicionais para obter consistência. Essa é outra razão por que a ignorância de persistência não é universal; ela depende do sistema de armazenamento que é usado.

### <a name="single-transaction-across-aggregates-versus-eventual-consistency-across-aggregates"></a>Transação única entre agregações versus consistência eventual entre agregações

Executar uma única transação entre agregações em vez de depender de consistência eventual entre essas agregações é uma questão controversa. Muitos autores de DDD, como Eric Evans e Vaughn Vernon, defendem a regra de que uma transação = uma agregação e, portanto, defendem a consistência eventual entre agregações. Por exemplo, em seu livro *Domain-Driven Design*, Eric Evans diz:

> Não é esperado que toda regra que abrange Agregações esteja atualizada em todos os momentos. Por meio de processamento de eventos, processamento em lote ou de outros mecanismos de atualização, outras dependências podem ser resolvidas dentro de um período específico. (página 128)

Vaughn Vernon diz o seguinte em [design agregado efetivo. Parte II: fazer com que as agregações funcionem juntas](https://dddcommunity.org/wp-content/uploads/files/pdf_articles/Vernon_2011_2.pdf):

> Portanto, se a execução de um comando em uma instância de agregação exigir que regras de negócios adicionais sejam executadas em uma ou mais agregações, use a consistência eventual \[ ... \] Há uma maneira prática de dar suporte à consistência eventual em um modelo DDD. Um método de agregação publica um evento de domínio que é entregue no momento exato a um ou mais assinantes assíncronos.

Essa lógica é baseada na adoção de transações refinadas em vez de transações que abrangem muitas agregações ou entidades. A ideia é que, no segundo caso, o número de bloqueios de banco de dados será significativo em aplicativos de larga escala com necessidades de alta escalabilidade. Aceitar o fato de que aplicativos altamente escalonáveis não precisam de consistência transacional instantânea entre várias agregações ajuda a aceitar o conceito de consistência eventual. Geralmente, as mudanças atômicas não são necessárias aos negócios e, em todo caso, é da responsabilidade dos especialistas de domínio dizer se operações específicas precisam ou não de transações atômicas. Se uma operação sempre precisar de uma transação atômica entre várias agregações, você poderá questionar se a agregação deveria ser maior ou se não foi corretamente projetada.

No entanto, outros desenvolvedores e arquitetos, como Jimmy Bogard, estão de acordo com a abrangência de uma única transação entre várias agregações, mas somente quando essas agregações adicionais forem relacionadas a efeitos colaterais do mesmo comando original. Por exemplo, em [A better domain events pattern (Um padrão melhor de eventos de domínio)](https://lostechies.com/jimmybogard/2014/05/13/a-better-domain-events-pattern/), Bogard diz:

> Normalmente, quero que os efeitos colaterais de um evento de domínio ocorram dentro da mesma transação lógica, mas não necessariamente no mesmo escopo de gerar o evento de domínio \[ ... \] Logo antes de confirmarmos nossa transação, expedimos nossos eventos para seus respectivos manipuladores.

Se você expedir os eventos de domínio imediatamente *antes* da confirmação da transação original, será porque você deseja que os efeitos colaterais desses eventos sejam incluídos na mesma transação. Por exemplo, se o método SaveChanges do DbContext do EF falhar, a transação reverterá todas as alterações, incluindo o resultado de qualquer operação de efeito colateral implementada pelos manipuladores de eventos de domínio relacionados. Isso ocorre porque o escopo de vida do DbContext é, por padrão, definido como "com escopo". Portanto, o objeto DbContext é compartilhado entre vários objetos de repositório que estão sendo instanciados dentro do mesmo escopo ou objeto graph. Isso coincide com o escopo de HttpRequest ao desenvolver aplicativos da API Web ou do MVC.

Na realidade, as duas abordagens (transação atômica única e consistência eventual) podem ser corretas. Isso realmente dependerá dos seus requisitos de negócios ou do domínio e o que os especialistas de domínio lhe indicarem. Também dependerá da escalabilidade necessária para o serviço (transações mais granulares tem menos impacto em relação aos bloqueios de banco de dados). E dependerá de quanto você está disposto a investir em seu código, pois a consistência eventual exige um código mais complexo, para detectar possíveis inconsistências entre agregações, e a necessidade de implementar ações compensatórias. Considere que, se você confirmar as alterações à agregação original e posteriormente, quando os eventos estiverem sendo expedidos, houver um problema e os manipuladores de eventos não puderem confirmar os efeitos colaterais, você terá inconsistências entre agregações.

Uma forma de permitir ações compensatórias seria armazenar os eventos de domínio em tabelas de banco de dados adicionais para que eles possam fazer parte da transação original. Posteriormente, você poderia ter um processo em lote para detectar inconsistências e executar ações compensatórias por meio da comparação da lista de eventos com o estado atual das agregações. As ações compensatórias fazem parte de um tópico complexo que exigirá uma análise detalhada, incluindo discuti-las com o usuário empresarial e com os especialistas de domínio.

De qualquer maneira, você pode optar pela abordagem que seja necessária. Mas a abordagem adiada inicial — disparar os eventos antes da confirmação, de forma a usar uma única transação – é a abordagem mais simples ao usar o EF Core e um banco de dados relacional. Ela é mais fácil de implementar e é válida em muitos casos de negócio. Ela também é a abordagem usada no microsserviço de pedidos no eShopOnContainers.

Mas, de que maneira você realmente envia esses eventos aos respectivos manipuladores de eventos? O que é o objeto `_mediator` visto no exemplo anterior? Ele tem a ver com as técnicas e artefatos que você usa para mapear entre eventos e os respectivos manipuladores de eventos.

### <a name="the-domain-event-dispatcher-mapping-from-events-to-event-handlers"></a>O dispatcher de evento de domínio: mapeamento de eventos a manipuladores de eventos

Assim que estiver pronto para expedir ou publicar os eventos, você precisará de algum tipo de artefato que publicará o evento, para que cada manipulador relacionado possa obtê-lo e processar os efeitos colaterais com base nesse evento.

Uma abordagem é um sistema de mensagens real ou até mesmo um barramento de eventos, possivelmente baseado em um barramento de serviço, em vez de eventos na memória. No entanto, no primeiro caso, os sistema de mensagens real seria um exagero para processar eventos de domínio, pois você só precisa processar esses eventos dentro do mesmo processo (ou seja, no mesmo domínio e na mesma camada de aplicativo).

Outra maneira de mapear eventos para vários manipuladores de eventos é o uso de registro de tipos em um contêiner de IoC para que você possa inferir dinamicamente o local para expedir os eventos. Em outras palavras, você precisa saber quais manipuladores de eventos precisam obter um evento específico. A figura 7-16 mostra uma abordagem simplificada para esta abordagem.

![Diagrama que mostra um distribuidor de eventos de domínio enviando eventos para os manipuladores apropriados.](./media/domain-events-design-implementation/domain-event-dispatcher.png)

**Figura 7-16**. Dispatcher de evento de domínio usando IoC

Você pode criar todos os detalhes técnicos e artefatos para implementar essa abordagem por si só. No entanto, você também pode usar as bibliotecas disponíveis, como a [MediatR](https://github.com/jbogard/MediatR), que usa seu contêiner de IoC nos bastidores. Portanto, você pode usar diretamente as interfaces predefinidas e os métodos de publicação/expedição do objeto mediador.

No código, primeiro você precisa registrar os tipos de manipulador de eventos no contêiner de IoC, conforme mostrado no seguinte exemplo no [microsserviço de pedidos eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Infrastructure/AutofacModules/MediatorModule.cs):

```csharp
public class MediatorModule : Autofac.Module
{
    protected override void Load(ContainerBuilder builder)
    {
        // Other registrations ...
        // Register the DomainEventHandler classes (they implement IAsyncNotificationHandler<>)
        // in assembly holding the Domain Events
        builder.RegisterAssemblyTypes(typeof(ValidateOrAddBuyerAggregateWhenOrderStartedDomainEventHandler)
                                       .GetTypeInfo().Assembly)
                                         .AsClosedTypesOf(typeof(IAsyncNotificationHandler<>));
        // Other registrations ...
    }
}
```

Primeiro, o código identifica o assembly que contém os manipuladores de eventos do domínio ao localizar o assembly que contém qualquer um dos manipuladores (usando typeof(ValidateOrAddBuyerAggregateWhenXxxx), mas você poderia escolher qualquer outro manipulador de eventos para localizar o assembly). Como todos os manipuladores de eventos implementam a interface IAsyncNotificationHandler, então o código pesquisas apenas esses tipos e registra todos os manipuladores de eventos.

### <a name="how-to-subscribe-to-domain-events"></a>Como assinar eventos de domínio

Quando você usa o MediatR, cada manipulador de eventos deve usar um tipo de evento que é fornecido no parâmetro genérico da interface INotificationHandler, como pode ser visto no código a seguir:

```csharp
public class ValidateOrAddBuyerAggregateWhenOrderStartedDomainEventHandler
  : IAsyncNotificationHandler<OrderStartedDomainEvent>
```

Com base na relação entre o evento e o manipulador de eventos, que pode ser considerada a assinatura, o artefato do MediatR consegue descobrir todos os manipuladores de eventos de cada evento e disparar cada um desses manipuladores de eventos.

### <a name="how-to-handle-domain-events"></a>Como manipular eventos de domínio

Por fim, o manipulador de eventos geralmente implementa o código da camada de aplicativo, que usa os repositórios de infraestrutura para obter as agregações adicionais necessárias e executar a lógica de domínio do efeito colateral. O seguinte [código de manipulador de eventos de domínio em eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/DomainEventHandlers/OrderStartedEvent/ValidateOrAddBuyerAggregateWhenOrderStartedDomainEventHandler.cs), mostra um exemplo de implementação.

```csharp
public class ValidateOrAddBuyerAggregateWhenOrderStartedDomainEventHandler
                   : INotificationHandler<OrderStartedDomainEvent>
{
    private readonly ILoggerFactory _logger;
    private readonly IBuyerRepository<Buyer> _buyerRepository;
    private readonly IIdentityService _identityService;

    public ValidateOrAddBuyerAggregateWhenOrderStartedDomainEventHandler(
        ILoggerFactory logger,
        IBuyerRepository<Buyer> buyerRepository,
        IIdentityService identityService)
    {
        // ...Parameter validations...
    }

    public async Task Handle(OrderStartedDomainEvent orderStartedEvent)
    {
        var cardTypeId = (orderStartedEvent.CardTypeId != 0) ? orderStartedEvent.CardTypeId : 1;
        var userGuid = _identityService.GetUserIdentity();
        var buyer = await _buyerRepository.FindAsync(userGuid);
        bool buyerOriginallyExisted = (buyer == null) ? false : true;

        if (!buyerOriginallyExisted)
        {
            buyer = new Buyer(userGuid);
        }

        buyer.VerifyOrAddPaymentMethod(cardTypeId,
                                       $"Payment Method on {DateTime.UtcNow}",
                                       orderStartedEvent.CardNumber,
                                       orderStartedEvent.CardSecurityNumber,
                                       orderStartedEvent.CardHolderName,
                                       orderStartedEvent.CardExpiration,
                                       orderStartedEvent.Order.Id);

        var buyerUpdated = buyerOriginallyExisted ? _buyerRepository.Update(buyer)
                                                                      : _buyerRepository.Add(buyer);

        await _buyerRepository.UnitOfWork
                .SaveEntitiesAsync();

        // Logging code using buyerUpdated info, etc.
    }
}
```

O código de manipulador de eventos de domínio anterior é considerado um código da camada de aplicativo porque ele usa repositórios de infraestrutura, conforme explicado na próxima seção sobre a camada de persistência de infraestrutura. Os manipuladores de eventos também podem usar outros componentes de infraestrutura.

#### <a name="domain-events-can-generate-integration-events-to-be-published-outside-of-the-microservice-boundaries"></a>Eventos de domínio podem gerar eventos de integração para serem publicados fora dos limites do microsserviço

Por fim, é importante mencionar que, às vezes, convém propagar eventos entre vários microsserviços. Essa propagação é considerada um evento de integração e ele pode ser publicado por meio de um barramento de eventos proveniente de qualquer manipulador de eventos de domínio específico.

## <a name="conclusions-on-domain-events"></a>Conclusões sobre eventos de domínio

Conforme mencionado, use eventos de domínio para implementar explicitamente os efeitos colaterais de alterações em seu domínio. Para usar a terminologia DDD, use eventos de domínio para implementar explicitamente efeitos colaterais entre uma ou várias agregações. Além disso e para melhor escalabilidade e menor impacto em bloqueios de banco de dados, use consistência eventual entre agregações dentro do mesmo domínio.

O aplicativo de referência usa [mediador](https://github.com/jbogard/MediatR) para propagar eventos de domínio de forma síncrona entre agregações em uma única transação. No entanto, você também pode usar alguma implementação de AMQP como [RabbitMQ](https://www.rabbitmq.com/) ou [barramento de serviço do Azure](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview) para propagar eventos de domínio de forma assíncrona, usando a consistência eventual, mas, como mencionado acima, você precisa considerar a necessidade de ações de compensação em caso de falhas.

## <a name="additional-resources"></a>Recursos adicionais

- **Greg Young. O que é um evento de domínio?** \
  <https://cqrs.files.wordpress.com/2010/11/cqrs_documents.pdf#page=25>

- **Stenberg de Jan. Eventos de domínio e consistência eventual** \
  <https://www.infoq.com/news/2015/09/domain-events-consistency>

- **Jimmy Bogard. Um padrão de eventos de domínio melhor** \
  <https://lostechies.com/jimmybogard/2014/05/13/a-better-domain-events-pattern/>

- **Vaughn Vernon. Design de agregação efetivo parte II: fazer com que as agregações funcionem juntas** \
  [https://dddcommunity.org/wp-content/uploads/files/pdf\_articles/Vernon\_2011\_2.pdf](https://dddcommunity.org/wp-content/uploads/files/pdf_articles/Vernon_2011_2.pdf)

- **Jimmy Bogard. Fortalecendo seu domínio: eventos de domínio** \
  <https://lostechies.com/jimmybogard/2010/04/08/strengthening-your-domain-domain-events/>

- **Tony Truong. Exemplo de padrão de eventos de domínio** \
  <https://www.tonytruong.net/domain-events-pattern-example/>

- **Udi Dahan. Como criar modelos de domínio totalmente encapsulados** \
  <https://udidahan.com/2008/02/29/how-to-create-fully-encapsulated-domain-models/>

- **Udi Dahan. Eventos de domínio – Take 2** \
  <https://udidahan.com/2008/08/25/domain-events-take-2/>

- **Udi Dahan. Eventos de domínio – Salvation** \
  <https://udidahan.com/2009/06/14/domain-events-salvation/>

- **Kronquist de Jan. Não publique eventos de domínio, retorne-os!** \
  <https://blog.jayway.com/2013/06/20/dont-publish-domain-events-return-them/>

- **Cesar de la Torre. Eventos de domínio vs. eventos de integração em arquiteturas DDD e de microservices** \
  <https://devblogs.microsoft.com/cesardelatorre/domain-events-vs-integration-events-in-domain-driven-design-and-microservices-architectures/>

>[!div class="step-by-step"]
>[Anterior](client-side-validation.md) 
> [Avançar](infrastructure-persistence-layer-design.md)
