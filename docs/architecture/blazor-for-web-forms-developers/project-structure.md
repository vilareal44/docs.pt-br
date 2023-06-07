---
title: Estrutura do projeto para Blazor aplicativos
description: Saiba como as estruturas de projeto de ASP.NET Web Forms e Blazor projetos são comparadas.
author: danroth27
ms.author: daroth
no-loc:
- Blazor
- WebAssembly
ms.date: 09/11/2019
ms.openlocfilehash: 225ebbdd5e23516ae7d5465371e95c73c440c82b
ms.sourcegitcommit: 0100be20fcf23f61dab672deced70059ed71bb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88267770"
---
# <a name="project-structure-for-no-locblazor-apps"></a>Estrutura do projeto para Blazor aplicativos

Apesar das diferenças significativas na estrutura do projeto, ASP.NET Web Forms e Blazor Compartilhe muitos conceitos semelhantes. Aqui, veremos a estrutura de um Blazor projeto e o comparamos a um projeto ASP.NET Web Forms.

Para criar seu primeiro Blazor aplicativo, siga as instruções nas [ Blazor etapas de introdução](/aspnet/core/blazor/get-started). Você pode seguir as instruções para criar um Blazor aplicativo de servidor ou um Blazor WebAssembly aplicativo hospedado no ASP.NET Core. Exceto para a lógica específica do modelo de hospedagem, a maior parte do código em ambos os projetos é a mesma.

## <a name="project-file"></a>Arquivo de projeto

Blazor Os aplicativos de servidor são projetos .NET Core. O arquivo de projeto para o Blazor aplicativo de servidor é quase tão simples quanto pode ser:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp3.0</TargetFramework>
  </PropertyGroup>

</Project>
```

O arquivo de projeto para um Blazor WebAssembly aplicativo parece um pouco mais envolvido (números de versão exatas podem variar):

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netstandard2.0</TargetFramework>
    <RazorLangVersion>3.0</RazorLangVersion>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.Blazor" Version="3.1.0" />
    <PackageReference Include="Microsoft.AspNetCore.Blazor.Build" Version="3.1.0" PrivateAssets="all" />
    <PackageReference Include="Microsoft.AspNetCore.Blazor.HttpClient" Version="3.1.0" />
    <PackageReference Include="Microsoft.AspNetCore.Blazor.DevServer" Version="3.1.0" PrivateAssets="all" />
  </ItemGroup>

  <ItemGroup>
    <ProjectReference Include="..\Shared\BlazorWebAssemblyApp1.Shared.csproj" />
  </ItemGroup>

</Project>
```

Blazoros WebAssembly projetos visam .net Standard em vez do .NET Core porque eles são executados no navegador em um WebAssembly tempo de execução do .NET com base em um. Você não pode instalar o .NET em um navegador da Web como você pode em um computador de servidor ou de desenvolvedor. Consequentemente, o projeto faz referência à Blazor estrutura usando referências de pacote individuais.

Por comparação, um projeto ASP.NET Web Forms padrão inclui quase 300 linhas de XML em seu arquivo *. csproj* , a maior parte deles está listando explicitamente os vários arquivos de código e conteúdo no projeto. Muitas das simplificações dos projetos baseados no .NET Core e no .NET Standard são provenientes dos destinos e das propriedades padrão importadas referenciando o `Microsoft.NET.Sdk.Web` SDK, geralmente conhecido como simplesmente o SDK da Web. O SDK da Web inclui curingas e outras conveniências que simplificam a inclusão de código e arquivos de conteúdo no projeto. Você não precisa listar os arquivos explicitamente. Ao direcionar o .NET Core, o Web SDK também adiciona referências de estrutura às estruturas compartilhadas do .NET Core e do ASP.NET Core. As estruturas são visíveis no nó de estruturas de **dependências**  >  **Frameworks** na janela **Gerenciador de soluções** . As estruturas compartilhadas são coleções de assemblies que foram instalados no computador durante a instalação do .NET Core.

Embora eles tenham suporte, as referências de assembly individuais são menos comuns em projetos do .NET Core. A maioria das dependências do projeto é tratada como referências de pacote NuGet. Você só precisa referenciar as dependências de pacote de nível superior em projetos do .NET Core. Dependências transitivas são incluídas automaticamente. Em vez de usar o arquivo de *packages.config* normalmente encontrado em projetos de Web Forms de ASP.net para referenciar pacotes, as referências de pacote são adicionadas ao arquivo de projeto usando o `<PackageReference>` elemento.

```xml
<ItemGroup>
  <PackageReference Include="Newtonsoft.Json" Version="12.0.2" />
</ItemGroup>
```

## <a name="entry-point"></a>Ponto de entrada

