---
title: Desenvolvendo aplicativos ASP.NET Core MVC
description: Projetar aplicativos Web modernos com o ASP.NET Core e o Azure | desenvolvendo aplicativos ASP.NET Core MVC
author: ardalis
ms.author: wiwagn
ms.date: 12/04/2019
ms.openlocfilehash: be674f3292238b1983064408184777d379cf52a7
ms.sourcegitcommit: 5280b2aef60a1ed99002dba44e4b9e7f6c830604
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84307001"
---
# <a name="develop-aspnet-core-mvc-apps"></a>Desenvolver aplicativos ASP.NET Core MVC

> "Não é importante acertar na primeira vez. É extremamente importante acertar na última vez."  
> _– Andrew Hunt e David Thomas_

O ASP.NET Core é uma estrutura multiplataforma de software livre para a criação de aplicativos Web modernos otimizados para a nuvem. Os aplicativos ASP.NET Core são leves e modulares, com suporte interno para a injeção de dependência, o que aumenta a capacidade de teste e a facilidade de manutenção. Combinado com o MVC, que é compatível com a criação de APIs Web modernas, além de aplicativos baseados em exibição, o ASP.NET Core é uma estrutura avançada para a criação de aplicativos Web empresariais.

## <a name="mvc-and-razor-pages"></a>MVC e Razor Pages

O ASP.NET Core MVC oferece diversos recursos úteis para a criação de APIs e aplicativos baseados na Web. O termo MVC significa "Model-View-Controller", um padrão de interface do usuário que divide a responsabilidade de responder às solicitações do usuário em várias partes. Além de seguir esse padrão, você também pode implementar recursos em seus aplicativos ASP.NET Core, como as Razor Pages. Os Razor Pages são incorporados ao ASP.NET Core MVC e usam os mesmos recursos para roteamento, associação de modelo, etc. No entanto, em vez de ter pastas e arquivos separados para controladores, exibições, etc. e usando roteamento baseado em atributo, os Razor Pages são colocados em uma única pasta ("/Pages"), roteiam-se com base em seu local relativo nessa pasta e lidam com as solicitações com manipuladores em vez de ações do controlador.

Ao criar um aplicativo ASP.NET Core, você deve ter um plano em mente para o tipo de aplicativo que deseja. No Visual Studio, você poderá escolher entre vários modelos. Os três modelos de projeto mais comuns são a API Web, o aplicativo Web e aplicativo Web (Model-View-Controller). Embora você só possa tomar essa decisão quando cria um projeto pela primeira vez, ela não é uma decisão irrevogável. O projeto de API Web usa controladores Model-View-Controller padrão. Ele apenas não tem Exibições por padrão. Da mesma forma, o modelo de Aplicativo Web padrão usa Razor Pages e, portanto, também não tem uma pasta de Exibições. Você poderá adicionar uma pasta de Exibições a esses projetos mais tarde para permitir o comportamento com base na exibição. Os projetos de API Web e Model-View-Controller não incluem uma pasta Pages por padrão, mas você poderá adicioná-la mais tarde para permitir o comportamento com base em Razor Pages. Considere esses três modelos como suportes a três tipos diferentes de interação do usuário padrão: dados (API Web), baseado em página e baseado em exibição. No entanto, você poderá combiná-los e usar um deles ou todos eles em um único projeto, se desejar.

### <a name="why-razor-pages"></a>Por que usar Razor Pages?

As Razor Pages são a abordagem padrão para novos aplicativos Web no Visual Studio. Elas oferecem uma maneira mais simples de criar recursos de aplicativo baseados em página, como formulários que não são de SPA. Com controladores e exibições, era comum que os aplicativos tivessem controladores muito grandes que funcionavam com muitas dependências e modelos de exibição diferentes e retornavam várias exibições. Isso resultou em mais complexidade e, muitas vezes, resultou em controladores que não seguiram o princípio de responsabilidade única ou princípios abertos/fechados com eficiência. As Razor Pages resolvem esse problema encapsulando a lógica do lado do servidor para uma determinada "página" lógica em um aplicativo Web com sua marcação Razor. Uma Razor Page sem nenhuma lógica do lado do servidor pode consistir simplesmente em um arquivo Razor (por exemplo, "Index.cshtml"). No entanto, a maioria das Razor Pages menos triviais têm uma classe de modelo de página associada, que, por convenção, tem o mesmo nome que o arquivo do Razor, com uma extensão ".cs" (por exemplo, "Index.cshtml.cs").

O modelo de página de uma página Razor combina as responsabilidades de um controlador MVC e um ViewModel. Em vez de manipular as solicitações com métodos de ação do controlador, são executados manipuladores de modelo de página, como "OnGet()", renderizando suas próprias páginas associadas por padrão. As Razor Pages simplificam o processo de criação de páginas individuais em um aplicativo ASP.NET Core, fornecendo ainda todos os recursos de arquiteturas ASP.NET Core MVC. Elas são uma boa opção padrão para a nova funcionalidade baseada em página.

### <a name="when-to-use-mvc"></a>Quando usar o MVC

Se você estiver criando APIs da Web, o padrão MVC faz mais sentido do que tentar usar Razor Pages. Se o seu projeto apenas expor pontos de extremidade da API Web, você deve começar a usar o modelo de projeto de API Web. Caso contrário, é fácil adicionar controladores e pontos de extremidade de API associados a qualquer aplicativo ASP.NET Core. Use a abordagem MVC baseada em exibição se estiver migrando um aplicativo existente do ASP.NET MVC 5 ou anterior para ASP.NET Core MVC e desejar fazer isso com a menor quantidade de esforço. Depois de fazer a migração inicial, você pode avaliar se faz sentido adotar Razor Pages para novos recursos ou até mesmo como uma migração de atacado.

Independentemente de você optar por criar seu aplicativo Web usando exibições Razor Pages ou MVC, seu aplicativo terá um desempenho semelhante e incluirá suporte para injeção de dependência, filtros, associação de modelo, validação e assim por diante.

## <a name="mapping-requests-to-responses"></a>Mapeando solicitações para respostas

