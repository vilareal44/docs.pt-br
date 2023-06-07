---
title: 'Segurança: autenticação e autorização no ASP.NET Web Forms e Blazor'
description: Saiba como lidar com autenticação e autorização no ASP.NET Web Forms e Blazor .
author: ardalis
ms.author: daroth
no-loc:
- Blazor
ms.date: 09/11/2019
ms.openlocfilehash: 1cc82b14a940465c26377f9181a2e20b46b0783f
ms.sourcegitcommit: 0100be20fcf23f61dab672deced70059ed71bb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88267812"
---
# <a name="security-authentication-and-authorization-in-aspnet-web-forms-and-no-locblazor"></a>Segurança: autenticação e autorização no ASP.NET Web Forms e Blazor

A migração de um aplicativo ASP.NET Web Forms para Blazor quase certamente exigirá a atualização de como a autenticação e a autorização são executadas, supondo que o aplicativo tenha a autenticação configurada. Este capítulo explicará como migrar do modelo de provedor universal do ASP.NET Web Forms (para associação, funções e perfis de usuário) e como trabalhar com ASP.NET Core identidade de Blazor aplicativos. Embora este capítulo aborde as etapas e considerações de alto nível, as etapas e os scripts detalhados podem ser encontrados na documentação referenciada.

## <a name="aspnet-universal-providers"></a>Provedores universais ASP.NET

