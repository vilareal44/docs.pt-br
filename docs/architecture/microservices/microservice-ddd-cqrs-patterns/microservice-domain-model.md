---
title: Criando um modelo de domínio de microsserviço
description: Arquitetura de Microsserviços .NET para aplicativos .NET em contêineres | Entenda os principais conceitos ao projetar um modelo de domínio orientado a DDD.
ms.date: 01/30/2020
ms.openlocfilehash: fe78e719570d5758b71531beab883e5c24a88dca
ms.sourcegitcommit: 5280b2aef60a1ed99002dba44e4b9e7f6c830604
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84306897"
---
# <a name="design-a-microservice-domain-model"></a>Projetar um modelo de domínio de microsserviço

*Definir um modelo de domínio avançado para cada microsserviço empresarial ou Contexto Limitado.*

Sua meta é criar um único modelo de domínio coeso para cada microsserviço de negócios ou BC (Contexto Limitado). No entanto, tenha em mente que um microsserviço de BC ou de continuidade de negócios, às vezes, pode ser composto por vários serviços físicos que compartilham um único modelo de domínio. O modelo de domínio precisa capturar as regras, o comportamento, a linguagem de negócios e as restrições do único Contexto Limitado ou microsserviço de negócios que ele representa.

## <a name="the-domain-entity-pattern"></a>O padrão de entidade de domínio

As entidades representam objetos de domínio e são definidas principalmente pela identidade, a continuidade e a persistência ao longo do tempo e não apenas pelos atributos que as compõem. Como Eric Evans diz, "um objeto definido principalmente por sua identidade é chamado de entidade". As entidades são muito importantes no modelo de domínio, pois elas são a base para um modelo. Portanto, você deve identificá-las e criá-las com cuidado.

*A identidade de uma entidade pode cruzar vários microserviços ou contextos limitados.*

A mesma identidade (ou seja, o mesmo valor `Id`, embora talvez não a mesma entidade de domínio) pode ser modelada em vários Contextos Limitados ou microsserviços. No entanto, isso não significa que a mesma entidade, com os mesmos atributos e a mesma lógica, possa ser implementada em vários Contextos Limitados. Em vez disso, as entidades em cada contexto limitado limitam seus atributos e comportamentos aos necessários no domínio do contexto limitado.

Por exemplo, a entidade comprador pode ter a maioria dos atributos de uma pessoa que são definidos na entidade usuário no microserviço de perfil ou de identidade, incluindo a identidade. Mas a entidade de comprador no microsserviço de pedidos pode ter menos atributos, porque somente determinados dados do comprador estão relacionados ao processo de pedido. O contexto de cada microsserviço ou o Contexto Limitado afeta o modelo de domínio.

*As entidades de domínio precisam implementar o comportamento, além de implementar os atributos de dados.*

Uma entidade de domínio no DDD precisa implementar a lógica do domínio ou o comportamento relacionado aos dados da entidade (o objeto acessado na memória). Por exemplo, como parte de uma classe de entidade de pedido, você precisa ter a lógica de negócios e as operações implementadas como métodos para tarefas como adicionar um item de pedido, validação de dados e cálculo de total. Os métodos da entidade cuidam das invariáveis e das regras da entidade, em vez de fazer com que essas regras se espalhem pela camada do aplicativo.

A Figura 7-8 mostra uma entidade de domínio que, além de implementar os atributos de dados, também implementa as operações ou os métodos com a lógica de domínio relacionada.

![Diagrama mostrando o padrão de uma entidade de domínio.](./media/microservice-domain-model/domain-entity-pattern.png)

**Figura 7-8**. Exemplo de um projeto de entidade de domínio implementando dados e também comportamento

Uma entidade de modelo de domínio implementa comportamentos por meio de métodos, ou seja, não é um modelo "anêmico". Obviamente, também é possível haver entidades que não implementam nenhuma lógica como parte da classe da entidade. Isso poderá ocorrer em entidades filhas dentro de uma agregação se a entidade filha não tiver nenhuma lógica especial porque a maioria da lógica está definida na raiz da agregação. Se você tiver um microserviço complexo que tenha uma lógica implementada nas classes de serviço em vez de nas entidades de domínio, poderá estar se enquadrando no modelo de domínio anêmico, explicado na seção a seguir.