O Blazor ponto de entrada do aplicativo de servidor é definido no arquivo *Program.cs* , como você veria em um aplicativo de console. Quando o aplicativo é executado, ele cria e executa uma instância de host Web usando padrões específicos para aplicativos Web. O host da Web gerencia o Blazor ciclo de vida do aplicativo do servidor e configura os serviços de nível de host. Exemplos desses serviços são configuração, registro em log, injeção de dependência e o servidor HTTP. Esse código é basicamente clichê e, muitas vezes, permanece inalterado.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStartup<Startup>();
            });
}
```

Blazoros WebAssembly aplicativos também definem um ponto de entrada em *Program.cs*. O código parece um pouco diferente. O código é semelhante, pois está configurando o host de aplicativo para fornecer os mesmos serviços de nível de host para o aplicativo. WebAssemblyNo entanto, o host de aplicativo não configura um servidor http porque ele é executado diretamente no navegador.

Blazor os aplicativos têm uma `Startup` classe em vez de um arquivo *global. asax* para definir a lógica de inicialização para o aplicativo. A `Startup` classe é usada para configurar o aplicativo e quaisquer serviços específicos do aplicativo. No Blazor aplicativo de servidor, a `Startup` classe é usada para configurar o ponto de extremidade para a conexão em tempo real usada pelo Blazor entre os navegadores cliente e o servidor. No Blazor WebAssembly aplicativo, a `Startup` classe define os componentes raiz para o aplicativo e onde eles devem ser renderizados. Vamos dar uma olhada mais detalhada na `Startup` classe na seção de [inicialização do aplicativo](./app-startup.md) .

## <a name="static-files"></a>Arquivos estáticos

Ao contrário dos projetos do ASP.NET Web Forms, nem todos os arquivos em um Blazor projeto podem ser solicitados como arquivos estáticos. Somente os arquivos na pasta *wwwroot* são endereçáveis da Web. Essa pasta é referida na "raiz da Web" do aplicativo. Qualquer coisa fora da raiz da Web do aplicativo *não é* endereçável da Web. Essa configuração fornece um nível adicional de segurança que impede a exposição acidental de arquivos de projeto na Web.

## <a name="configuration"></a>Configuração

A configuração no ASP.NET Web Forms aplicativos normalmente é manipulada usando um ou mais arquivos de *web.config* . Blazor os aplicativos normalmente não têm *web.config* arquivos. Se isso for feito, o arquivo será usado apenas para definir configurações específicas do IIS quando hospedado no IIS. Em vez disso, Blazor os aplicativos de servidor usam as abstrações de configuração ASP.NET Core ( Blazor WebAssembly os aplicativos atualmente não dão suporte às mesmas abstrações de configuração, mas podem ser um recurso adicionado no futuro). Por exemplo, o Blazor aplicativo de servidor padrão armazena algumas configurações no *appsettings.jsem*.

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "AllowedHosts": "*"
}
```

Aprenderemos mais sobre a configuração em projetos ASP.NET Core na seção de [configuração](./config.md) .

## <a name="razor-components"></a>Componentes do Razor

A maioria dos arquivos em Blazor projetos são arquivos *. Razor* . O Razor é uma linguagem de modelagem baseada em HTML e em C# que é usada para gerar dinamicamente a interface do usuário da Web. Os arquivos *. Razor* definem componentes que compõem a interface do usuário do aplicativo. Para a maior parte, os componentes são idênticos para o Blazor servidor e os Blazor WebAssembly aplicativos. Os componentes no Blazor são análogos aos controles de usuário no ASP.NET Web Forms.

Cada arquivo de componente do Razor é compilado em uma classe do .NET quando o projeto é compilado. A classe gerada captura o estado do componente, a lógica de renderização, os métodos de ciclo de vida, os manipuladores de eventos e outras lógicas. Vamos examinar os componentes de criação na seção [criando componentes Blazor de interface do usuário reutilizáveis](./components.md) .

Os arquivos *_Imports. Razor* não são arquivos de componente do Razor. Em vez disso, eles definem um conjunto de diretivas do Razor para importar para outros arquivos *. Razor* dentro da mesma pasta e em suas subpastas. Por exemplo, um arquivo *_Imports. Razor* é uma maneira convencional de adicionar `using` diretivas para namespaces comumente usados:

```razor
@using System.Net.Http
@using Microsoft.AspNetCore.Authorization
@using Microsoft.AspNetCore.Components.Authorization
@using Microsoft.AspNetCore.Components.Forms
@using Microsoft.AspNetCore.Components.Routing
@using Microsoft.AspNetCore.Components.Web
@using Microsoft.JSInterop
@using BlazorApp1
@using BlazorApp1.Shared
```

## <a name="pages"></a>Páginas

Onde estão as páginas nos Blazor aplicativos? Blazor não define uma extensão de arquivo separada para páginas endereçáveis, como os arquivos *. aspx* em ASP.NET Web Forms aplicativos. Em vez disso, as páginas são definidas por meio da atribuição de rotas a componentes. Uma rota é normalmente atribuída usando a `@page` diretiva Razor. Por exemplo, o `Counter` componente criado no arquivo *pages/Counter. Razor* define a seguinte rota:

```razor
@page "/counter"
```

O roteamento no Blazor é tratado pelo lado do cliente, não no servidor. À medida que o usuário navega no navegador, Blazor intercepta a navegação e, em seguida, renderiza o componente com a rota correspondente.