Desde ASP.NET 2,0, a plataforma de Web Forms ASP.NET tem suporte para um modelo de provedor para uma variedade de recursos, incluindo associação. O provedor de Associação Universal, junto com o provedor de função opcional, é muito comumente implantado com aplicativos de Web Forms de ASP.NET. Ele oferece uma maneira robusta e segura de gerenciar a autenticação e a autorização que continuam funcionando bem hoje. A oferta mais recente desses provedores universais está disponível como um pacote NuGet, [Microsoft. AspNet. Providers](https://www.nuget.org/packages/Microsoft.AspNet.Providers).

O provedores universais trabalhar com um esquema de banco de dados SQL que inclui tabelas como `aspnet_Applications` ,, `aspnet_Membership` `aspnet_Roles` e `aspnet_Users` . Quando configurado executando o [ comandoaspnet_regsql.exe](https://docs.microsoft.com/previous-versions/ms229862(v=vs.140)), os provedores instalam tabelas e procedimentos armazenados que fornecem todas as consultas necessárias e os comandos necessários para trabalhar com os dados subjacentes. O esquema de banco de dados e esses procedimentos armazenados não são compatíveis com os sistemas de identidade ASP.NET Identity e ASP.NET Core mais recentes, portanto, os dados existentes devem ser migrados para o novo sistema. A Figura 1 mostra um esquema de tabela de exemplo configurado para provedores universais.

![esquema de provedores universais](./media/security/membership-tables.png)

O provedor universal trata usuários, associação, funções e perfis. Os usuários recebem identificadores globais exclusivos e as informações muito básicas (userId, userName) são armazenadas na `aspnet_Users` tabela. As informações de autenticação, como senha, formato de senha, Salt de senha, contadores de bloqueio e detalhes, etc., são armazenadas na `aspnet_Membership` tabela. As funções consistem simplesmente em nomes e identificadores exclusivos, que são atribuídos aos usuários por meio da `aspnet_UsersInRoles` tabela de associação, fornecendo uma relação muitos-para-muitos.

Se o sistema existente estiver usando funções além da associação, você precisará migrar as contas de usuário, as senhas associadas, as funções e a associação de função em ASP.NET Core identidade. Você também precisará atualizar seu código, no qual você está executando verificações de função usando instruções IF para utilizar filtros declarativos, atributos e/ou auxiliares de marca. Analisaremos as considerações de migração com mais detalhes no final deste capítulo.

### <a name="authorization-configuration-in-web-forms"></a>Configuração de autorização no Web Forms

Para configurar o acesso autorizado a determinadas páginas em um aplicativo ASP.NET Web Forms, normalmente você especifica que determinadas páginas ou pastas estão inacessíveis para usuários anônimos. Isso é feito no arquivo de web.config:

```xml
<?xml version="1.0"?>
<configuration>
    <system.web>
      <authentication mode="Forms">
        <forms defaultUrl="~/home.aspx" loginUrl="~/login.aspx"
          slidingExpiration="true" timeout="2880"></forms>
      </authentication>

      <authorization>
        <deny users="?" />
      </authorization>
    </system.web>
</configuration>
```

A `authentication` seção de configuração configura a autenticação de formulários para o aplicativo. A `authorization` seção é usada para não permitir usuários anônimos para todo o aplicativo. No entanto, você pode fornecer regras de autorização mais granulares em uma base por local, bem como aplicar verificações de autorização baseadas em função.

```xml
<location path="login.aspx">
  <system.web>
    <authorization>
      <allow users="*" />
    </authorization>
  </system.web>
</location>
```

A configuração acima, quando combinada com a primeira, permitiria que usuários anônimos acessassem a página de logon, substituindo a restrição em todo o site em usuários não autenticados.

```xml
<location path="/admin">
  <system.web>
    <authorization>
      <allow roles="Administrators" />
      <deny users="*" />
    </authorization>
  </system.web>
</location>
```

A configuração acima, quando combinada com as outras, restringe o acesso à `/admin` pasta e a todos os recursos dentro dela para os membros da função "administradores". Isso também pode ser aplicado colocando um arquivo separado `web.config` dentro da `/admin` raiz da pasta.

### <a name="authorization-code-in-web-forms"></a>Código de autorização no Web Forms

Além de configurar o acesso usando `web.config` o, você também pode configurar programaticamente o acesso e o comportamento em seu aplicativo Web Forms. Por exemplo, você pode restringir a capacidade de executar determinadas operações ou exibir determinados dados com base na função do usuário.

Esse código pode ser usado na lógica codebehind, bem como na própria página:

```html
<% if (HttpContext.Current.User.IsInRole("Administrators")) { %>
  <a href="/admin">Go To Admin</a>
<% } %>
```

Além de verificar a associação de função de usuário, você também pode determinar se elas são autenticadas (embora geralmente isso seja melhor usando a configuração baseada em local abordada acima). Veja abaixo um exemplo dessa abordagem.

```csharp
protected void Page_Load(object sender, EventArgs e)
{
    if (!User.Identity.IsAuthenticated)
    {
        FormsAuthentication.RedirectToLoginPage();
    }
    if (!Roles.IsUserInRole(User.Identity.Name, "Administrators"))
    {
        MessageLabel.Text = "Only administrators can view this.";
        SecretPanel.Visible = false;
    }
}
```

No código acima, o RBAC (controle de acesso baseado em função) é usado para determinar se determinados elementos da página, como um `SecretPanel` , são visíveis com base na função do usuário atual.

Normalmente, ASP.NET Web Forms aplicativos configuram a segurança dentro do `web.config` arquivo e, em seguida, adicionam verificações adicionais onde for necessário em `.aspx` páginas e seus `.aspx.cs` arquivos code-behind relacionados. A maioria dos aplicativos aproveita o provedor de Associação Universal, frequentemente com o provedor de função adicional.

## <a name="aspnet-core-identity"></a>Identidade do ASP.NET Core

Embora ainda sejam tarefas com autenticação e autorização, a identidade ASP.NET Core usa um conjunto diferente de abstrações e suposições quando comparada aos provedores universais. Por exemplo, o novo modelo de identidade dá suporte à autenticação de terceiros, permitindo que os usuários se autentiquem usando uma conta de mídia social ou outro provedor de autenticação confiável. ASP.NET Core identidade dá suporte à interface do usuário para páginas comumente necessárias, como logon, Logout e registro. Ele aproveita EF Core para seu acesso a dados e usa EF Core migrações para gerar o esquema necessário necessário para dar suporte ao seu modelo de dados. Esta [introdução à identidade em ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/authentication/identity) fornece uma boa visão geral do que está incluído com a identidade ASP.NET Core e como começar a trabalhar com ele. Se você ainda não tiver configurado ASP.NET Core identidade em seu aplicativo e seu banco de dados, ele o ajudará a começar.

### <a name="roles-claims-and-policies"></a>Funções, declarações e políticas

Tanto os provedores universais quanto a identidade de ASP.NET Core dão suporte ao conceito de funções. Você pode criar funções para usuários e atribuir usuários a funções. Os usuários podem pertencer a qualquer número de funções e você pode verificar a associação de função como parte da sua implementação de autorização.

Além das funções, ASP.NET Core identidade dá suporte aos conceitos de declarações e políticas. Embora uma função deva corresponder especificamente a um conjunto de recursos que um usuário nessa função deve ser capaz de acessar, uma declaração é simplesmente parte da identidade de um usuário. Uma declaração é um par de valor de nome que representa qual é o assunto, não o que o assunto pode fazer.

É possível inspecionar as declarações de um usuário diretamente e determinar com base nesses se um usuário deve receber acesso a um recurso. No entanto, essas verificações costumam ser repetitivas e espalhadas em todo o sistema. Uma abordagem melhor é definir uma *política*.

Uma política de autorização consiste em um ou mais requisitos. As políticas são registradas como parte da configuração do serviço de autorização no `ConfigureServices` método de `Startup.cs` . Por exemplo, o trecho de código a seguir configura uma política chamada "CanadiansOnly", que tem o requisito de que o usuário tenha a declaração Country com o valor de "Canada".

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy("CanadiansOnly", policy => policy.RequireClaim(ClaimTypes.Country, "Canada"));
});
```

Você pode [aprender mais sobre como criar políticas personalizadas na documentação do](https://docs.microsoft.com/aspnet/core/security/authorization/policies).

Se você estiver usando políticas ou funções, poderá especificar que uma página específica em seu Blazor aplicativo exija essa função ou política com o `[Authorize]` atributo, aplicada com a `@attribute` diretiva.

Exigindo uma função:

```csharp
@attribute [Authorize(Roles ="administrators")]
```

Exigir que uma política seja satisfeita:

```csharp
@attribute [Authorize(Policy ="CanadiansOnly")]
```

Se você precisar acessar o estado de autenticação, as funções ou as declarações de um usuário em seu código, há duas maneiras principais de fazer isso. A primeira é receber o estado de autenticação como um parâmetro em cascata. A segunda é acessar o estado usando um injetado `AuthenticationStateProvider` . Os detalhes de cada uma dessas abordagens são descritos na [ Blazor documentação de segurança](https://docs.microsoft.com/aspnet/core/blazor/security/).

O código a seguir mostra como receber o `AuthenticationState` como um parâmetro em cascata:

```csharp
[CascadingParameter]
private Task<AuthenticationState> authenticationStateTask { get; set; }
```

Com esse parâmetro em vigor, você pode obter o usuário usando este código:

```csharp
var authState = await authenticationStateTask;
var user = authState.User;
```

O código a seguir mostra como injetar `AuthenticationStateProvider` :

```csharp
@using Microsoft.AspNetCore.Components.Authorization
@inject AuthenticationStateProvider AuthenticationStateProvider
```

Com o provedor em vigor, você pode obter acesso ao usuário com o seguinte código:

```csharp
AuthenticationState authState = await AuthenticationStateProvider.GetAuthenticationStateAsync();
ClaimsPrincipal user = authState.User;

