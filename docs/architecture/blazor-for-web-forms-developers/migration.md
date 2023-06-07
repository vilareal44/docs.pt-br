---
title: Migrar do ASP.NET Web Forms para Blazor
description: Saiba como abordar a migração de um aplicativo ASP.NET Web Forms existente para o Blazor .
author: twsouthwick
ms.author: tasou
no-loc:
- Blazor
- WebAssembly
ms.date: 09/19/2019
ms.openlocfilehash: ba6dbfdf9a4fa9973dfe84cf5d58f1300f5d0cb4
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88557536"
---
# <a name="migrate-from-aspnet-web-forms-to-no-locblazor"></a>Migrar do ASP.NET Web Forms para Blazor

Migrar uma base de código do ASP.NET Web Forms para Blazor é uma tarefa demorada que requer planejamento. Este capítulo descreve o processo. Algo que pode facilitar a transição é garantir que o aplicativo obedeça a uma arquitetura de *N camadas* , na qual o modelo de aplicativo (nesse caso, Web Forms) é separado da lógica de negócios. Essa separação lógica de camadas torna claro o que precisa ser movido para o .NET Core e Blazor .

Para este exemplo, o aplicativo eShop disponível no [GitHub](https://github.com/dotnet-architecture/eShopOnBlazor) é usado. eShop é um serviço de catálogo que fornece recursos CRUD por meio de entrada e validação de formulário.

Por que um aplicativo de trabalho deve ser migrado para Blazor ? Muitas vezes, não há necessidade. O ASP.NET Web Forms continuará a ter suporte por muitos anos. No entanto, muitos dos recursos que o Blazor fornece têm suporte apenas em um aplicativo migrado. Esses recursos incluem:

- Melhorias de desempenho na estrutura, como `Span<T>`
- Capacidade de executar como WebAssembly
- Suporte de plataforma cruzada para Linux e macOS
- Implantação de aplicativo-local ou implantação de estrutura compartilhada sem afetar outros aplicativos

Se esses ou outros novos recursos forem atraentes o suficiente, pode haver um valor na migração do aplicativo. A migração pode ter formas diferentes; pode ser o aplicativo inteiro ou apenas determinados pontos de extremidade que exigem as alterações. A decisão de migrar é, por fim, baseada nos problemas de negócios a serem resolvidos pelo desenvolvedor.

## <a name="server-side-versus-client-side-hosting"></a>Hospedagem do lado do servidor em comparação com o lado do cliente

Conforme descrito no capítulo [modelos de hospedagem](hosting-models.md) , um Blazor aplicativo pode ser hospedado de duas maneiras diferentes: lado do servidor e do cliente. O modelo do lado do servidor usa ASP.NET Core conexões de sinalização para gerenciar as atualizações do DOM durante a execução de qualquer código real no servidor. O modelo do lado do cliente é executado como WebAssembly em um navegador e não requer conexões de servidor. Há várias diferenças que podem afetar o que é melhor para um aplicativo específico:

- Em execução como WebAssembly ainda está em desenvolvimento e pode não dar suporte a todos os recursos (como Threading) no momento atual
- A comunicação informal entre o cliente e o servidor pode causar problemas de latência no modo do servidor
- O acesso a bancos de dados e serviços internos ou protegidos requer um serviço separado com hospedagem do lado do cliente

No momento da elaboração do artigo, o modelo do lado do servidor se assemelha mais à Web Forms. A maior parte deste capítulo se concentra no modelo de hospedagem do lado do servidor, pois ele está pronto para produção.

## <a name="create-a-new-project"></a>Criar um novo projeto

Essa etapa inicial de migração é criar um novo projeto. Esse tipo de projeto se baseia nos projetos de estilo do SDK do .NET Core e simplifica grande parte do timbre que foi usado em formatos de projeto anteriores. Para obter mais detalhes, consulte o capítulo sobre [estrutura do projeto](project-structure.md).

Depois que o projeto tiver sido criado, instale as bibliotecas que foram usadas no projeto anterior. Em projetos de Web Forms mais antigos, você pode ter usado o arquivo de *packages.config* para listar os pacotes NuGet necessários. No novo projeto no estilo SDK, *packages.config* foi substituído por `<PackageReference>` elementos no arquivo de projeto. Um benefício para essa abordagem é que todas as dependências são instaladas de maneira transitiva. Você só lista as dependências de nível superior que se preocupam.

Muitas das dependências que você está usando estão disponíveis para o .NET Core, incluindo Entity Framework 6 e log4net. Se não houver nenhuma versão do .NET Core ou do .NET Standard disponível, a versão .NET Framework poderá ser usada com frequência. Sua quilometragem pode variar. Qualquer API usada que não esteja disponível no .NET Core causa um erro de tempo de execução. O Visual Studio o notifica sobre esses pacotes. Um ícone amarelo aparece no nó **referências** do projeto no **Gerenciador de soluções**.

No Blazor projeto eshop baseado em, você pode ver os pacotes que estão instalados. Anteriormente, o arquivo de *packages.config* listava cada pacote usado no projeto, resultando em um arquivo com quase 50 linhas de comprimento. Um trecho de *packages.config* é:

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
  ...
  <package id="Microsoft.ApplicationInsights.Agent.Intercept" version="2.4.0" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.DependencyCollector" version="2.9.1" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.PerfCounterCollector" version="2.9.1" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.Web" version="2.9.1" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.WindowsServer" version="2.9.1" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel" version="2.9.1" targetFramework="net472" />
  <package id="Microsoft.AspNet.FriendlyUrls" version="1.0.2" targetFramework="net472" />
  <package id="Microsoft.AspNet.FriendlyUrls.Core" version="1.0.2" targetFramework="net472" />
  <package id="Microsoft.AspNet.ScriptManager.MSAjax" version="5.0.0" targetFramework="net472" />
  <package id="Microsoft.AspNet.ScriptManager.WebForms" version="5.0.0" targetFramework="net472" />
  ...
  <package id="System.Memory" version="4.5.1" targetFramework="net472" />
  <package id="System.Numerics.Vectors" version="4.4.0" targetFramework="net472" />
  <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.0" targetFramework="net472" />
  <package id="System.Threading.Channels" version="4.5.0" targetFramework="net472" />
  <package id="System.Threading.Tasks.Extensions" version="4.5.1" targetFramework="net472" />
  <package id="WebGrease" version="1.6.0" targetFramework="net472" />
</packages>
```

O `<packages>` elemento inclui todas as dependências necessárias. É difícil identificar quais desses pacotes são incluídos porque você precisa deles. Alguns `<package>` elementos são listados simplesmente para atender às necessidades de dependências que você precisa.

O Blazor projeto lista as dependências que você precisa dentro de um `<ItemGroup>` elemento no arquivo de projeto:

```xml
<ItemGroup>
    <PackageReference Include="Autofac" Version="4.9.3" />
    <PackageReference Include="EntityFramework" Version="6.3.0-preview9-19423-04" />
    <PackageReference Include="log4net" Version="2.0.8" />
</ItemGroup>
```

Um pacote NuGet que simplifica a vida de Web Forms desenvolvedores é o [pacote de compatibilidade do Windows](../../core/porting/windows-compat-pack.md). Embora o .NET Core seja uma plataforma cruzada, alguns recursos estão disponíveis apenas no Windows. Os recursos específicos do Windows são disponibilizados pela instalação do Compatibility Pack. Exemplos de tais recursos incluem o registro, o WMI e os serviços de diretório. O pacote acrescenta cerca de 20.000 APIs e ativa muitos serviços com os quais você já deve estar familiarizado. O projeto eShop não requer o Compatibility Pack; Mas se seus projetos usarem recursos específicos do Windows, o pacote facilitará os esforços de migração.

## <a name="enable-startup-process"></a>Habilitar processo de inicialização

O processo de inicialização do Blazor foi alterado de Web Forms e segue uma configuração semelhante para outros serviços de ASP.NET Core. Quando hospedados no lado do servidor, Blazor os componentes são executados como parte de um aplicativo ASP.NET Core normal. Quando hospedado no navegador com WebAssembly , os Blazor componentes usam um modelo de hospedagem semelhante. A diferença é que os componentes são executados como um serviço separado de qualquer um dos processos de back-end. De qualquer forma, a inicialização é semelhante.

O arquivo *global.asax.cs* é a página de inicialização padrão para projetos Web Forms. No projeto eShop, esse arquivo configura o contêiner inversão de controle (IoC) e manipula os vários eventos de ciclo de vida do aplicativo ou da solicitação. Alguns desses eventos são tratados com middleware (como `Application_BeginRequest` ). Outros eventos exigem a substituição de serviços específicos por meio de injeção de dependência (DI).

Por exemplo, o arquivo *global.asax.cs* para eshop contém o seguinte código:

```csharp
public class Global : HttpApplication, IContainerProviderAccessor
{
    private static readonly ILog _log = LogManager.GetLogger(System.Reflection.MethodBase.GetCurrentMethod().DeclaringType);

    static IContainerProvider _containerProvider;
    IContainer container;

    public IContainerProvider ContainerProvider
    {
        get { return _containerProvider; }
    }

    protected void Application_Start(object sender, EventArgs e)
    {
        // Code that runs on app startup
        RouteConfig.RegisterRoutes(RouteTable.Routes);
        BundleConfig.RegisterBundles(BundleTable.Bundles);
        ConfigureContainer();
        ConfigDataBase();
    }

    /// <summary>
    /// Track the machine name and the start time for the session inside the current session
    /// </summary>
    protected void Session_Start(Object sender, EventArgs e)
    {
        HttpContext.Current.Session["MachineName"] = Environment.MachineName;
        HttpContext.Current.Session["SessionStartTime"] = DateTime.Now;
    }

    /// <summary>
    /// https://autofaccn.readthedocs.io/en/latest/integration/webforms.html
    /// </summary>
    private void ConfigureContainer()
    {
        var builder = new ContainerBuilder();
        var mockData = bool.Parse(ConfigurationManager.AppSettings["UseMockData"]);
        builder.RegisterModule(new ApplicationModule(mockData));
        container = builder.Build();
        _containerProvider = new ContainerProvider(container);
    }

    private void ConfigDataBase()
    {
        var mockData = bool.Parse(ConfigurationManager.AppSettings["UseMockData"]);

        if (!mockData)
        {
            Database.SetInitializer<CatalogDBContext>(container.Resolve<CatalogDBInitializer>());
        }
    }

    protected void Application_BeginRequest(object sender, EventArgs e)
    {
        //set the property to our new object
        LogicalThreadContext.Properties["activityid"] = new ActivityIdHelper();

        LogicalThreadContext.Properties["requestinfo"] = new WebRequestInfo();

        _log.Debug("Application_BeginRequest");
    }
}
```

O arquivo anterior torna-se a `Startup` classe no lado do servidor Blazor :

```csharp
public class Startup
{
    public Startup(IConfiguration configuration, IWebHostEnvironment env)
    {
        Configuration = configuration;
        Env = env;
    }

    public IConfiguration Configuration { get; }

    public IWebHostEnvironment Env { get; }

    // This method gets called by the runtime. Use this method to add services to the container.
    // For more information on how to configure your application, visit https://go.microsoft.com/fwlink/?LinkID=398940
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddRazorPages();
        services.AddServerSideBlazor();

        if (Configuration.GetValue<bool>("UseMockData"))
        {
            services.AddSingleton<ICatalogService, CatalogServiceMock>();
        }
        else
        {
            services.AddScoped<ICatalogService, CatalogService>();
            services.AddScoped<IDatabaseInitializer<CatalogDBContext>, CatalogDBInitializer>();
            services.AddSingleton<CatalogItemHiLoGenerator>();
            services.AddScoped(_ => new CatalogDBContext(Configuration.GetConnectionString("CatalogDBContext")));
        }
    }

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app, ILoggerFactory loggerFactory)
    {
        loggerFactory.AddLog4Net("log4Net.xml");

        if (Env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }
        else
        {
            app.UseExceptionHandler("/Home/Error");
        }

        // Middleware for Application_BeginRequest
        app.Use((ctx, next) =>
        {
            LogicalThreadContext.Properties["activityid"] = new ActivityIdHelper(ctx);
            LogicalThreadContext.Properties["requestinfo"] = new WebRequestInfo(ctx);
            return next();
        });

        app.UseStaticFiles();

        app.UseRouting();

        app.UseEndpoints(endpoints =>
        {
            endpoints.MapBlazorHub();
            endpoints.MapFallbackToPage("/_Host");
        });

        ConfigDataBase(app);
    }

    private void ConfigDataBase(IApplicationBuilder app)
    {
        using (var scope = app.ApplicationServices.CreateScope())
        {
            var initializer = scope.ServiceProvider.GetService<IDatabaseInitializer<CatalogDBContext>>();

            if (initializer != null)
            {
                Database.SetInitializer(initializer);
            }
        }
    }
}
```

Uma alteração significativa que você pode observar de Web Forms é a proeminência de DI. DI foi um princípio de orientação no design de ASP.NET Core. Ele dá suporte à personalização de quase todos os aspectos da estrutura de ASP.NET Core. Há até mesmo um provedor de serviços interno que pode ser usado em muitos cenários. Se for necessária mais personalização, ela poderá ser suportada pelos vários projetos da Comunidade. Por exemplo, você pode transportar seu investimento de biblioteca de DI de terceiros.

No aplicativo eShop original, há alguma configuração para o gerenciamento de sessão. Como o lado do servidor Blazor usa ASP.NET Core signalr para comunicação, o estado da sessão não é suportado, pois as conexões podem ocorrer independentemente de um contexto http. Um aplicativo que usa o estado de sessão requer a reestruturação antes da execução como um Blazor aplicativo.

Para obter mais informações sobre a inicialização do aplicativo, consulte [inicialização do aplicativo](app-startup.md).

## <a name="migrate-http-modules-and-handlers-to-middleware"></a>Migrar módulos e manipuladores HTTP para middleware

Os módulos e manipuladores HTTP são padrões comuns em Web Forms para controlar o pipeline de solicitação HTTP. Classes que implementam `IHttpModule` ou `IHttpHandler` podem ser registradas e processar solicitações de entrada. Web Forms configura módulos e manipuladores no arquivo *web.config* . O Web Forms também é muito baseado na manipulação de eventos do ciclo de vida do aplicativo. O ASP.NET Core usa o middleware em vez disso. O middleware é registrado no `Configure` método da `Startup` classe. A ordem de execução do middleware é determinada pela ordem de registro.

Na seção [habilitar processo de inicialização](#enable-startup-process) , um evento de ciclo de vida foi gerado por Web Forms como o `Application_BeginRequest` método. Esse evento não está disponível no ASP.NET Core. Uma maneira de atingir esse comportamento é implementar o middleware como visto no exemplo de arquivo *Startup.cs* . Esse middleware faz a mesma lógica e, em seguida, transfere o controle para o próximo manipulador no pipeline de middleware.

Para obter mais informações sobre como migrar módulos e manipuladores, consulte [migrar manipuladores e módulos http para ASP.NET Core middleware](/aspnet/core/migration/http-modules).

## <a name="migrate-static-files"></a>Migrar arquivos estáticos

Para servir arquivos estáticos (por exemplo, HTML, CSS, imagens e JavaScript), os arquivos devem ser expostos por middleware. Chamar o `UseStaticFiles` método permite o serviço de arquivos estáticos do caminho raiz da Web. O diretório raiz padrão da Web é *wwwroot*, mas pode ser personalizado. Conforme incluído no `Configure` método da `Startup` classe eshop:

```csharp
public void Configure(IApplicationBuilder app)
{
    ...

    app.UseStaticFiles();

    ...
}
```

O projeto eShop habilita o acesso básico a arquivos estáticos. Há muitas personalizações disponíveis para acesso a arquivos estáticos. Para obter informações sobre como habilitar arquivos padrão ou um navegador de arquivos, consulte [arquivos estáticos em ASP.NET Core](/aspnet/core/fundamentals/static-files).

## <a name="migrate-runtime-bundling-and-minification-setup"></a>Migrar a configuração de agrupamento e minificação de tempo de execução

O agrupamento e o minificação são técnicas de otimização de desempenho para reduzir o número e o tamanho das solicitações de servidor para recuperar determinados tipos de arquivo. O JavaScript e o CSS geralmente passam por alguma forma de agrupamento ou minificação antes de serem enviados ao cliente. No ASP.NET Web Forms, essas otimizações são tratadas em tempo de execução. As convenções de otimização são definidas como um arquivo *App_Start/bundleconfig.cs* . No ASP.NET Core, uma abordagem mais declarativa é adotada. Um arquivo lista os arquivos a serem reduzidosdos, juntamente com configurações específicas de minificação.

Para obter mais informações sobre agrupamento e minificação, consulte [ativos estáticos de pacote e reduzir em ASP.NET Core](/aspnet/core/client-side/bundling-and-minification).

## <a name="migrate-aspx-pages"></a>Migrar páginas ASPX

Uma página em um aplicativo Web Forms é um arquivo com a extensão *. aspx* . Uma página de Web Forms geralmente pode ser mapeada para um componente no Blazor . Um Blazor componente é criado em um arquivo com a extensão *. Razor* . Para o projeto eShop, cinco páginas são convertidas em uma página Razor.

Por exemplo, a exibição de detalhes compreende três arquivos na Web Forms projeto: *Details. aspx*, *Details.aspx.cs*e *Details.aspx.designer.cs*. Ao converter para Blazor , o code-behind e a marcação são combinados em *Details. Razor*. A compilação do Razor (equivalente ao que está nos arquivos *. designer.cs* ) é armazenada no diretório *obj* e não é, por padrão, visível no **Gerenciador de soluções**. A página Web Forms consiste na seguinte marcação:

```aspx-csharp
<%@ Page Title="Details" Language="C#" MasterPageFile="~/Site.Master" AutoEventWireup="true" CodeBehind="Details.aspx.cs" Inherits="eShopLegacyWebForms.Catalog.Details" %>

