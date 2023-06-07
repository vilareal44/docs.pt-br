---
title: Implementando a camada de aplicativos de microsserviço usando a API Web
description: Entenda a injeção de dependência e os padrões de mediador e seus detalhes de implementação na camada de aplicativo da API Web.
ms.date: 08/17/2020
ms.openlocfilehash: 72395acafb403a4e34858eb2b982ec83b9f3cee1
ms.sourcegitcommit: cbb19e56d48cf88375d35d0c27554d4722761e0d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88608115"
---
# <a name="implement-the-microservice-application-layer-using-the-web-api"></a>Implementar a camada de aplicativos de microsserviço usando a API Web

## <a name="use-dependency-injection-to-inject-infrastructure-objects-into-your-application-layer"></a>Usar a injeção de dependência para injetar objetos de infraestrutura em sua camada de aplicativo

Conforme mencionado anteriormente, a camada de aplicativo pode ser implementada como parte do artefato (assembly) que você está criando, tal como em um projeto de API Web ou um projeto de aplicativo Web do MVC. No caso de um microsserviço criado com o ASP.NET Core, a camada de aplicativo geralmente será a biblioteca da API Web. Se quiser separar o que é proveniente do ASP.NET Core (a infraestrutura e os controladores) do código personalizado da camada de aplicativo, você também poderá colocar a camada de aplicativo em uma biblioteca de classes separada, mas isso é opcional.

Por exemplo, o código da camada de aplicativo do microsserviço de ordenação é implementado diretamente como parte do projeto **Ordering.API** (um projeto da API Web ASP.NET Core), como mostrado na Figura 7-23.

:::image type="complex" source="./media/microservice-application-layer-implementation-web-api/ordering-api-microservice.png" alt-text="Captura de tela do microserviço de classificação. API no Gerenciador de Soluções.":::
O Gerenciador de Soluções exibição do microserviço de classificação. API, mostrando as subpastas na pasta do aplicativo: comportamentos, comandos, DomainEventHandlers, IntegrationEvents, modelos, consultas e validações.
:::image-end:::

**Figura 7-23**. A camada de aplicativo no projeto Ordering.API da API Web ASP.NET Core

O ASP.NET Core inclui um [contêiner interno de IoC](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) simples (representado pela interface IServiceProvider) que é compatível com a injeção de construtor por padrão, e o ASP.NET disponibiliza alguns serviços por meio da DI (injeção de dependência). O ASP.NET Core usa o termo *serviço* para qualquer um dos tipos que você registra e que serão injetados pela DI. Você configura os serviços internos do contêiner no método ConfigureServices na classe Startup do seu aplicativo. As dependências são implementadas nos serviços que são necessários para um tipo e que você registra no contêiner de IoC.

Normalmente, você deseja injetar dependências que implementam objetos de infraestrutura. Uma dependência típica para injetar é um repositório. Mas você poderá injetar qualquer outra dependência de infraestrutura que tiver. Para implementações mais simples, você injeta diretamente o objeto de padrão da Unidade de Trabalho (o objeto DbContext do EF), porque o DBContext também é a implementação dos objetos de persistência da sua infraestrutura.

Veja no exemplo a seguir como o .NET Core está injetando os objetos de repositório necessários por meio do construtor. A classe é um manipulador de comando, que será abordado na próxima seção.

```csharp
public class CreateOrderCommandHandler
        : IRequestHandler<CreateOrderCommand, bool>
{
    private readonly IOrderRepository _orderRepository;
    private readonly IIdentityService _identityService;
    private readonly IMediator _mediator;
    private readonly IOrderingIntegrationEventService _orderingIntegrationEventService;
    private readonly ILogger<CreateOrderCommandHandler> _logger;

    // Using DI to inject infrastructure persistence Repositories
    public CreateOrderCommandHandler(IMediator mediator,
        IOrderingIntegrationEventService orderingIntegrationEventService,
        IOrderRepository orderRepository,
        IIdentityService identityService,
        ILogger<CreateOrderCommandHandler> logger)
    {
        _orderRepository = orderRepository ?? throw new ArgumentNullException(nameof(orderRepository));
        _identityService = identityService ?? throw new ArgumentNullException(nameof(identityService));
        _mediator = mediator ?? throw new ArgumentNullException(nameof(mediator));
        _orderingIntegrationEventService = orderingIntegrationEventService ?? throw new ArgumentNullException(nameof(orderingIntegrationEventService));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));
    }

    public async Task<bool> Handle(CreateOrderCommand message, CancellationToken cancellationToken)
    {
        // Add Integration event to clean the basket
        var orderStartedIntegrationEvent = new OrderStartedIntegrationEvent(message.UserId);
        await _orderingIntegrationEventService.AddAndSaveEventAsync(orderStartedIntegrationEvent);

        // Add/Update the Buyer AggregateRoot
        // DDD patterns comment: Add child entities and value-objects through the Order Aggregate-Root
        // methods and constructor so validations, invariants and business logic
        // make sure that consistency is preserved across the whole aggregate
        var address = new Address(message.Street, message.City, message.State, message.Country, message.ZipCode);
        var order = new Order(message.UserId, message.UserName, address, message.CardTypeId, message.CardNumber, message.CardSecurityNumber, message.CardHolderName, message.CardExpiration);

        foreach (var item in message.OrderItems)
        {
            order.AddOrderItem(item.ProductId, item.ProductName, item.UnitPrice, item.Discount, item.PictureUrl, item.Units);
        }

        _logger.LogInformation("----- Creating Order - Order: {@Order}", order);

        _orderRepository.Add(order);

        return await _orderRepository.UnitOfWork
            .SaveEntitiesAsync(cancellationToken);
    }
}

```