if (user.Identity.IsAuthenticated)
{
  // work with user.Claims and/or user.Roles
}
```

**Observação:** O `AuthorizeView` componente, abordado mais adiante neste capítulo, fornece uma maneira declarativa de controlar o que um usuário vê em uma página ou componente.

Para trabalhar com usuários e declarações (em Blazor aplicativos de servidor), talvez você também precise injetar um `UserManager<T>` (use `IdentityUser` para o padrão), que pode ser usado para enumerar e modificar declarações para um usuário. Primeiro, insira o tipo e atribua-o a uma propriedade:

```csharp
@inject UserManager<IdentityUser> MyUserManager
```

Em seguida, use-o para trabalhar com as declarações do usuário. O exemplo a seguir mostra como adicionar e manter uma declaração em um usuário:

```csharp
private async Task AddCountryClaim()
{
    var authState = await AuthenticationStateProvider.GetAuthenticationStateAsync();
    var user = authState.User;
    var identityUser = await MyUserManager.FindByNameAsync(user.Identity.Name);

    if (!user.HasClaim(c => c.Type == ClaimTypes.Country))
    {
        // stores the claim in the cookie
        ClaimsIdentity id = new ClaimsIdentity();
        id.AddClaim(new Claim(ClaimTypes.Country, "Canada"));
        user.AddIdentity(id);

        // save the claim in the database
        await MyUserManager.AddClaimAsync(identityUser, new Claim(ClaimTypes.Country, "Canada"));
    }
}
```

Se você precisar trabalhar com funções, siga a mesma abordagem. Talvez seja necessário injetar um `RoleManager<T>` (usar `IdentityRole` para o tipo padrão) para listar e gerenciar as funções em si.

**Observação:** Em Blazor projetos Webassembly, você precisará fornecer APIs de servidor para executar essas operações (em vez de usar `UserManager<T>` ou `RoleManager<T>` diretamente). Um Blazor aplicativo cliente Webassembly gerenciaria declarações e/ou funções chamando com segurança pontos de extremidade de API expostos para essa finalidade.

### <a name="migration-guide"></a>Guia de migração

Migrar do ASP.NET Web Forms e provedores universais para ASP.NET Core identidade requer várias etapas:

1. Criar ASP.NET Core esquema de banco de dados de identidade no banco de dados de destino
2. Migrar dados do esquema do provedor universal para ASP.NET Core esquema de identidade
3. Migre a configuração de web.config para middleware e serviços, normalmente em `Startup.cs`
4. Atualize páginas individuais usando controles e condicionais para usar auxiliares de marca e novas APIs de identidade.

Cada uma dessas etapas é descrita mais detalhadamente nas seções a seguir.

### <a name="creating-the-aspnet-core-identity-schema"></a>Criando o esquema de identidade ASP.NET Core

Há várias maneiras de criar a estrutura de tabela necessária usada para ASP.NET Core identidade. A mais simples é criar um novo aplicativo Web ASP.NET Core. Escolha aplicativo Web e, em seguida, altere a autenticação para usar contas de usuário individuais.

![novo projeto com contas de usuário individuais](./media/security/individual-user-accounts.png)

Na linha de comando, você pode fazer a mesma coisa executando `dotnet new webapp -au Individual` . Depois que o aplicativo tiver sido criado, execute-o e registre-se no site. Você deve disparar uma página como a mostrada abaixo:

![página aplicar migrações](./media/security/apply-migrations-page.png)

Clique no botão "aplicar migrações" e as tabelas de banco de dados necessárias devem ser criadas para você. Além disso, os arquivos de migração devem aparecer em seu projeto, conforme mostrado:

![arquivos de migração](./media/security/migration-files.png)

Você pode executar a migração por conta própria, sem executar o aplicativo Web, usando essa ferramenta de linha de comando:

```powershell
dotnet ef database update
```

Se você preferir executar um script para aplicar o novo esquema a um banco de dados existente, poderá gerar script dessas migrações na linha de comando. Execute este comando para gerar o script:

```powershell
dotnet ef migrations script -o auth.sql
```

Isso produzirá um script SQL no arquivo de saída `auth.sql` que pode ser executado em qualquer banco de dados que você desejar. Se você tiver problemas para executar `dotnet ef` comandos, [Verifique se você tem as ferramentas de EF Core instaladas no sistema](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).

Caso você tenha colunas adicionais em suas tabelas de origem, será necessário identificar o melhor local para essas colunas no novo esquema. Em geral, as colunas encontradas na `aspnet_Membership` tabela devem ser mapeadas para a `AspNetUsers` tabela. As colunas em `aspnet_Roles` devem ser mapeadas para `AspNetRoles` . Todas as colunas adicionais na `aspnet_UsersInRoles` tabela seriam adicionadas à `AspNetUserRoles` tabela.

Também vale a pena considerar colocar qualquer coluna adicional em tabelas separadas, para que as migrações futuras não precisem levar em conta essas personalizações do esquema de identidade padrão.

### <a name="migrating-data-from-universal-providers-to-aspnet-core-identity"></a>Migrando dados de provedores universais para identidade ASP.NET Core

Quando você tiver o esquema da tabela de destino em vigor, a próxima etapa será migrar seus registros de usuário e função para o novo esquema. Uma lista completa das diferenças de esquema, incluindo quais colunas são mapeadas para quais novas colunas podem ser encontradas [aqui](https://docs.microsoft.com/aspnet/core/migration/proper-to-2x/membership-to-core-identity).

Para migrar os usuários da associação às novas tabelas de identidade, você deve [seguir as etapas descritas na documentação do](https://docs.microsoft.com/aspnet/core/migration/proper-to-2x/membership-to-core-identity). Depois de seguir essas etapas e o script fornecido, os usuários precisarão alterar sua senha na próxima vez que fizerem logon.

É possível migrar senhas de usuário, mas o processo é muito mais envolvido. Exigir que os usuários atualizem suas senhas como parte do processo de migração e incentivando-as a usar senhas novas e exclusivas, provavelmente aumentará a segurança geral do aplicativo.

### <a name="migrating-security-settings-from-webconfig-to-startupcs"></a>Migrando configurações de segurança de web.config para Startup.cs

Conforme observado acima, a associação de ASP.NET e os provedores de função são configurados no arquivo de web.config do aplicativo. Como os aplicativos ASP.NET Core não estão vinculados ao IIS e usam um sistema separado para configuração, essas configurações devem ser configuradas em outro lugar. Para a maior parte, ASP.NET Core identidade é configurada no `Startup.cs` arquivo. Abra o projeto Web que foi criado anteriormente (para gerar o esquema da tabela de identidade) e examine seu `Startup.cs` arquivo.

O método configureservices padrão adiciona suporte para EF Core e identidade:

```csharp
// This method gets called by the runtime. Use this method to add services to the container.
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(
            Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<IdentityUser>(options => options.SignIn.RequireConfirmedAccount = true)
        .AddEntityFrameworkStores<ApplicationDbContext>();

    services.AddRazorPages();
}
```

O `AddDefaultIdentity` método de extensão é usado para configurar a identidade para usar o padrão `ApplicationDbContext` e o `IdentityUser` tipo da estrutura. Se você estiver usando um personalizado `IdentityUser` , certifique-se de especificar seu tipo aqui. Se esses métodos de extensão não estiverem funcionando em seu aplicativo, verifique se você tem as instruções de uso apropriadas e se você tem as referências de pacote NuGet necessárias. Por exemplo, seu projeto deve ter alguma versão dos `Microsoft.AspNetCore.Identity.EntityFrameworkCore` pacotes e `Microsoft.AspNetCore.Identity.UI` referenciados.

Além disso `Startup.cs` , você deve ver o middleware necessário configurado para o site. Especificamente, `UseAuthentication` e `UseAuthorization` deve ser configurado e no local apropriado.

```csharp