Em sua essência, os aplicativos ASP.NET Core mapeiam as solicitações de entrada para as respostas de saída. Em um nível inferior, isso é feito com um middleware, e aplicativos ASP.NET Core e microsserviços simples podem ser compostos apenas por um middleware personalizado. Ao usar o ASP.NET Core MVC, você pode trabalhar em um nível um pouco superior, pensando em termos de _rotas_, _controladores_ e _ações_. Cada solicitação de entrada é comparada com a tabela de roteamento do aplicativo e se uma rota correspondente é encontrada, o método de ação associado (pertencente a um controlador) é chamado para manipular a solicitação. Se nenhuma rota correspondente é encontrada, um manipulador de erro (nesse caso, retornando um resultado NotFound) é chamado.

Os aplicativos ASP.NET Core MVC podem usar rotas convencionais, rotas de atributo ou ambas. As rotas convencionais são definidas no código, especificando as _convenções_ de roteamento com uma sintaxe parecida com o exemplo abaixo:

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllerRoute(name: "default", pattern: "{controller=Home}/{action=Index}/{id?}");
});
```

Nesse exemplo, uma rota chamada "default" foi adicionada à tabela de roteamento. Ele define um modelo de rota com espaços reservados para o _controlador_, a _ação_e a _ID_. Os espaços reservados de controlador e ação têm o padrão especificado ("início" e "índice", respectivamente) e o espaço reservado de ID é opcional (em virtude de um "?" aplicado a ele). A convenção definida aqui indica que a primeira parte de uma solicitação deve corresponder ao nome do controlador, a segunda parte à ação, e, em seguida, se for necessário, uma terceira parte representará um parâmetro de ID. As rotas convencionais costumam ser definidas em um lugar para o aplicativo, como no método Configure na classe Startup.

As rotas de atributo são aplicadas aos controladores e às ações diretamente, em vez de serem especificadas globalmente. Isso oferece a vantagem de torná-las muito mais detectáveis quando você estiver procurando um método específico, mas significa que as informações de roteamento não são mantidas em um único lugar no aplicativo. Com as rotas de atributo, você pode especificar com facilidade várias rotas para determinada ação, além de combinar rotas entre controladores e ações. Por exemplo:

```csharp
[Route("Home")]
public class HomeController : Controller
{
    [Route("")] // Combines to define the route template "Home"
    [Route("Index")] // Combines to define route template "Home/Index"
    [Route("/")] // Does not combine, defines the route template ""
    public IActionResult Index() {}
}
```

As rotas podem ser especificadas no [HttpGet] e em atributos semelhantes, evitando a necessidade de adicionar atributos [Route] separados. As rotas de atributo também podem usar tokens para reduzir a necessidade de repetir os nomes do controlador ou da ação, conforme mostrado abaixo:

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
    [Route("")] // Matches 'Products'
    [Route("Index")] // Matches 'Products/Index'
    public IActionResult Index() {}
}
```

Razor Pages não usa o roteamento de atributos. Você pode especificar informações de modelo de rota adicionais para uma Razor Page como parte de sua diretiva `@page`:

```csharp
@page "{id:int}"
```

No exemplo anterior, a página em questão corresponderia a uma rota com um parâmetro `id` inteiro. Por exemplo, a página *Products.cshtml* localizada na raiz da `/Pages` teria esta rota:

```csharp
"/Products/123"
```

Depois que for feita a correspondência de uma solicitação específica a uma rota, mas antes da chamada do método de ação, o ASP.NET Core MVC executará o [model binding](/aspnet/core/mvc/models/model-binding) e a [validação de modelos](/aspnet/core/mvc/models/validation) na solicitação. O model binding é responsável por converter os dados HTTP de entrada nos tipos .NET especificados como parâmetros do método de ação a ser chamado. Por exemplo, se o método de ação espera um `int id` parâmetro, a associação de modelo tentará fornecer esse parâmetro de um valor fornecido como parte da solicitação. Para fazer isso, o model binding procurará valores em um formulário publicado, valores na própria rota e valores de cadeia de caracteres de consulta. Supondo que um valor de ID seja encontrado, ele será convertido em um inteiro antes de ser passado para o método de ação.

Após a associação do modelo, mas antes da chamada do método de ação, ocorre a validação de modelos. A validação de modelos usa atributos opcionais no tipo de modelo e pode ajudar a garantir que o objeto de modelo fornecido está em conformidade com determinados requisitos de dados. Determinados valores podem ser especificados como obrigatórios ou limitados a um determinado tamanho ou intervalo numérico, etc. Se os atributos de validação forem especificados, mas o modelo não estiver de acordo com seus requisitos, a Propriedade ModelState. IsValid será false e o conjunto de regras de validação com falha estará disponível para envio ao cliente que faz a solicitação.

Se você estiver usando a validação de modelos, sempre verifique se o modelo é válido antes de executar comandos de alteração do estado, para garantir que o aplicativo não seja corrompido por dados inválidos. Você pode usar um [filtro](/aspnet/core/mvc/controllers/filters) para evitar a necessidade de adicionar um código para isso em cada ação. Os filtros do ASP.NET Core MVC oferecem uma maneira de interceptar grupos de solicitações, de modo que as políticas comuns e os interesses paralelos possam ser aplicados de forma direcionada. Os filtros podem ser aplicados a ações individuais, a controladores inteiros ou globalmente a um aplicativo.

Para APIs Web, o ASP.NET Core MVC é compatível com a [_negociação de conteúdo_](/aspnet/core/mvc/models/formatting), permitindo que as solicitações especifiquem como as respostas devem ser formatadas. Com base nos cabeçalhos fornecidos na solicitação, as ações que retornam dados formatarão a resposta em XML, JSON ou outro formato compatível. Esse recurso permite que a mesma API seja usada por vários clientes com diferentes requisitos de formato de dados.

Os projetos de API Web devem considerar o uso do atributo `[ApiController]`, que pode ser aplicado aos controladores individuais, a uma classe base de controlador ou ao assembly inteiro. Esse atributo adiciona a verificação de validação automática de modelos, portanto, qualquer ação com um modelo inválido retornará um BadRequest com os detalhes dos erros de validação. O atributo também requer que todas as ações tenham uma rota de atributos, em vez de uma rota convencional, e retorna informações de ProblemDetails mais detalhadas em resposta aos erros.

> ### <a name="references--mapping-requests-to-responses"></a>Referências – Mapeando solicitações para respostas
>
> - **Roteamento para ações do controlador**
 > <https://docs.microsoft.com/aspnet/core/mvc/controllers/routing>
