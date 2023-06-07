---
title: Projetando validações na camada de modelo de domínio
description: Arquitetura de microsserviços .NET para aplicativos .NET em contêineres | Compreenda conceitos-chave de validações de modelo de domínio.
ms.date: 10/08/2018
ms.openlocfilehash: f1e2d7430c642ad47f79cdd34d3a65e2cc70e239
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87164273"
---
# <a name="design-validations-in-the-domain-model-layer"></a>Projetar validações na camada de modelo de domínio

Em DDD, as regras de validação podem ser consideradas invariáveis. A principal responsabilidade de uma agregação é impor invariáveis entre as alterações de estado para todas as entidades dentro daquela agregação.

Entidades de domínio devem ser sempre entidades válidas. Existe um determinado número de invariáveis para um objeto que devem ser sempre verdadeiras. Por exemplo, um objeto de item do pedido sempre deve ter uma quantidade que deve ser um inteiro positivo, além de um nome de artigo e preço. Portanto, a imposição de invariáveis é de responsabilidade das entidades de domínio (especialmente da raiz de agregação) e um objeto de entidade não deve ser capaz de existir sem ser válido. Regras invariáveis simplesmente são expressas como contratos e exceções ou notificações são geradas quando elas são violadas.

O raciocínio por trás disso é que vários bugs ocorrerem porque os objetos estão em um estado em que nunca deveriam ter ficado. Esta [discussão online](http://codebetter.com/gregyoung/2009/05/22/always-valid/) é uma boa explicação de Greg Young.

Vamos propor que agora temos um SendUserCreationEmailService que usa um UserProfile..., como podemos racionalizar nesse serviço que o Nome não é nulo? Podemos verificar novamente? Ou, mais provável… você apenas não se preocupa em verificar e "espera pelo melhor": você espera que alguém tenha se preocupado em validá-lo antes de enviá-lo a você. É claro que, usando TDD, um dos primeiros testes que devemos escrever é que, se eu enviar um cliente com um nome nulo, isso deverá gerar um erro. Mas depois de começarmos a escrever esses tipos de testes repetidamente, percebemos... "Espere se nunca permitisse que o nome se torne nulo, não teríamos todos esses testes".

## <a name="implement-validations-in-the-domain-model-layer"></a>Implementar validações na camada de modelo de domínio

As validações normalmente são implementadas em construtores de entidade de domínio ou em métodos que podem atualizar a entidade. Existem várias maneiras de implementar validações, como verificar dados e aumentar exceções se a validação falhar. Também há padrões mais avançados, como usar o padrão de Especificação para validações e o padrão de Notificação para retornar uma coleção de erros, em vez de retornar uma exceção para cada validação conforme ela ocorre.

### <a name="validate-conditions-and-throw-exceptions"></a>Validar condições e gerar exceções

O exemplo de código a seguir mostra a abordagem mais simples de validação em uma entidade de domínio gerando uma exceção. Na tabela de referências no final desta seção, você encontrará links para implementações mais avançadas com base nos padrões que discutimos anteriormente.

```csharp
public void SetAddress(Address address)
{
    _shippingAddress = address?? throw new ArgumentNullException(nameof(address));
}
```

Um exemplo melhor seria demonstrar a necessidade de garantir que o estado interno não tenha mudado ou que todas as mutações para um método ocorreram. Por exemplo, a implementação a seguir deixará o objeto em um estado inválido:

```csharp
public void SetAddress(string line1, string line2,
    string city, string state, int zip)
{
    _shippingAddress.line1 = line1 ?? throw new ...
    _shippingAddress.line2 = line2;
    _shippingAddress.city = city ?? throw new ...
    _shippingAddress.state = (IsValid(state) ? state : throw new …);
}
```

Se o valor do estado for inválido, a primeira linha de endereço e a cidade já terão sido alteradas. Isso pode tornar o endereço inválido.

Uma abordagem semelhante pode ser usada no construtor da entidade, gerando uma exceção para garantir que a entidade seja válida depois de ser criada.

### <a name="use-validation-attributes-in-the-model-based-on-data-annotations"></a>Usar atributos de validação no modelo com base em anotações de dados

Anotações de dados, como os atributos Required ou MaxLength necessários, pode ser usado para configurar propriedades de campo de banco de dados do EF Core, conforme explicado em detalhes na seção [Mapeamento de tabela](infrastructure-persistence-layer-implementation-entity-framework-core.md#table-mapping), mas [elas não funcionam mais para validação de entidade no EF Core](https://github.com/dotnet/efcore/issues/3680) (o método <xref:System.ComponentModel.DataAnnotations.IValidatableObject.Validate%2A?displayProperty=nameWithType> também não funciona mais para isso) como ocorria desde o EF 4.x no .NET Framework.

As anotações de dados e a <xref:System.ComponentModel.DataAnnotations.IValidatableObject> interface ainda podem ser usadas para validação de modelo durante a associação de modelo, antes da invocação de ações do controlador como de costume, mas esse modelo é destinado a ser um ViewModel ou dto e isso é um problema de API ou do MVC que não é uma preocupação com o modelo de domínio.

Tendo esclarecido a diferença conceitual, você ainda poderá usar anotações de dados e `IValidatableObject` na classe de entidade para a validação se as ações receberem um parâmetro de objeto de classe de entidade, o que não é recomendado. Nesse caso, a validação ocorrerá na associação de modelo, logo antes de invocar a ação, e você poderá verificar a Propriedade ModelState. IsValid do controlador para verificar o resultado, mas, novamente, isso ocorrerá no controlador, não antes de persistir o objeto de entidade no DbContext, como foi feito desde o EF 4. x.

Você ainda pode implementar a validação personalizada na classe de entidade usando as anotações de dados e o `IValidatableObject.Validate` método, substituindo o método SaveChanges do DbContext.

Você pode ver um exemplo de implementação para validar entidades `IValidatableObject`[neste comentário no GitHub](https://github.com/dotnet/efcore/issues/3680#issuecomment-155502539). Esse exemplo não faz validações baseadas em atributo, mas elas devem ser fáceis de implementar usando reflexão na mesma substituição.

No entanto, de um ponto de vista do DDD, o modelo de domínio é mais bem mantido com o uso de exceções nos métodos de comportamento de sua entidade ou pela implementação dos padrões de especificação e de notificação para impor regras de validação.

Pode fazer sentido usar anotações de dados na camada de aplicativo em classes ViewModel (em vez de entidades de domínio) que aceitem a entrada para permitir a validação do modelo na camada da interface do usuário. No entanto, isso não deve ser feito na exclusão de validação dentro do modelo de domínio.

### <a name="validate-entities-by-implementing-the-specification-pattern-and-the-notification-pattern"></a>Validar entidades implementando o padrão de Especificação e o padrão de Notificação

Por fim, uma abordagem mais elaborada para implementar a validação no modelo de domínio é implementando o padrão de Especificação em conjunto com o padrão de Notificação, conforme explicado em alguns dos recursos adicionais listados posteriormente.

Vale a pena mencionar que você também pode usar apenas um desses padrões — por exemplo, validação manual com instruções de controle, mas usando o padrão de Notificação para empilhar e retornar uma lista de erros de validação.

### <a name="use-deferred-validation-in-the-domain"></a>Usar a validação adiada no domínio

Existem várias abordagens para lidar com validações adiadas no domínio. Em seu livro [Implementing Domain-Driven Design](https://www.amazon.com/Implementing-Domain-Driven-Design-Vaughn-Vernon/dp/0321834577) (Implementando design controlado por domínio), Vaughn Vernon discute isso na seção sobre a validação.

### <a name="two-step-validation"></a>Validação de duas etapas

Considere também a validação de duas etapas. Use a validação em nível de campo em seu comando de DTOs (Objetos de Transferência de Dados) e a validação em nível de domínio dentro de suas entidades. Você pode fazer isso retornando um objeto de resultado, em vez de exceções para tornar mais fácil lidar com os erros de validação.

Usando a validação de campo com anotações de dados, por exemplo, você não duplica a definição de validação. A execução, no entanto, pode estar do lado do servidor e do lado do cliente no caso de DTOs (comandos e ViewModels, por exemplo).

## <a name="additional-resources"></a>Recursos adicionais

- **Rachel Appel. Introdução à validação de modelo no ASP.NET Core MVC** \
  <https://docs.microsoft.com/aspnet/core/mvc/models/validation>

- **Rick Anderson. Adicionando validação** \
  <https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/validation>

- **Martin Fowler. Substituindo exceções de lançamento com notificação em validações** \
  <https://martinfowler.com/articles/replaceThrowWithNotification.html>

- **Padrões de especificação e de notificação** \
  <https://www.codeproject.com/Tips/790758/Specification-and-Notification-Patterns>

- **Nível Gorodinski. Validação no design controlado por domínio (DDD)** \
  <http://gorodinski.com/blog/2012/05/19/validation-in-domain-driven-design-ddd/>

- **Tomada Colin. Validação do modelo de domínio** \
  <https://colinjack.blogspot.com/2008/03/domain-model-validation.html>

- **Jimmy Bogard. Validação em um mundo DDD** \
  <https://lostechies.com/jimmybogard/2009/02/15/validation-in-a-ddd-world/>

> [!div class="step-by-step"]
> [Anterior](enumeration-classes-over-enum-types.md) 
>  [Avançar](client-side-validation.md)
