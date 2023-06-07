---
title: Criando um microsserviço de CRUD simples controlado por dados
description: Arquitetura de microserviços .NET para aplicativos .NET em contêineres | Entenda a criação de um microserviço CRUD (controlado por dados) simples no contexto de um aplicativo de microserviços.
ms.date: 08/14/2020
ms.openlocfilehash: 4d475ba42cb0f86b57b2467549635556cab1136d
ms.sourcegitcommit: 0100be20fcf23f61dab672deced70059ed71bb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88267952"
---
# <a name="creating-a-simple-data-driven-crud-microservice"></a>Criando um microsserviço de CRUD simples controlado por dados

Esta seção descreve como criar um microsserviço simples que execute operações de CRUD (criar, ler, atualizar e excluir) em uma fonte de dados.

## <a name="designing-a-simple-crud-microservice"></a>Criando um microsserviço de CRUD simples

Do ponto de vista do design, esse tipo de microsserviço em contêineres é muito simples. Talvez o problema a ser resolvido seja simples ou talvez a implementação seja apenas uma prova de conceito.

![Diagrama mostrando um padrão de design interno de microserviço CRUD simples.](./media/data-driven-crud-microservice/internal-design-simple-crud-microservices.png)

**Figura 6-4**. Design interno para microsserviços de CRUD simples

Um exemplo desse tipo de serviço de unidade de dados é o microsserviço de catálogo do aplicativo de exemplo eShopOnContainers. Esse tipo de serviço implementa todas as suas funcionalidades em um único projeto da API Web ASP.NET Core que inclui classes para seu modelo de dados, sua lógica de negócios e seu código de acesso a dados. Ele também armazena os dados relacionados em um banco de dados em execução no SQL Server (como outro contêiner para fins de Desenvolvimento/Teste), mas também pode ser qualquer host normal do SQL Server, conforme mostrado na Figura 6-5.

![Diagrama mostrando um contêiner de microserviço controlado por dados/CRUD.](./media/data-driven-crud-microservice/simple-data-driven-crud-microservice.png)

**Figura 6-5**. Design de microsserviço controlado por dados/CRUD simples

