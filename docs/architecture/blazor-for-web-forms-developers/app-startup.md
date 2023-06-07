---
title: Inicialização do aplicativo
description: Saiba como definir a lógica de inicialização para seu aplicativo.
author: csharpfritz
ms.author: jefritz
ms.date: 02/25/2020
ms.openlocfilehash: ea2ea458011d8351a834aa12db02e5d2bac2dc65
ms.sourcegitcommit: 0100be20fcf23f61dab672deced70059ed71bb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88267692"
---
# <a name="app-startup"></a>Inicialização do aplicativo

Os aplicativos escritos para ASP.NET normalmente têm um `global.asax.cs` arquivo que define o `Application_Start` evento que controla quais serviços são configurados e disponibilizados para processamento em HTML e .net. Este capítulo analisa como as coisas são ligeiramente diferentes com ASP.NET Core e um servidor mais incrivelmente.

## <a name="application_start-and-web-forms"></a>Application_Start e Web Forms

O método padrão de formulários da Web aumentou com a `Application_Start` finalidade de anos para lidar com muitas tarefas de configuração.  Um novo projeto Web Forms com o modelo padrão no Visual Studio 2019 agora contém a seguinte lógica de configuração:

- `RouteConfig` -Roteamento de URL de aplicativo
- `BundleConfig` -Agrupamento e minificação do CSS e do JavaScript

Cada um desses arquivos individuais reside na `App_Start` pasta e é executado apenas uma vez no início do nosso aplicativo.  `RouteConfig` no modelo de projeto padrão, o adiciona o `FriendlyUrlSettings` para Web Forms para permitir que as URLs de aplicativo omitam a `.ASPX` extensão de arquivo.  O modelo padrão também contém uma diretiva que fornece códigos de status de redirecionamento HTTP permanentes (HTTP 301) para as `.ASPX` páginas para a URL amigável com o nome do arquivo que omite a extensão.

Com o ASP.NET Core e o mais claro, esses métodos são simplificados e consolidados na `Startup` classe ou são eliminados em favor de tecnologias da Web comuns.

## <a name="blazor-server-startup-structure"></a>Estrutura de inicialização de servidor mais incrivelmente

Os aplicativos de servidor mais recentes residem na parte superior de um aplicativo ASP.NET Core 3,0 ou posterior.  ASP.NET Core aplicativos Web são configurados por meio de um par de métodos na `Startup.cs` classe na pasta raiz do aplicativo.  O conteúdo padrão da classe de inicialização está listado abaixo

```csharp
public class Startup
{
  public Startup(IConfiguration configuration)
  {
    Configuration = configuration;
  }

  public IConfiguration Configuration { get; }

  // This method gets called by the runtime. Use this method to add services to the container.
  // For more information on how to configure your application, visit https://go.microsoft.com/fwlink/?LinkID=398940
  public void ConfigureServices(IServiceCollection services)
  {
    services.AddRazorPages();
    services.AddServerSideBlazor();
    services.AddSingleton<WeatherForecastService>();
  }

  // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
  public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
  {
    if (env.IsDevelopment())
    {
      app.UseDeveloperExceptionPage();
    }
    else
    {
      app.UseExceptionHandler("/Error");
      // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
      app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();

    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
      endpoints.MapBlazorHub();
      endpoints.MapFallbackToPage("/_Host");
   });
  }
 }
```

Como o restante do ASP.NET Core, a classe de inicialização é criada com princípios de injeção de dependência.  O `IConfiguration` é fornecido ao construtor e Stash em uma propriedade pública para acesso posterior durante a configuração.

O `ConfigureServices` método introduzido no ASP.NET Core permite que vários serviços do ASP.NET Core Framework sejam configurados para o contêiner de injeção de dependência interna da estrutura.  Os vários `services.Add*` métodos adicionam serviços que habilitam recursos como autenticação, páginas do Razor, roteamento do controlador MVC, signalr e interações de servidor mais incrivelmente entre muitos outros.  Esse método não era necessário em Web Forms, pois a análise e o manuseio dos arquivos ASPX, ASCX, ASHX e ASMX foram definidos por meio da referência a ASP.NET no arquivo de configuração web.config.  Mais informações sobre injeção de dependência no ASP.NET Core estão disponíveis na [documentação online](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection).