<asp:Content ID="Details" ContentPlaceHolderID="MainContent" runat="server">
    <h2 class="esh-body-title">Details</h2>

    <div class="container">
        <div class="row">
            <asp:Image runat="server" CssClass="col-md-6 esh-picture" ImageUrl='<%#"/Pics/" + product.PictureFileName%>' />
            <dl class="col-md-6 dl-horizontal">
                <dt>Name
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.Name%>' />
                </dd>

                <dt>Description
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.Description%>' />
                </dd>

                <dt>Brand
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.CatalogBrand.Brand%>' />
                </dd>

                <dt>Type
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.CatalogType.Type%>' />
                </dd>
                <dt>Price
                </dt>

                <dd>
                    <asp:Label CssClass="esh-price" runat="server" Text='<%#product.Price%>' />
                </dd>

                <dt>Picture name
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.PictureFileName%>' />
                </dd>

                <dt>Stock
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.AvailableStock%>' />
                </dd>

                <dt>Restock
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.RestockThreshold%>' />
                </dd>

                <dt>Max stock
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.MaxStockThreshold%>' />
                </dd>

            </dl>
        </div>

        <div class="form-actions no-color esh-link-list">
            <a runat="server" href='<%# GetRouteUrl("EditProductRoute", new {id =product.Id}) %>' class="esh-link-item">Edit
            </a>
            |
            <a runat="server" href="~" class="esh-link-item">Back to list
            </a>
        </div>

    </div>