### <a name="rich-domain-model-versus-anemic-domain-model"></a>Modelo de domínio avançado em comparação com o modelo de domínio anêmico

Em sua postagem [AnemicDomainModel](https://martinfowler.com/bliki/AnemicDomainModel.html), Martin Fowler descreve um modelo de domínio fraco desta forma:

O sintoma básico de um modelo de domínio anêmico é que, à primeira vista, ele parece real. Existem objetos, muitos nomeados de acordo com os nomes no espaço de domínio, conectados com as relações avançadas e a estrutura que os modelos de domínio verdadeiros têm. A verdade aparece quando você observa o comportamento e percebe que não há praticamente nenhum comportamento nesses objetos, ou seja, eles não passam de pacotes de getters e setters.

É claro que, quando você usar um modelo de domínio anêmico, serão usados os modelos de dados de um conjunto de objetos de serviço (tradicionalmente chamado de *camada comercial*) que captura toda a lógica de negócios ou do domínio. A camada de negócios fica acima do modelo de dados e usa o modelo de dados apenas como dados.

O modelo de domínio anêmico é apenas um design de estilo de procedimento. Os objetos da entidade anêmica não são objetos reais, porque eles não têm comportamento (métodos). Eles apenas mantêm as propriedades dos dados e, portanto, não são de design orientado a objeto. Ao colocar todo o comportamento em objetos de serviço (a camada de negócios), essencialmente você acaba com o [código espaguete](https://en.wikipedia.org/wiki/Spaghetti_code) ou com os [scripts de transação](https://martinfowler.com/eaaCatalog/transactionScript.html)e, portanto, perde as vantagens que um modelo de domínio fornece.

De qualquer maneira, se o microsserviço ou o Contexto Limitado for muito simples (um serviço de CRUD), o modelo de domínio anêmico na forma de objetos de entidade apenas com as propriedades dos dados já poderá ser suficiente e talvez não compense implementar os padrões mais complexos de DDD. Nesse caso, ele será simplesmente um modelo de persistência, porque você criou uma entidade intencionalmente apenas com os dados para fins de CRUD.

É por isso que as arquiteturas de microsserviços são perfeitas para uma abordagem de várias arquiteturas, dependendo de cada Contexto Limitado. Por exemplo, no eShopOnContainers, o microsserviço de pedidos implementa os padrões de DDD, mas o microsserviço de catálogo, que é um serviço de CRUD simples, não implementa.

Algumas pessoas dizem que o modelo de domínio anêmico é um antipadrão. Isso realmente depende do que está sendo implementando. Se o microsserviço que você estiver criando for simples o suficiente (por exemplo, um serviço de CRUD), seguir o modelo de domínio anêmico não será um antipadrão. No entanto, se você precisar lidar com a complexidade do domínio de um microserviço que tem muitas regras de negócio em constante mudança, o modelo de domínio anêmico pode ser um antipadrão para esse microserviço ou contexto limitado. Nesse caso, criá-lo como um modelo avançado com entidades contendo dados e comportamentos, bem como implementar os padrões de DDD adicionais (agregações, objetos de valor, etc.) pode trazer enormes benefícios para o sucesso a longo prazo desse microsserviço.

#### <a name="additional-resources"></a>Recursos adicionais

- **DevIQ. Entidade de domínio** \
  <https://deviq.com/entity/>

- **Martin Fowler. O modelo de domínio** \
  <https://martinfowler.com/eaaCatalog/domainModel.html>

- **Martin Fowler. O modelo de domínio anêmico** \
  <https://martinfowler.com/bliki/AnemicDomainModel.html>

### <a name="the-value-object-pattern"></a>O padrão de objeto de valor

Como Eric Evans foi observado, "muitos objetos não têm identidade conceitual. Esses objetos descrevem determinadas características de algo. "

Uma entidade requer uma identidade, mas há muitos objetos em um sistema que não requerem, como o padrão de objeto de valor. Um objeto de valor é um objeto sem nenhuma identidade conceitual que descreve um aspecto de domínio. Esses são objetos dos quais uma instância é criada para representar os elementos de design que interessam apenas temporariamente. Importa *o que* eles são e não *quem* são. Exemplos incluem números e cadeias de caracteres, mas também podem ser conceitos de nível superior como grupos de atributos.

Algo que é uma entidade em um microsserviço pode não ser uma entidade em outro microsserviço, porque no segundo caso, o Contexto Limitado pode ter um significado diferente. Por exemplo, um endereço em um aplicativo de comércio eletrônico pode não ter uma identidade, já que ele pode representar apenas um grupo de atributos do perfil do cliente para uma pessoa ou empresa. Nesse caso, o endereço deve ser classificado como um objeto de valor. No entanto, em um aplicativo de uma empresa utilitária de energia elétrica, o endereço do cliente pode ser importante para o domínio de negócios. Portanto, o endereço precisa ter uma identidade para que o sistema de cobrança possa ser vinculado diretamente ao endereço. Nesse caso, o endereço deve ser classificado como uma entidade de domínio.

Uma pessoa com um nome e sobrenome geralmente é uma entidade porque uma pessoa tem identidade, mesmo que o nome e sobrenome coincidam com outro conjunto de valores, como se esses nomes também se referirem a uma pessoa diferente.

Os objetos de valor são difíceis de gerenciar em bancos de dados relacionais e ORMs como Entity Framework (EF), enquanto em bancos de dados orientados a documentos eles são mais fáceis de implementar e usar.

EF Core 2,0 e versões posteriores incluem o recurso de [entidades de propriedade](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-0/#owned-entities-and-table-splitting) que facilita o tratamento de objetos de valor, como veremos detalhadamente mais adiante.

#### <a name="additional-resources"></a>Recursos adicionais

- **Martin Fowler. Padrão de objeto de valor** \
  <https://martinfowler.com/bliki/ValueObject.html>

- **Objeto de valor** \
  <https://deviq.com/value-object/>

- **Objetos de valor no desenvolvimento controlado por testes** \
  [https://leanpub.com/tdd-ebook/read\#leanpub-auto-value-objects](https://leanpub.com/tdd-ebook/read#leanpub-auto-value-objects)

- **Eric Evans. Design controlado por domínio: solução de complexidade no coração do software.** (Livro; inclui uma discussão sobre objetos de valor) \
  <https://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215/>

### <a name="the-aggregate-pattern"></a>O padrão de agregação

Um modelo de domínio contém diferentes clusters de entidades de dados e processos que podem controlar uma área significativa de funcionalidades, como inventário ou execução de pedidos. Uma unidade de DDD mais refinada é a agregação, que descreve um cluster ou o grupo de entidades e os comportamentos que podem ser tratados como uma unidade coesa.

Normalmente, a agregação é definida com base nas transações que são necessárias. Um exemplo clássico é um pedido que também contém uma lista de itens do pedido. Geralmente, um item do pedido será uma entidade. Mas ela será uma entidade filha dentro da agregação de pedido, que também conterá a entidade de pedido como entidade raiz, geralmente chamada de raiz de agregação.

Pode ser difícil identificar agregações. Uma agregação é um grupo de objetos que precisam ser consistentes em conjunto, mas não basta apenas selecionar um grupo de objetos e rotulá-lo como uma agregação. Você precisa começar com um conceito de domínio e pensar sobre as entidades que são usadas nas transações mais comuns relacionadas a esse conceito. As entidades que precisam ser consistentes entre as transações são as que formam uma agregação. Pensar sobre as operações de transação provavelmente é a melhor maneira de identificar as agregações.

### <a name="the-aggregate-root-or-root-entity-pattern"></a>O padrão de raiz de agregação ou de entidade raiz

Uma agregação é composta por pelo menos uma entidade: a raiz de agregação, também chamada de entidade raiz ou entidade principal. Além disso, ela pode ter várias entidades filhas e objetos de valor, com todas as entidades e os objetos trabalhando juntos para implementar as transações e os comportamentos necessários.

A finalidade de uma raiz de agregação é garantir a consistência da agregação. Ela deve ser o único ponto de entrada para as atualizações da agregação por meio de métodos ou operações na classe raiz de agregação. Você deve fazer alterações nas entidades na agregação apenas por meio da raiz de agregação. É o guardião de consistência da agregação, considerando todas as invariáveis e regras de consistência que você pode precisar obedecer em sua agregação. Se você alterar uma entidade filha ou um objeto de valor de forma independente, a raiz de agregação não poderá garantir que a agregação esteja em um estado válido. Isso seria semelhante a uma tabela com um segmento flexível. Manter a consistência é o principal objetivo da raiz de agregação.

Na Figura 7-9, veja as agregações de exemplo, como a agregação de comprador, que contém uma única entidade (a raiz de agregação de comprador). A agregação de pedido contém várias entidades e um objeto de valor.

![Diagrama comparando uma agregação de comprador e uma agregação de ordem.](./media/microservice-domain-model/buyer-order-aggregate-pattern.png)

**Figura 7-9**. Exemplo de agregações com uma única entidade ou com várias entidades

Um modelo de domínio de DDD é composto por agregações, uma agregação pode ter uma única entidade ou mais e pode incluir também objetos de valor. Observe que a agregação de comprador poderá ter entidades filhas adicionais, dependendo do domínio, como ocorre no microsserviço de pedidos no aplicativo eShopOnContainers de referência. A Figura 7-9 apenas ilustra um caso em que o comprador tem uma única entidade, como exemplo de uma agregação que contém somente uma raiz de agregação.

Para manter a separação de agregações e manter limites claros entre elas, uma prática recomendada em um modelo de domínio de DDD é não permitir a navegação direta entre as agregações e ter apenas o campo de FK (chave estrangeira), como foi implementado no [Modelo de domínio do microsserviço de pedidos](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.Domain/AggregatesModel/OrderAggregate/Order.cs) no eShopOnContainers. A entidade Order tem apenas um campo de chave estrangeira para o comprador, mas não uma EF Core propriedade de navegação, conforme mostrado no código a seguir:

```csharp
public class Order : Entity, IAggregateRoot
{
    private DateTime _orderDate;
    public Address Address { get; private set; }
    private int? _buyerId; // FK pointing to a different aggregate root
    public OrderStatus OrderStatus { get; private set; }
    private readonly List<OrderItem> _orderItems;
    public IReadOnlyCollection<OrderItem> OrderItems => _orderItems;
    // ... Additional code
}
```

Para identificar e trabalhar com agregações é necessário fazer pesquisas e ter experiência. Para obter mais informações, consulte a lista de Recursos adicionais a seguir.

#### <a name="additional-resources"></a>Recursos adicionais

- **Vaughn Vernon. Design agregado efetivo-parte I: modelando uma única agregação** (de <https://dddcommunity.org/> ) \
  <https://dddcommunity.org/wp-content/uploads/files/pdf_articles/Vernon_2011_1.pdf>

- **Vaughn Vernon. Design agregado efetivo-parte II: fazer com que as agregações funcionem juntas** (de <https://dddcommunity.org/> ) \
  <https://dddcommunity.org/wp-content/uploads/files/pdf_articles/Vernon_2011_2.pdf>

- **Vaughn Vernon. Design agregado efetivo-parte III: obtendo informações sobre a descoberta** (de <https://dddcommunity.org/> ) \
  <https://dddcommunity.org/wp-content/uploads/files/pdf_articles/Vernon_2011_3.pdf>

- **Sergey Grybniak. Padrões de design tático DDD** \
  <https://www.codeproject.com/Articles/1164363/Domain-Driven-Design-Tactical-Design-Patterns-Part>

- **Chris Richardson. Desenvolvendo microserviços transacionais usando agregações** \
  <https://www.infoq.com/articles/microservices-aggregates-events-cqrs-part-1-richardson>

- **DevIQ. O padrão agregado** \
  <https://deviq.com/aggregate-pattern/>

>[!div class="step-by-step"]
>[Anterior](ddd-oriented-microservice.md) 
> [Avançar](net-core-microservice-domain-model.md)
