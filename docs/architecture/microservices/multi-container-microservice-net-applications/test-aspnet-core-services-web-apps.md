---
title: Testar serviços e aplicativos Web do ASP.NET Core
description: Arquitetura de microsserviços do .NET para aplicativos .NET em contêineres | Explorar uma arquitetura para testar serviços e aplicativos Web do ASP.NET Core em contêineres.
ms.date: 08/07/2020
ms.openlocfilehash: a27b3b8d392c5e1a7d1961307e6de95659cd823e
ms.sourcegitcommit: 1e6439ec4d5889fc08cf3bfb4dac2b91931eb827
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/08/2020
ms.locfileid: "88024596"
---
# <a name="testing-aspnet-core-services-and-web-apps"></a>Testar serviços e aplicativos Web do ASP.NET Core

Os controladores são uma parte central de qualquer serviço de API do ASP.NET Core e aplicativo Web ASP.NET MVC. Assim, você precisa estar confiante de que eles se comportarão conforme o esperado para o aplicativo. Testes automatizados podem fornecer essa confiança e detectar erros antes que eles atinjam a produção.

É necessário testar como o controlador se comporta com base em entradas válidas ou inválidas, bem como testar suas respostas com base nos resultados da operação de negócios realizada. No entanto, você deveria ter esses tipos de testes nos seus microsserviços:

- Testes de unidade. Eles garantem que os componentes individuais do aplicativo funcionam como esperado. Instruções assert testam a API do componente.

- Testes de integração. Garantem que as interações do componente funcionam como esperado em artefatos externos, como bancos de dados. Instruções assert podem testar a API do componente, a interface do usuário ou os efeitos colaterais de ações, como E/S de banco de dados, registros em log etc.

- Testes funcionais para cada microsserviço. Eles garantem que o aplicativo funcione conforme o esperado da perspectiva do usuário.

- Testes de serviço. Garantem que os casos de uso do serviço de ponta a ponta sejam testados, incluindo testes de vários serviços ao mesmo tempo. Para esse tipo de teste, é necessário preparar o ambiente primeiro. Nesse caso, significa iniciar os serviços (por exemplo, usando o docker-compose up).

### <a name="implementing-unit-tests-for-aspnet-core-web-apis"></a>Implementar testes de unidade para APIs Web do ASP.NET Core

O teste de unidade envolve o teste de uma parte de um aplicativo em isolamento de sua infraestrutura e suas dependências. Ao executar a lógica do controlador de teste de unidade, somente o conteúdo de uma única ação ou método é testado, e não o comportamento de suas dependências ou da estrutura. Os testes de unidade não detectam problemas na interação entre componentes, o que é a finalidade dos testes de integração.

Ao executar o teste de unidade nas ações do controlador, concentre-se apenas em seu comportamento. Um teste de unidade de controlador evita itens como filtros, roteamento ou model binding (o mapeamento dos dados de solicitação a um ViewModel ou DTO). Ao se concentrarem no teste de apenas um item, os testes de unidade geralmente são simples de serem gravados e rápidos de serem executados. Um conjunto bem escrito de testes de unidade pode ser executado com frequência sem muita sobrecarga.

Testes de unidade são implementados com base em estruturas de teste como xUnit.net, MSTest, Moq ou NUnit. Para o aplicativo de exemplo eShopOnContainers, estamos usando o xUnit.

Ao gravar um teste de unidade de um controlador de API Web, você cria instâncias da classe do controlador usando diretamente a nova palavra-chave no C\#, para que o teste seja executado o mais rápido possível. O exemplo a seguir mostra como fazer isso usando o [xUnit](https://xunit.github.io/) como a estrutura de teste.

```csharp
[Fact]
public async Task Get_order_detail_success()
{
    //Arrange
    var fakeOrderId = "12";
    var fakeOrder = GetFakeOrder();

    //...

    //Act
    var orderController = new OrderController(
        _orderServiceMock.Object,
        _basketServiceMock.Object,
        _identityParserMock.Object);

    orderController.ControllerContext.HttpContext = _contextMock.Object;
    var actionResult = await orderController.Detail(fakeOrderId);

    //Assert
    var viewResult = Assert.IsType<ViewResult>(actionResult);
    Assert.IsAssignableFrom<Order>(viewResult.ViewData.Model);
}
```