> - **Associação de modelo**
 > <https://docs.microsoft.com/aspnet/core/mvc/models/model-binding>
> - **Validação do modelo**
 > <https://docs.microsoft.com/aspnet/core/mvc/models/validation>
> - **Filter**
 > <https://docs.microsoft.com/aspnet/core/mvc/controllers/filters>
> - **Atributo ApiController**
 > <https://docs.microsoft.com/aspnet/core/web-api/>

## <a name="working-with-dependencies"></a>Trabalhando com dependências

O ASP.NET Core tem suporte interno para uma técnica conhecida como [injeção de dependência](/aspnet/core/fundamentals/dependency-injection), além de fazer uso dela internamente. A injeção de dependência é uma técnica que permite um acoplamento flexível entre diferentes partes de um aplicativo. Um acoplamento mais flexível é desejável porque facilita o isolamento de partes do aplicativo, permitindo o teste ou a substituição. Ele também torna menos provável que uma alteração em uma parte do aplicativo tenha um impacto inesperado em outro lugar do aplicativo. A injeção de dependência baseia-se no princípio da inversão de dependência e costuma ser fundamental para alcançar o princípio do aberto/fechado. Ao avaliar como o aplicativo funciona com suas dependências, tenha cuidado com o code smell [adesão estática](https://deviq.com/static-cling/) e lembre-se do aforismo "[new é associação](https://ardalis.com/new-is-glue)".

A adesão estática ocorre quando as classes fazem chamadas a métodos estáticos ou acessam propriedades estáticas que têm efeitos colaterais ou dependências na infraestrutura. Por exemplo, se você tiver um método que chama um método estático, que, por sua vez, grava em um banco de dados, o método terá um acoplamento rígido com o banco de dados. Qualquer coisa que interrompa essa chamada de banco de dados interromperá o método. O teste desses métodos é notoriamente difícil, pois testes desse tipo exigem bibliotecas fictícias comerciais para simular as chamadas estáticas ou podem ser testados somente com um banco de dados de teste em vigor. As chamadas estáticas que não têm nenhuma dependência na infraestrutura, especialmente aquelas que estão completamente sem estado, têm a permissão de chamar e não têm nenhum impacto sobre o acoplamento ou a capacidade de teste (além do acoplamento do código com a própria chamada estática).

Muitos desenvolvedores entendem os riscos da adesão estática e do estado global, mas ainda acoplarão rigidamente o código com implementações específicas por meio da criação de instância direta. A frase "new é associação" deve ser um lembrete desse acoplamento e não uma reprovação geral do uso da palavra-chave `new`. Assim como ocorre com chamadas de método estático, novas instâncias de tipos que não têm nenhuma dependência externa normalmente não acoplam rigidamente o código com detalhes de implementação nem dificultam mais o teste. No entanto, sempre que for criada uma instância para uma classe, reserve um breve momento para considerar se faz sentido embutir essa instância específica em código em um local específico ou se é um melhor design solicitar essa instância como uma dependência.

### <a name="declare-your-dependencies"></a>Declarar as dependências

O ASP.NET Core foi criado com a ideia de que os métodos e as classes devem declarar suas dependências, solicitando-as como argumentos. Os aplicativos ASP.NET normalmente são definidos em uma classe Startup, que por si só é configurada para dar suporte à injeção de dependência em vários pontos. Se a classe Startup tem um construtor, ela pode solicitar dependências por meio do construtor, da seguinte maneira:

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        var builder = new ConfigurationBuilder()
            .SetBasePath(env.ContentRootPath)
            .AddJsonFile("appsettings.json", optional: false, reloadOnChange: true)
            .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true);
    }
}
```

A classe Startup é interessante, pois não há nenhum requisito de tipo explícito para ela. Ela não herda de uma classe base Startup especial nem implementa nenhuma interface específica. Você pode atribuir um construtor a ela, ou não, e pode especificar quantos parâmetros no construtor desejar. Quando o host da Web que você configurou para o aplicativo for iniciado, ele chamará a classe Startup que você o instruiu a usar e usará a injeção de dependência para popular todas as dependências exigidas pela classe Startup. É claro que, se você solicitar parâmetros que não estiverem configurados no contêiner de serviços usado pelo ASP.NET Core, receberá uma exceção, mas desde que você continue usando as dependências reconhecidas pelo contêiner, poderá solicitar o que desejar.

A injeção de dependência baseia-se nos aplicativos ASP.NET Core desde o início, quando a instância de Startup é criada. Ela não para na classe Startup. Você também pode solicitar dependências no método Configure:

```csharp
public void Configure(IApplicationBuilder app,
    IHostingEnvironment env,
    ILoggerFactory loggerFactory)
{

}
```

O método ConfigureServices é a exceção a esse comportamento; ele precisa ter apenas um parâmetro do tipo IServiceCollection. Ele realmente não precisa dar suporte à injeção de dependência, pois, por um lado, é responsável por adicionar objetos ao contêiner de serviços e, por outro, tem acesso a todos os serviços atualmente configurados por meio do parâmetro IServiceCollection. Portanto, você pode trabalhar com as dependências definidas na coleção de serviços do ASP.NET Core em todas as partes da classe Startup, solicitando o serviço necessário como um parâmetro ou trabalhando com a IServiceCollection em ConfigureServices.

> [!NOTE]
> Se você precisar garantir que determinados serviços estejam disponíveis para sua classe de inicialização, você pode configurá-los usando um IWebHostBuilder e seu método configuraservices dentro da chamada CreateDefaultBuilder.

A classe Startup é um modelo de como você deve estruturar outras partes do aplicativo ASP.NET Core, de Controladores, Middleware, Filtros a seus próprios Serviços. Em cada caso, você deve seguir o [Princípio das Dependências Explícitas](https://deviq.com/explicit-dependencies-principle/), solicitando as dependências em vez de criá-las diretamente e aproveitando a injeção de dependência em todo o aplicativo. Tenha cuidado com o local em que você cria instâncias de implementações diretamente e como você as cria, especialmente serviços e objetos que trabalham com a infraestrutura ou que têm efeitos colaterais. Prefira trabalhar com as abstrações definidas no núcleo do aplicativo e passadas como argumentos para referências embutidas em código a tipos de implementação específicos.

## <a name="structuring-the-application"></a>Estruturando o aplicativo

Normalmente, os aplicativos monolíticos têm um único ponto de entrada. No caso de um aplicativo Web ASP.NET Core, o ponto de entrada será o projeto Web ASP.NET Core. No entanto, isso não significa que a solução precise consistir em apenas um único projeto. É útil dividir o aplicativo em camadas diferentes para seguir a separação de interesses. Depois de dividido em camadas, é útil ir além de pastas para projetos separados, que podem ajudar a obter o melhor encapsulamento. A melhor abordagem para atingir essas metas com um aplicativo ASP.NET Core é uma variação da Arquitetura Limpa abordada no capítulo 5. Seguindo essa abordagem, a solução do aplicativo incluirá bibliotecas separadas para a interface do usuário, a infraestrutura e a ApplicationCore.

Além desses projetos, projetos de teste separados são incluídos também (o teste é abordado no capítulo 9).

O modelo de objeto do aplicativo e as interfaces devem ser colocados no projeto de ApplicationCore. Esse projeto terá o menor número possível de dependências e os outros projetos na solução o referenciarão. As entidades de negócios que precisam ser persistidas são definidas no projeto de ApplicationCore, assim como os serviços que não dependem diretamente da infraestrutura.

Os detalhes de implementação, como a persistência é executada ou como as notificações podem ser enviadas a um usuário, são mantidos no projeto de Infraestrutura. Esse projeto referenciará pacotes específicos à implementação, como o Entity Framework Core, mas não deve expor os detalhes sobre essas implementações fora do projeto. Os repositórios e serviços de infraestrutura devem implementar interfaces que são definidas no projeto de ApplicationCore e suas implementações de persistência são responsáveis por recuperar e armazenar entidades definidas no ApplicationCore.

O projeto de interface do usuário do ASP.NET Core é responsável pelas preocupações no nível da interface do usuário, mas não deve incluir detalhes da lógica de negócios ou infraestrutura. Na verdade, o ideal é que ele não tenha nem mesmo uma dependência no projeto de Infraestrutura, o que ajudará a garantir que nenhuma dependência entre os dois projetos seja introduzida acidentalmente. Isso pode ser feito usando um contêiner de DI de terceiros, como o Autofac, que permite que você defina regras de DI em classes de Módulo em cada projeto.

Outra abordagem para desacoplar o aplicativo dos detalhes de implementação é fazer com que o aplicativo chame microsserviços, talvez implantados em contêineres individuais do Docker. Isso fornece uma separação de interesses e um desacoplamento ainda maiores do que a utilização da DI entre dois projetos, mas traz uma complexidade adicional.

### <a name="feature-organization"></a>Organização do recurso

Por padrão, os aplicativos ASP.NET Core organizam sua estrutura de pastas para incluir Controladores e Exibições e, frequentemente, ViewModels. Em geral, o código do lado do cliente para dar suporte a essas estruturas do lado do servidor é armazenado separadamente na pasta wwwroot. No entanto, os aplicativos grandes podem enfrentar problemas com essa organização, pois o trabalho em um recurso específico geralmente exige o salto entre essas pastas. Isso fica cada vez mais difícil à medida que aumenta o número de arquivos e subpastas em cada pasta, resultando em uma grande quantidade de rolagem pelo Gerenciador de Soluções. Uma solução para esse problema é organizar o código do aplicativo por _recurso_, em vez de por tipo de arquivo. Esse estilo organizacional é normalmente chamado de pastas de recursos ou [fatias de recursos](https://docs.microsoft.com/archive/msdn-magazine/2016/september/asp-net-core-feature-slices-for-asp-net-core-mvc) (consulte também: [fatias verticais](https://deviq.com/vertical-slices/)).

O ASP.NET Core MVC é compatível com Áreas para essa finalidade. Usando áreas, você pode criar conjuntos separados de pastas de Controladores e Exibições (bem como os modelos associados) em cada pasta de Área. A Figura 7-1 mostra uma estrutura de pastas de exemplo, usando Áreas.

![Organização da área de exemplo](./media/image7-1.png)

**Figura 7-1**. Organização da área de exemplo

Ao usar Áreas, você precisa usar atributos para decorar os controladores com o nome da área à qual eles pertencem:

```csharp
[Area("Catalog")]
public class HomeController
{}
```

Você também precisa adicionar o suporte para área às rotas:

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllerRoute(name: "areaRoute", pattern: "{area:exists}/{controller=Home}/{action=Index}/{id?}");
    endpoints.MapControllerRoute(name: "default", pattern: "{controller=Home}/{action=Index}/{id?}");
});
```