</asp:Content>
```

O code-behind da marcação anterior inclui o seguinte código:

```csharp
using eShopLegacyWebForms.Models;
using eShopLegacyWebForms.Services;
using log4net;
using System;
using System.Web.UI;

namespace eShopLegacyWebForms.Catalog
{
    public partial class Details : System.Web.UI.Page
    {
        private static readonly ILog _log = LogManager.GetLogger(System.Reflection.MethodBase.GetCurrentMethod().DeclaringType);

        protected CatalogItem product;

        public ICatalogService CatalogService { get; set; }

        protected void Page_Load(object sender, EventArgs e)
        {
            var productId = Convert.ToInt32(Page.RouteData.Values["id"]);
            _log.Info($"Now loading... /Catalog/Details.aspx?id={productId}");
            product = CatalogService.FindCatalogItem(productId);

            this.DataBind();
        }
    }
}
```

Quando convertido em Blazor , a página Web Forms se traduz no seguinte código:

```razor
@page "/Catalog/Details/{id:int}"
@inject ICatalogService CatalogService
@inject ILogger<Details> Logger

<h2 class="esh-body-title">Details</h2>

<div class="container">
    <div class="row">
        <img class="col-md-6 esh-picture" src="@($"/Pics/{_item.PictureFileName}")">

        <dl class="col-md-6 dl-horizontal">
            <dt>
                Name
            </dt>

            <dd>
                @_item.Name
            </dd>

            <dt>
                Description
            </dt>

            <dd>
                @_item.Description
            </dd>

            <dt>
                Brand
            </dt>

            <dd>
                @_item.CatalogBrand.Brand
            </dd>

            <dt>
                Type
            </dt>

            <dd>
                @_item.CatalogType.Type
            </dd>
            <dt>
                Price
            </dt>

            <dd>
                @_item.Price
            </dd>

            <dt>
                Picture name
            </dt>

            <dd>
                @_item.PictureFileName
            </dd>

            <dt>
                Stock
            </dt>

            <dd>
                @_item.AvailableStock
            </dd>

            <dt>
                Restock
            </dt>

            <dd>
                @_item.RestockThreshold
            </dd>

            <dt>
                Max stock
            </dt>

            <dd>
                @_item.MaxStockThreshold
            </dd>

        </dl>
    </div>

    <div class="form-actions no-color esh-link-list">
        <a href="@($"/Catalog/Edit/{_item.Id}")" class="esh-link-item">
            Edit
        </a>
        |
        <a href="/" class="esh-link-item">
            Back to list
        </a>
    </div>