// This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseDatabaseErrorPage();
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

    app.UseAuthentication();
    app.UseAuthorization();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapRazorPages();
    });
}
```

ASP.NET Identity não configura o acesso anônimo ou baseado em função a locais do `Startup.cs` . Você precisará migrar quaisquer dados de configuração de autorização específicos do local para filtros no ASP.NET Core. Anote quais pastas e páginas exigirão essas atualizações. Você fará essas alterações na próxima seção.

### <a name="updating-individual-pages-to-use-aspnet-core-identity-abstractions"></a>Atualizando páginas individuais para usar ASP.NET Core abstrações de identidade

No aplicativo ASP.NET Web Forms, se você tiver web.config configurações para negar acesso a determinadas páginas ou pastas a usuários anônimos, você migrará esses arquivos simplesmente adicionando o `[Authorize]` atributo a essas páginas:

```razor
@attribute [Authorize]
```

Se você tiver mais acesso negado, exceto para os usuários que pertencem a uma determinada função, você migraria da mesma forma adicionando um atributo especificando uma função:

```razor
@attribute [Authorize(Roles ="administrators")]
```

Observe que o `[Authorize]` atributo só funciona em `@page` componentes que são alcançados por meio do Blazor roteador. O atributo não funciona com componentes filho que, em vez disso, devem usar `AuthorizeView` .

Se você tiver lógica dentro da marcação de página para determinar se um código deve ser exibido para um determinado usuário, você poderá substituí-lo pelo `AuthorizeView` componente. O [componente AuthorizeView](https://docs.microsoft.com/aspnet/core/blazor/security#authorizeview-component) exibe de forma seletiva a interface do usuário, dependendo se ele está autorizado a vê-la. Ele também expõe uma `context` variável que pode ser usada para acessar informações do usuário.

```razor
<AuthorizeView>
    <Authorized>
        <h1>Hello, @context.User.Identity.Name!</h1>
        <p>You can only see this content if you are authenticated.</p>
    </Authorized>
    <NotAuthorized>
        <h1>Authentication Failure!</h1>
        <p>You are not signed in.</p>
    </NotAuthorized>
