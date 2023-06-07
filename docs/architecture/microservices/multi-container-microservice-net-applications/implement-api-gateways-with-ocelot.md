---
title: Implementação de Gateways de API com o Ocelot
description: Saiba como implementar Gateways de API com o Ocelot e como usar o Ocelot em um ambiente baseado em contêiner.
ms.date: 03/02/2020
ms.openlocfilehash: f103c1e394a3f829489b61fd17af749798b02f70
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86864092"
---
# <a name="implement-api-gateways-with-ocelot"></a>Implementar Gateways de API com o Ocelot

> [!IMPORTANT]
> O aplicativo de desserviço de referência [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers) está usando atualmente os recursos fornecidos pelo [Envoy](https://www.envoyproxy.io/) para implementar o gateway de API em vez do [Ocelot](https://github.com/ThreeMammals/Ocelot)referenciado anterior.
> Fizemos essa escolha de design devido ao suporte interno do Envoy para o protocolo WebSocket, exigido pelas novas comunicações entre serviços gRPC implementadas no eShopOnContainers.
> No entanto, reguardamos esta seção no guia para que você possa considerar o Ocelot como um gateway de API simples, compatível e leve, adequado para cenários de nível de produção.

## <a name="architect-and-design-your-api-gateways"></a>Arquitetar e projetar seus Gateways de API

O diagrama de arquitetura a seguir mostra como os gateways de API foram implementados com Ocelot em eShopOnContainers.

![Diagrama mostrando a arquitetura eShopOnContainers.](./media/implement-api-gateways-with-ocelot/eshoponcontainers-architecture.png)

**Figura 6-28**. Arquitetura do eShopOnContainers com Gateways de API

Esse diagrama mostra como todo o aplicativo é implantado em um único host do Docker ou em um PC de desenvolvimento com "Docker for Windows" ou "Docker para Mac". No entanto, a implantação em qualquer orquestrador seria semelhante, mas qualquer contêiner no diagrama poderia ser escalado horizontalmente no orquestrador.

Além disso, os ativos de infraestrutura, como bancos de dados, cache e agentes de mensagens devem ser descarregados do orquestrador e implantados em sistemas altamente disponíveis para a infraestrutura, como o Banco de Dados SQL do Azure, o Azure Cosmos DB, o Redis do Azure, o Barramento de Serviço do Azure ou qualquer solução de cluster de HA local.

Como você também pode observar no diagrama, ter vários gateways de API permite que várias equipes de desenvolvimento sejam autônomas (neste caso, recursos de marketing versus recursos de compra) ao desenvolver e implantar seus microserviços, além de seus próprios gateways de API relacionados.

Se você tivesse um único Gateway de API monolítico, isso significaria um único ponto a ser atualizado por várias equipes de desenvolvimento, o que poderia vincular todos os microsserviços com uma única parte do aplicativo.

Avançado bastante no design, às vezes, um Gateway de API refinado também pode ser limitado a um único microsserviço de negócios, dependendo da arquitetura escolhida. Ter os limites do gateway de API ditados pelo negócio ou pelo domínio ajudará você a obter um design melhor.

Por exemplo, a granularidade refinada na camada do Gateway de API pode ser útil principalmente para aplicativos de interface do usuário compostos, mais avançados e baseados em microsserviços, porque o conceito de Gateway de API refinado é semelhante ao de serviço de composição de interface do usuário.

Apresentamos mais detalhes na seção anterior [Criando uma interface do usuário composta baseada em microsserviços](../architect-microservice-container-applications/microservice-based-composite-ui-shape-layout.md).

Como principal vantagem para muitos aplicativos de médio e grande porte, o uso de um produto de Gateway de API personalizado costuma ser uma boa abordagem, mas não como um único agregador monolítico ou um Gateway de API personalizado central exclusivo, a menos que esse Gateway de API permita várias áreas de configuração independentes para as diversas equipes de desenvolvimento que criam microsserviços autônomos.

### <a name="sample-microservicescontainers-to-reroute-through-the-api-gateways"></a>Microsserviços/contêineres de amostra a serem reencaminhados por meio dos Gateways de API

Por exemplo, o eShopOnContainers tem cerca de seis tipos de microsserviços internos que precisam ser publicados por meio dos Gateways de API, conforme mostrado na imagem a seguir.

![Captura de tela da pasta serviços mostrando suas subpastas.](./media/implement-api-gateways-with-ocelot/eshoponcontainers-microservice-folders.png)

**Figura 6-29**. Pastas do microsserviço na solução eShopOnContainers no Visual Studio

Sobre o serviço de Identidade, no design, ele fica fora do roteamento do Gateway de API porque é o único interesse paralelo no sistema, embora com o Ocelot também seja possível incluí-lo como parte das listas de reencaminhamento.

Todos esses serviços são implementados atualmente como serviços de API Web do ASP.NET Core, como você pode observar no código. Vamos nos concentrar em um dos microserviços como o código de microserviço de catálogo.

![Captura de tela de Gerenciador de Soluções mostrando o conteúdo do projeto Catalog. API.](./media/implement-api-gateways-with-ocelot/catalog-api-microservice-folders.png)

**Figura 6-30**. Microsserviço de API Web de exemplo (microsserviço Catálogo)

Você pode ver que o microsserviço Catálogo é um projeto de API Web ASP.NET Core típico com vários controladores e métodos como no código a seguir.

```csharp
[HttpGet]
[Route("items/{id:int}")]
[ProducesResponseType((int)HttpStatusCode.BadRequest)]
[ProducesResponseType((int)HttpStatusCode.NotFound)]
[ProducesResponseType(typeof(CatalogItem),(int)HttpStatusCode.OK)]
public async Task<IActionResult> GetItemById(int id)
{
    if (id <= 0)
    {
        return BadRequest();
    }
    var item = await _catalogContext.CatalogItems.
                                          SingleOrDefaultAsync(ci => ci.Id == id);
    //…

    if (item != null)
    {
        return Ok(item);
    }
    return NotFound();
}
```

A solicitação HTTP acabará executando esse tipo de código C# acessando o banco de dados de microsserviço e qualquer ação adicional necessária.

Em relação à URL do Microservice, quando os contêineres são implantados em seu PC de desenvolvimento local (host do Docker local), cada contêiner de microserviço sempre tem uma porta interna (geralmente a porta 80) especificada em seu dockerfile, como no seguinte dockerfile:

```Dockerfile
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1 AS base
WORKDIR /app
EXPOSE 80
```

A porta 80 mostrada no código é interna no host do Docker e, portanto, não pode ser acessada por aplicativos cliente.

Os aplicativos cliente poderão acessar apenas as portas externas (se houver) publicadas durante a implantação com `docker-compose`.

Essas portas externas não devem ser publicadas durante a implantação em um ambiente de produção. Esse é exatamente o motivo para usar o Gateway de API, ou seja, para evitar a comunicação direta entre os aplicativos clientes e os microsserviços.

No entanto, durante o desenvolvimento, é necessário acessar o microsserviço/contêiner diretamente e executá-lo por meio do Swagger. É por isso que, em eShopOnContainers, as portas externas ainda são especificadas mesmo quando não serão usadas pelo gateway de API ou pelos aplicativos cliente.

Aqui está um exemplo do `docker-compose.override.yml` arquivo para o microserviço de catálogo:

```yml
catalog-api:
  environment:
    - ASPNETCORE_ENVIRONMENT=Development
    - ASPNETCORE_URLS=http://0.0.0.0:80
    - ConnectionString=YOUR_VALUE
    - ... Other Environment Variables
  ports:
    - "5101:80"   # Important: In a production environment you should remove the external port (5101) kept here for microservice debugging purposes.
                  # The API Gateway redirects and access through the internal port (80).
```

Você pode ver como na configuração do docker-compose.override.yml a porta interna do contêiner de catálogo é a porta 80, mas a porta para acesso externo é a 5101. Mas essa porta não deve ser usada pelo aplicativo ao usar um gateway de API, somente para depurar, executar e testar apenas o microserviço de catálogo.

Normalmente, você não estará implantando com Docker-Compose em um ambiente de produção porque o ambiente de implantação de produção certo para microserviços é um orquestrador como kubernetes ou Service Fabric. Ao implantar nesses ambientes, você usa arquivos de configuração diferentes em que você não publicará diretamente nenhuma porta externa para os microserviços, mas, sempre usará o proxy reverso do gateway de API.

Execute o microserviço de catálogo no host do Docker local. Execute a solução eShopOnContainers completa do Visual Studio (ela executa todos os serviços nos arquivos do Docker-Compose) ou inicie o microserviço de catálogo com o comando Docker-compote a seguir no CMD ou no PowerShell posicionado na pasta em que o `docker-compose.yml` e `docker-compose.override.yml` são colocados.

```console
docker-compose run --service-ports catalog-api
```

Esse comando só executa o contêiner do serviço Catalog-API, além das dependências especificadas no Docker-Compose. yml. Nesse caso, o contêiner do SQL Server e o contêiner do RabbitMQ.

Em seguida, você pode acessar diretamente o microserviço de catálogo e ver seus métodos por meio da interface do usuário do Swagger acessando diretamente por meio dessa porta "externa", neste caso `http://localhost:5101/swagger` :

![Captura de tela da interface do usuário do Swagger mostrando a API REST do Catalog. API.](./media/implement-api-gateways-with-ocelot/test-catalog-microservice.png)

**Figura 6-31**. Testando o microsserviço Catálogo com sua interface do usuário do Swagger

Neste ponto, você pode definir um ponto de interrupção no código C# no Visual Studio, testar o microsserviço com os métodos expostos na interface do usuário do Swagger e, por fim, limpar tudo com o comando `docker-compose down`.

No entanto, a comunicação de acesso direto com o microsserviço, nesse caso, pela porta 5101 externa, é exatamente o que você deseja evitar em seu aplicativo. E você pode evitar isso definindo o nível adicional de indireção do Gateway de API (o Ocelot, neste caso). Dessa forma, o aplicativo cliente não acessará diretamente o Microservice.

## <a name="implementing-your-api-gateways-with-ocelot"></a>Implementando Gateways de API com o Ocelot

O Ocelot é basicamente um conjunto de middleware que você pode aplicar em uma ordem específica.

O Ocelot foi projetado para funcionar apenas com o ASP.NET Core. A versão mais recente do pacote é direcionada `.NETCoreApp 3.1` e, portanto, não é adequada para aplicativos .NET Framework.

Instale o Ocelot e suas dependências no projeto ASP.NET Core com o [pacote NuGet do Ocelot](https://www.nuget.org/packages/Ocelot/), por meio do Visual Studio.

```powershell
Install-Package Ocelot
```

No eShopOnContainers, sua implementação de gateway de API é um projeto Webhost simples ASP.NET Core, e o middleware da Ocelot lida com todos os recursos do gateway de API, conforme mostrado na imagem a seguir:

![Captura de tela de Gerenciador de Soluções mostrando o projeto de gateway de API do Ocelot.](./media/implement-api-gateways-with-ocelot/ocelotapigw-base-project.png)

**Figura 6-32**. Projeto base OcelotApiGw no eShopOnContainers

Basicamente, este projeto WebHost do ASP.NET Core é criado com dois arquivos simples: `Program.cs` e `Startup.cs`.

O Program.cs precisa apenas criar e configurar o BuildWebHost típico do ASP.NET Core.

```csharp
namespace OcelotApiGw
{
    public class Program
    {
        public static void Main(string[] args)
        {
            BuildWebHost(args).Run();
        }

        public static IWebHost BuildWebHost(string[] args)
        {
            var builder = WebHost.CreateDefaultBuilder(args);

            builder.ConfigureServices(s => s.AddSingleton(builder))
                    .ConfigureAppConfiguration(
                          ic => ic.AddJsonFile(Path.Combine("configuration",
                                                            "configuration.json")))
                    .UseStartup<Startup>();
            var host = builder.Build();
            return host;
        }
    }
}
```

O ponto importante para Ocelot é o arquivo `configuration.json` que você precisa fornecer para o construtor pelo método `AddJsonFile()`. Esse `configuration.json` é onde você especifica todos os reencaminhamentos do Gateway de API, o que significa os pontos de extremidade externos com portas específicas e os pontos de extremidade internos correlacionados, geralmente usando portas diferentes.

```json
{
    "ReRoutes": [],
    "GlobalConfiguration": {}
}
```

Há duas seções para a configuração. Uma matriz de redirecionamentos e um GlobalConfiguration. As rerotas são os objetos que dizem ao Ocelot como tratar uma solicitação upstream. A configuração global permite substituições de redirecionar configurações específicas. É útil se você não quiser gerenciar muitas configurações específicas de redirecionamento.

Aqui está um exemplo simplificado de [redirecionar o arquivo de configuração](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/ApiGateways/Web.Bff.Shopping/apigw/configuration.json) de um dos gateways de API do eShopOnContainers.

```json
{
  "ReRoutes": [
    {
      "DownstreamPathTemplate": "/api/{version}/{everything}",
      "DownstreamScheme": "http",
      "DownstreamHostAndPorts": [
        {
          "Host": "catalog-api",
          "Port": 80
        }
      ],
      "UpstreamPathTemplate": "/api/{version}/c/{everything}",
      "UpstreamHttpMethod": [ "POST", "PUT", "GET" ]
    },
    {
      "DownstreamPathTemplate": "/api/{version}/{everything}",
      "DownstreamScheme": "http",
      "DownstreamHostAndPorts": [
        {
          "Host": "basket-api",
          "Port": 80
        }
      ],
      "UpstreamPathTemplate": "/api/{version}/b/{everything}",
      "UpstreamHttpMethod": [ "POST", "PUT", "GET" ],
      "AuthenticationOptions": {
        "AuthenticationProviderKey": "IdentityApiKey",
        "AllowedScopes": []
      }
    }

  ],
    "GlobalConfiguration": {
      "RequestIdKey": "OcRequestId",
      "AdministrationPath": "/administration"
    }
  }
```

A funcionalidade principal de um Gateway de API Ocelot é obter solicitações HTTP de entrada e encaminhá-las para um serviço downstream, simultaneamente à outra solicitação HTTP. Ocelot descreve o roteamento de uma solicitação para outra como uma redirecionamento.

Por exemplo, vamos nos concentrar em uma das rerotas na configuration.jsacima, a configuração do microserviço basket.

```json
{
      "DownstreamPathTemplate": "/api/{version}/{everything}",
      "DownstreamScheme": "http",
      "DownstreamHostAndPorts": [
        {
          "Host": "basket-api",
          "Port": 80
        }
      ],
      "UpstreamPathTemplate": "/api/{version}/b/{everything}",
      "UpstreamHttpMethod": [ "POST", "PUT", "GET" ],
      "AuthenticationOptions": {
        "AuthenticationProviderKey": "IdentityApiKey",
        "AllowedScopes": []
      }
}
```

O DownstreamPathTemplate, o esquema e o DownstreamHostAndPorts compõem a URL interna do microsserviço ao qual essa solicitação será encaminhada.

A porta é a porta interna usada pelo serviço. Ao usar contêineres, é a porta especificada no dockerfile.

O `Host` é um nome de serviço que depende da resolução de nomes de serviço que você está usando. Quando o docker-compose é usado, os serviços de nomes são fornecidos pelo Host do Docker, que está usando os nomes de serviço fornecidos nos arquivos docker-compose. Quando é usado um orquestrador, como o Kubernetes ou o Service Fabric, esse nome deve ser resolvido pelo DNS ou pela resolução de nomes fornecida por cada orquestrador.

DownstreamHostAndPorts é uma matriz que contém o host e a porta de todos os serviços downstream para os quais você deseja encaminhar solicitações. Normalmente, ela conterá apenas uma entrada, mas, às vezes, convém balancear a carga de solicitações para os serviços downstream e, para isso, o Ocelot permite que você adicione mais de uma entrada e, em seguida, selecione um balanceador de carga. Mas se você estiver usando o Azure e algum orquestrador, provavelmente, será melhor balancear a carga com a infraestrutura de nuvem e do orquestrador.

O UpstreamPathTemplate é a URL que o Ocelot usará para identificar qual DownstreamPathTemplate será usado para uma determinada solicitação do cliente. Por fim, o UpstreamHttpMethod é usado para que Ocelot possa distinguir entre diferentes solicitações (GET, POST, PUT) para a mesma URL.

Neste ponto, você poderia usar um único Gateway de API do Ocelot (WebHost do ASP.NET Core), com um ou [vários arquivos configuration.json mesclados](https://ocelot.readthedocs.io/en/latest/features/configuration.html#merging-configuration-files), ou armazenar a [configuração em um repositório KV Consul](https://ocelot.readthedocs.io/en/latest/features/configuration.html#store-configuration-in-consul).

Mas, conforme apresentado nas seções de arquitetura e design, se você realmente deseja usar microsserviços autônomos, talvez seja melhor dividir esse único Gateway de API monolítico em vários Gateways de API e/ou BFFs (back-end para front-end). Para essa finalidade, vamos ver como implementar essa abordagem com contêineres do Docker.

### <a name="using-a-single-docker-container-image-to-run-multiple-different-api-gateway--bff-container-types"></a>Usando uma única imagem de contêiner do Docker para executar o vários Gateway de API diferentes ou tipos de BFF contêineres

No eShopOnContainers, estamos usando uma única imagem de contêiner do Docker com o gateway de API do Ocelot, mas então, em tempo de execução, criamos diferentes serviços/contêineres para cada tipo de API-Gateway/BFF fornecendo um configuration.jsdiferente no arquivo, usando um volume do Docker para acessar uma pasta de computador diferente para cada serviço.

![Diagrama de uma única imagem do Docker de gateway Ocelot para todos os gateways de API.](./media/implement-api-gateways-with-ocelot/reusing-single-ocelot-docker-image.png)

**Figura 6-33**. Reutilizando uma única imagem do Docker do Ocelot em vários tipos de Gateway de API

No eShopOnContainers, a "imagem do Docker do gateway de API Ocelot genérica" é criada com o projeto chamado ' OcelotApiGw ' e o nome da imagem "eshop/OcelotApiGw" que é especificado no arquivo Docker-Compose. yml. Em seguida, ao implantar no Docker, haverá quatro contêineres de Gateway de API criados com essa mesma imagem do Docker, conforme é mostrado na seguinte extração do arquivo docker-compose.yml.

```yml
  mobileshoppingapigw:
    image: eshop/ocelotapigw:${TAG:-latest}
    build:
      context: .
      dockerfile: src/ApiGateways/ApiGw-Base/Dockerfile

  mobilemarketingapigw:
    image: eshop/ocelotapigw:${TAG:-latest}
    build:
      context: .
      dockerfile: src/ApiGateways/ApiGw-Base/Dockerfile

  webshoppingapigw:
    image: eshop/ocelotapigw:${TAG:-latest}
    build:
      context: .
      dockerfile: src/ApiGateways/ApiGw-Base/Dockerfile

  webmarketingapigw:
    image: eshop/ocelotapigw:${TAG:-latest}
    build:
      context: .
      dockerfile: src/ApiGateways/ApiGw-Base/Dockerfile
```

Além disso, como você pode ver no arquivo docker-compose.override.yml a seguir, a única diferença entre esses contêineres de Gateway de API é o arquivo de configuração do Ocelot, que é diferente para cada contêiner de serviço e é especificado em runtime por meio de um volume do Docker.

```yml
mobileshoppingapigw:
  environment:
    - ASPNETCORE_ENVIRONMENT=Development
    - IdentityUrl=http://identity-api
  ports:
    - "5200:80"
  volumes:
    - ./src/ApiGateways/Mobile.Bff.Shopping/apigw:/app/configuration

mobilemarketingapigw:
  environment:
    - ASPNETCORE_ENVIRONMENT=Development
    - IdentityUrl=http://identity-api
  ports:
    - "5201:80"
  volumes:
    - ./src/ApiGateways/Mobile.Bff.Marketing/apigw:/app/configuration

webshoppingapigw:
  environment:
    - ASPNETCORE_ENVIRONMENT=Development
    - IdentityUrl=http://identity-api
  ports:
    - "5202:80"
  volumes:
    - ./src/ApiGateways/Web.Bff.Shopping/apigw:/app/configuration

webmarketingapigw:
  environment:
    - ASPNETCORE_ENVIRONMENT=Development
    - IdentityUrl=http://identity-api
  ports:
    - "5203:80"
  volumes:
    - ./src/ApiGateways/Web.Bff.Marketing/apigw:/app/configuration
```

Devido ao código anterior e, como é mostrado no Gerenciador do Visual Studio abaixo, o único arquivo necessário para definir cada Gateway de API de negócios/BFF é apenas um arquivo configuration.json, porque os quatro Gateways de API são baseados na mesma imagem do Docker.

![Captura de tela mostrando todos os gateways de API com configuration.jsem arquivos.](./media/implement-api-gateways-with-ocelot/ocelot-configuration-files.png)

**Figura 6-34**. O único arquivo necessário para definir cada Gateway de API/BFF com o Ocelot é um arquivo de configuração

Dividindo o Gateway de API em vários Gateways de API, diferentes equipes de desenvolvimento concentrando-se em diferentes subconjuntos de microsserviços podem gerenciar seus próprios Gateways de API usando arquivos de configuração do Ocelot independentes. Além disso, ao mesmo tempo, elas podem reutilizar a mesma imagem do Docker do Ocelot.

Agora, se você executar o eShopOnContainers com os gateways de API (incluídos por padrão no VS ao abrir a solução eShopOnContainers-ServicesAndWebApps. sln ou se estiver executando "Docker-compor"), as rotas de exemplo a seguir serão executadas.

Por exemplo, ao visitar a URL upstream `http://localhost:5202/api/v1/c/catalog/items/2/` atendida pelo Gateway de API webshoppingapigw, você obterá o mesmo resultado da URL Downstream interna `http://catalog-api/api/v1/2` dentro do host do Docker, como no navegador a seguir.

![Captura de tela de um navegador mostrando uma resposta que passa pelo gateway de API.](./media/implement-api-gateways-with-ocelot/access-microservice-through-url.png)

**Figura 6-35**. Acessando um microsserviço por meio de uma URL fornecida pelo Gateway de API

Devido a motivos de teste ou depuração, se você quisesse acessar diretamente o contêiner do Docker do catálogo (somente no ambiente de desenvolvimento) sem passar pelo gateway de API, como ' Catalog-API ' é uma resolução de DNS interna ao host do Docker (descoberta de serviço manipulada por nomes de serviço do Docker-Compose), a única maneira de acessar diretamente o contêiner é por meio da porta externa publicada no Docker-Compose. Override. yml, que é fornecida somente para testes de desenvolvimento, como `http://localhost:5101/api/v1/Catalog/items/1` no navegador a seguir.

![Captura de tela de um navegador que mostra uma resposta direta para o Catalog. API.](./media/implement-api-gateways-with-ocelot/direct-access-microservice-testing.png)

**Figura 6-36**. Acesso direto a um microsserviço para fins de teste

Mas o aplicativo está configurado para que ele acesse todos os microserviços por meio dos gateways de API, não apesar da porta direta "atalhos".

### <a name="the-gateway-aggregation-pattern-in-eshoponcontainers"></a>O padrão de agregação do Gateway em eShopOnContainers

Conforme apresentado anteriormente, uma maneira flexível de implementar a agregação de solicitações é com os serviços personalizados, por código. Você também pode implementar a agregação de solicitação com o [recurso de agregação de solicitação no Ocelot](https://ocelot.readthedocs.io/en/latest/features/requestaggregation.html#request-aggregation), mas talvez não seja tão flexível quanto necessário. Portanto, a maneira selecionada de implementar a agregação em eShopOnContainers é com um serviço de API Web explícito ASP.NET Core para cada agregador.

De acordo com essa abordagem, o diagrama de composição do Gateway de API é, na realidade, um pouco mais amplo ao considerar os serviços de agregador que não são mostrados no diagrama de arquitetura global simplificado mostrado anteriormente.

No diagrama a seguir, você também poderá ver como os serviços do agregador funcionam com seus Gateways de API relacionados.

![Diagrama da arquitetura eShopOnContainers que mostra os serviços do agregador.](./media/implement-api-gateways-with-ocelot/eshoponcontainers-architecture-aggregator-services.png)

**Figura 6-37**. Arquitetura de eShopOnContainers com serviços do agregador

Ampliando ainda mais, na área de negócios de "compras" na imagem a seguir, você pode ver que a informação entre os aplicativos cliente e os microserviços é reduzida ao usar os serviços de agregador nos gateways de API.

![Diagrama mostrando o zoom da arquitetura eShopOnContainers.](./media/implement-api-gateways-with-ocelot/zoom-in-vision-aggregator-services.png)

**Figura 6-38**. Visão ampliada do agregador de serviços

Você pode observar como quando o diagrama mostra as possíveis solicitações provenientes dos gateways de API que podem ser complexas. Apesar disso, veja como as setas em azul seriam simplificadas, da perspectiva dos aplicativos clientes, ao usar o padrão de agregador reduzindo o excesso de comunicação e a latência na comunicação, por fim, melhorando significativamente a experiência do usuário para os aplicativos remotos ( aplicativos móveis e SPA), principalmente.

No caso da área de negócios e dos microserviços de "marketing", é um caso de uso simples, portanto, não há necessidade de usar agregadores, mas também pode ser possível, se necessário.

### <a name="authentication-and-authorization-in-ocelot-api-gateways"></a>Autenticação e autorização em Gateways de API do Ocelot

Em um Gateway de API do Ocelot, você pode usar o serviço de autenticação, como um serviço de API Web ASP.NET Core, usando [IdentityServer](https://identityserver.io/) e fornecendo o token de autenticação dentro ou fora do Gateway de API.

Como o eShopOnContainers está usando vários Gateways de API com limites baseados em BFF e em áreas de negócios, o serviço de identidade/autenticação é deixado de fora dos Gateways de API, conforme está realçado em amarelo no diagrama a seguir.

![Diagrama mostrando o microserviço de identidade sob o gateway de API.](./media/implement-api-gateways-with-ocelot/eshoponcontainers-identity-service-position.png)

**Figura 6-39**. A posição do serviço de identidade em eShopOnContainers

No entanto, o Ocelot também dá suporte ao uso do microsserviço de Identidade/Autenticação dentro do limite do Gateway de API, como neste outro diagrama.

![Diagrama mostrando a autenticação em um gateway de API do Ocelot.](./media/implement-api-gateways-with-ocelot/ocelot-authentication.png)

**Figura 6-40**. Autenticação no Ocelot

Como mostra o diagrama anterior, quando o microserviço de identidade está abaixo do gateway de API (AG): 1) o AG solicita um token de autenticação do microserviço de identidade, 2) o microserviço de identidade retorna token para AG, 3-4) solicitações AG de microserviços usando o token de autenticação. Como o aplicativo eShopOnContainers dividiu o gateway de API em vários BFF (backend para front-end) e gateways de API de áreas de negócios, outra opção seria criar um gateway de API adicional para preocupações abrangentes. Essa opção seria razoável em uma arquitetura de microsserviço bem mais complexa com vários microsserviços de interesses paralelos. Como há apenas uma preocupação cruzada em eShopOnContainers, foi decidido simplesmente lidar com o serviço de segurança fora do realm do gateway de API, para simplificar a simplicidade.

Em qualquer caso, se o aplicativo estiver protegido no nível do Gateway de API, o módulo de autenticação do Gateway de API do Ocelot será acessado primeiro quando houver uma tentativa de usar qualquer microsserviço protegido. Isso redireciona a solicitação HTTP para visitar o microserviço de identidade ou autenticação para obter o token de acesso para que você possa visitar os serviços protegidos com o access_token.

É a maneira de proteger com autenticação qualquer serviço no nível do Gateway de API é definir o AuthenticationProviderKey em suas configurações relacionadas no configuration.json.

```json
    {
      "DownstreamPathTemplate": "/api/{version}/{everything}",
      "DownstreamScheme": "http",
      "DownstreamHostAndPorts": [
        {
          "Host": "basket-api",
          "Port": 80
        }
      ],
      "UpstreamPathTemplate": "/api/{version}/b/{everything}",
      "UpstreamHttpMethod": [],
      "AuthenticationOptions": {
        "AuthenticationProviderKey": "IdentityApiKey",
        "AllowedScopes": []
      }
    }
```

Quando o Ocelot for executado, ele examinará o redirecionations. AuthenticationProviderKey e verificará se há um provedor de autenticação registrado com a chave fornecida. Se não houver, o Ocelot não será iniciado. Se houver, o reencaminhamento usará esse provedor quando for executado.

Como o WebHost do Ocelot é configurado com o `authenticationProviderKey = "IdentityApiKey"`, ele exigirá a autenticação sempre que esse serviço tiver alguma solicitação sem nenhum token de autenticação.

```csharp
namespace OcelotApiGw
{
    public class Startup
    {
        private readonly IConfiguration _cfg;

        public Startup(IConfiguration configuration) => _cfg = configuration;

        public void ConfigureServices(IServiceCollection services)
        {
            var identityUrl = _cfg.GetValue<string>("IdentityUrl");
            var authenticationProviderKey = "IdentityApiKey";
                         //…
            services.AddAuthentication()
                .AddJwtBearer(authenticationProviderKey, x =>
                {
                    x.Authority = identityUrl;
                    x.RequireHttpsMetadata = false;
                    x.TokenValidationParameters = new Microsoft.IdentityModel.Tokens.TokenValidationParameters()
                    {
                        ValidAudiences = new[] { "orders", "basket", "locations", "marketing", "mobileshoppingagg", "webshoppingagg" }
                    };
                });
            //...
        }
    }
}
```

Em seguida, você também precisará definir a autorização com o atributo [Authorize] nos recursos a serem acessados, como os microsserviços, como no seguinte controlador do microsserviço Cesta.

```csharp
namespace Microsoft.eShopOnContainers.Services.Basket.API.Controllers
{
    [Route("api/v1/[controller]")]
    [Authorize]
    public class BasketController : Controller
    {
      //...
    }
}
```

O ValidAudiences, como "cesta", está correlacionado ao público definido em cada microserviço com `AddJwtBearer()` em configuraservices () da classe de inicialização, como no código a seguir.

```csharp
// prevent from mapping "sub" claim to nameidentifier.
JwtSecurityTokenHandler.DefaultInboundClaimTypeMap.Clear();

var identityUrl = Configuration.GetValue<string>("IdentityUrl");

services.AddAuthentication(options =>
{
    options.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;

}).AddJwtBearer(options =>
{
    options.Authority = identityUrl;
    options.RequireHttpsMetadata = false;
    options.Audience = "basket";
});
```

Se você tentar acessar qualquer microserviço protegido, como o microserviço da cesta com uma URL de redirecionamento com base no gateway de API como `http://localhost:5202/api/v1/b/basket/1` , você receberá um 401 não autorizado, a menos que forneça um token válido. Por outro lado, se uma URL de redirecionamento for autenticada, o Ocelot invocará qualquer esquema downstream associado a ele (a URL interna de microatendimento).

**Autorização na camada de redirecionamentos do Ocelot.**  O Ocelot dá suporte a autorização baseada em declarações avaliada após a autenticação. Defina a autorização em um nível de rota adicionando as linhas a seguir à configuração de Reencaminhamento.

```json
"RouteClaimsRequirement": {
    "UserType": "employee"
}
```

Nesse exemplo, quando o middleware de autorização for chamado, o Ocelot descobrirá se o usuário tem o tipo de declaração 'UserType' no token e se o valor dessa declaração é 'employee'. Se não estiver, o usuário não será autorizado e a resposta será 403 Proibido.

## <a name="using-kubernetes-ingress-plus-ocelot-api-gateways"></a>Usando a entrada do Kubernetes e os Gateways de API do Ocelot

Ao usar o kubernetes (como em um cluster do serviço kubernetes do Azure), você geralmente unificará todas as solicitações HTTP por meio da [camada de entrada do kubernetes](https://kubernetes.io/docs/concepts/services-networking/ingress/) com base em *Nginx*.

No kubernetes, se você não usar nenhuma abordagem de entrada, seus serviços e pods terão IPs somente roteáveis pela rede de cluster.

Mas se você usar uma abordagem de entrada, haverá uma camada intermediária entre a Internet e seus serviços (incluindo seus Gateways de API), atuando como um proxy reverso.

Como uma definição, uma entrada é uma coleção de regras que permitem que conexões de entrada acessem os serviços de cluster. Uma entrada é geralmente configurada para fornecer aos serviços URLs acessadas externamente, balancear a carga do tráfego, terminação SSL e muito mais. Os usuários solicitam a entrada postando o recurso de entrada no servidor de API.

Em eShopOnContainers, ao desenvolver localmente e usar apenas o computador de desenvolvimento como o host do Docker, você não está usando nenhuma entrada, mas apenas vários Gateways de API.

No entanto, ao direcionar um ambiente de "produção" baseado em kubernetes, eShopOnContainers está usando uma entrada na frente dos gateways de API. Dessa forma, os clientes ainda chamam a mesma URL base, mas as solicitações são encaminhadas para vários Gateways de API ou BFFs.

Os gateways de API são front-ends ou as fachadas identificando apenas os serviços, mas não os aplicativos Web que normalmente estão fora de seu escopo. Além disso, os Gateways de API podem ocultar determinados microsserviços internos.

A entrada, no entanto, está apenas redirecionando solicitações HTTP, mas não está tentando ocultar nenhum microsserviço ou aplicativo Web.

A arquitetura ideal é uma camada Nginx de entrada no Kubernetes na frente dos aplicativos Web além de vários Gateways de API/BFF do Ocelot, conforme é mostrado no diagrama a seguir.

![Um diagrama que mostra como uma camada de entrada se encaixa no ambiente AKS.](./media/implement-api-gateways-with-ocelot/eshoponcontainer-ingress-tier.png)

**Figura 6-41**. A camada de entrada em eShopOnContainers, quando implantada no Kubernetes

Uma Entrada do Kubernetes funciona como um proxy reverso para todo o tráfego para o aplicativo, incluindo os aplicativos Web, que geralmente estão fora do escopo do gateway de API. Quando você implanta o eShopOnContainers no Kubernetes, ele expõe apenas alguns serviços ou pontos de extremidade via _entrada_, ou seja, basicamente, a seguinte lista de pós-fixados nas URLs:

- `/` para o aplicativo Web cliente SPA
- `/webmvc` para o aplicativo Web cliente MVC
- `/webstatus` para o aplicativo Web cliente mostrando o status e as verificações de integridade
- `/webshoppingapigw` para os processos de negócios Web BFF e de compras
- `/webmarketingapigw` para os processos de negócios Web BFF e de marketing
- `/mobileshoppingapigw` para os processos de negócios móveis BFF e de compras
- `/mobilemarketingapigw` para os processos de negócios móveis BFF e de marketing

Ao implantar no kubernetes, cada gateway de API do Ocelot está usando um arquivo "configuration.jsem" diferente para cada _Pod_ que executa os gateways de API. Esses arquivos "configuration.jss" são fornecidos pela montagem (originalmente com o script deploy.ps1) de um volume criado com base em um _mapa de configuração_ kubernetes chamado ' Ocelot '. Cada contêiner monta seu arquivo de configuração relacionado na pasta do contêiner chamada `/app/configuration` .

Nos arquivos de código-fonte do eShopOnContainers, os arquivos originais "configuration.jsem" podem ser encontrados na `k8s/ocelot/` pasta. Há um arquivo para cada BFF/APIGateway.

## <a name="additional-cross-cutting-features-in-an-ocelot-api-gateway"></a>Recursos de interesses paralelos adicionais em um Gateway de API do Ocelot

Há outros recursos importantes que podem ser pesquisados e usados ao usar um Gateway de API do Ocelot, descritos nos links a seguir.

- **Descoberta de serviço no lado do cliente, integrando Ocelot com Consul ou Eureka** \
  <https://ocelot.readthedocs.io/en/latest/features/servicediscovery.html>

- **Caching na camada de gateway de API** \
  <https://ocelot.readthedocs.io/en/latest/features/caching.html>

- **Registro em log na camada de gateway de API** \
  <https://ocelot.readthedocs.io/en/latest/features/logging.html>

- **Qualidade de serviço (repetições e disjuntores de circuito) na camada de gateway de API** \
  <https://ocelot.readthedocs.io/en/latest/features/qualityofservice.html>

- **Limitação de taxa** \
  [https://ocelot.readthedocs.io/en/latest/features/ratelimiting.html](https://ocelot.readthedocs.io/en/latest/features/ratelimiting.html )

> [!div class="step-by-step"]
> [Anterior](background-tasks-with-ihostedservice.md) 
>  [Avançar](../microservice-ddd-cqrs-patterns/index.md)