</div>

@code {
    private CatalogItem _item;

    [Parameter]
    public int Id { get; set; }

    protected override void OnInitialized()
    {
        Logger.LogInformation("Now loading... /Catalog/Details/{Id}", Id);

        _item = CatalogService.FindCatalogItem(Id);
    }
}
```

Observe que o código e a marcação estão no mesmo arquivo. Todos os serviços necessários são tornados acessíveis com o `@inject` atributo. De acordo `@page` com a diretiva, essa página pode ser acessada na `Catalog/Details/{id}` rota. O valor do espaço reservado da rota foi `{id}` restrito a um número inteiro. Conforme descrito na seção de [Roteamento](pages-routing-layouts.md) , ao contrário de Web Forms, um componente Razor declara explicitamente sua rota e quaisquer parâmetros que estejam incluídos. Muitos controles de Web Forms podem não ter as contrapartes exatas no Blazor . Geralmente, há um trecho HTML equivalente que terá a mesma finalidade. Por exemplo, o `<asp:Label />` controle pode ser substituído por um `<label>` elemento HTML.

### <a name="model-validation-in-no-locblazor"></a>Validação de modelo em Blazor

Se seu código de Web Forms inclui validação, você pode transferir grande parte do que tem com pouca ou nenhuma alteração. Um benefício para executar Blazor o no é que a mesma lógica de validação pode ser executada sem a necessidade de JavaScript personalizado. As anotações de dados permitem uma validação fácil de modelo.

Por exemplo, a página *Create. aspx* tem um formulário de entrada de dados com validação. Um trecho de código de exemplo ficaria assim:

```aspx
<div class="form-group">
    <label class="control-label col-md-2">Name</label>
    <div class="col-md-3">
        <asp:TextBox ID="Name" runat="server" CssClass="form-control"></asp:TextBox>
        <asp:RequiredFieldValidator runat="server" ControlToValidate="Name" Display="Dynamic"
            CssClass="field-validation-valid text-danger" ErrorMessage="The Name field is required." />
    </div>