Além do suporte interno para Áreas, você também pode usar sua própria estrutura de pastas e convenções no lugar de atributos e rotas personalizadas. Isso permite que você tenha pastas de recurso que não incluíam pastas separadas para Exibições, Controladores, etc., mantendo a hierarquia mais simples e facilitando a visualização de todos os arquivos relacionados em um único local para cada recurso.

O ASP.NET Core usa tipos de convenção internos para controlar seu comportamento. Você pode modificar ou substituir essas convenções. Por exemplo, você pode criar uma convenção que receberá automaticamente o nome de recurso para determinado controlador com base em seu namespace (que geralmente se correlaciona com a pasta na qual o controlador está localizado):

```csharp
public class FeatureConvention : IControllerModelConvention
{
    public void Apply(ControllerModel controller)
    {
        controller.Properties.Add("feature",
        GetFeatureName(controller.ControllerType));
    }

    private string GetFeatureName(TypeInfo controllerType)
    {
        string[] tokens = controllerType.FullName.Split('.');
        if (!tokens.Any(t => t == "Features")) return "";
        string featureName = tokens
        .SkipWhile(t => !t.Equals("features",
        StringComparison.CurrentCultureIgnoreCase))
        .Skip(1)
        .Take(1)
        .FirstOrDefault();
        return featureName;
    }
}
```

Em seguida, especifique essa convenção como uma opção quando você adicionar suporte para o MVC ao seu aplicativo em ConfigureServices:

```csharp
services.AddMvc(o => o.Conventions.Add(new FeatureConvention()));
```