As rotas de componentes não são inferidas no momento pelo local do arquivo do componente como estão com páginas *. aspx* . Esse recurso pode ser adicionado no futuro. Cada rota deve ser especificada explicitamente no componente. O armazenamento de componentes roteáveis em uma pasta de *páginas* não tem significado especial e é puramente uma convenção.

Veremos mais detalhadamente no roteamento no Blazor na seção [páginas, roteamento e layouts](./pages-routing-layouts.md) .

## <a name="layout"></a>Layout

No ASP.NET Web Forms aplicativos, o layout de página comum é manipulado usando páginas mestras (*site. Master*). Em Blazor aplicativos, o layout de página é manipulado usando componentes de layout (*Shared/MainLayout. Razor*). Os componentes de layout serão discutidos em mais detalhes na seção [página, roteamento e layouts](./pages-routing-layouts.md) .

## <a name="bootstrap-no-locblazor"></a>Inicialização Blazor

Para inicializar Blazor , o aplicativo deve:

- Especifique o local na página em que o componente raiz (*app. Razor*) deve ser renderizado.
- Adicione o Blazor script de estrutura correspondente.

No Blazor aplicativo de servidor, a página host do componente raiz é definida no arquivo *_Host. cshtml* . Esse arquivo define uma página Razor, não um componente. Razor Pages usar sintaxe Razor para definir uma página endereçável do servidor, muito parecida com uma página *. aspx* . O `Html.RenderComponentAsync<TComponent>(RenderMode)` método é usado para definir onde um componente de nível raiz deve ser renderizado. A `RenderMode` opção indica a maneira como o componente deve ser renderizado. A tabela a seguir descreve as opções com suporte `RenderMode` .

|Opção                        |Descrição       |
|------------------------------|------------------|
|`RenderMode.Server`           |Renderizado interativamente quando uma conexão com o navegador é estabelecida|
|`RenderMode.ServerPrerendered`|Primeiro renderizado e, em seguida, renderizado interativamente|
|`RenderMode.Static`           |Renderizado como conteúdo estático|

A referência de script para *_framework/blazor.server.js* estabelece a conexão em tempo real com o servidor e, em seguida, lida com todas as interações do usuário e as atualizações da interface de usuário.

```razor
@page "/"
@namespace BlazorApp1.Pages
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>BlazorApp1</title>
    <base href="~/" />
    <link rel="stylesheet" href="css/bootstrap/bootstrap.min.css" />
    <link href="css/site.css" rel="stylesheet" />
</head>
<body>
    <app>
        @(await Html.RenderComponentAsync<App>(RenderMode.ServerPrerendered))
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
</html>
```

No Blazor WebAssembly aplicativo, a página host é um arquivo HTML estático simples em *wwwroot/index.html*. O `<app>` elemento é usado para indicar onde o componente raiz deve ser renderizado.

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width" />
    <title>BlazorApp2</title>
    <base href="/" />
    <link href="css/bootstrap/bootstrap.min.css" rel="stylesheet" />
    <link href="css/site.css" rel="stylesheet" />
</head>
<body>
    <app>Loading...</app>

    <script src="_framework/blazor.webassembly.js"></script>
</body>
</html>
```

O componente específico a ser renderizado é configurado no método do aplicativo `Startup.Configure` com um seletor CSS correspondente, indicando onde o componente deve ser renderizado.

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IComponentsApplicationBuilder app)
    {
        app.AddComponent<App>("app");
    }
}
```

## <a name="build-output"></a>Saída do build

Quando um Blazor projeto é compilado, todos os arquivos de código e componente do Razor são compilados em um único assembly. Diferentemente do ASP.NET Web Forms projetos, Blazor o não dá suporte à compilação em tempo de execução da lógica da interface do usuário

## <a name="run-the-app"></a>Executar o aplicativo

Para executar o Blazor aplicativo de servidor, pressione `F5` no Visual Studio. Blazor os aplicativos não oferecem suporte à compilação em tempo de execução. Para ver os resultados das alterações de marcação de código e de componente, recompile e reinicie o aplicativo com o depurador anexado. Se você executar sem o depurador anexado ( `Ctrl+F5` ), o Visual Studio inspecionará as alterações de arquivo e reiniciará o aplicativo conforme as alterações forem feitas. Você atualiza manualmente o navegador conforme as alterações são feitas.

Para executar o Blazor WebAssembly aplicativo, escolha uma das seguintes abordagens:

- Execute o projeto do cliente diretamente usando o servidor de desenvolvimento.
- Execute o projeto de servidor ao hospedar o aplicativo com ASP.NET Core.

Blazoros WebAssembly aplicativos não oferecem suporte à depuração usando o Visual Studio. Para executar o aplicativo, use `Ctrl+F5` em vez de `F5` . Em vez disso, você pode depurar Blazor WebAssembly aplicativos diretamente no navegador. Consulte [depurar ASP.NET Core Blazor ](/aspnet/core/blazor/debug) para obter detalhes.

>[!div class="step-by-step"]
>[Anterior](hosting-models.md) 
> [Avançar](app-startup.md)