O diagrama anterior mostra o microserviço de catálogo lógico, que inclui seu banco de dados de catálogo, que pode estar ou não no mesmo host do Docker. Ter o banco de dados no mesmo host do Docker pode ser bom para desenvolvimento, mas não para produção. Quando estiver desenvolvendo esse tipo de serviço, você somente precisará do [ASP.NET Core](https://docs.microsoft.com/aspnet/core/) e de uma API de acesso a dados ou de um ORM (mapeador relacional de objeto) como o [Entity Framework Core](https://docs.microsoft.com/ef/core/index). Você também pode gerar metadados do [Swagger](https://swagger.io/) automaticamente por meio do [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) para fornecer uma descrição do que o serviço oferece, conforme será explicado na próxima seção.

Observe que executar um servidor de banco de dados como o SQL Server em um contêiner do Docker é ótimo para ambientes de desenvolvimento, porque todas as dependências podem funcionar sem precisar provisionar um banco de dados na nuvem ou localmente. Isso é muito conveniente ao executar testes de integração. No entanto, para ambientes de produção, executar um servidor de banco de dados em um contêiner não é recomendável, porque, geralmente, essa abordagem não oferece alta disponibilidade. Para um ambiente de produção no Azure, é recomendável usar o BD SQL do Azure ou qualquer outra tecnologia de banco de dados que possa fornecer alta disponibilidade e alta escalabilidade. Por exemplo, para uma abordagem NoSQL, você pode escolher o CosmosDB.

Por fim, editando os arquivos de metadados Dockerfile e docker-compose.yml, você pode configurar como a imagem desse contêiner será criada, ou seja, qual imagem base ele usará, além das configurações de design, como nomes internos e externos e portas TCP.

## <a name="implementing-a-simple-crud-microservice-with-aspnet-core"></a>Implementando um microsserviço de CRUD simples com o ASP.NET Core

Para implementar um microsserviço CRUD simples usando o .NET Core e o Visual Studio, comece criando um projeto simples de API Web do ASP.NET Core (em execução no .NET Core, para que ele possa ser executado em um host Linux do Docker), como é mostrado na Figura 6-6.

![Captura de tela do Visual estúdios mostrando a configuração do projeto.](./media/data-driven-crud-microservice/create-asp-net-core-web-api-project.png)

**Figura 6-6**. Criando um projeto de API Web ASP.NET Core no Visual Studio 2019

Para criar um Projeto de API Web do ASP.NET Core, primeiro selecione um Aplicativo Web do ASP.NET Core e, em seguida, selecione o tipo de API. Depois de criar o projeto, você poderá implementar os controladores de MVC como faria em qualquer outro projeto de API Web, usando a API do Entity Framework ou uma outra API. Em um novo projeto de API Web, você verá que a única dependência existente nesse microsserviço é em relação ao próprio ASP.NET Core. Internamente, dentro da dependência *Microsoft. AspNetCore. All* , ele faz referência a Entity Framework e a muitos outros pacotes NuGet do .NET Core, como mostra a Figura 6-7.

![Captura de tela do VS mostrando as dependências do NuGet de Catalog. API.](./media/data-driven-crud-microservice/simple-crud-web-api-microservice-dependencies.png)

**Figura 6-7**. Dependências em um microsserviço de API Web de CRUD simples

O projeto de API inclui referências ao pacote NuGet Microsoft. AspNetCore. app, que inclui referências a todos os pacotes essenciais. Ele pode incluir alguns outros pacotes também.

### <a name="implementing-crud-web-api-services-with-entity-framework-core"></a>Implementando serviços da API Web de CRUD com o Entity Framework Core

O Entity Framework (EF) Core é uma versão de multiplaforma leve, extensível e de plataforma cruzada da popular tecnologia de acesso a dados do Entity Framework. O EF Core é um ORM (mapeador relacional de objeto) que permite que os desenvolvedores do .NET trabalhem com um banco de dados usando objetos .NET.

O microsserviço de catálogo usa o EF e o provedor do SQL Server porque seu banco de dados está em execução em um contêiner com o SQL Server para a imagem do Docker do Linux. No entanto, o banco de dados poderia ser implantado em qualquer SQL Server, como o Windows local ou o BD SQL do Azure. A única coisa que você precisaria alterar seria a cadeia de conexão no microsserviço do ASP.NET Web API.

#### <a name="the-data-model"></a>O modelo de dados

Com o EF Core, o acesso a dados é executado usando um modelo. Um modelo é composto por classes de entidade (modelo de domínio) e um contexto derivado (DbContext) que representa uma sessão com o banco de dados, permitindo que você consulte e salve os dados. Você pode gerar um modelo a partir de um banco de dados existente, codificar manualmente um modelo para corresponder ao banco de dados ou usar a técnica de migrações do EF para criar um banco de dados do seu modelo, usando a abordagem Code-First (que facilita a evolução do banco de dados à medida que seu modelo muda ao longo do tempo). Para o microserviço de catálogo, a última abordagem foi usada. Veja um exemplo da classe de entidade CatalogItem no exemplo de código a seguir, que é uma classe de entidade [POCO](https://en.wikipedia.org/wiki/Plain_Old_CLR_Object) (objeto CRL básico) simples.

```csharp
public class CatalogItem
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Description { get; set; }
    public decimal Price { get; set; }
    public string PictureFileName { get; set; }
    public string PictureUri { get; set; }
    public int CatalogTypeId { get; set; }
    public CatalogType CatalogType { get; set; }
    public int CatalogBrandId { get; set; }
    public CatalogBrand CatalogBrand { get; set; }
    public int AvailableStock { get; set; }
    public int RestockThreshold { get; set; }
    public int MaxStockThreshold { get; set; }

    public bool OnReorder { get; set; }
    public CatalogItem() { }

    // Additional code ...
}
```

Você também precisará de um DbContext que represente uma sessão com o banco de dados. Para o microsserviço de catálogo, a classe CatalogContext é derivada da classe base DbContext, conforme é mostrado no exemplo a seguir:

```csharp
public class CatalogContext : DbContext
{
    public CatalogContext(DbContextOptions<CatalogContext> options) : base(options)
    { }
    public DbSet<CatalogItem> CatalogItems { get; set; }
    public DbSet<CatalogBrand> CatalogBrands { get; set; }
    public DbSet<CatalogType> CatalogTypes { get; set; }

    // Additional code ...
}
```

É possível ter implementações adicionais do `DbContext`. Por exemplo, no microsserviço Catalog.API de exemplo, há um segundo `DbContext` chamado `CatalogContextSeed` no qual ele populará automaticamente os dados de exemplo na primeira vez que tentar acessar o banco de dados. Esse método é útil para dados de demonstração e também para cenários de teste automatizados.

Dentro do `DbContext`, use o método `OnModelCreating` para personalizar os mapeamentos de entidade de banco de dados/objeto e outros [pontos de extensibilidade do EF](https://devblogs.microsoft.com/dotnet/implementing-seeding-custom-conventions-and-interceptors-in-ef-core-1-0/).

##### <a name="querying-data-from-web-api-controllers"></a>Consultando dados de controladores da API Web

As instâncias das classes de entidade normalmente são recuperadas do banco de dados usando LINQ (consulta integrada à linguagem), conforme é mostrado no exemplo a seguir:

```csharp
[Route("api/v1/[controller]")]
public class CatalogController : ControllerBase
{
    private readonly CatalogContext _catalogContext;
    private readonly CatalogSettings _settings;
    private readonly ICatalogIntegrationEventService _catalogIntegrationEventService;

    public CatalogController(
        CatalogContext context,
        IOptionsSnapshot<CatalogSettings> settings,
        ICatalogIntegrationEventService catalogIntegrationEventService)
    {
        _catalogContext = context ?? throw new ArgumentNullException(nameof(context));
        _catalogIntegrationEventService = catalogIntegrationEventService
            ?? throw new ArgumentNullException(nameof(catalogIntegrationEventService));

        _settings = settings.Value;
        context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;
    }

    // GET api/v1/[controller]/items[?pageSize=3&pageIndex=10]
    [HttpGet]
    [Route("items")]
    [ProducesResponseType(typeof(PaginatedItemsViewModel<CatalogItem>), (int)HttpStatusCode.OK)]
    [ProducesResponseType(typeof(IEnumerable<CatalogItem>), (int)HttpStatusCode.OK)]
    [ProducesResponseType((int)HttpStatusCode.BadRequest)]
    public async Task<IActionResult> ItemsAsync(
        [FromQuery]int pageSize = 10,
        [FromQuery]int pageIndex = 0,
        string ids = null)
    {
        if (!string.IsNullOrEmpty(ids))
        {
            var items = await GetItemsByIdsAsync(ids);

            if (!items.Any())
            {
                return BadRequest("ids value invalid. Must be comma-separated list of numbers");
            }

            return Ok(items);
        }

        var totalItems = await _catalogContext.CatalogItems
            .LongCountAsync();

        var itemsOnPage = await _catalogContext.CatalogItems
            .OrderBy(c => c.Name)
            .Skip(pageSize * pageIndex)
            .Take(pageSize)
            .ToListAsync();

        itemsOnPage = ChangeUriPlaceholder(itemsOnPage);

        var model = new PaginatedItemsViewModel<CatalogItem>(
            pageIndex, pageSize, totalItems, itemsOnPage);

        return Ok(model);
    }
    //...
}
```

##### <a name="saving-data"></a>Salvando dados

Dados são criados, excluídos e modificados no banco de dados usando as instâncias de suas classes de entidade. Você poderia adicionar um código como o exemplo embutido em código a seguir (dados fictícios, neste caso) aos controladores da API Web.

```csharp
var catalogItem = new CatalogItem() {CatalogTypeId=2, CatalogBrandId=2,
                                     Name="Roslyn T-Shirt", Price = 12};
_context.Catalog.Add(catalogItem);
_context.SaveChanges();
```

##### <a name="dependency-injection-in-aspnet-core-and-web-api-controllers"></a>Injeção de dependência nos controladores da API Web e do ASP.NET Core

Na ASP.NET Core, você pode usar a DI (injeção de dependência) pronta para uso. Não é necessário configurar um contêiner de IoC (inversão de controle) de terceiros, embora seja possível conectar o contêiner de IoC de sua preferência à infraestrutura do ASP.NET Core, caso deseje. Nesse caso, isso significa que você pode injetar diretamente o DBContext do EF necessário ou repositórios adicionais por meio do construtor do controlador.

Na `CatalogController` classe mencionada anteriormente, `CatalogContext` (que herda de `DbContext` ) tipo é injetado junto com os outros objetos necessários no `CatalogController()` Construtor.

Uma configuração importante a ser definida no projeto de API Web é o registro da classe DbContext no contêiner de IoC do serviço. Normalmente, você faz isso na `Startup` classe chamando o `services.AddDbContext<CatalogContext>()` método dentro do `ConfigureServices()` método, conforme mostrado no exemplo **simplificado** a seguir:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Additional code...
    services.AddDbContext<CatalogContext>(options =>
    {
        options.UseSqlServer(Configuration["ConnectionString"],
        sqlServerOptionsAction: sqlOptions =>
        {
            sqlOptions.MigrationsAssembly(
                typeof(Startup).GetTypeInfo().Assembly.GetName().Name);

           //Configuring Connection Resiliency:
           sqlOptions.
               EnableRetryOnFailure(maxRetryCount: 5,
               maxRetryDelay: TimeSpan.FromSeconds(30),
               errorNumbersToAdd: null);

       });

       // Changing default behavior when client evaluation occurs to throw.
       // Default in EFCore would be to log warning when client evaluation is done.
       options.ConfigureWarnings(warnings => warnings.Throw(
           RelationalEventId.QueryClientEvaluationWarning));
   });

  //...
}
```

### <a name="additional-resources"></a>Recursos adicionais

- **Consultando dados** \
  [https://docs.microsoft.com/ef/core/querying/index](/ef/core/querying/index)

- **Salvando dados** \
  [https://docs.microsoft.com/ef/core/saving/index](/ef/core/saving/index)

## <a name="the-db-connection-string-and-environment-variables-used-by-docker-containers"></a>A cadeia de conexão e as variáveis de ambiente do BD usadas pelos contêineres do Docker

É possível usar as configurações do ASP.NET Core e adicionar uma propriedade ConnectionString no arquivo settings.json, conforme é mostrado no exemplo a seguir:

```json
{
    "ConnectionString": "Server=tcp:127.0.0.1,5433;Initial Catalog=Microsoft.eShopOnContainers.Services.CatalogDb;User Id=sa;Password=Pass@word",
    "ExternalCatalogBaseUrl": "http://localhost:5101",
    "Logging": {
        "IncludeScopes": false,
        "LogLevel": {
            "Default": "Debug",
            "System": "Information",
            "Microsoft": "Information"
        }
    }
}
```

O arquivo settings.json pode ter valores padrão para a propriedade ConnectionString ou para qualquer outra propriedade. No entanto, essas propriedades serão substituídas pelos valores das variáveis de ambiente que você especificar no arquivo docker-compose.override.yml, ao usar o Docker.

Usando os arquivos docker-compose.yml ou docker-compose.override.yml, é possível inicializar essas variáveis de ambiente para que o Docker as configure como variáveis de ambiente do sistema operacional, como é mostrado no arquivo docker-compose.override.yml a seguir (a cadeia de conexão e as outras linhas são quebradas automaticamente neste exemplo, mas não são quebradas automaticamente em seu próprio arquivo).

```yml
# docker-compose.override.yml

#
catalog-api:
  environment:
    - ConnectionString=Server=sqldata;Database=Microsoft.eShopOnContainers.Services.CatalogDb;User Id=sa;Password=Pass@word
    # Additional environment variables for this service
  ports:
    - "5101:80"
```

Os arquivos docker-compose.yml no nível da solução não são apenas mais flexíveis do que arquivos de configuração no nível do projeto ou do microsserviço, mas também serão mais seguros se você substituir as variáveis de ambiente declaradas em arquivos docker-compose com valores definidos das suas ferramentas de implantação, como de tarefas de implantação do Docker do Azure DevOps Services.

Por fim, você pode obter esse valor do código usando Configuration\["ConnectionString"\], como foi mostrado no método ConfigureServices em um exemplo de código anterior.

No entanto, para ambientes de produção, é recomendável explorar outras maneiras de armazenar segredos como as cadeias de conexão. Uma excelente maneira de gerenciar segredos do aplicativo é usar [Azure Key Vault](https://azure.microsoft.com/services/key-vault/).

O Azure Key Vault ajuda a armazenar e proteger as chaves de criptografia e os segredos usados por aplicativos e serviços de nuvem. Um segredo é qualquer coisa sobre a qual você deseja manter controle estrito, como chaves de API, cadeias de conexão, senhas, etc., e o controle estrito inclui log de uso, definição de expiração, gerenciamento de acesso, *entre outros*.

O Azure Key Vault permite um nível de controle muito detalhado do uso de segredos do aplicativo sem a necessidade de permitir que qualquer pessoa os conheça. Os segredos podem até mesmo ser girados para segurança avançada, sem interromper o desenvolvimento ou as operações.

Os aplicativos precisam ser registrados no Active Directory da organização, para que possam usar o Key Vault.

Confira a *documentação Conceitos do Key Vault* para obter mais detalhes.

### <a name="implementing-versioning-in-aspnet-web-apis"></a>Implementando o controle de versão em ASP.NET Web APIs

À medida que os requisitos de negócios mudam, novas coleções de recursos podem ser adicionadas, as relações entre os recursos podem mudar e a estrutura dos dados nos recursos pode ser corrigida. Atualizar uma API Web para lidar com novos requisitos é um processo relativamente simples, mas você precisa considerar os efeitos que essas alterações causarão nos aplicativos cliente que consomem a API Web. Embora o desenvolvedor que projeta e implementa uma API da Web tenha controle total sobre essa API, o desenvolvedor não tem o mesmo grau de controle sobre os aplicativos cliente que podem ser criados por organizações de terceiros operando remotamente.

O controle de versão permite que uma API Web indique as funcionalidades e os recursos que ela expõe. Assim, um aplicativo cliente pode enviar solicitações para uma versão específica de uma funcionalidade ou de um recurso. Há várias abordagens para implementar o controle de versão:

- Controle de versão de URI

- Controle de versão de cadeia de consulta

- Controle de versão de cabeçalho

O controle de versão de cadeia de caracteres de consulta e de URI são os mais simples de implementar. O controle de versão de cabeçalho é uma boa abordagem. No entanto, o controle de versão de cabeçalho não é tão explícito e simples como o controle de versão de URI. Como o controle de versão de URL é o mais simples e o mais explícito, o aplicativo de exemplo eShopOnContainers usa esse o controle de versão de URI.

Com o controle de versão de URI, como no aplicativo de exemplo eShopOnContainers, sempre que você modificar a API Web ou alterar o esquema de recursos, você adicionará um número de versão ao URI de cada recurso. Os URIs existentes devem continuar a operar como antes, retornando os recursos que estão em conformidade com o esquema que corresponde à versão solicitada.

Como mostrado no exemplo de código a seguir, a versão pode ser definida usando o atributo Rota no controlador de API Web, o que torna a versão explícita no URI (v1, neste caso).

```csharp
[Route("api/v1/[controller]")]
public class CatalogController : ControllerBase
{
    // Implementation ...
```

Esse mecanismo de controle de versão é simples e depende do roteamento que o servidor faz da solicitação para o ponto de extremidade apropriado. No entanto, para obter um controle de versão mais sofisticado, e o melhor método ao usar o REST, você deve usar hipermídia e implementar o [HATEOAS (Hypertext as the Engine of Application State)](https://docs.microsoft.com/azure/architecture/best-practices/api-design#use-hateoas-to-enable-navigation-to-related-resources).

### <a name="additional-resources"></a>Recursos adicionais

- **Scott Hanselman. ASP.NET Core facilitar o controle de versão da API Web RESTful** \
  <https://www.hanselman.com/blog/ASPNETCoreRESTfulWebAPIVersioningMadeEasy.aspx>

- **Controle de versão de uma API Web RESTful** \
  <https://docs.microsoft.com/azure/architecture/best-practices/api-design#versioning-a-restful-web-api>

- **Roy de campo. Controle de versão, hipermídia e REST** \
  <https://www.infoq.com/articles/roy-fielding-on-versioning>

## <a name="generating-swagger-description-metadata-from-your-aspnet-core-web-api"></a>Gerando metadados de descrição do Swagger para a API Web ASP.NET Core

O [Swagger](https://swagger.io/) é uma estrutura de software livre muito usada, apoiada por um grande ecossistema de ferramentas que ajuda a projetar, compilar, documentar e consumir APIs RESTful. Ele está se tornando o padrão no domínio de metadados de descrição de APIs. Você deve incluir metadados de descrição do Swagger com qualquer tipo de microserviço, microserviços controlados por dados ou microserviços avançados controlados por domínio (conforme explicado na seção a seguir).

A essência do Swagger é a especificação do Swagger, que são metadados de descrição de API em um arquivo JSON ou YAML. A especificação cria o contrato RESTful para a API, detalhando todos os recursos e as operações em dois formatos, legível por pessoas e legível por computadores, para facilitar o desenvolvimento, a descoberta e a integração.

A especificação é a base da OAS (especificação OpenAPI) e é desenvolvida em uma comunidade aberta, transparente e colaborativa para padronizar a maneira que as interfaces RESTful são definidas.

A especificação define a estrutura de como um serviço pode ser descoberto e como seus recursos são entendidos. Para obter mais informações, incluindo um editor na Web e exemplos de especificações do Swagger de empresas como Spotify, Uber, Slack e Microsoft, consulte o site do Swagger (<https://swagger.io>).

### <a name="why-use-swagger"></a>Por que usar Swagger?

As principais razões para gerar metadados do Swagger para suas APIs são as seguintes.

**Capacidade para outros produtos consumirem e integrarem suas APIs automaticamente**. Dezenas de produtos e [ferramentas comerciais](https://swagger.io/commercial-tools/) e diversas [estruturas e bibliotecas](https://swagger.io/open-source-integrations/) são compatíveis com o Swagger. A Microsoft tem produtos e ferramentas de alto nível que podem consumir automaticamente as APIs baseadas no Swagger, como os seguintes:

- [AutoRest](https://github.com/Azure/AutoRest). É possível gerar classes de cliente do .NET automaticamente para chamar o Swagger. Essa ferramenta pode ser usada na CLI e também se integra ao Visual Studio, facilitando o uso por meio da GUI.

- [Microsoft Flow](https://flow.microsoft.com/). É possível [usar e integrar sua API](https://flow.microsoft.com/blog/integrating-custom-api/) automaticamente em um fluxo de trabalho do Microsoft Flow de alto nível, sem precisar de nenhuma habilidade de programação.

- [Microsoft PowerApps](https://powerapps.microsoft.com/). É possível consumir a API Automaticamente de [aplicativos móveis PowerApps](https://powerapps.microsoft.com/blog/register-and-use-custom-apis-in-powerapps/) criados com o [PowerApps Studio](https://powerapps.microsoft.com/build-powerapps/), sem precisar de nenhuma habilidade de programação.

- [Aplicativos Lógicos do Serviço de Aplicativo do Azure](https://docs.microsoft.com/azure/app-service-logic/app-service-logic-what-are-logic-apps). É possível [usar e integrar sua API a um Aplicativo Lógico do Serviço de Aplicativo do Azure](https://docs.microsoft.com/azure/app-service-logic/app-service-logic-custom-hosted-api) automaticamente, sem precisar de nenhuma habilidade de programação.

**Capacidade de gerar a documentação da API automaticamente**. Ao criar APIs RESTful em grande escala, como aplicativos complexos baseados em microsserviços, você precisa lidar com vários pontos de extremidade com modelos de dados diferentes usados no conteúdo de solicitação e de resposta. Ter uma documentação adequada e um gerenciador de API sólido, como o Swagger oferece, é fundamental para o sucesso da API e da adoção pelos desenvolvedores.

Os metadados do Swagger são o que o Microsoft Flow, o PowerApps e os Aplicativos Lógicos do Azure usam para entender como usar as APIs e conectar-se a elas.

Há várias opções para automatizar a geração de metadados do Swagger para aplicativos de API REST do ASP.NET Core, na forma de páginas de ajuda de API funcional, baseadas na *swagger-ui*.

Provavelmente, o melhor conhecimento é o [swashbuckle](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) que está sendo usado atualmente no [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers) e abordaremos em detalhes neste guia, mas também há a opção de usar o [NSwag](https://github.com/RSuter/NSwag), que pode gerar clientes de \# API do typescript e do c, bem como \# controladores c, de uma especificação Swagger ou openapi e até mesmo examinando o. dll que contém os controladores, usando [NSwagStudio](https://github.com/RSuter/NSwag/wiki/NSwagStudio).

### <a name="how-to-automate-api-swagger-metadata-generation-with-the-swashbuckle-nuget-package"></a>Como automatizar a geração de metadados do Swagger para a API com o pacote NuGet Swashbuckle

A geração manual de metadados do Swagger (em um arquivo JSON ou YAML) pode ser um trabalho entediante. No entanto, é possível automatizar a descoberta de serviços do ASP.NET Web API usando o [pacote NuGet Swashbuckle](https://aka.ms/swashbuckledotnetcore) para gerar dinamicamente os metadados do Swagger para a API.

O Swashbuckle gera automaticamente os metadados do Swagger para os projetos do ASP.NET Web API. Ele é compatível com projetos da API Web ASP.NET Core, do ASP.NET Web API tradicional e de outros tipos, como os microsserviços de Aplicativo de API do Azure, Aplicativo Móvel Azure, Azure Service Fabric com base no ASP.NET. Ele também é compatível com a API Web simples implantada em contêineres, como no aplicativo de referência.

O Swashbuckle combina o Gerenciador de API e o Swagger ou o [swagger-ui](https://github.com/swagger-api/swagger-ui) para fornecer uma experiência avançada de descoberta e de documentação aos consumidores da API. Além do mecanismo gerador de metadados do Swagger, o Swashbuckle também contém uma versão incorporada do swagger-ui, que ele oferecerá automaticamente quando for instalado.

Isso significa que você pode complementar a API com uma ótima interface do usuário de descoberta para ajudar os desenvolvedores a usarem a API. Isso exige uma quantidade muito pequena de código e manutenção porque ela é gerada automaticamente, permitindo que você se concentre na API. O resultado do Gerenciador de API é semelhante ao da Figura 6-8.

![Captura de tela do Gerenciador de API do Swagger exibindo a API eShopOContainers.](./media/data-driven-crud-microservice/swagger-metadata-eshoponcontainers-catalog-microservice.png)

**Figura 6-8**. Gerenciador de API do Swashbuckle baseado nos metadados do Swagger – microsserviço de catálogo do eShopOnContainers

A documentação da API de interface do usuário do Swagger gerada pelo Swashbuckle inclui todas as ações publicadas. O Gerenciador de API não é o mais importante aqui. Quando a API Web consegue se descrever nos metadados do Swagger, ela pode ser usada diretamente por meio das ferramentas baseadas no Swagger, incluindo geradores de código de classe de proxy de cliente que podem direcionar a várias plataformas. Por exemplo, conforme mencionado, o [AutoRest](https://github.com/Azure/AutoRest) gera as classes de cliente do .NET automaticamente. Mas ferramentas adicionais como [swagger-codegen](https://github.com/swagger-api/swagger-codegen) também estão disponíveis, permitindo automaticamente a geração de código das bibliotecas clientes da API, de stubs de servidor e da documentação.

Atualmente, o Swashbuckle consiste em cinco pacotes NuGet internos no metapacote de alto nível [swashbuckle. AspNetCore](https://www.nuget.org/packages/Swashbuckle.AspNetCore) para aplicativos ASP.NET Core.

Depois de instalar esses pacotes NuGet em seu projeto de API Web, você precisa configurar o Swagger na classe de inicialização, como no seguinte código **simplificado** :

```csharp
public class Startup
{
    public IConfigurationRoot Configuration { get; }
    // Other startup code...

    public void ConfigureServices(IServiceCollection services)
    {
        // Other ConfigureServices() code...

        // Add framework services.
        services.AddSwaggerGen(options =>
        {
            options.DescribeAllEnumsAsStrings();
            options.SwaggerDoc("v1", new OpenApiInfo
            {
                Title = "eShopOnContainers - Catalog HTTP API",
                Version = "v1",
                Description = "The Catalog Microservice HTTP API. This is a Data-Driven/CRUD microservice sample"
            });
        });

        // Other ConfigureServices() code...
    }

    public void Configure(IApplicationBuilder app,
        IHostingEnvironment env,
        ILoggerFactory loggerFactory)
    {
        // Other Configure() code...
        // ...
        app.UseSwagger()
            .UseSwaggerUI(c =>
            {
                c.SwaggerEndpoint("/swagger/v1/swagger.json", "My API V1");
            });
    }
}
```

Depois que isso for feito, você poderá iniciar o aplicativo e procurar o JSON do Swagger e os pontos de extremidade da interface do usuário usando URLs como estas:

```console
  http://<your-root-url>/swagger/v1/swagger.json

  http://<your-root-url>/swagger/
```

Você já viu a interface do usuário gerada, criada pelo Swashbuckle para uma URL como `http://<your-root-url>/swagger`. Na Figura 6-9, veja também como é possível testar qualquer método de API.

![Captura de tela da interface do usuário do Swagger mostrando as ferramentas de teste disponíveis.](./media/data-driven-crud-microservice/swashbuckle-ui-testing.png)

**Figura 6-9**. Interface do usuário do Swashbuckle testando o método da API de itens/catálogo

O detalhe da API de interface do usuário do Swagger apresenta uma amostra da resposta e pode ser usado para executar a API real, que é ótima para a descoberta do desenvolvedor. A Figura 6-10 mostra os metadados JSON do Swagger gerados por meio do microsserviço eShopOnContainers (que é o que as ferramentas usam em segundo plano) quando você solicita `http://<your-root-url>/swagger/v1/swagger.json` usando o [Postman](https://www.getpostman.com/).

![Captura de tela de uma interface do usuário do postmaster de exemplo mostrando metadados de JSON do Swagger.](./media/data-driven-crud-microservice/swagger-json-metadata.png)

**Figura 6-10**. Metadados JSON do Swagger

É simples assim. E como são gerados automaticamente, os metadados do Swagger aumentarão à medida que você adicionar mais funcionalidades à API.

### <a name="additional-resources"></a>Recursos adicionais

- **ASP.NET Web API páginas de ajuda usando o Swagger** \
  [https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger)

- **Introdução ao swashbuckle e ASP.NET Core** \
  [https://docs.microsoft.com/aspnet/core/tutorials/getting-started-with-swashbuckle](/aspnet/core/tutorials/getting-started-with-swashbuckle)

- **Introdução ao NSwag e ASP.NET Core** \
  [https://docs.microsoft.com/aspnet/core/tutorials/getting-started-with-nswag](/aspnet/core/tutorials/getting-started-with-nswag)

> [!div class="step-by-step"]
> [Anterior](microservice-application-design.md) 
>  [Avançar](multi-container-applications-docker-compose.md)