</div>
```

No Blazor , a marcação equivalente é fornecida em um arquivo *Create. Razor* :

```razor
<EditForm Model="_item" OnValidSubmit="@...">
    <DataAnnotationsValidator />

    <div class="form-group">
        <label class="control-label col-md-2">Name</label>
        <div class="col-md-3">
            <InputText class="form-control" @bind-Value="_item.Name" />
            <ValidationMessage For="(() => _item.Name)" />
        </div>
    </div>

    ...
</EditForm>
```

O `EditForm` contexto inclui suporte à validação e pode ser disposto em torno da entrada. As anotações de dados são uma maneira comum de adicionar validação. Esse suporte à validação pode ser adicionado por meio do `DataAnnotationsValidator` componente. Para obter mais informações sobre esse mecanismo, consulte [ASP.NET Core Blazor Forms and Validation](/aspnet/core/blazor/forms-validation).

## <a name="migrate-configuration"></a>Migrar configuração

Em um projeto Web Forms, os dados de configuração são mais comumente armazenados no arquivo *web.config* . Os dados de configuração são acessados com o `ConfigurationManager` . Normalmente, os serviços eram necessários para analisar objetos. Com o .NET Framework 4.7.2, a capacidade de composição foi adicionada à configuração via `ConfigurationBuilders` . Esses construtores permitiam que os desenvolvedores adicionassem várias fontes de configuração que, em seguida, eram compostas em tempo de execução para recuperar os valores necessários.

ASP.NET Core introduziu um sistema de configuração flexível que permite que você defina a fonte de configuração ou as fontes usadas pelo seu aplicativo e sua implantação. A `ConfigurationBuilder` infraestrutura que você pode estar usando em seu aplicativo Web Forms foi modelada após os conceitos usados no sistema de configuração de ASP.NET Core.

O trecho a seguir demonstra como o projeto Web Forms eShop usa *web.config* para armazenar valores de configuração:

```xml
<configuration>
  <configSections>
    <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
  </configSections>
  <connectionStrings>
    <add name="CatalogDBContext" connectionString="Data Source=(localdb)\MSSQLLocalDB; Initial Catalog=Microsoft.eShopOnContainers.Services.CatalogDb; Integrated Security=True; MultipleActiveResultSets=True;" providerName="System.Data.SqlClient" />
  </connectionStrings>
  <appSettings>
    <add key="UseMockData" value="true" />
    <add key="UseCustomizationData" value="false" />
  </appSettings>
