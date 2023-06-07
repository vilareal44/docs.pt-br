---
title: Páginas, roteamento e layouts
description: Saiba como criar páginas no Blazor , trabalhar com o roteamento do lado do cliente e gerenciar layouts de página.
author: danroth27
ms.author: daroth
no-loc:
- Blazor
ms.date: 09/19/2019
ms.openlocfilehash: 714ba0be7c2014895a75250a47e6ce448863eb6c
ms.sourcegitcommit: 0100be20fcf23f61dab672deced70059ed71bb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88267783"
---
# <a name="pages-routing-and-layouts"></a>Páginas, roteamento e layouts

ASP.NET Web Forms aplicativos são compostos de páginas definidas em arquivos *. aspx* . O endereço de cada página é baseado em seu caminho de arquivo físico no projeto. Quando um navegador faz uma solicitação para a página, o conteúdo da página é processado dinamicamente no servidor. As contas de renderização para a marcação HTML da página e seus controles de servidor.

No Blazor , cada página no aplicativo é um componente, normalmente definido em um arquivo *. Razor* , com uma ou mais rotas especificadas. O roteamento ocorre principalmente no lado do cliente sem envolver uma solicitação de servidor específica. O navegador primeiro faz uma solicitação para o endereço raiz do aplicativo. Um `Router` componente raiz no Blazor aplicativo, em seguida, lida com a interceptação de solicitações de navegação e com o componente correto.

Blazor também dá suporte à *vinculação profunda*. A vinculação profunda ocorre quando o navegador faz uma solicitação para uma rota específica diferente da raiz do aplicativo. As solicitações de links profundos enviados ao servidor são roteadas para o Blazor aplicativo, que roteia a solicitação do lado do cliente para o componente correto.

Uma página simples no ASP.NET Web Forms pode conter a seguinte marcação:

*Name. aspx*

```aspx-csharp
<%@ Page Title="Name" Language="C#" MasterPageFile="~/Site.Master" AutoEventWireup="true" CodeBehind="Name.aspx.cs" Inherits="WebApplication1.Name" %>

<asp:Content ID="BodyContent" ContentPlaceHolderID="MainContent" runat="server">
    <div>
        What is your name?<br />
        <asp:TextBox ID="TextBox1" runat="server"></asp:TextBox>
        <asp:Button ID="Button1" runat="server" Text="Submit" OnClick="Button1_Click" />
    </div>
    <div>
        <asp:Literal ID="Literal1" runat="server" />
    </div>
</asp:Content>
```

*Name.aspx.cs*

```csharp
public partial class Name : System.Web.UI.Page
{
    protected void Button1_Click1(object sender, EventArgs e)
    {
        Literal1.Text = "Hello " + TextBox1.Text;
    }
}
```

A página equivalente em um Blazor aplicativo ficaria assim:

*Nome. Razor*

```razor
@page "/Name"
@layout MainLayout

<div>
    What is your name?<br />
    <input @bind="text" />
    <button @onclick="OnClick">Submit</button>
</div>
<div>
    @if (name != null)
    {
        @:Hello @name
    }
</div>

@code {
    string text;
    string name;

    void OnClick() {
        name = text;
    }
}
```

## <a name="create-pages"></a>Criar páginas

Para criar uma página no Blazor , crie um componente e adicione a `@page` diretiva Razor para especificar a rota para o componente. A `@page` diretiva usa um único parâmetro, que é o modelo de rota a ser adicionado a esse componente.

```razor
@page "/counter"
```

O parâmetro de modelo de rota é necessário. Ao contrário de ASP.NET Web Forms, a rota para um Blazor componente *não é* inferida de seu local de arquivo (embora isso possa ser um recurso adicionado no futuro).

A sintaxe do modelo de rota é a mesma sintaxe básica usada para roteamento no ASP.NET Web Forms. Os parâmetros de rota são especificados no modelo usando chaves. Blazor associará valores de rota a parâmetros de componente com o mesmo nome (não diferencia maiúsculas de minúsculas).

