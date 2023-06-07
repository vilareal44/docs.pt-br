---
title: Implementando um modelo de domínio de microsserviço com o .NET Core
description: Arquitetura de microsserviços do .NET para aplicativos .NET em contêineres | Obtenha os detalhes de implementação de um modelo de domínio orientado a DDD.
ms.date: 10/08/2018
ms.openlocfilehash: 0b42ecc2440faf5870b2d99e31d03cda00b21ce0
ms.sourcegitcommit: 5280b2aef60a1ed99002dba44e4b9e7f6c830604
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84306898"
---
# <a name="implement-a-microservice-domain-model-with-net-core"></a>Implementar um modelo de domínio de microsserviço com o .NET Core

Na seção anterior, foram explicados os princípios de design fundamentais e os padrões para criar um modelo de domínio. Agora é hora de explorar possíveis maneiras de implementar o modelo de domínio usando o .NET Core (código C\# simples) e EF Core. Seu modelo de domínio será composto simplesmente pelo seu código. Ele terá apenas os requisitos do modelo EF Core, mas não reais dependências do EF. Você não deve ter dependências rígidas nem referências ao EF Core ou qualquer outro ORM em seu modelo de domínio.

## <a name="domain-model-structure-in-a-custom-net-standard-library"></a>Estrutura do modelo de domínio em uma biblioteca .NET Standard personalizada

A organização de pastas usada para o aplicativo de referência eShopOnContainers demonstra o modelo DDD para o aplicativo. Você pode considerar que uma organização de pastas diferente comunica mais claramente as escolhas de design feitas para o seu aplicativo. Como é possível ver na Figura 7-10, no modelo de domínio de ordenação, há duas agregações: a agregação de ordem e a agregação de comprador. Cada agregação é um grupo de entidades de domínio e objetos de valor, embora você possa ter uma agregação composta por uma única entidade de domínio (a raiz de agregação ou entidade raiz) também.

:::image type="complex" source="./media/net-core-microservice-domain-model/ordering-microservice-container.png" alt-text="Captura de tela do projeto de ordenação. Domain no Gerenciador de Soluções.":::
O Gerenciador de Soluções exibição para o projeto de ordenação. domínio, mostrando a pasta AggregatesModel que contém as pastas BuyerAggregate e OrderAggregate, cada uma contendo suas classes de entidade, arquivos de objeto de valor e assim por diante.
:::image-end:::

**Figura 7-10**. Estrutura de modelo de domínio para o microsserviço de ordenação em eShopOnContainers

Além disso, a camada de modelo de domínio inclui os contratos de repositório (interfaces) que são os requisitos de infraestrutura do seu modelo de domínio. Em outras palavras, essas interfaces expressam quais repositórios e os métodos que a camada de infraestrutura deve implementar. É fundamental que a implementação dos repositórios seja colocada fora da camada de modelo de domínio, na biblioteca de camadas de infraestrutura, de modo que a camada de modelo de domínio não seja "contaminada" por API ou classes de tecnologias de infraestrutura, como Entity Framework.

Você também pode ver uma pasta de tarefa de [propagação](https://martinfowler.com/bliki/Seedwork.html) que contém classes base personalizadas que você pode usar como base para suas entidades de domínio e objetos de valor, para que você não tenha código redundante na classe de objeto de cada domínio.

## <a name="structure-aggregates-in-a-custom-net-standard-library"></a>Estruturar agregações em uma biblioteca .NET Standard personalizada

Uma agregação refere-se a um cluster de objetos de domínio agrupados de acordo com a consistência transacional. Esses objetos podem ser instâncias de entidades (uma das quais é a raiz de agregação ou uma entidade raiz) além de qualquer objeto de valor adicional.

Consistência transacional significa que uma agregação está garantidamente consistente e atualizada ao final de uma ação de negócios. Por exemplo, a agregação de ordem do modelo de domínio de microsserviços de pedidos eShopOnContainers é composta conforme mostra a Figura 7-11.

:::image type="complex" source="./media/net-core-microservice-domain-model/vs-solution-explorer-order-aggregate.png" alt-text="Captura de tela da pasta OrderAggregate e suas classes.":::
Uma exibição detalhada da pasta OrderAggregate: Address.cs é um objeto de valor, IOrderRepository é uma interface de repositório, Order.cs é uma raiz de agregação, OrderItem.cs é uma entidade filho e OrderStatus.cs é uma classe de enumeração.
:::image-end:::

**Figura 7-11**. A agregação de ordem na solução do Visual Studio

Se você abrir qualquer um dos arquivos em uma pasta de agregação, verá como ele está marcado como classe base personalizada ou interface, como objeto de entidade ou valor, conforme implementado na pasta [SeedWork](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/Services/Ordering/Ordering.Domain/SeedWork).

## <a name="implement-domain-entities-as-poco-classes"></a>Implementar entidades de domínio como classes POCO

Você implementa um modelo de domínio no .NET criando classes POCO que implementam suas entidades de domínio. No exemplo a seguir, a classe Ordem é definida como uma entidade e como uma raiz de agregação. Porque a classe Ordem deriva da classe base da Entidade, ela pode reutilizar o código comum relacionado a entidades. Tenha em mente que essas interfaces e classes base são definidas por você no projeto de modelo de domínio, portanto, é o seu código, não o código de infraestrutura de um ORM, como EF.

```csharp
// COMPATIBLE WITH ENTITY FRAMEWORK CORE 2.0
// Entity is a custom base class with the ID
public class Order : Entity, IAggregateRoot
{
    private DateTime _orderDate;
    public Address Address { get; private set; }
    private int? _buyerId;

    public OrderStatus OrderStatus { get; private set; }
    private int _orderStatusId;

    private string _description;
    private int? _paymentMethodId;

    private readonly List<OrderItem> _orderItems;
    public IReadOnlyCollection<OrderItem> OrderItems => _orderItems;

    public Order(string userId, Address address, int cardTypeId, string cardNumber, string cardSecurityNumber,
            string cardHolderName, DateTime cardExpiration, int? buyerId = null, int? paymentMethodId = null)
    {
        _orderItems = new List<OrderItem>();
        _buyerId = buyerId;
        _paymentMethodId = paymentMethodId;
        _orderStatusId = OrderStatus.Submitted.Id;
        _orderDate = DateTime.UtcNow;
        Address = address;

        // ...Additional code ...
    }

    public void AddOrderItem(int productId, string productName,
                            decimal unitPrice, decimal discount,
                            string pictureUrl, int units = 1)
    {
        //...
        // Domain rules/logic for adding the OrderItem to the order
        // ...

        var orderItem = new OrderItem(productId, productName, unitPrice, discount, pictureUrl, units);

        _orderItems.Add(orderItem);

    }
    // ...
    // Additional methods with domain rules/logic related to the Order aggregate
    // ...
}
```

É importante observar que essa é uma entidade de domínio implementada como uma classe POCO. Ela não tem nenhuma dependência direta do Entity Framework Core nem qualquer outra estrutura de infraestrutura. Essa implementação é como deve estar no DDD, apenas código C# que implementa um modelo de domínio.

Além disso, a classe é decorada com uma interface denominada IAggregateRoot. Essa interface é uma interface vazia, às vezes chamada de uma *interface de marcador*, que é usada apenas para indicar que essa classe de entidade também é uma raiz de agregação.

Uma interface de marcador às vezes é considerada como um antipadrão; no entanto, também é uma maneira simples de marcar uma classe, especialmente quando essa interface pode estar em evolução. Um atributo pode ser outra escolha para o marcador, mas é mais rápido ver a classe base (Entity) ao lado da interface IAggregate, em vez de colocar um marcador de atributo Aggregate acima da classe. É uma questão de preferências, de qualquer forma.

Ter uma raiz agregada significa que a maior parte do código relacionado às regras de consistência e de negócios das entidades da agregação deve ser implementada como métodos na classe raiz de ordem agregada (por exemplo, AddOrderItem ao adicionar um objeto OrderItem à agregação). Você não deve criar nem atualizar objetos OrderItems de modo independente ou direto; a classe AggregateRoot deve manter o controle e a consistência de qualquer operação de atualização com relação às suas entidades filho.

## <a name="encapsulate-data-in-the-domain-entities"></a>Encapsular dados nas Entidades de Domínio

Um problema comum em modelos de entidade é que eles expõem propriedades de navegação da coleção como tipos de lista publicamente acessíveis. Isso permite que qualquer desenvolvedor do colaborador manipule o conteúdo desses tipos de coleção, o que pode ignorar importantes regras de negócio relacionadas à coleção, possivelmente deixando o objeto em um estado inválido. A solução para isso é expor o acesso somente leitura a coleções relacionadas e fornecer explicitamente métodos que definem maneiras como os clientes podem manipulá-los.

No código anterior, observe que muitos atributos são somente leitura ou privados e só são atualizáveis pelos métodos de classe, assim, qualquer atualização considera invariáveis de domínio empresarial da conta e lógica especificada dentro do método de classe.

Por exemplo, após os padrões DDD, você ***não* deve fazer o seguinte** de nenhum método de manipulador de comando ou classe de camada de aplicativo (na verdade, deve ser impossível você fazer isso):

```csharp
// WRONG ACCORDING TO DDD PATTERNS – CODE AT THE APPLICATION LAYER OR
// COMMAND HANDLERS
// Code in command handler methods or Web API controllers
//... (WRONG) Some code with business logic out of the domain classes ...
OrderItem myNewOrderItem = new OrderItem(orderId, productId, productName,
    pictureUrl, unitPrice, discount, units);

//... (WRONG) Accessing the OrderItems collection directly from the application layer // or command handlers
myOrder.OrderItems.Add(myNewOrderItem);
//...
```

Neste caso, o método Add é meramente uma operação para adicionar dados, com acesso direto à coleção OrderItems. Portanto, a maioria da lógica, das regras ou das validações de domínio relacionada àquela operação com as entidades filho será espalhada pela camada de aplicativo (manipuladores de comandos e controladores de API da Web).

Se você usar uma solução alternativa para a raiz de agregação, a raiz de agregação não assegurará suas invariáveis, sua validade nem sua consistência. Por fim, você terá código espaguete ou código de script transacional.

Para seguir padrões DDD, as entidades não devem ter setters públicos em nenhuma propriedade de entidade. Alterações em uma entidade devem ser controladas pelos métodos explícitos com linguagem ubíqua explícita sobre a alteração que estão sendo executadas na entidade.

Além disso, coleções na entidade (como os itens do pedido) devem ser propriedades somente leitura (o método AsReadOnly explicado posteriormente). Você deve ser capaz de atualizá-lo somente de dentro de métodos da classe raiz agregada ou de métodos de entidade filho.

Como você pode ver no código da raiz de agregação Order, todos os setters devem ser privados ou, pelo menos, somente leitura externamente, para que qualquer operação em relação aos dados da entidade ou suas entidades filho precise ser executada por meio de métodos na classe de entidade. Isso mantém a consistência de maneira controlada e orientada a objeto, em vez de implementar o código de script transacional.

O snippet de código a seguir mostra a maneira adequada de codificar a tarefa de adicionar um objeto OrderItem para a agregação de ordem de código.

```csharp
// RIGHT ACCORDING TO DDD--CODE AT THE APPLICATION LAYER OR COMMAND HANDLERS
// The code in command handlers or WebAPI controllers, related only to application stuff
// There is NO code here related to OrderItem object's business logic
myOrder.AddOrderItem(productId, productName, pictureUrl, unitPrice, discount, units);

// The code related to OrderItem params validations or domain rules should
// be WITHIN the AddOrderItem method.

//...
```

Neste snippet, a maioria das validações ou da lógica relacionada à criação de um objeto OrderItem estará sob o controle da raiz de agregação de Ordem (no método AddOrderItem), especialmente validações e lógica relacionadas a outros elementos no agregado. Por exemplo, você pode obter o mesmo item de produto como resultado de várias chamadas para AddOrderItem. Nesse método, você pode examinar os itens de produto e consolidar os mesmos itens de produto em um único objeto OrderItem com várias unidades. Além disso, se houver valores de desconto diferentes, mas a ID do produto (product ID) for a mesma, você provavelmente aplicará o desconto maior. Esse princípio se aplica a qualquer outra lógica do domínio para o objeto OrderItem.

Além disso, a nova operação OrderItem(params) também será controlada e executada pelo método AddOrderItem da raiz de agregação de Ordem. Portanto, a maioria da lógica ou das validações relacionadas àquela operação (especialmente tudo o que afeta a consistência entre outras entidades filho) estará em um único lugar na raiz da agregação. Esta é a principal finalidade do padrão de raiz de agregação.

Quando você usar o Entity Framework Core 1.1 ou posterior, uma entidade DDD pode ser melhor expressada porque ela permite [mapear para os campos](https://docs.microsoft.com/ef/core/modeling/backing-field) além das propriedades. Isso é útil ao proteger as coleções de entidades filho ou objetos de valor. Com esse aprimoramento, você pode usar os campos privados, em vez das propriedades, e pode implementar qualquer atualização à coleção de campo nos métodos públicos e fornecer acesso somente leitura por meio do método AsReadOnly.

No DDD, você deseja atualizar a entidade somente por meio de métodos na entidade (ou do Construtor) para controlar qualquer invariável e a consistência dos dados, para que as propriedades sejam definidas somente com um acessador get. As propriedades têm o respaldo de campos privados. Membros privados só pode ser acessados de dentro da classe. No entanto, há uma exceção: o EF Core precisa definir esses campos também (para poder retornar o objeto com os valores adequados).

### <a name="map-properties-with-only-get-accessors-to-the-fields-in-the-database-table"></a>Mapear propriedades com apenas acessadores get para os campos na tabela de banco de dados

Mapeamento de propriedades para colunas de tabela do banco de dados não é uma responsabilidade do domínio, mas parte da camada de infraestrutura e persistência. Mencionamos isso aqui apenas para que você esteja ciente das novas funcionalidades no EF Core 1.1 ou posterior relacionadas a como você pode modelar entidades. Detalhes adicionais sobre este tópico são explicados na seção de infraestrutura e persistência.

Quando você usa o EF Core 1.0 ou posterior, no DbContext, precisa mapear as propriedades definidas somente com getters para os campos reais na tabela de banco de dados. Isso é feito com o método HasField da classe PropertyBuilder.

### <a name="map-fields-without-properties"></a>Mapear campos sem propriedades

Com o recurso no EF Core 1.1 ou posterior para mapear colunas para campos, também é possível não usar propriedades. Em vez disso, você pode apenas mapear as colunas de uma tabela para campos. Um caso de uso comum para isso são campos privados para um estado interno que não precisam ser acessados de fora da entidade.

Por exemplo, no exemplo de código anterior, OrderAggregate, há vários campos privados, como o campo `_paymentMethodId`, que não têm nenhuma propriedade relacionada para um setter ou um getter. Esse campo também pode ser calculado dentro da lógica de negócios do pedido e usado a partir dos métodos do pedido, mas ele também precisa ser persistido no banco de dados. Assim, no EF Core (desde a v1.1), existe uma maneira de mapear um campo sem uma propriedade relacionada a uma coluna no banco de dados. Isso também é explicado na seção [Camada de infraestrutura](ddd-oriented-microservice.md#the-infrastructure-layer) deste guia.

### <a name="additional-resources"></a>Recursos adicionais

- **Vaughn Vernon. A modelagem agrega com DDD e Entity Framework.** Observe que este *não* é um Entity Framework Core. \
  <https://kalele.io/blog-posts/modeling-aggregates-with-ddd-and-entity-framework/>

- **Julie Lerman. Pontos de dados-codificação para o design controlado por domínio: dicas para desenvolvedores focados em dados** \
  <https://docs.microsoft.com/archive/msdn-magazine/2013/august/data-points-coding-for-domain-driven-design-tips-for-data-focused-devs>

- **Udi Dahan. Como criar modelos de domínio totalmente encapsulados** \
  <https://udidahan.com/2008/02/29/how-to-create-fully-encapsulated-domain-models/>

> [!div class="step-by-step"]
> [Anterior](microservice-domain-model.md) 
>  [Avançar](seedwork-domain-model-base-classes-interfaces.md)