O `Configure` método apresenta o conceito do pipeline http para ASP.NET Core.  Nesse método, declaramos de cima para baixo o [middleware](middleware.md) que tratará cada solicitação enviada ao nosso aplicativo. A maioria desses recursos na configuração padrão foi espalhada pelos arquivos de configuração do Web Forms e agora está em um único lugar para facilitar a referência.

Não é mais a configuração da página de erro personalizada colocada em um `web.config` arquivo, mas agora está configurada para ser sempre exibida se o ambiente do aplicativo não estiver rotulado `Development` .  Além disso, ASP.NET Core aplicativos agora estão configurados para servir páginas seguras com o TLS por padrão com a `UseHttpsRedirection` chamada de método.

Em seguida, um método de configuração inesperado é listado para `UseStaticFiles` .  No ASP.NET Core, o suporte para solicitações de arquivos estáticos (como JavaScript, CSS e arquivos de imagem) deve ser explicitamente habilitado e somente os arquivos na pasta *wwwroot* do aplicativo são publicamente endereçáveis por padrão.

A próxima linha é a primeira que replica uma das opções de configuração do Web Forms: `UseRouting` .  Esse método adiciona o ASP.NET Core roteador ao pipeline e pode ser configurado aqui ou nos arquivos individuais aos quais ele pode considerar o roteamento.  Mais informações sobre a configuração de roteamento podem ser encontradas na [seção roteamento](pages-routing-layouts.md).

A instrução final nesse método define os pontos de extremidade que ASP.NET Core está escutando.  Esses são os locais acessíveis pela Web que você pode acessar no servidor Web e receber algum conteúdo manipulado pelo .NET e retornado a você.  A primeira entrada, `MapBlazorHub` configura um Hub do signalr para uso no fornecimento da conexão em tempo real e persistente ao servidor onde o estado e a renderização de componentes mais fáceis são tratados.  A `MapFallbackToPage` chamada do método indica o local acessível pela Web da página que inicia o aplicativo mais sincero e também configura o aplicativo para lidar com solicitações de vinculação profunda do lado do cliente.  Você verá esse recurso no trabalho se abrir um navegador e navegar diretamente para uma rota manipulada mais incrivelmente em seu aplicativo, como `/counter` no modelo de projeto padrão. A solicitação é manipulada pela página de fallback *_Host. cshtml* , que, em seguida, executa o roteador mais incrivelmente e renderiza a página do contador.

## <a name="upgrading-the-bundleconfig-process"></a>Atualizando o processo BundleConfig

As tecnologias para agrupar ativos como folhas de estilo CSS e arquivos JavaScript evoluíram significativamente, com outras tecnologias que fornecem ferramentas e técnicas rapidamente em evolução para o gerenciamento desses recursos.  Para esse fim, é recomendável usar uma ferramenta de linha de comando de nó, como Grunt/Gulp/webpack, para empacotar seus ativos estáticos.

As ferramentas de linha de comando Grunt, Gulp e webpack e suas configurações associadas podem ser adicionadas ao seu aplicativo e ASP.NET Core ignorarão silenciosamente esses arquivos durante o processo de compilação do aplicativo.  Você pode adicionar uma chamada para executar suas tarefas adicionando um `Target` dentro do arquivo de projeto com sintaxe semelhante à seguinte que dispararia um script Gulp e o `min` destino dentro desse script

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp min" />
</Target>
```

Mais detalhes sobre as duas estratégias para gerenciar seus arquivos CSS e JavaScript estão disponíveis no [pacote e reduzir ativos estáticos na documentação ASP.NET Core](https://docs.microsoft.com/aspnet/core/client-side/bundling-and-minification) .

>[!div class="step-by-step"]
>[Anterior](project-structure.md) 
> [Avançar](components.md)