### <a name="implementing-integration-and-functional-tests-for-each-microservice"></a>Implementar testes funcionais e de integração em cada microsserviço

Conforme observado, testes funcionais e de integração têm finalidades e objetivos diferentes. No entanto, a maneira de implementar ambos ao testar os controladores do ASP.NET Core é semelhante. Nesta seção, o foco é o teste de integração.

Testes de integração garantem que os componentes de um aplicativo funcionem corretamente quando montados. O ASP.NET Core é compatível com testes de integração por meio de estruturas de teste de unidade e um host Web de testes interno que pode ser utilizado para lidar com solicitações sem sobrecarga de rede.

Ao contrário do teste de unidade, os testes de integração frequentemente envolvem questões relacionadas com a infraestrutura do aplicativo, como banco de dados, sistema de arquivos, recurso de rede ou solicitações e respostas da Web. Testes de unidade usam objetos falsos ou fictícios em vez disso. Porém, a finalidade dos testes de integração é confirmar que o sistema funciona como o esperado com esses sistemas. Então, nos testes de integração não se usam objetos falsos ou fictícios. Em vez disso, você inclui a infraestrutura, como acesso ao banco de dados ou invocação de serviço de outros serviços.

Como os testes de integração exercitam segmentos de código maiores que os testes de unidade e contam com elementos de infraestrutura, eles tendem a ter ordens de magnitude mais lentas que os testes de unidade. Portanto, é uma boa ideia limitar a quantidade de testes de integração gravados e executados.

O ASP.NET Core inclui um host Web de teste interno que pode ser usado para lidar com solicitações HTTP sem sobrecarga de rede, o que significa que você pode executar esses testes mais rápido do que ao usar um host Web real. O host Web de testes (TestServer) está disponível em um componente NuGet como Microsoft.AspNetCore.TestHost. Ele pode ser adicionado a projetos de teste de integração e usados para hospedar aplicativos do ASP.NET Core.

Conforme mostrado no código a seguir, ao criar testes de integração para controladores do ASP.NET Core, você cria instâncias para os controladores por meio do host de teste. Isso é equivalente a uma solicitação HTTP, mas ele é executado mais rapidamente.

```csharp
public class PrimeWebDefaultRequestShould
{
    private readonly TestServer _server;
    private readonly HttpClient _client;

    public PrimeWebDefaultRequestShould()
    {
        // Arrange
        _server = new TestServer(new WebHostBuilder()
           .UseStartup<Startup>());
           _client = _server.CreateClient();
    }

    [Fact]
    public async Task ReturnHelloWorld()
    {
        // Act
        var response = await _client.GetAsync("/");
        response.EnsureSuccessStatusCode();
        var responseString = await response.Content.ReadAsStringAsync();
        // Assert
        Assert.Equal("Hello World!", responseString);
    }
}
```

#### <a name="additional-resources"></a>Recursos adicionais