O ASP.NET Core MVC também usa uma convenção para localizar exibições. Você pode substituí-la por uma convenção personalizada, de modo que as exibições estejam localizadas nas pastas de recurso (usando o nome de recurso fornecido pela FeatureConvention, acima). Você pode aprender mais sobre essa abordagem e baixar um exemplo funcional do artigo da MSDN Magazine, [fatias de recursos para ASP.NET Core MVC](https://docs.microsoft.com/archive/msdn-magazine/2016/september/asp-net-core-feature-slices-for-asp-net-core-mvc).

### <a name="cross-cutting-concerns"></a>Interesses paralelos

Conforme os aplicativos crescem, fica cada vez mais importante excluir interesses paralelos para eliminar a duplicação e manter a consistência. Alguns exemplos de interesses paralelos em aplicativos ASP.NET Core são autenticação, regras de validação de modelos, cache de saída e tratamento de erro, embora haja muitos outros. Os [filtros](/aspnet/core/mvc/controllers/filters) do ASP.NET Core MVC permitem executar o código antes ou depois de determinadas etapas do pipeline de processamento de solicitações. Por exemplo, um filtro pode ser executado antes e após o model binding, antes e após uma ação ou antes e após o resultado de uma ação. Você também pode usar um filtro de autorização para controlar o acesso ao restante do pipeline. A Figura 7-2 mostra como solicitar fluxos de execução por meio de filtros, caso eles estejam configurados.

![A solicitação é processada por meio de Filtros de autorização, Filtros de recurso, Model binding, Filtros de ação, Execução de ação e Conversão do resultado de ação, Filtros de exceção, Filtros de resultado e Execução de resultado. Na saída, a solicitação é processada somente por Filtros de resultado e Filtros de recurso antes de se tornar uma resposta enviada ao cliente.](./media/image7-2.png)

**Figura 7-2**. Solicite a execução por meio de filtros e do pipeline de solicitação.

Normalmente, os filtros são implementados como atributos, de modo que você possa aplicá-los aos controladores ou às ações (ou até mesmo globalmente). Quando adicionados dessa maneira, os filtros especificados no nível da ação substituem ou se baseiam nos filtros especificados no nível do controlador, que, por sua vez, substituem os filtros globais. Por exemplo, o atributo \[Route\] pode ser usado para criar rotas entre controladores e ações. Da mesma forma, a autorização pode ser configurada no nível do controlador e, em seguida, substituída por ações individuais, como mostra o seguinte exemplo:

```csharp
[Authorize]
public class AccountController : Controller

{
    [AllowAnonymous] // overrides the Authorize attribute
    public async Task<IActionResult> Login() {}
    public async Task<IActionResult> ForgotPassword() {}
}
```

O primeiro método, Login, usa o filtro AllowAnonymous (atributo) para substituir o filtro Authorize definido no nível do controlador. A ação ForgotPassword (e qualquer outra ação na classe que não tem um atributo AllowAnonymous) exigirá uma solicitação autenticada.

Os filtros podem ser usados para eliminar a duplicação na forma de tratamento de erro comum de políticas para APIs. Por exemplo, uma política de API típica é retornar uma resposta NotFound para solicitações que referenciam chaves que não existem e uma resposta BadRequest se há falha na validação de modelos. O seguinte exemplo demonstra essas duas políticas em ação:

```csharp
[HttpPut("{id}")]
public async Task<IActionResult> Put(int id, [FromBody]Author author)
{
    if ((await _authorRepository.ListAsync()).All(a => a.Id != id))
    {
        return NotFound(id);
    }
    if (!ModelState.IsValid)
    {
        return BadRequest(ModelState);
    }
    author.Id = id;
    await _authorRepository.UpdateAsync(author);
    return Ok();
}
```

Não permita que os métodos de ação fiquem desorganizados com um código condicional como este. Em vez disso, coloque as políticas em filtros que podem ser aplicados conforme necessário. Neste exemplo, a verificação de validação do modelo, que deve ocorrer sempre que um comando é enviado para a API, pode ser substituído pelo seguinte atributo:

```csharp
public class ValidateModelAttribute : ActionFilterAttribute
{
    public override void OnActionExecuting(ActionExecutingContext context)
    {
        if (!context.ModelState.IsValid)
        {
            context.Result = new BadRequestObjectResult(context.ModelState);
        }
    }
}
```

É possível adicionar o `ValidateModelAttribute` ao projeto como uma dependência do NuGet incluindo o pacote [Ardalis.ValidateModel](https://www.nuget.org/packages/Ardalis.ValidateModel). Para as APIs, você pode usar o atributo `ApiController` para impor esse comportamento sem precisar usar um filtro `ValidateModel` separado.

Da mesma forma, um filtro pode ser usado para verificar se um registro existe e retornar 404 antes que a ação seja executada, eliminando a necessidade de executar essas verificações na ação. Depois de retirar as convenções comuns e organizar sua solução para separar a lógica de negócios e o código de infraestrutura da interface do usuário, os métodos de ação do MVC devem ser extremamente dinâmicos:

```csharp
[HttpPut("{id}")]
[ValidateAuthorExists]
public async Task<IActionResult> Put(int id, [FromBody]Author author)
{
    await _authorRepository.UpdateAsync(author);
    return Ok();
}
```

Você pode ler mais sobre como implementar filtros e baixar um exemplo funcional do artigo da MSDN Magazine, [ASP.NET Core os filtros do MVC do mundo real](https://docs.microsoft.com/archive/msdn-magazine/2016/august/asp-net-core-real-world-asp-net-core-mvc-filters).

> ### <a name="references--structuring-applications"></a>Referências – estruturando aplicativos
>
> - **Áreas**  
>   <https://docs.microsoft.com/aspnet/core/mvc/controllers/areas>
> - **MSDN Magazine – fatias de recurso do ASP.NET Core MVC**  
>   <https://docs.microsoft.com/archive/msdn-magazine/2016/september/asp-net-core-feature-slices-for-asp-net-core-mvc>
> - **Filtros**  
>   <https://docs.microsoft.com/aspnet/core/mvc/controllers/filters>
> - **MSDN Magazine – filtros reais ASP.NET Core MVC**  
>   <https://docs.microsoft.com/archive/msdn-magazine/2016/august/asp-net-core-real-world-asp-net-core-mvc-filters>

## <a name="security"></a>Segurança

A proteção de aplicativos Web é um tópico extenso, com muitas considerações. Em seu nível mais básico, a segurança envolve a garantia de que você sabe de quem determinada solicitação é proveniente e, em seguida, a garantia de que essa solicitação tem acesso somente aos recursos que deveria. Autenticação é o processo de comparar as credenciais fornecidas com uma solicitação com aquelas em um armazenamento de dados confiável, para verificar se a solicitação deve ser tratada como proveniente de uma entidade conhecida. Autorização é o processo de restringir o acesso a determinados recursos com base na identidade do usuário. Uma terceira preocupação de segurança é proteger as solicitações contra interceptação por terceiros, para o qual você deve, pelo menos, [garantir que o SSL é usado pelo aplicativo](/aspnet/core/security/enforcing-ssl).

### <a name="authentication"></a>Autenticação

O ASP.NET Core Identity é um sistema de associação que pode ser usado para dar suporte à funcionalidade de logon para o aplicativo. Ele tem suporte para contas de usuário local, bem como suporte para provedores de logon externo de provedores como a conta da Microsoft, Twitter, Facebook, Google e muito mais. Além do ASP.NET Core Identity, o aplicativo pode usar a autenticação do Windows ou um provedor de identidade de terceiros, como o [Identity Server](https://github.com/IdentityServer/IdentityServer4).

O ASP.NET Core Identity é incluído em novos modelos de projeto se a opção Contas de Usuário Individuais é marcada. Esse modelo inclui suporte para registro, logon, logons externos, senhas esquecidas e funcionalidade adicional.

![Selecionar contas de usuário individuais para ter a identidade pré-configurada](./media/image7-3.png)

**Figura 7-3**. Selecione contas de usuário individuais para que a identidade seja pré-configurada.

O suporte para identidade é configurado em Startup, em ConfigureServices e Configure:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add framework services.
    services.AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));
    services.AddIdentity<ApplicationUser, IdentityRole>()
    .AddEntityFrameworkStores<ApplicationDbContext>()
    .AddDefaultTokenProviders();
    services.AddMvc();
}

public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();
    app.UseIdentity();
    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllerRoute(name: "default", pattern: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

É importante que UseIdentity apareça antes de UseMvc no método Configure. Ao configurar o Identity em ConfigureServices, você observará uma chamada a AddDefaultTokenProviders. Isso não tem nada a ver com tokens que podem ser usados para proteger comunicações da Web, mas refere-se a provedores que criam prompts que podem ser enviados aos usuários por SMS ou email para que eles confirmem sua identidade.

Saiba mais sobre como [configurar a autenticação de dois fatores](/aspnet/core/security/authentication/2fa) e [habilitar provedores de logon externo](/aspnet/core/security/authentication/social/) na documentação oficial do ASP.NET Core.

### <a name="authorization"></a>Autorização

A forma mais simples de autorização envolve a restrição do acesso a usuários anônimos. Para fazer isso, basta aplicar o atributo \[Authorize\] a determinados controladores ou ações. Se funções estiverem sendo usadas, o atributo poderá ser estendido ainda mais para restringir o acesso a usuários que pertencem a determinadas funções, conforme mostrado:

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{

}
```

Nesse caso, os usuários que pertencem as funções HRManager ou Finance (ou ambas) terão acesso ao SalaryController. Para exigir que um usuário pertença a várias funções (não apenas a uma de várias), aplique o atributo várias vezes, especificando uma função obrigatória por vez.

A especificação de determinados conjuntos de funções como cadeias de caracteres em muitos controladores e ações diferentes pode levar à repetição indesejável. Você pode configurar políticas de autorização, que encapsulam regras de autorização e, em seguida, especificar a política, em vez de funções individuais ao aplicar o atributo \[Authorize\]:

```csharp
[Authorize(Policy = "CanViewPrivateReport")]
public IActionResult ExecutiveSalaryReport()
{
    return View();
}
```

Usando políticas dessa forma, você pode separar os tipos de ações que estão sendo restringidos das funções ou regras específicas que se aplicam a eles. Posteriormente, se você criar uma nova função que precisa ter acesso a determinados recursos, basta atualizar uma política, em vez de atualizar cada lista de funções em cada atributo \[Authorize\].

#### <a name="claims"></a>Declarações

Declarações são pares de nome e valor que representam as propriedades de um usuário autenticado. Por exemplo, você pode armazenar o número de funcionário dos usuários como uma declaração. Em seguida, as declarações podem ser usadas como parte de políticas de autorização. Você pode criar uma política chamada "EmployeeOnly" que exige a existência de uma declaração chamada "EmployeeNumber", conforme mostrado neste exemplo:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    services.AddAuthorization(options =>
    {
        options.AddPolicy("EmployeeOnly", policy => policy.RequireClaim("EmployeeNumber"));
    });
}
```

Essa política pode então ser usada com o atributo \[Authorize\] para proteger qualquer controlador e/ou ação, conforme descrito acima.

#### <a name="securing-web-apis"></a>Proteger APIs Web

A maioria das APIs Web deve implementar um sistema de autenticação baseada em token. A autenticação de token é sem estado e foi projetada para ser escalonável. Em um sistema de autenticação baseada em token, o cliente deve primeiro ser autenticado no provedor de autenticação. Se ele for bem-sucedido, o cliente receberá um token, que é apenas uma cadeia de caracteres criptograficamente significativa. Em seguida, quando o cliente precisar emitir uma solicitação para uma API, ele adicionará esse token como um cabeçalho na solicitação. O servidor então validará o token encontrado no cabeçalho da solicitação antes de concluir a solicitação. A Figura 7-4 demonstra esse processo.

![TokenAuth](./media/image7-4.png)

**Figura 7-4.** Autenticação baseada em token para APIs Web.

Você pode criar seu próprio serviço de autenticação, integrá-lo ao Azure AD e ao OAuth ou implementar um serviço usando uma ferramenta de código-fonte aberto como [IdentityServer](https://github.com/IdentityServer).

#### <a name="custom-security"></a>Segurança personalizada

Tenha um cuidado especial ao "distribuir sua própria" implementação de criptografia, associação de usuário ou sistema de geração de token. Há várias alternativas comerciais e de software livre disponíveis, que certamente terão uma segurança melhor do que uma implementação personalizada.

> ### <a name="references--security"></a>Referências – Segurança
>
> - **Visão geral da documentação sobre segurança**  
>   <https://docs.microsoft.com/aspnet/core/security/>
> - **Impondo o SSL em um aplicativo ASP.NET Core**  
>   <https://docs.microsoft.com/aspnet/core/security/enforcing-ssl>
> - **Introdução ao Identity**  
>   <https://docs.microsoft.com/aspnet/core/security/authentication/identity>
> - **Introdução à autorização**  
>   <https://docs.microsoft.com/aspnet/core/security/authorization/introduction>
> - **Autenticação e autorização para aplicativos de API no Serviço de Aplicativo do Azure**  
>   <https://docs.microsoft.com/azure/app-service-api/app-service-api-authentication>
> - **Servidor de identidade**  
>   <https://github.com/IdentityServer>

## <a name="client-communication"></a>Comunicação com o cliente

Além de fornecer páginas e responder a solicitações de dados por meio de APIs Web, os aplicativos ASP.NET Core podem se comunicar diretamente com os clientes conectados. Essa comunicação de saída pode usar uma variedade de tecnologias de transporte, sendo a mais comum o WebSockets. O SignalR do ASP.NET Core é uma biblioteca que simplifica o acréscimo da funcionalidade de comunicação de servidor para cliente em tempo real aos aplicativos. O SignalR é compatível com uma variedade de tecnologias de transporte, incluindo o WebSockets, e abstrai muitos dos detalhes de implementação do desenvolvedor.

A comunicação do cliente em tempo real, seja ela por meio do WebSockets diretamente ou por outras técnicas, é útil em uma variedade de cenários de aplicativos. Veja a seguir alguns exemplos:

- Aplicativos de sala de chat ao vivo

- Monitoramento de aplicativos

- Atualizações de andamento do trabalho

- Notificações

- Aplicativos de formulários interativos

Ao desenvolver a comunicação do cliente nos aplicativos, normalmente, há dois componentes:

- Gerenciador de conexões do lado do servidor (Hub do SignalR, WebSocketManager, WebSocketHandler)

- Biblioteca do lado do cliente

Os clientes não estão limitados aos navegadores – aplicativos móveis, aplicativos de console e outros aplicativos nativos também podem se comunicar por meio do SignalR/WebSockets. O seguinte programa simples ecoa todo o conteúdo enviado a um aplicativo de chat para o console, como parte de um aplicativo de exemplo WebSocketManager:

```csharp
public class Program
{
    private static Connection _connection;
    public static void Main(string[] args)
    {
        StartConnectionAsync();
        _connection.On("receiveMessage", (arguments) =>
        {
            Console.WriteLine($"{arguments[0]} said: {arguments[1]}");
        });
        Console.ReadLine();
        StopConnectionAsync();
    }

    public static async Task StartConnectionAsync()
    {
        _connection = new Connection();
        await _connection.StartConnectionAsync("ws://localhost:65110/chat");
    }

    public static async Task StopConnectionAsync()
    {
        await _connection.StopConnectionAsync();
    }
}
```

Considere maneiras pelas quais os aplicativos se comunicam diretamente com aplicativos cliente e considere se a comunicação em tempo real melhorará a experiência do usuário do aplicativo.

> ### <a name="references--client-communication"></a>Referências – Comunicação do cliente
>
> - **ASP.NET Core SignalR**  
>   <https://github.com/dotnet/aspnetcore/tree/master/src/SignalR>
> - **WebSocket Manager**  
>   <https://github.com/radu-matei/websocket-manager>

## <a name="domain-driven-design--should-you-apply-it"></a>Design controlado por domínio: você deve aplicá-lo?

O DDD (Design Controlado por Domínio) é uma abordagem ágil para a criação de software que enfatiza o foco no _domínio de negócios_. Ele coloca uma grande ênfase na comunicação e interação com especialistas no domínio de negócios que podem relatar para os desenvolvedores como funciona o sistema do mundo real. Por exemplo, se você estiver criando um sistema que manipula mercados de ações, o especialista em domínio poderá ser um corretor de valores experiente. O DDD foi projetado para resolver problemas de negócios grandes e complexos e, geralmente, não é adequado para aplicativos menores e mais simples, pois o investimento no entendimento e na modelagem do domínio não vale a pena.

Ao criar um software seguindo uma abordagem de DDD, sua equipe (incluindo stakeholders não técnicos e colaboradores) deve desenvolver uma _linguagem ubíqua_ para o espaço do problema. Ou seja, a mesma terminologia deve ser usada para o conceito do mundo real que está sendo modelado, o software equivalente e as estruturas que podem existir para persistir o conceito (por exemplo, tabelas de banco de dados). Portanto, os conceitos descritos na linguagem ubíqua devem formar a base do _modelo de domínio_.

Seu modelo de domínio consiste em objetos que interagem entre si para representar o comportamento do sistema. Esses objetos podem se enquadrar nas seguintes categorias:

- [Entidades](https://deviq.com/entity/), que representam objetos com um thread de identidade. As entidades costumam ser armazenadas na persistência com uma chave pela qual elas podem ser recuperadas posteriormente.

- [Agregações](https://deviq.com/aggregate-pattern/), que representam grupos de objetos que devem ser persistidos como uma unidade.

- [Objetos de valor](https://deviq.com/value-object/), que representam conceitos que podem ser comparados com base na soma de seus valores de propriedade. Por exemplo, DateRange consiste em uma data de início e término.

- [Eventos de domínio](https://martinfowler.com/eaaDev/DomainEvent.html), que representam coisas que acontecem no sistema que são de interesse para outras partes do sistema.

Um modelo de domínio DDD deve encapsular um comportamento complexo dentro do modelo. As entidades, em particular, não devem ser apenas coleções de propriedades. Quando o modelo de domínio não tem um comportamento e representa apenas o estado do sistema, ele é chamado de [modelo anêmico](https://deviq.com/anemic-model/), o que é indesejável no DDD.

Além desses tipos de modelo, o DDD normalmente emprega uma variedade de padrões:

- [Repositório](https://deviq.com/repository-pattern/), para abstrair os detalhes de persistência.

- [Alocador](https://en.wikipedia.org/wiki/Factory_method_pattern), para encapsular a criação de objetos complexos.

- Eventos de domínio, para desacoplar um comportamento dependente do comportamento de gatilho.

- [Serviços](http://gorodinski.com/blog/2012/04/14/services-in-domain-driven-design-ddd/), para encapsular um comportamento complexo e/ou detalhes de implementação de infraestrutura.

- [Comando](https://en.wikipedia.org/wiki/Command_pattern), para desacoplar a emissão de comandos e executar o comando em si.

- [Especificação](https://deviq.com/specification-pattern/), para encapsular os detalhes de consulta.

O DDD também recomenda o uso da Arquitetura Limpa abordada anteriormente, permitindo o acoplamento flexível, o encapsulamento e um código que pode ser verificado com facilidade por meio de testes de unidade.

### <a name="when-should-you-apply-ddd"></a>Quando você deve aplicar o DDD

O DDD é bem adequado para aplicativos grandes com complexidade significativa (não apenas técnica). O aplicativo deve exigir o conhecimento de especialistas no domínio. Deve haver um comportamento significativo no próprio modelo de domínio, que representa as regras de negócio e as interações, além de simplesmente armazenar e recuperar o estado atual de vários registros de armazenamentos de dados.

### <a name="when-shouldnt-you-apply-ddd"></a>Quando você não deve aplicar o DDD

O DDD envolve investimentos em modelagem, arquitetura e comunicação que não podem ser garantidos para aplicativos menores ou aplicativos que são essencialmente apenas CRUD (criar/ler/atualizar/excluir). Caso você opte por abordar seu aplicativo seguindo o DDD, mas descubra que o domínio tem um modelo anêmico sem nenhum comportamento, talvez você precise repensar sua abordagem. O aplicativo pode não precisar do DDD ou você pode precisar de assistência na refatoração do aplicativo para encapsular a lógica de negócios no modelo de domínio, em vez de na interface do usuário ou no banco de dados.

Uma abordagem híbrida é usar o DDD somente para as áreas transacionais ou mais complexas do aplicativo, mas não para as partes CRUD ou somente leitura mais simples do aplicativo. Por exemplo, você não precisará das restrições de uma agregação se estiver consultando dados para exibir um relatório ou para visualizar dados de um Dashboard. É perfeitamente aceitável ter um modelo de leitura mais simples e separado para esses requisitos.

> ### <a name="references--domain-driven-design"></a>Referências – Design Controlado por Domínio
>
> - **DDD simplificado (resposta do StackOverflow)**  
>   <https://stackoverflow.com/questions/1222392/can-someone-explain-domain-driven-design-ddd-in-plain-english-please/1222488#1222488>

## <a name="deployment"></a>Implantação

Há algumas etapas envolvidas no processo de implantação do aplicativo ASP.NET Core, independentemente do local em que ele será hospedado. A primeira etapa é publicar o aplicativo, que pode ser feito usando o `dotnet publish` comando da CLI. Isso compilará o aplicativo e colocará todos os arquivos necessários para executar o aplicativo em uma pasta designada. Quando você faz a implantação por meio do Visual Studio, esta etapa é executada automaticamente para você. A pasta de publicação contém arquivos .exe e .dll para o aplicativo e suas dependências. Um aplicativo autossuficiente também incluirá uma versão do runtime do .NET. Os aplicativos ASP.NET Core também incluirão arquivos de configuração, ativos de cliente estático e exibições do MVC.

Os aplicativos ASP.NET Core são aplicativos de console que devem ser iniciados quando o servidor é inicializado e reiniciados quando há falhas no aplicativo (ou no servidor). Um gerenciador de processos pode ser usado para automatizar esse processo. Os gerenciadores de processos mais comuns para o ASP.NET Core são o Nginx e o Apache no Linux e o IIS ou o Serviço Windows no Windows.

Além de um Gerenciador de processos, ASP.NET Core aplicativos podem usar um servidor proxy reverso. Um servidor proxy reverso recebe solicitações HTTP da Internet e as encaminha para o Kestrel após algum tratamento preliminar. Os servidores proxy reverso fornecem uma camada de segurança para o aplicativo. O Kestrel também não dá suporte à hospedagem de vários aplicativos na mesma porta e, portanto, técnicas como cabeçalhos de host não podem ser usadas com ele para habilitar a hospedagem de vários aplicativos na mesma porta e endereço IP.

![Kestrel para a Internet](./media/image7-5.png)

**Figura 7-5**. ASP.NET hospedado em Kestrel atrás de um servidor proxy reverso

Outro cenário em que um proxy reverso pode ser útil é para proteger vários aplicativos usando SSL/HTTPS. Nesse caso, somente o proxy inverso precisa ter o SSL configurado. A comunicação entre o servidor proxy reverso e o Kestrel pode ocorrer por HTTP, conforme mostrado na Figura 7-6.

![ASP.NET hospedado atrás de um servidor proxy reverso protegido por HTTPS](./media/image7-6.png)

**Figura 7-6**. ASP.NET hospedado atrás de um servidor proxy reverso protegido por HTTPS

Uma abordagem cada vez mais popular é hospedar o aplicativo ASP.NET Core em um contêiner do Docker, que pode ser então hospedado localmente ou implantado no Azure para a hospedagem baseada em nuvem. O contêiner do Docker pode conter o código do aplicativo, em execução no Kestrel, e ser implantado atrás de um servidor proxy reverso, conforme mostrado acima.

Caso esteja hospedando seu aplicativo no Azure, use o Gateway de Aplicativo do Microsoft Azure como uma solução de virtualização dedicada para fornecer vários serviços. Além de atuar como um proxy reverso para aplicativos individuais, o Gateway de Aplicativo também pode oferecer os seguintes recursos:

- Balanceamento de carga HTTP

- Descarregamento SSL (SSL apenas para a Internet)

- SSL de ponta a ponta

- Roteamento de vários sites (consolide até 20 sites em um único Gateway de Aplicativo)

- Firewall do aplicativo Web

- Suporte do WebSockets

- Diagnóstico avançado

_Saiba mais sobre as opções de implantação do Azure no [capítulo 10](development-process-for-azure.md)._

> ### <a name="references--deployment"></a>Referências – Implantação
>
> - **Visão geral de hospedagem e implantação**  
>   <https://docs.microsoft.com/aspnet/core/publishing/>
> - **Quando usar o Kestrel com um proxy reverso**  
>   <https://docs.microsoft.com/aspnet/core/fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy>
> - **Hospedar aplicativos ASP.NET Core no Docker**  
>   <https://docs.microsoft.com/aspnet/core/publishing/docker>
> - **Introdução ao Gateway de Aplicativo do Azure**  
>   <https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction>

>[!div class="step-by-step"]
>[Anterior](common-client-side-web-technologies.md) 
> [Avançar](work-with-data-in-asp-net-core-apps.md)