A classe usa os repositórios injetados para executar a transação e persistir as alterações de estado. Não importa se a classe é um manipulador de comandos, um método de controlador da API Web ASP.NET Core ou um [Serviço de aplicativo DDD](https://lostechies.com/jimmybogard/2008/08/21/services-in-domain-driven-design/). Em última análise, ela uma classe simples que usa repositórios, entidades de domínio e outras coordenações de aplicativos de forma semelhante a um manipulador de comandos. A injeção de dependência funciona da mesma forma para todas as classes mencionadas, como no exemplo que usa a injeção de dependência com base no construtor.

### <a name="register-the-dependency-implementation-types-and-interfaces-or-abstractions"></a>Registrar tipos e interfaces ou abstrações da implementação de dependência

Antes de usar os objetos injetados por meio de construtores, você precisa saber em que lugar registrar as interfaces e classes que produzem os objetos injetados nas classes do aplicativo por meio da DI. (Como na DI baseada no construtor, conforme mostrado anteriormente).

#### <a name="use-the-built-in-ioc-container-provided-by-aspnet-core"></a>Usar o contêiner interno de IoC fornecido pelo ASP.NET Core

Ao usar o contêiner interno de IoC fornecido pelo ASP.NET Core, você registra os tipos que deseja injetar no método ConfigureServices no arquivo Startup.cs, como no código a seguir:

```csharp
// Registration of types into ASP.NET Core built-in container
public void ConfigureServices(IServiceCollection services)
{
    // Register out-of-the-box framework services.
    services.AddDbContext<CatalogContext>(c =>
        c.UseSqlServer(Configuration["ConnectionString"]),
        ServiceLifetime.Scoped);

    services.AddMvc();
    // Register custom application dependencies.
    services.AddScoped<IMyCustomRepository, MyCustomSQLRepository>();
}
```

O padrão mais comum ao registrar tipos em um contêiner de IoC é registrar um par de tipos: uma interface e a respectiva classe de implementação. Então, quando solicita um objeto do contêiner de IoC por meio de nenhum construtor, você solicita um objeto de um determinado tipo de interface. Assim, como visto no exemplo anterior, a última linha indica que, quando qualquer um dos seus construtores tiver uma dependência em IMyCustomRepository (interface ou abstração), o contêiner de IoC injetará uma instância da classe de implementação MyCustomSQLServerRepository.

#### <a name="use-the-scrutor-library-for-automatic-types-registration"></a>Usar a biblioteca Scrutor para registro automático de tipos

Ao usar a DI no .NET Core, talvez seja interessante verificar um assembly e registrar automaticamente seus tipos por convenção. Este recurso não está disponível atualmente no ASP.NET Core. No entanto, você pode usar a biblioteca [Scrutor](https://github.com/khellang/Scrutor) para fazer isso. Essa abordagem é conveniente quando você tem muitos tipos que precisam ser registrados no contêiner de IoC.

#### <a name="additional-resources"></a>Recursos adicionais

- **Matthew King. Registrando serviços com o Scrutor** \
  <https://www.mking.net/blog/registering-services-with-scrutor>

- **Kristian Hellang. Scrutor.** Repositório do GitHub. \
  <https://github.com/khellang/Scrutor>

#### <a name="use-autofac-as-an-ioc-container"></a>Usar o Autofac como um contêiner de IoC

Você também pode usar mais contêineres de IoC e conectá-los no pipeline do ASP.NET Core, como no microsserviço de ordenação em eShopOnContainers, que usa o [Autofac](https://autofac.org/). Ao usar Autofac, você normalmente registra os tipos por meio de módulos, que permitem dividir os tipos de registro entre vários arquivos, dependendo do local em que seus tipos estão, assim como os tipos de aplicativos podem ser distribuídos entre várias bibliotecas de classes.

Por exemplo, a seguir está o [módulo de aplicativo do Autofac](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Infrastructure/AutofacModules/ApplicationModule.cs) para o projeto de [API Web Ordering.API](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/Services/Ordering/Ordering.API) com os tipos que você pode injetar.

```csharp
public class ApplicationModule : Autofac.Module
{
    public string QueriesConnectionString { get; }
    public ApplicationModule(string qconstr)
    {
        QueriesConnectionString = qconstr;
    }

    protected override void Load(ContainerBuilder builder)
    {
        builder.Register(c => new OrderQueries(QueriesConnectionString))
            .As<IOrderQueries>()
            .InstancePerLifetimeScope();
        builder.RegisterType<BuyerRepository>()
            .As<IBuyerRepository>()
            .InstancePerLifetimeScope();
        builder.RegisterType<OrderRepository>()
            .As<IOrderRepository>()
            .InstancePerLifetimeScope();
        builder.RegisterType<RequestManager>()
            .As<IRequestManager>()
            .InstancePerLifetimeScope();
   }
}
```

O Autofac também tem um recurso para [digitalizar assemblies e registrar tipos pelas convenções de nome](https://autofac.readthedocs.io/en/latest/register/scanning.html).

O processo e os conceitos de registro são muito semelhantes à forma pela qual você registra tipos com o contêiner de IoC interno do ASP.NET Core, mas a sintaxe, ao usar Autofac, é um pouco diferente.

No código de exemplo, a abstração IOrderRepository é registrada juntamente com a classe de implementação OrderRepository. Isso significa que, sempre que um construtor estiver declarando uma dependência por meio da abstração ou interface IOrderRepository, o contêiner de IoC injetará uma instância da classe OrderRepository.

O tipo de escopo da instância determina como uma instância é compartilhada entre as solicitações para o mesmo serviço ou dependência. Quando uma solicitação é feita a uma dependência, o contêiner de IoC pode retornar o seguinte:

- Uma única instância por escopo de tempo de vida (conhecida no contêiner de IoC do ASP.NET Core como *com escopo*).

- Uma nova instância por dependência (conhecida no contêiner de IoC do ASP.NET Core como *transitória*).

- Uma única instância compartilhada entre todos os objetos que usam o contêiner de IoC (conhecida no contêiner de IoC do ASP.NET Core como *singleton*).

#### <a name="additional-resources"></a>Recursos adicionais

- **Introdução à injeção de dependência no ASP.NET Core** \
  [https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection](/aspnet/core/fundamentals/dependency-injection)

- **Autofac.** Documentação oficial. \
  <https://docs.autofac.org/en/latest/>

- **Comparando os tempos de vida do serviço de contêiner de IoC do ASP.NET Core com os escopos de instância de contêiner de IoC do Autofac – Cesar de la Torre.** \
  <https://devblogs.microsoft.com/cesardelatorre/comparing-asp-net-core-ioc-service-life-times-and-autofac-ioc-instance-scopes/>

## <a name="implement-the-command-and-command-handler-patterns"></a>Implementar os padrões de Comando e Manipulador de Comandos

No exemplo de DI por meio de construtor, mostrado na seção anterior, o contêiner de IoC injetou repositórios por meio de um construtor em uma classe. Mas em que local eles foram exatamente injetados? Em uma API Web simples (por exemplo, o microserviço de catálogo em eShopOnContainers), você os injeta no nível dos controladores MVC, em um construtor de controlador, como parte do pipeline de solicitação de ASP.NET Core. Entretanto, no código inicial desta seção (a classe [CreateOrderCommandHandler](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Application/Commands/CreateOrderCommandHandler.cs) do serviço Ordering.API em eShopOnContainers), a injeção de dependências é feita por meio do construtor de um manipulador comandos específico. Vamos explicar o que é um manipulador de comandos e por que você o usaria.

O padrão Command é intrinsecamente relacionado ao padrão CQRS, apresentado anteriormente neste guia. O CQRS tem dois lados. A primeira área é a de consultas, usando consultas simplificadas com o micro ORM [Dapper](https://github.com/StackExchange/dapper-dot-net), explicado anteriormente. A segunda área é a de comandos, os quais são o ponto de partida para transações e o canal de entrada do serviço.

Como mostra a Figura 7-24, o padrão é baseado em aceitar comandos do lado do cliente, processá-los com base nas regras de modelo de domínio e, finalmente, persistir os Estados com transações.

![Diagrama que mostra o fluxo de dados de alto nível do cliente para o Database.](./media/microservice-application-layer-implementation-web-api/high-level-writes-side.png)

**Figura 7-24**. Exibição de alto nível dos comandos ou "lado transacional" em um padrão CQRS

A Figura 7-24 mostra que o aplicativo de interface do usuário envia um comando por meio da API que chega a um `CommandHandler` , que depende do modelo de domínio e da infraestrutura, para atualizar o banco de dados.

### <a name="the-command-class"></a>A classe de comando

Um comando é uma solicitação ao sistema para executar uma ação que altera o estado do sistema. Os comandos são imperativos e devem ser processados apenas uma vez.

Como são imperativos, os comandos geralmente são nomeados com um verbo no modo imperativo, por exemplo, "criar" ou "atualizar", e eles podem incluir o tipo de agregação, como CreateOrderCommand. Ao contrário de um evento, um comando não é um fato do passado; ele é apenas uma solicitação e, portanto, pode ser recusado.

Os comandos podem se originar na interface do usuário, como resultado do início de uma solicitação por um usuário ou de um gerenciador de processos, quando ele está instruindo uma agregação a executar uma ação.

Uma característica importante de um comando é que ele deve ser processado apenas uma vez por um único destinatário. Isso porque um comando é uma ação ou transação única que você deseja executar no aplicativo. Por exemplo, o mesmo comando de criação de pedido não deve ser processado mais de uma vez. Essa é uma importante diferença entre comandos e eventos. Os eventos podem ser processados várias vezes, porque muitos sistemas ou microsserviços podem estar interessados no evento.

Além disso, é importante que um comando seja processado apenas uma vez, caso ele não seja idempotente. Um comando será idempotente se ele puder ser executado várias vezes sem alterar o resultado, devido à natureza do comando ou por causa da forma como o sistema manipula o comando.

É uma boa prática fazer seus comandos e atualizações idempotentes quando fizer sentido nas regras de negócios e nas invariáveis de seu domínio. Assim, para usar o mesmo exemplo, se por algum motivo (lógica de repetição, hackers, etc.) o mesmo comando CreateOrder chegar até seu sistema diversas vezes, você deverá ser capaz de identificá-lo e ter a certeza de não criar vários pedidos. Para fazer isso, você precisa anexar algum tipo de identidade nas operações e identificar se o comando ou a atualização já foi processada.

Você envia um comando a um único destinatário; você não publica um comando. A publicação serve para eventos que declaram um fato, ou seja, que algo aconteceu e pode ser interessante para destinatários de eventos. No caso de eventos, o publicador não tem preocupações sobre quais destinatários recebem o evento ou o que eles fazem com isso. No entanto, os eventos de domínio ou de integração são outra história, já apresentada nas seções anteriores.

Um comando é implementado com uma classe que contém campos de dados ou coleções com todas as informações necessárias para executar esse comando. Um comando é um tipo especial de DTO (Objeto de Transferência de Dados), usado especificamente para solicitar alterações ou transações. O próprio comando baseia-se estritamente nas informações necessárias para processar o comando e nada mais.

O exemplo a seguir mostra a `CreateOrderCommand` classe simplificada. Este é um comando imutável que é usado no microsserviço de ordenação em eShopOnContainers.

```csharp
// DDD and CQRS patterns comment: Note that it is recommended to implement immutable Commands
// In this case, its immutability is achieved by having all the setters as private
// plus only being able to update the data just once, when creating the object through its constructor.
// References on Immutable Commands:
// http://cqrs.nu/Faq
// https://docs.spine3.org/motivation/immutability.html
// http://blog.gauffin.org/2012/06/griffin-container-introducing-command-support/
// https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/how-to-implement-a-lightweight-class-with-auto-implemented-properties

[DataContract]
public class CreateOrderCommand
    : IRequest<bool>
{
    [DataMember]
    private readonly List<OrderItemDTO> _orderItems;

    [DataMember]
    public string UserId { get; private set; }

    [DataMember]
    public string UserName { get; private set; }

    [DataMember]
    public string City { get; private set; }

    [DataMember]
    public string Street { get; private set; }

    [DataMember]
    public string State { get; private set; }

    [DataMember]
    public string Country { get; private set; }

    [DataMember]
    public string ZipCode { get; private set; }

    [DataMember]
    public string CardNumber { get; private set; }

    [DataMember]
    public string CardHolderName { get; private set; }

    [DataMember]
    public DateTime CardExpiration { get; private set; }

    [DataMember]
    public string CardSecurityNumber { get; private set; }

    [DataMember]
    public int CardTypeId { get; private set; }

    [DataMember]
    public IEnumerable<OrderItemDTO> OrderItems => _orderItems;

    public CreateOrderCommand()
    {
        _orderItems = new List<OrderItemDTO>();
    }

    public CreateOrderCommand(List<BasketItem> basketItems, string userId, string userName, string city, string street, string state, string country, string zipcode,
        string cardNumber, string cardHolderName, DateTime cardExpiration,
        string cardSecurityNumber, int cardTypeId) : this()
    {
        _orderItems = basketItems.ToOrderItemsDTO().ToList();
        UserId = userId;
        UserName = userName;
        City = city;
        Street = street;
        State = state;
        Country = country;
        ZipCode = zipcode;
        CardNumber = cardNumber;
        CardHolderName = cardHolderName;
        CardExpiration = cardExpiration;
        CardSecurityNumber = cardSecurityNumber;
        CardTypeId = cardTypeId;
        CardExpiration = cardExpiration;
    }


    public class OrderItemDTO
    {
        public int ProductId { get; set; }

        public string ProductName { get; set; }

        public decimal UnitPrice { get; set; }

        public decimal Discount { get; set; }

        public int Units { get; set; }

        public string PictureUrl { get; set; }
    }
}
```

Basicamente, a classe de comando contém todos os dados necessários para realizar uma transação comercial, usando os objetos do modelo de domínio. Assim, os comandos são simplesmente estruturas de dados que contêm dados somente leitura e nenhum comportamento. O nome do comando indica sua finalidade. Em várias linguagens, como C#, os comandos são representados como classes, mas eles não são verdadeiramente classes, no real sentido de serem orientados a objetos.

Como uma característica adicional, os comandos são imutáveis, porque o uso esperado é que eles sejam processados diretamente pelo modelo de domínio. Eles não precisam ser alterados durante o tempo de vida projetado. Em uma classe C#, a imutabilidade pode ser obtida não tendo nenhum setter ou outro método que altere o estado interno.

Tenha em mente que, se você pretender ou esperar que os comandos passem por um processo de serialização/desserialização, as propriedades deverão ter um setter particular e o `[DataMember]` atributo (ou `[JsonProperty]` ). Caso contrário, o desserializador não poderá reconstruir o objeto no destino com os valores necessários. Você também pode usar Propriedades verdadeiramente somente leitura se a classe tiver um construtor com parâmetros para todas as propriedades, com a Convenção de nomenclatura camelCase comum e anotar o construtor como `[JsonConstructor]` . No entanto, essa opção requer mais código.

Por exemplo, a classe de comando para a criação de um pedido é, provavelmente, semelhante em relação aos dados para o pedido que você deseja criar, mas é provável que você não precise dos mesmos atributos. Por exemplo, não `CreateOrderCommand` tem uma ID de pedido, porque a ordem ainda não foi criada.

Muitas classes de comando podem ser simples, exigindo apenas alguns campos de algum estado que tenha que ser alterado. Esse seria o caso se você estiver apenas alterando o status de um pedido de "em processo" para "pago" ou "enviado" usando um comando semelhante ao seguinte:

```csharp
[DataContract]
public class UpdateOrderStatusCommand
    :IRequest<bool>
{
    [DataMember]
    public string Status { get; private set; }

    [DataMember]
    public string OrderId { get; private set; }

    [DataMember]
    public string BuyerIdentityGuid { get; private set; }
}
```

Alguns desenvolvedores criam os objetos de solicitação da interface do usuário separados dos DTOs do comando, mas essa é apenas uma questão de preferência. É uma separação entediante, sem muito valor adicional, e os objetos são quase exatamente a mesma forma. Por exemplo, no eShopOnContainers, alguns comandos vêm diretamente do lado do cliente.

### <a name="the-command-handler-class"></a>A classe do manipulador de comandos

Você deve implementar uma classe manipuladora de comandos específica para cada comando. É assim que o padrão funciona, e é onde você usará o objeto de comando, os objetos de domínio e os objetos de repositório de infraestrutura. O manipulador de comandos é, na verdade, a essência da camada de aplicativo em termos de CQRS e DDD. No entanto, toda a lógica de domínio deve estar contida nas classes de domínio — dentro das raízes de agregação (entidades raiz), entidades filho ou [serviços de domínio](https://lostechies.com/jimmybogard/2008/08/21/services-in-domain-driven-design/), mas não dentro do manipulador de comandos, que é uma classe da camada de aplicativo.

A classe de manipulador de comando oferece um forte ponto de partida no caminho para alcançar o SRP (Princípio de Responsabilidade Único) mencionado em uma seção anterior.

Um manipulador de comandos recebe um comando e obtém um resultado da agregação que é usada. O resultado deverá ser a execução bem-sucedida do comando ou uma exceção. No caso de uma exceção, o estado do sistema deve permanecer inalterado.

O manipulador de comandos geralmente realiza as seguintes etapas:

- Ele recebe o objeto de comando, como um DTO (do [mediador](https://en.wikipedia.org/wiki/Mediator_pattern) ou de outro objeto da infraestrutura).

- Ele verifica se o comando é válido (se não tiver sido validado pelo mediador).

- Ele cria a instância da raiz de agregação que é o destino do comando atual.

- Ele executa o método na instância raiz da agregação, obtendo os dados necessários do comando.

- Ele persiste o novo estado da agregação no respectivo banco de dados. Esta última operação é, verdadeiramente, a transação.

Normalmente, um manipulador de comandos lida com uma única agregação orientada por sua raiz de agregação (entidade de raiz). Se várias agregações devem ser afetadas pela recepção de um único comando, você pode usar eventos de domínio para propagar estados ou ações entre várias agregações.

O ponto importante aqui é que, quando um comando está sendo processado, toda a lógica do domínio deve estar dentro do modelo de domínio (as agregações), totalmente encapsulada e pronta para o teste de unidade. O manipulador de comandos atua apenas como uma maneira de obter o modelo de domínio do banco de dados e como a etapa final, para informar à camada de infraestrutura (aos repositórios) para persistir as alterações quando o modelo for alterado. A vantagem dessa abordagem é que você pode refatorar a lógica do domínio em um modelo de domínio isolado, totalmente encapsulado, avançado e comportamental sem alterar o código do aplicativo ou as camadas de infraestrutura, que fazem parte do nível de detalhes técnicos (manipuladores de comandos, API Web, repositórios, etc.).

Quando manipuladores de comandos se tornam complexos, com muita lógica, começam a ficar parecidos com código. Examine-os e, se você encontrar lógica de domínio, refatore o código para mover esse comportamento de domínio para os métodos dos objetos de domínio (a raiz de agregação e a entidade filho).

Como exemplo de uma classe de manipulador de comando, o código a seguir mostra a mesma `CreateOrderCommandHandler` classe que você viu no início deste capítulo. Nesse caso, ele também realça o método Handle e as operações com os objetos/agregados de modelo de domínio.

```csharp
public class CreateOrderCommandHandler
        : IRequestHandler<CreateOrderCommand, bool>
{
    private readonly IOrderRepository _orderRepository;
    private readonly IIdentityService _identityService;
    private readonly IMediator _mediator;
    private readonly IOrderingIntegrationEventService _orderingIntegrationEventService;
    private readonly ILogger<CreateOrderCommandHandler> _logger;

    // Using DI to inject infrastructure persistence Repositories
    public CreateOrderCommandHandler(IMediator mediator,
        IOrderingIntegrationEventService orderingIntegrationEventService,
        IOrderRepository orderRepository,
        IIdentityService identityService,
        ILogger<CreateOrderCommandHandler> logger)
    {
        _orderRepository = orderRepository ?? throw new ArgumentNullException(nameof(orderRepository));
        _identityService = identityService ?? throw new ArgumentNullException(nameof(identityService));
        _mediator = mediator ?? throw new ArgumentNullException(nameof(mediator));
        _orderingIntegrationEventService = orderingIntegrationEventService ?? throw new ArgumentNullException(nameof(orderingIntegrationEventService));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));
    }

    public async Task<bool> Handle(CreateOrderCommand message, CancellationToken cancellationToken)
    {
        // Add Integration event to clean the basket
        var orderStartedIntegrationEvent = new OrderStartedIntegrationEvent(message.UserId);
        await _orderingIntegrationEventService.AddAndSaveEventAsync(orderStartedIntegrationEvent);

        // Add/Update the Buyer AggregateRoot
        // DDD patterns comment: Add child entities and value-objects through the Order Aggregate-Root
        // methods and constructor so validations, invariants and business logic
        // make sure that consistency is preserved across the whole aggregate
        var address = new Address(message.Street, message.City, message.State, message.Country, message.ZipCode);
        var order = new Order(message.UserId, message.UserName, address, message.CardTypeId, message.CardNumber, message.CardSecurityNumber, message.CardHolderName, message.CardExpiration);

        foreach (var item in message.OrderItems)
        {
            order.AddOrderItem(item.ProductId, item.ProductName, item.UnitPrice, item.Discount, item.PictureUrl, item.Units);
        }

        _logger.LogInformation("----- Creating Order - Order: {@Order}", order);

        _orderRepository.Add(order);

        return await _orderRepository.UnitOfWork
            .SaveEntitiesAsync(cancellationToken);
    }
}
```

Aqui estão etapas adicionais que um manipulador de comandos deve realizar:

- Use os dados do comando para operar com os métodos e o comportamento da raiz agregada.

- Internamente, dentro dos objetos de domínio, acionar eventos de domínio enquanto a transação é executada, mas isso é transparente do ponto de vista de um manipulador comandos.

- Se o resultado da operação da agregação for bem-sucedido e depois que a transação for concluída, aumente os eventos de integração. (Eles também podem ser acionados por classes de infraestrutura como repositórios).

#### <a name="additional-resources"></a>Recursos adicionais

- **Marque Seemann. Nos limites, os aplicativos não são orientados a objeto** \
  <https://blog.ploeh.dk/2011/05/31/AttheBoundaries,ApplicationsareNotObject-Oriented/>

- **Comandos e eventos** \
  <https://cqrs.nu/Faq/commands-and-events>

- **O que faz um manipulador de comandos?** \
  <https://cqrs.nu/Faq/command-handlers>

- **Jimmy Bogard. Padrões de comando de domínio – manipuladores** \
  <https://jimmybogard.com/domain-command-patterns-handlers/>

- **Jimmy Bogard. Padrões de comando de domínio – validação** \
  <https://jimmybogard.com/domain-command-patterns-validation/>

## <a name="the-command-process-pipeline-how-to-trigger-a-command-handler"></a>O pipeline de processo Comando: como disparar um manipulador de comandos

A próxima pergunta é como invocar um manipulador de comandos. Você poderia chamá-lo manualmente em cada controlador do ASP.NET Core relacionado. No entanto, essa abordagem seria muito acoplada e não seria o ideal.

As outras duas opções principais, que são as opções recomendadas, são:

- Por meio de um artefato de padrão Mediador na memória.

- Com uma fila de mensagens assíncronas, entre os controladores e manipuladores.

### <a name="use-the-mediator-pattern-in-memory-in-the-command-pipeline"></a>Usar o padrão Mediador (na memória) no pipeline de comando

Conforme mostrado na Figura 7-25, em uma abordagem CQRS você usa um mediador, semelhante a um barramento na memória, que é inteligente o suficiente para redirecionar para o manipulador de comandos correto com base no tipo de comando ou DTO que está sendo recebido. As setas pretas simples entre componentes representam as dependências entre objetos (em muitos casos, injetadas por meio da DI) com as respectivas interações.

![Diagrama que mostra um fluxo de dados mais detalhado do cliente para o Database.](./media/microservice-application-layer-implementation-web-api/mediator-cqrs-microservice.png)

**Figura 7-25**. Usando o padrão Mediador no processo em um único microsserviço CQRS

O diagrama acima mostra um zoom da imagem 7-24: o controlador de ASP.NET Core envia o comando para o pipeline de comando do mediador, para que eles obtenham o manipulador apropriado.

O motivo pelo qual o uso do padrão Mediador faz sentido é porque, em aplicativos empresariais, as solicitações de processamento podem ficar complicadas. Você almeja adicionar um número indefinido de interesses transversais como registro em log, validações, auditoria e segurança. Nesses casos, você pode confiar em um pipeline mediador (veja [Padrão mediador](https://en.wikipedia.org/wiki/Mediator_pattern)) para oferecer um meio para esses comportamentos adicionais ou interesses transversais.

Um mediador é um objeto que encapsula o "como" desse processo: ele coordena a execução com base no estado, a maneira como um manipulador de comando é invocado ou a carga que você fornece ao manipulador. Com um componente do mediador, você pode aplicar preocupações abrangentes de forma centralizada e transparente aplicando decoradores (ou [comportamentos de pipeline](https://github.com/jbogard/MediatR/wiki/Behaviors) desde o [mediador 3](https://www.nuget.org/packages/MediatR/3.0.0)). Para obter mais informações, consulte o [Padrão decorador](https://en.wikipedia.org/wiki/Decorator_pattern).

Os decoradores e comportamentos são semelhantes à [AOP (Programação orientada a aspectos)](https://en.wikipedia.org/wiki/Aspect-oriented_programming), aplicada somente a um pipeline de processo específico gerenciado pelo componente mediador. Os aspectos na AOP, que implementam interesses transversais, são aplicados com base em *construtores de aspecto* injetados em tempo de compilação ou com base na interceptação da chamada de objeto. Às vezes, essas duas abordagens de AOP típicas parecem funcionar "como mágica", porque não é fácil entender como a AOP faz seu trabalho. Ao lidar com problemas sérios ou bugs, pode ser difícil depurar a AOP. Por outro lado, esses decoradores/comportamentos são explícitos e aplicados apenas no contexto do mediador, assim, a depuração fica muito mais fácil e previsível.

Por exemplo, no microserviço de ordenação eShopOnContainers, o tem uma implementação de dois comportamentos de exemplo, uma classe [LogBehavior](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Behaviors/LoggingBehavior.cs) e uma classe [ValidatorBehavior](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Behaviors/ValidatorBehavior.cs) . A implementação dos comportamentos é explicada na próxima seção mostrando como o eShopOnContainers usa os [comportamentos](https://github.com/jbogard/MediatR/wiki/Behaviors)do [mediador](https://www.nuget.org/packages/MediatR) .

### <a name="use-message-queues-out-of-proc-in-the-commands-pipeline"></a>Usar filas de mensagens (fora do processo) no pipeline do comando

Outra opção é usar mensagens assíncronas com base em agentes ou filas de mensagens, conforme mostrado na Figura 7-26. Essa opção também pode ser combinada com o componente mediador imediatamente antes do manipulador de comandos.

![Diagrama mostrando o Dataflow usando uma fila de mensagens de alta disponibilidade.](./media/microservice-application-layer-implementation-web-api/add-ha-message-queue.png)

**Figura 7-26**. Usando filas de mensagens (fora do processo e comunicação entre processos) com comandos CQRS

O pipeline do comando também pode ser tratado por uma fila de mensagens de alta disponibilidade para entregar os comandos ao manipulador adequado. O uso de filas de mensagens para aceitar os comandos pode complicar ainda mais o pipeline do comando, pois você provavelmente precisará dividir o pipeline em dois processos conectados por meio da fila de mensagens externa. Ainda assim, isso deve ser usado se você precisa ter melhor escalabilidade e desempenho com base no sistema de mensagens assíncrono. Considere que, no caso da Figura 7-26, o controlador apenas posta a mensagem de comando na fila e retorna. Em seguida, o manipulador de comandos processa as mensagens em seu próprio ritmo. Esse é um grande benefício das filas: a fila de mensagens pode agir como um buffer nos casos em que a hiperescalabilidade é necessária, como para estoques ou qualquer outro cenário com um alto volume de dados de entrada.

No entanto, devido à natureza assíncrona das filas de mensagens, você precisa descobrir como se comunicar com o aplicativo cliente sobre o êxito ou a falha do processo do comando. Como regra, você nunca deve usar comandos "Fire and esqueça". Todos os aplicativos de negócios precisam saber se um comando foi processado com êxito ou, pelo menos, validado e aceito.

Portanto, ser capaz de responder ao cliente depois de validar uma mensagem de comando que foi enviada para uma fila assíncrona adiciona complexidade ao seu sistema, em comparação com um processo de comando em processo que retorna o resultado da operação após a execução da transação. Ao usar filas, talvez seja necessário retornar o resultado do processo do comando por meio de outras mensagens de resultado da operação, o que exigirá comunicação personalizada e componentes adicionais em seu sistema.

Além disso, os comandos assíncronos são unidirecionais, o que, em muitos casos, pode não ser necessário, conforme explicado na seguinte discussão interessante entre Burtsev Alexey e Greg Young em uma [conversa online](https://groups.google.com/forum/#!msg/dddcqrs/xhJHVxDx2pM/WP9qP8ifYCwJ):

> \[Burtsev Alexey \] eu encontrei muitos códigos em que as pessoas usam o manuseio de comandos assíncronos ou mensagens de comando unidirecionais sem qualquer motivo para fazer isso (eles não estão fazendo uma operação longa, eles não estão executando código assíncrono externo, eles nem mesmo limites entre aplicativos para usar o barramento de mensagem). Por que eles introduzem essa complexidade desnecessária? E, na verdade, eu não vi nenhum exemplo de código CQRS com manipuladores de comando de bloqueio até o momento, embora isso funcionaria muito bem na maioria dos casos.
>
> \[Greg Young \] \[ ... \] um comando assíncrono não existe; na verdade, ele é outro evento. Se eu precisar aceitar o que você me envia e gerar um evento se eu discordar, não será mais que você me informasse a fazer algo \[ , não é um comando \] . É você me informando que algo foi feito. Parece que essa é apenas uma pequena diferença inicialmente, mas isso tem várias implicações.

Os comandos assíncronos aumentam significativamente a complexidade de um sistema, porque não há nenhuma maneira simples de indicar falhas. Portanto, os comandos assíncronos não são recomendados a não ser quando há requisitos de dimensionamento ou em casos especiais, ao comunicar os microsserviços internos por meio do sistema de mensagens. Nesses casos, você deve projetar um sistema de relatórios e de recuperação separado para falhas.

Na versão inicial do eShopOnContainers, foi decidido usar o processamento de comando síncrono, iniciado a partir de solicitações HTTP e controladas pelo padrão mediador. Isso permite retornar o êxito ou a falha do processo com facilidade, como na implementação de [CreateOrderCommandHandler](https://github.com/dotnet-architecture/eShopOnContainers/blob/netcore1.1/src/Services/Ordering/Ordering.API/Application/Commands/CreateOrderCommandHandler.cs).

Em qualquer caso, isso deve ser uma decisão com base nos requisitos de negócios do seu aplicativo ou do microserviço.

## <a name="implement-the-command-process-pipeline-with-a-mediator-pattern-mediatr"></a>Implementar o pipeline de processo de comando com um padrão mediador (MediatR)

Como uma implementação de exemplo, este guia propõe o uso do pipeline em processo baseado no padrão Mediador para orientar a ingestão de comando e rotear comandos, na memória, para os manipuladores de comando corretos. O guia também propõe a aplicação de [comportamentos](https://github.com/jbogard/MediatR/wiki/Behaviors) para separar interesses transversais.

Para a implementação no .NET Core, há várias bibliotecas de software livre disponíveis que implementam o padrão Mediador. A biblioteca usada neste guia é a [MediatR](https://github.com/jbogard/MediatR), biblioteca de software livre criada por Jimmy Bogard, mas você pode usar outra abordagem. A MediatR é uma biblioteca pequena e simples que permite que você processe mensagens na memória como um comando, aplicando, ao mesmo tempo, decoradores ou comportamentos.

O uso do padrão Mediador ajuda a reduzir o acoplamento e isolar as preocupações com o trabalho solicitado, ao conectar-se automaticamente com manipulador que executa esse trabalho — nesse caso, os manipuladores de comando.

Outra boa razão para usar o padrão Mediador foi explicada por Jimmy Bogard durante a revisão desse guia:

> Acho que vale a pena mencionar testes aqui – eles oferecem uma janela consistente e adequada sobre o comportamento do seu sistema. Solicitação-entrada, resposta. Descobrimos que o aspecto é bastante valioso na criação de testes com consistência.

Primeiro, vamos dar uma olhada em um controlador WebAPI de exemplo onde você realmente usaria o objeto mediador. Se você não estiver usando o objeto mediador, precisará injetar todas as dependências para esse controlador, coisas como um objeto logger e outros. Portanto, o Construtor seria complicado. Por outro lado, se você usasse o objeto mediador, o construtor do controlador poderia ser muito mais simples, com apenas algumas dependências em vez de muitas dependências, se você tivesse um por operação transversal, como no exemplo a seguir:

```csharp
public class MyMicroserviceController : Controller
{
    public MyMicroserviceController(IMediator mediator,
                                    IMyMicroserviceQueries microserviceQueries)
    {
        // ...
    }
}
```

Veja que o mediador fornece um construtor de controlador da API Web simples e eficiente. Além disso, dentro dos métodos do controlador, o código para enviar um comando para o objeto mediador tem quase uma linha:

```csharp
[Route("new")]
[HttpPost]
public async Task<IActionResult> ExecuteBusinessOperation([FromBody]RunOpCommand
                                                               runOperationCommand)
{
    var commandResult = await _mediator.SendAsync(runOperationCommand);

    return commandResult ? (IActionResult)Ok() : (IActionResult)BadRequest();
}
```

### <a name="implement-idempotent-commands"></a>Implementar comandos idempotentes

Em **eShopOnContainers**, um exemplo mais avançado que o que foi visto está enviando um objeto CreateOrderCommand do microsserviço de pedidos. Mas como o processo comercial de ordenação é um pouco mais complexo e, em nosso caso, ele realmente começa no microserviço basket, essa ação de enviar o objeto CreateOrderCommand é executada de um manipulador de eventos de integração chamado [UserCheckoutAcceptedIntegrationEventHandler](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/IntegrationEvents/EventHandling/UserCheckoutAcceptedIntegrationEventHandler.cs) em vez de um controlador WebAPI simples chamado do aplicativo cliente como no exemplo mais simples anterior.

Mesmo assim, a ação de enviar o Comando para o MediatR é bem semelhante, conforme mostrado no código a seguir.

```csharp
var createOrderCommand = new CreateOrderCommand(eventMsg.Basket.Items,
                                                eventMsg.UserId, eventMsg.City,
                                                eventMsg.Street, eventMsg.State,
                                                eventMsg.Country, eventMsg.ZipCode,
                                                eventMsg.CardNumber,
                                                eventMsg.CardHolderName,
                                                eventMsg.CardExpiration,
                                                eventMsg.CardSecurityNumber,
                                                eventMsg.CardTypeId);

var requestCreateOrder = new IdentifiedCommand<CreateOrderCommand,bool>(createOrderCommand,
                                                                        eventMsg.RequestId);
result = await _mediator.Send(requestCreateOrder);
```

No entanto, esse caso também é um pouco mais avançado, pois também estamos implementando comandos idempotentes. O processo CreateOrderCommand deve ser idempotente, portanto, se a mesma mensagem vier duplicada pela rede, independentemente do motivo, como repetições, a mesma ordem de negócios será processada apenas uma vez.

Isso é implementado encapsulando o comando de negócios (neste caso, CreateOrderCommand) e inserindo-o em um IdentifiedCommand genérico, que é acompanhado por uma ID de cada mensagem proveniente da rede que precisa ser idempotente.

Veja no código abaixo que o IdentifiedCommand não passa de um DTO com uma ID, além do objeto do comando de negócios encapsulado.

```csharp
public class IdentifiedCommand<T, R> : IRequest<R>
    where T : IRequest<R>
{
    public T Command { get; }
    public Guid Id { get; }
    public IdentifiedCommand(T command, Guid id)
    {
        Command = command;
        Id = id;
    }
}
```

Então, o CommandHandler do IdentifiedCommand chamado [IdentifiedCommandHandler.cs](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Commands/IdentifiedCommandHandler.cs) vai basicamente verificar se a ID que está chegando como parte da mensagem já existe em uma tabela. Se ele já existir, esse comando não será processado novamente, portanto, ele se comporta como um comando idempotente. Esse código de infraestrutura é executado pela chamada de método `_requestManager.ExistAsync` abaixo.

```csharp
// IdentifiedCommandHandler.cs
public class IdentifiedCommandHandler<T, R> : IRequestHandler<IdentifiedCommand<T, R>, R>
        where T : IRequest<R>
{
    private readonly IMediator _mediator;
    private readonly IRequestManager _requestManager;
    private readonly ILogger<IdentifiedCommandHandler<T, R>> _logger;

    public IdentifiedCommandHandler(
        IMediator mediator,
        IRequestManager requestManager,
        ILogger<IdentifiedCommandHandler<T, R>> logger)
    {
        _mediator = mediator;
        _requestManager = requestManager;
        _logger = logger ?? throw new System.ArgumentNullException(nameof(logger));
    }

    /// <summary>
    /// Creates the result value to return if a previous request was found
    /// </summary>
    /// <returns></returns>
    protected virtual R CreateResultForDuplicateRequest()
    {
        return default(R);
    }

    /// <summary>
    /// This method handles the command. It just ensures that no other request exists with the same ID, and if this is the case
    /// just enqueues the original inner command.
    /// </summary>
    /// <param name="message">IdentifiedCommand which contains both original command & request ID</param>
    /// <returns>Return value of inner command or default value if request same ID was found</returns>
    public async Task<R> Handle(IdentifiedCommand<T, R> message, CancellationToken cancellationToken)
    {
        var alreadyExists = await _requestManager.ExistAsync(message.Id);
        if (alreadyExists)
        {
            return CreateResultForDuplicateRequest();
        }
        else
        {
            await _requestManager.CreateRequestForCommandAsync<T>(message.Id);
            try
            {
                var command = message.Command;
                var commandName = command.GetGenericTypeName();
                var idProperty = string.Empty;
                var commandId = string.Empty;

                switch (command)
                {
                    case CreateOrderCommand createOrderCommand:
                        idProperty = nameof(createOrderCommand.UserId);
                        commandId = createOrderCommand.UserId;
                        break;

                    case CancelOrderCommand cancelOrderCommand:
                        idProperty = nameof(cancelOrderCommand.OrderNumber);
                        commandId = $"{cancelOrderCommand.OrderNumber}";
                        break;

                    case ShipOrderCommand shipOrderCommand:
                        idProperty = nameof(shipOrderCommand.OrderNumber);
                        commandId = $"{shipOrderCommand.OrderNumber}";
                        break;

                    default:
                        idProperty = "Id?";
                        commandId = "n/a";
                        break;
                }

                _logger.LogInformation(
                    "----- Sending command: {CommandName} - {IdProperty}: {CommandId} ({@Command})",
                    commandName,
                    idProperty,
                    commandId,
                    command);

                // Send the embeded business command to mediator so it runs its related CommandHandler
                var result = await _mediator.Send(command, cancellationToken);

                _logger.LogInformation(
                    "----- Command result: {@Result} - {CommandName} - {IdProperty}: {CommandId} ({@Command})",
                    result,
                    commandName,
                    idProperty,
                    commandId,
                    command);

                return result;
            }
            catch
            {
                return default(R);
            }
        }
    }
}
```

Como o IdentifiedCommand atua como um envelope de comando de negócios, quando o comando comercial precisa ser processado porque não é uma ID repetida, ele pega o comando comercial interno e o reenvia para mediador, como na última parte do código mostrado acima durante `_mediator.Send(message.Command)` a execução, do [IdentifiedCommandHandler.cs](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Commands/IdentifiedCommandHandler.cs).

Ao fazer isso, ele vinculará e executará o manipulador de comandos de negócios, nesse caso, o [CreateOrderCommandHandler](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Commands/CreateOrderCommandHandler.cs), que está executando transações em relação ao banco de dados de ordenação, conforme mostrado no código a seguir.

```csharp
// CreateOrderCommandHandler.cs
public class CreateOrderCommandHandler
        : IRequestHandler<CreateOrderCommand, bool>
{
    private readonly IOrderRepository _orderRepository;
    private readonly IIdentityService _identityService;
    private readonly IMediator _mediator;
    private readonly IOrderingIntegrationEventService _orderingIntegrationEventService;
    private readonly ILogger<CreateOrderCommandHandler> _logger;

    // Using DI to inject infrastructure persistence Repositories
    public CreateOrderCommandHandler(IMediator mediator,
        IOrderingIntegrationEventService orderingIntegrationEventService,
        IOrderRepository orderRepository,
        IIdentityService identityService,
        ILogger<CreateOrderCommandHandler> logger)
    {
        _orderRepository = orderRepository ?? throw new ArgumentNullException(nameof(orderRepository));
        _identityService = identityService ?? throw new ArgumentNullException(nameof(identityService));
        _mediator = mediator ?? throw new ArgumentNullException(nameof(mediator));
        _orderingIntegrationEventService = orderingIntegrationEventService ?? throw new ArgumentNullException(nameof(orderingIntegrationEventService));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));
    }

    public async Task<bool> Handle(CreateOrderCommand message, CancellationToken cancellationToken)
    {
        // Add Integration event to clean the basket
        var orderStartedIntegrationEvent = new OrderStartedIntegrationEvent(message.UserId);
        await _orderingIntegrationEventService.AddAndSaveEventAsync(orderStartedIntegrationEvent);

        // Add/Update the Buyer AggregateRoot
        // DDD patterns comment: Add child entities and value-objects through the Order Aggregate-Root
        // methods and constructor so validations, invariants and business logic
        // make sure that consistency is preserved across the whole aggregate
        var address = new Address(message.Street, message.City, message.State, message.Country, message.ZipCode);
        var order = new Order(message.UserId, message.UserName, address, message.CardTypeId, message.CardNumber, message.CardSecurityNumber, message.CardHolderName, message.CardExpiration);

        foreach (var item in message.OrderItems)
        {
            order.AddOrderItem(item.ProductId, item.ProductName, item.UnitPrice, item.Discount, item.PictureUrl, item.Units);
        }

        _logger.LogInformation("----- Creating Order - Order: {@Order}", order);

        _orderRepository.Add(order);

        return await _orderRepository.UnitOfWork
            .SaveEntitiesAsync(cancellationToken);
    }
}
```

### <a name="register-the-types-used-by-mediatr"></a>Registrar os tipos usados pelo MediatR

Para que o MediatR tome ciência de suas classes de manipulador de comando, é necessário registrar as classes de mediador e as classes de manipulador de comandos em seu contêiner de IoC. Por padrão, o MediatR usa o Autofac como contêiner de IoC, mas você também pode usar o contêiner de IoC interno do ASP.NET Core ou qualquer outro contêiner compatível com o MediatR.

O código a seguir mostra como registrar tipos e comandos do mediador ao usar módulos Autofac.

```csharp
public class MediatorModule : Autofac.Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterAssemblyTypes(typeof(IMediator).GetTypeInfo().Assembly)
            .AsImplementedInterfaces();

        // Register all the Command classes (they implement IRequestHandler)
        // in assembly holding the Commands
        builder.RegisterAssemblyTypes(typeof(CreateOrderCommand).GetTypeInfo().Assembly)
                .AsClosedTypesOf(typeof(IRequestHandler<,>));
        // Other types registration
        //...
    }
}
```

É aí que "a mágica acontece" com o mediador.

Como cada manipulador de comandos implementa a `IRequestHandler<T>` interface genérica, quando você registra os assemblies usando o `RegisteredAssemblyTypes` método, todos os tipos marcados como `IRequestHandler` também são registrados com seus `Commands` . Por exemplo:

```csharp
public class CreateOrderCommandHandler
  : IRequestHandler<CreateOrderCommand, bool>
{
```

Esse é o código que correlaciona comandos com manipuladores de comandos. O manipulador é apenas uma classe simples, mas ele herda de `RequestHandler<T>`, em que T é o tipo de comando, enquanto o MediatR garante que ele seja invocado com o conteúdo correto.

## <a name="apply-cross-cutting-concerns-when-processing-commands-with-the-behaviors-in-mediatr"></a>Aplicar questões abrangentes ao processar comandos com os Comportamentos no MediatR

Há mais um assunto para discutir: a capacidade de aplicar interesses transversais ao pipeline mediador. Você também pode ver, no final do código do módulo de registro do Autofac, como ele registra um tipo de comportamento, especificamente uma classe LoggingBehavior personalizada e uma classe ValidatorBehavior. Mas você também pode adicionar outros comportamentos personalizados.

```csharp
public class MediatorModule : Autofac.Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterAssemblyTypes(typeof(IMediator).GetTypeInfo().Assembly)
            .AsImplementedInterfaces();

        // Register all the Command classes (they implement IRequestHandler)
        // in assembly holding the Commands
        builder.RegisterAssemblyTypes(
                              typeof(CreateOrderCommand).GetTypeInfo().Assembly).
                                   AsClosedTypesOf(typeof(IRequestHandler<,>));
        // Other types registration
        //...
        builder.RegisterGeneric(typeof(LoggingBehavior<,>)).
                                                   As(typeof(IPipelineBehavior<,>));
        builder.RegisterGeneric(typeof(ValidatorBehavior<,>)).
                                                   As(typeof(IPipelineBehavior<,>));
    }
}
```

Essa classe [LoggingBehavior](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Behaviors/LoggingBehavior.cs), que registra informações sobre o manipulador de comando que está sendo executado e se ele foi bem-sucedido ou não, pode ser implementada como no código a seguir.

```csharp
public class LoggingBehavior<TRequest, TResponse>
         : IPipelineBehavior<TRequest, TResponse>
{
    private readonly ILogger<LoggingBehavior<TRequest, TResponse>> _logger;
    public LoggingBehavior(ILogger<LoggingBehavior<TRequest, TResponse>> logger) =>
                                                                  _logger = logger;

    public async Task<TResponse> Handle(TRequest request,
                                        RequestHandlerDelegate<TResponse> next)
    {
        _logger.LogInformation($"Handling {typeof(TRequest).Name}");
        var response = await next();
        _logger.LogInformation($"Handled {typeof(TResponse).Name}");
        return response;
    }
}
```

Basta implementar essa classe de comportamento e registrar o pipeline com ela (no MediatorModule acima) e todos os comandos processados por meio do MediatR registrarão em log as informações sobre a execução.

O microsserviço de pedidos eShopOnContainers também aplica um segundo comportamento para validações básicas, a classe [ValidatorBehavior](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Behaviors/ValidatorBehavior.cs), que depende da biblioteca [FluentValidation](https://github.com/JeremySkinner/FluentValidation), conforme mostrado no código a seguir:

```csharp
public class ValidatorBehavior<TRequest, TResponse>
         : IPipelineBehavior<TRequest, TResponse>
{
    private readonly IValidator<TRequest>[] _validators;
    public ValidatorBehavior(IValidator<TRequest>[] validators) =>
                                                         _validators = validators;

    public async Task<TResponse> Handle(TRequest request,
                                        RequestHandlerDelegate<TResponse> next)
    {
        var failures = _validators
            .Select(v => v.Validate(request))
            .SelectMany(result => result.Errors)
            .Where(error => error != null)
            .ToList();

        if (failures.Any())
        {
            throw new OrderingDomainException(
                $"Command Validation Errors for type {typeof(TRequest).Name}",
                        new ValidationException("Validation exception", failures));
        }

        var response = await next();
        return response;
    }
}
```

Aqui, o comportamento é gerar uma exceção se a validação falhar, mas você também poderá retornar um objeto de resultado, que contém o resultado do comando se ele tiver êxito ou as mensagens de validação, caso não tenha sido. Isso provavelmente tornaria mais fácil exibir os resultados da validação para o usuário.

Em seguida, com base na biblioteca [FluentValidation](https://github.com/JeremySkinner/FluentValidation) , você criaria a validação para os dados passados com CreateOrderCommand, como no código a seguir:

```csharp
public class CreateOrderCommandValidator : AbstractValidator<CreateOrderCommand>
{
    public CreateOrderCommandValidator()
    {
        RuleFor(command => command.City).NotEmpty();
        RuleFor(command => command.Street).NotEmpty();
        RuleFor(command => command.State).NotEmpty();
        RuleFor(command => command.Country).NotEmpty();
        RuleFor(command => command.ZipCode).NotEmpty();
        RuleFor(command => command.CardNumber).NotEmpty().Length(12, 19);
        RuleFor(command => command.CardHolderName).NotEmpty();
        RuleFor(command => command.CardExpiration).NotEmpty().Must(BeValidExpirationDate).WithMessage("Please specify a valid card expiration date");
        RuleFor(command => command.CardSecurityNumber).NotEmpty().Length(3);
        RuleFor(command => command.CardTypeId).NotEmpty();
        RuleFor(command => command.OrderItems).Must(ContainOrderItems).WithMessage("No order items found");
    }

    private bool BeValidExpirationDate(DateTime dateTime)
    {
        return dateTime >= DateTime.UtcNow;
    }

    private bool ContainOrderItems(IEnumerable<OrderItemDTO> orderItems)
    {
        return orderItems.Any();
    }
}

```

Você pode criar validações adicionais. Essa é uma maneira muito eficiente e elegante de se implementar validações de comando.

De maneira semelhante, você pode implementar outros comportamentos para aspectos adicionais ou interesses transversais que você deseja aplicar aos comandos ao manipulá-los.

#### <a name="additional-resources"></a>Recursos adicionais

##### <a name="the-mediator-pattern"></a>O padrão mediador

- **Padrão mediador** \
  [https://en.wikipedia.org/wiki/Mediator\_pattern](https://en.wikipedia.org/wiki/Mediator_pattern)

##### <a name="the-decorator-pattern"></a>O padrão decorador

- **Padrão de decorador** \
  [https://en.wikipedia.org/wiki/Decorator\_pattern](https://en.wikipedia.org/wiki/Decorator_pattern)

##### <a name="mediatr-jimmy-bogard"></a>MediatR (Jimmy Bogard)

- **MediatR.** Repositório do GitHub. \
  <https://github.com/jbogard/MediatR>

- **CQRS com mediador e AutoMapper** \
  <https://lostechies.com/jimmybogard/2015/05/05/cqrs-with-mediatr-and-automapper/>

- **Coloque os controladores de dieta: POSTs e comandos.** \
  <https://lostechies.com/jimmybogard/2013/12/19/put-your-controllers-on-a-diet-posts-and-commands/>

- **Lidando com preocupações abrangentes com um pipeline mediador** \
  <https://lostechies.com/jimmybogard/2014/09/09/tackling-cross-cutting-concerns-with-a-mediator-pipeline/>

- **CQRS e REST: a combinação perfeita** \
  <https://lostechies.com/jimmybogard/2016/06/01/cqrs-and-rest-the-perfect-match/>

- **Exemplos de pipeline do mediador** \
  <https://lostechies.com/jimmybogard/2016/10/13/mediatr-pipeline-examples/>

- **Acessórios de teste de fatia vertical para mediador e ASP.NET Core** \
  <https://lostechies.com/jimmybogard/2016/10/24/vertical-slice-test-fixtures-for-mediatr-and-asp-net-core/>

- **Extensões do mediador para injeção de dependência da Microsoft liberadas** \
  <https://lostechies.com/jimmybogard/2016/07/19/mediatr-extensions-for-microsoft-dependency-injection-released/>

##### <a name="fluent-validation"></a>Validação fluente

- **Jeremy Skinner. FluentValidation.** Repositório do GitHub. \
  <https://github.com/JeremySkinner/FluentValidation>

> [!div class="step-by-step"]
> [Anterior](microservice-application-layer-web-api-design.md) 
>  [Avançar](../implement-resilient-applications/index.md)