</AuthorizeView>
```

Você pode acessar o estado de autenticação na lógica de procedimento acessando o usuário de um `Task<AuthenticationState` configurado com o `[CascadingParameter]` atributo. Isso fará com que você tenha acesso ao usuário, o que pode permitir que você determine se eles são autenticados e se eles pertencem a uma função específica. Se você precisar avaliar uma política com um procedimento, poderá injetar uma instância do `IAuthorizationService` e chamar o `AuthorizeAsync` método nele. O código de exemplo a seguir demonstra como obter informações do usuário e permitir que um usuário autorizado execute uma tarefa restrita pela `content-editor` política.

```razor
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService

<button @onclick="@DoSomething">Do something important</button>

@code {
    [CascadingParameter]
    private Task<AuthenticationState> authenticationStateTask { get; set; }

    private async Task DoSomething()
    {
        var user = (await authenticationStateTask).User;

        if (user.Identity.IsAuthenticated)
        {
            // Perform an action only available to authenticated (signed-in) users.
        }

        if (user.IsInRole("admin"))
        {
            // Perform an action only available to users in the 'admin' role.
        }

        if ((await AuthorizationService.AuthorizeAsync(user, "content-editor"))
            .Succeeded)
        {
            // Perform an action only available to users satisfying the
            // 'content-editor' policy.
        }
    }
}
```

O `AuthenticationState` primeiro precisa ser configurado como um valor em cascata antes que possa ser associado a um parâmetro em cascata como este. Isso normalmente é feito usando o `CascadingAuthenticationState` componente. Normalmente, isso é feito em `App.razor` :

```razor
<CascadingAuthenticationState>
    <Router AppAssembly="@typeof(Program).Assembly">
        <Found Context="routeData">
            <AuthorizeRouteView RouteData="@routeData"
                DefaultLayout="@typeof(MainLayout)" />
        </Found>
        <NotFound>
            <LayoutView Layout="@typeof(MainLayout)">
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </NotFound>
    </Router>
</CascadingAuthenticationState>
```

## <a name="summary"></a>Resumo

Blazor usa o mesmo modelo de segurança que ASP.NET Core, que é ASP.NET Core identidade. A migração de provedores universais para ASP.NET Core identidade é relativamente simples, supondo que uma personalização muito grande foi aplicada ao esquema de dados original. Depois que os dados são migrados, trabalhar com autenticação e autorização em Blazor aplicativos é bem documentado, com suporte configurável e programático para a maioria dos requisitos de segurança.

## <a name="references"></a>Referências

- [Introdução à identidade no ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/authentication/identity)
- [Migrar da autenticação de associação do ASP.NET para a identidade do ASP.NET Core 2,0](https://docs.microsoft.com/aspnet/core/migration/proper-to-2x/membership-to-core-identity)
- [Migrar autenticação e identidade para ASP.NET Core](https://docs.microsoft.com/aspnet/core/migration/identity)
- [BlazorAutenticação e autorização do ASP.NET Core](https://docs.microsoft.com/aspnet/core/blazor/security/)

>[!div class="step-by-step"]
>[Anterior](config.md) 
> [Avançar](migration.md)