- **Steve Smith. Testando controladores** (ASP.NET Core) \
    [https://docs.microsoft.com/aspnet/core/mvc/controllers/testing](/aspnet/core/mvc/controllers/testing)

- **Steve Smith. Testes de integração** (ASP.NET Core) \
    [https://docs.microsoft.com/aspnet/core/test/integration-tests](/aspnet/core/test/integration-tests)

- **Teste de unidade no .NET Core usando o teste dotnet** \
    [https://docs.microsoft.com/dotnet/core/testing/unit-testing-with-dotnet-test](../../../core/testing/unit-testing-with-dotnet-test.md)

- **xUnit.net**. Site oficial. \
    <https://xunit.github.io/>

- **Noções básicas do teste de unidade.** \
    [https://docs.microsoft.com/visualstudio/test/unit-test-basics](/visualstudio/test/unit-test-basics)

- **Moq**. Repositório do GitHub. \
    <https://github.com/moq/moq>

- **NUnit**. Site oficial. \
    <https://www.nunit.org/>

### <a name="implementing-service-tests-on-a-multi-container-application"></a>Implementar testes de serviço em um aplicativo com vários contêineres

Conforme mencionado anteriormente, ao testar aplicativos com vários contêineres, todos os microsserviços precisam ser executados no host do Docker ou no cluster do contêiner. Testes de serviço de ponta a ponta que incluem várias operações envolvendo diversos microsserviços exigem a implantação e inicialização do aplicativo inteiro no host do Docker por meio da execução do docker-compose up (ou um mecanismo semelhante se você estiver usando um orquestrador). Quando o aplicativo inteiro e todos os seus serviços estiverem em execução, você pode executar testes funcionais e de integração de ponta a ponta.

Existem algumas abordagens que você pode usar. No arquivo docker-compose.yml usado para implantar o aplicativo, é possível expandir o ponto de entrada no nível da solução para usar o [dotnet test](../../../core/tools/dotnet-test.md). Também é possível usar outro arquivo do Compose para executar testes na imagem de destino. Ao utilizar outro arquivo do Compose para testes de integração que inclui microsserviços e bancos de dados em contêineres, você garante que dados relacionados sempre sejam redefinidos para seu estado original antes de executar os testes.

Se o Visual Studio estiver em execução, será possível aproveitar pontos de interrupção e exceções após o aplicativo do Compose entrar em funcionamento. Outra opção é executar os testes de integração automaticamente no pipeline de CI no Azure DevOps Services ou em qualquer outro sistema CI/CD compatível com contêineres do Docker.

## <a name="testing-in-eshoponcontainers"></a>Testando em eShopOnContainers

Os testes do aplicativo (eShopOnContainers) de referência foram recentemente restruturados e agora há quatro categorias:

1. **Unidade** testa, os antigos testes de unidade regulares, contidos nos projetos **{MicroserviceName}.UnitTests**

2. **Testes funcionais/integração de microsserviço**, com casos de teste que envolvem a infraestrutura para cada microsserviço, mas isolado dos outros; estão contidos nos projetos **{MicroserviceName}.FunctionalTests**.

3. **Testes funcionais/de integração do aplicativo**, que se concentram na integração de microserviços, com casos de teste que exercem vários microserviços. Esses testes estão localizados no projeto **Application.FunctionalTests**.

Os testes de unidade e de integração por microsserviço estão contidos em uma pasta de teste em cada microsserviço e o testes de carga e de aplicativo estão contidos na pasta teste da pasta da solução, conforme mostrado na Figura 6-25.

![Captura de tela do VS apontando alguns dos projetos de teste na solução.](./media/test-aspnet-core-services-web-apps/eshoponcontainers-test-folder-structure.png)

**Figura 6-25**. Estrutura da pastas de teste em eShopOnContainers

Testes funcionais/integração de aplicativos e microsserviços são executados no Visual Studio, usando o executor de testes regular, mas primeiro você precisa iniciar os serviços de infraestrutura necessários, por meio de um conjunto de arquivos docker-compose contidos na pasta de teste da solução:

**docker-compose-test.yml**

```yml
version: '3.4'

services:
  redis.data:
    image: redis:alpine
  rabbitmq:
    image: rabbitmq:3-management-alpine
  sqldata:
    image: mcr.microsoft.com/mssql/server:2017-latest
  nosqldata:
    image: mongo
```

**docker-compose-test.override.yml**

```yml
version: '3.4'

services:
  redis.data:
    ports:
      - "6379:6379"
  rabbitmq:
    ports:
      - "15672:15672"
      - "5672:5672"
  sqldata:
    environment:
      - SA_PASSWORD=Pass@word
      - ACCEPT_EULA=Y
    ports:
      - "5433:1433"
  nosqldata:
    ports:
      - "27017:27017"
```

Portanto, para executar os testes funcionais/integração, primeiro você deve executar esse comando na pasta de teste da solução:

```console
docker-compose -f docker-compose-test.yml -f docker-compose-test.override.yml up
```

Como você pode ver, esses arquivos docker-compose só iniciam os microsserviços Redis, RabbitMQ, SQL Server e MongoDB.

### <a name="additional-resources"></a>Recursos adicionais

- **Teste de integração de & de unidade** no eShopOnContainers \
    <https://github.com/dotnet-architecture/eShopOnContainers/wiki/Unit-and-integration-testing>

- **Teste de carga** no eShopOnContainers
    <https://github.com/dotnet-architecture/eShopOnContainers/wiki/Load-testing>

> [!div class="step-by-step"]
> [Anterior](subscribe-events.md) 
>  [Avançar](background-tasks-with-ihostedservice.md)