```razor
@page "/product/{id}"

<h1>Product @Id</h1>

@code {
    [Parameter]
    public string Id { get; set; }
}
```

Você também pode especificar restrições no valor do parâmetro de rota. Por exemplo, para restringir a ID do produto a `int` :

```razor
@page "/product/{id:int}"

<h1>Product @Id</h1>

@code {
    [Parameter]
    public int Id { get; set; }
}
```

Para obter uma lista completa das restrições de rota com suporte pelo Blazor , consulte [restrições de rota](/aspnet/core/blazor/routing#route-constraints).

## <a name="router-component"></a>Componente do roteador

O roteamento no Blazor é tratado pelo `Router` componente. O `Router` componente é normalmente usado no componente raiz do aplicativo (*app. Razor*).

```razor
<Router AppAssembly="@typeof(Program).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <LayoutView Layout="@typeof(MainLayout)">
            <p>Sorry, there's nothing at this address.</p>
        </LayoutView>
    </NotFound>
</Router>
```

O `Router` componente descobre os componentes roteáveis no especificado `AppAssembly` e na opção especificada `AdditionalAssemblies` . Quando o navegador navega, o `Router` intercepta a navegação e renderiza o conteúdo de seu `Found` parâmetro com o extraído `RouteData` se uma rota corresponde ao endereço, caso contrário, o `Router` renderiza seu `NotFound` parâmetro.

O `RouteView` componente lida com a renderização do componente correspondente especificado pelo `RouteData` com seu layout, se ele tiver um. Se o componente correspondente não tiver um layout, a opção opcionalmente especificada `DefaultLayout` será usada.

O `LayoutView` componente renderiza seu conteúdo filho dentro do layout especificado. Veremos os layouts mais detalhadamente mais adiante neste capítulo.

## <a name="navigation"></a>Navegação

No ASP.NET Web Forms, você dispara a navegação para uma página diferente retornando uma resposta de redirecionamento para o navegador. Por exemplo:

```csharp
protected void NavigateButton_Click(object sender, EventArgs e)
{
    Response.Redirect("Counter");
}
```

O retorno de uma resposta de redirecionamento geralmente não é possível no Blazor . Blazor Não usa um modelo de solicitação-resposta. No entanto, é possível disparar navegações do navegador diretamente, como você pode com o JavaScript.

Blazor fornece um `NavigationManager` serviço que pode ser usado para:

- Obter o endereço do navegador atual
- Obter o endereço base
- Disparar navegações
- Seja notificado quando o endereço for alterado

Para navegar para um endereço diferente, use o `NavigateTo` método:

```razor
@page "/"
@inject NavigationManager NavigationManager

<button @onclick="Navigate">Navigate</button>

@code {
    void Navigate() {
        NavigationManager.NavigateTo("counter");
    }
}
```

Para obter uma descrição de todos os `NavigationManager` Membros, consulte [URI e auxiliares de estado de navegação](/aspnet/core/blazor/routing#uri-and-navigation-state-helpers).

## <a name="base-urls"></a>URLs base

Se seu Blazor aplicativo for implantado em um caminho base, você precisará especificar a URL base nos metadados da página usando a `<base>` marca para roteamento para a propriedade de trabalho. Se a página host do aplicativo for renderizada pelo servidor usando o Razor, você poderá usar a `~/` sintaxe para especificar o endereço base do aplicativo. Se a página host for HTML estático, você precisará especificar a URL base explicitamente.

```html
<base href="~/" />
```

## <a name="page-layout"></a>Layout de página

O layout de página no ASP.NET Web Forms é manipulado por páginas mestras. As páginas mestras definem um modelo com um ou mais espaços reservados de conteúdo que podem ser fornecidos por páginas individuais. As páginas mestras são definidas em arquivos *. Master* e começam com a `<%@ Master %>` diretiva. O conteúdo dos arquivos *. Master* é codificado como você faria com uma página *. aspx* , mas com a adição de `<asp:ContentPlaceHolder>` controles para marcar onde as páginas podem fornecer conteúdo.

*Site.master*

```aspx-csharp
<%@ Master Language="C#" AutoEventWireup="true" CodeBehind="Site.master.cs" Inherits="WebApplication1.SiteMaster" %>

<!DOCTYPE html>
<html lang="en">
<head runat="server">
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title><%: Page.Title %> - My ASP.NET Application</title>
    <link href="~/favicon.ico" rel="shortcut icon" type="image/x-icon" />
</head>
<body>
    <form runat="server">
        <div class="container body-content">
            <asp:ContentPlaceHolder ID="MainContent" runat="server">
            </asp:ContentPlaceHolder>
            <hr />
            <footer>
                <p>&copy; <%: DateTime.Now.Year %> - My ASP.NET Application</p>
            </footer>
        </div>
    </form>
</body>
</html>
```

No Blazor , você lida com o layout da página usando componentes de layout. Os componentes de layout herdam de `LayoutComponentBase` , que define uma única `Body` Propriedade do tipo `RenderFragment` , que pode ser usada para renderizar o conteúdo da página.

*MainLayout. Razor*

```razor
@inherits LayoutComponentBase
<h1>Main layout</h1>
<div>
    @Body
</div>
```

Quando a página com um layout é renderizada, a página é renderizada dentro do conteúdo do layout especificado no local em que o layout renderiza sua `Body` propriedade.

Para aplicar um layout a uma página, use a `@layout` diretiva:

```razor
@layout MainLayout
```

Você pode especificar o layout de todos os componentes em uma pasta e subpastas usando um arquivo *_Imports. Razor* . Você também pode especificar um layout padrão para todas as suas páginas usando o [componente do roteador](#router-component).

As páginas mestras podem definir vários espaços reservados de conteúdo, mas os layouts Blazor têm apenas uma única `Body` propriedade. Essa limitação dos Blazor componentes de layout, felizmente, será abordada em uma versão futura.

As páginas mestras no ASP.NET Web Forms podem ser aninhadas. Ou seja, uma página mestra também pode usar uma página mestra. Os componentes de layout no Blazor também podem estar aninhados. Você pode aplicar um componente de layout a um componente de layout. O conteúdo do layout interno será renderizado dentro do layout externo.

*ChildLayout. Razor*

```razor
@layout MainLayout
<h2>Child layout</h2>
<div>
    @Body
</div>
```

*Index. Razor*

```razor
@page "/"
@layout ChildLayout
<p>I'm in a nested layout!</p>
```

A saída renderizada para a página seria:

```html
<h1>Main layout</h1>
<div>
    <h2>Child layout</h2>
    <div>
        <p>I'm in a nested layout!</p>
    </div>
</div>
```

Os layouts no Blazor não definem normalmente os elementos HTML raiz de uma página ( `<html>` ,, `<body>` `<head>` e assim por diante). Os elementos HTML raiz são definidos na Blazor página host de um aplicativo, que é usada para renderizar o conteúdo HTML inicial para o aplicativo (consulte [Bootstrap Blazor ](project-structure.md#bootstrap-blazor)). A página host pode renderizar vários componentes raiz para o aplicativo com marcação ao redor.

Os componentes no Blazor , incluindo páginas, não podem renderizar `<script>` marcas. Essa restrição de renderização existe porque as `<script>` marcas são carregadas uma vez e não podem ser alteradas. Pode ocorrer um comportamento inesperado se você tentar renderizar as marcas dinamicamente usando sintaxe Razor. Em vez disso, todas as `<script>` marcas devem ser adicionadas à página host do aplicativo.

>[!div class="step-by-step"]
>[Anterior](components.md) 
> [Avançar](state-management.md)