</configuration>
```

É comum que segredos, como cadeias de conexão de banco de dados, sejam armazenados dentro do *web.config*. Os segredos são inevitavelmente persistentes em locais não seguros, como o controle do código-fonte. Com o Blazor no ASP.NET Core, a configuração baseada em XML anterior é substituída pelo JSON a seguir:

```json
{
  "ConnectionStrings": {
    "CatalogDBContext": "Data Source=(localdb)\\MSSQLLocalDB; Initial Catalog=Microsoft.eShopOnContainers.Services.CatalogDb; Integrated Security=True; MultipleActiveResultSets=True;"
  },
  "UseMockData": true,
  "UseCustomizationData": false
}
```

JSON é o formato de configuração padrão; no entanto, o ASP.NET Core dá suporte a muitos outros formatos, incluindo XML. Também há vários formatos com suporte da Comunidade.

O Construtor na Blazor classe do projeto `Startup` aceita uma `IConfiguration` instância por meio de uma técnica de di, conhecida como injeção de construtor:

```csharp
public class Startup
{
    public Startup(IConfiguration configuration, IWebHostEnvironment env)
    {
        Configuration = configuration;
        Env = env;
    }

    ...
}
```

Por padrão, variáveis de ambiente, arquivos JSON (*appsettings.jsem* e *appSettings. { Environment}. JSON*) e as opções de linha de comando são registradas como fontes de configuração válidas no objeto de configuração. As fontes de configuração podem ser acessadas via `Configuration[key]` . Uma técnica mais avançada é associar os dados de configuração a objetos usando o padrão de opções. Para obter mais informações sobre configuração e o padrão de opções, consulte [configuração no](/aspnet/core/fundamentals/configuration/) padrão de ASP.NET Core e [opções em ASP.NET Core](/aspnet/core/fundamentals/configuration/options), respectivamente.

## <a name="migrate-data-access"></a>Migrar acesso a dados

O acesso a dados é um aspecto importante de qualquer aplicativo. O projeto eShop armazena as informações de catálogo em um banco de dados e recupera os dados com Entity Framework (EF) 6. Como o EF 6 tem suporte no .NET Core 3,0, o projeto pode continuar a usá-lo.

As seguintes alterações relacionadas ao EF foram necessárias para eShop:

- Em .NET Framework, o `DbContext` objeto aceita uma cadeia de caracteres do formato *Name = ConnectionString* e usa a cadeia de conexão de `ConfigurationManager.AppSettings[ConnectionString]` para se conectar. No .NET Core, não há suporte para isso. A cadeia de conexão deve ser fornecida.
- O banco de dados foi acessado de forma síncrona. Embora isso funcione, a escalabilidade pode ser afetada. Essa lógica deve ser movida para um padrão assíncrono.

Embora não haja o mesmo suporte nativo para associação de conjunto de recursos, o Blazor fornece flexibilidade e potência com seu suporte C# em uma página Razor. Por exemplo, você pode executar cálculos e exibir o resultado. Para obter mais informações sobre padrões de dados no Blazor , consulte o capítulo de [acesso a dados](data.md) .

## <a name="architectural-changes"></a>Alterações de arquitetura

Por fim, há algumas diferenças de arquitetura importantes a serem consideradas ao migrar para o Blazor . Muitas dessas alterações são aplicáveis a qualquer coisa com base no .NET Core ou ASP.NET Core.

Como o Blazor é criado no .NET Core, há considerações sobre como garantir o suporte no .NET Core. Algumas das principais alterações incluem a remoção dos seguintes recursos:

- Vários AppDomains
- Comunicação remota
- CAS (segurança de acesso ao código)
- Transparência de Segurança

Para obter mais informações sobre técnicas para identificar as alterações necessárias para dar suporte à execução no .NET Core, consulte [portar seu código do .NET Framework para o .NET Core](/dotnet/core/porting).

ASP.NET Core é uma versão reimaginada do ASP.NET e tem algumas alterações que podem não parecer óbvias inicialmente. As principais alterações são:

- Nenhum contexto de sincronização, o que significa que não há `HttpContext.Current` , `Thread.CurrentPrincipal` ou outros acessadores estáticos
- Sem cópia de sombra
- Nenhuma fila de solicitações

Muitas operações no ASP.NET Core são assíncronas, o que permite o descarregamento mais fácil de tarefas vinculadas a e/s. É importante nunca Bloquear usando `Task.Wait()` `Task.GetResult()` o ou o, que pode esgotar rapidamente os recursos do pool de threads.

## <a name="migration-conclusion"></a>Conclusão da migração

Neste ponto, você viu muitos exemplos do que é necessário para mover um projeto Web Forms para o Blazor . Para obter um exemplo completo, consulte o projeto [eShopOn Blazor ](https://github.com/dotnet-architecture/eShopOnBlazor) .

>[!div class="step-by-step"]
>[Anterior](security-authentication-authorization.md)
