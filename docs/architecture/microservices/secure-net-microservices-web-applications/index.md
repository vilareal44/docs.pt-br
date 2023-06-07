---
title: Protegendo microsserviços e aplicativos Web .NET
description: Segurança nos Microsserviços do .NET e aplicativos Web – Conheça as opções de autenticação em aplicativos Web ASP.NET Core.
author: mjrousos
ms.date: 08/07/2020
ms.openlocfilehash: 1dcdb5d2987360ac583fa700a387d977f498d1d9
ms.sourcegitcommit: cbb19e56d48cf88375d35d0c27554d4722761e0d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88608089"
---
# <a name="make-secure-net-microservices-and-web-applications"></a>Proteger microsserviços .NET e aplicativos Web

Há muitos aspectos sobre segurança em microserviços e aplicativos Web que o tópico poderia facilmente pegar vários livros como este. Portanto, nesta seção, nos concentraremos na autenticação, na autorização e nos segredos do aplicativo.

## <a name="implement-authentication-in-net-microservices-and-web-applications"></a>Implementar a autenticação em microsserviços .NET e aplicativos Web

Geralmente é necessário que os recursos e as APIs publicados por um serviço sejam limitados a determinados usuários ou clientes confiáveis. A primeira etapa para tomar esses tipos de decisões de confiança no nível da API é a autenticação. A autenticação é o processo de verificar a identidade de um usuário de forma confiável.

Em cenários de microsserviço, a autenticação é normalmente feita de forma centralizada. Se você estiver usando um Gateway de API, o gateway será um bom lugar para fazer a autenticação, conforme é mostrado na Figura 9-1. Se você usar esta abordagem, verifique se os microsserviços individuais não podem ser acessados diretamente (sem o Gateway de API), a não ser que haja uma segurança adicional em vigor para autenticar mensagens que entram ou não pelo gateway.

![Diagrama mostrando como o aplicativo móvel cliente interage com o back-end.](./media/index/api-gateway-centralized-authentication.png)

**Figura 9-1**. Autenticação centralizada com um Gateway de API

Quando o Gateway de API centraliza a autenticação, ele adiciona informações do usuário ao encaminhar solicitações para os microsserviços. Se os serviços puderem ser acessados diretamente, um serviço de autenticação, como o Azure Active Directory ou um microsserviço de autenticação dedicado atuando como um serviço de token de segurança (STS), poderá ser usado para autenticar os usuários. As decisões de confiança são compartilhadas entre os serviços com tokens de segurança ou cookies. (Esses tokens podem ser compartilhados entre ASP.NET Core aplicativos, se necessário, implementando o [compartilhamento de cookies](/aspnet/core/security/cookie-sharing).) Esse padrão é ilustrado na Figura 9-2.

![Diagrama mostrando a autenticação por meio de microserviços de back-end.](./media/index/identity-microservice-authentication.png)

**Figura 9-2**. Autenticação por microsserviço de identidade; a confiança é compartilhada usando um token de autorização

Quando os microsserviços são acessados diretamente, a confiança, que inclui autenticação e autorização, é tratada por um token de segurança emitido por um microsserviço dedicado, compartilhado entre os microsserviços.

### <a name="authenticate-with-aspnet-core-identity"></a>Autenticar usando o ASP.NET Core Identity

O mecanismo principal no ASP.NET Core para identificar os usuários de um aplicativo é o sistema de associação de [identidade ASP.NET Core](/aspnet/core/security/authentication/identity) . A Identidade do ASP.NET Core armazena informações de usuário (incluindo informações de logon, funções e declarações) em um repositório de dados configurado pelo desenvolvedor. Normalmente, o armazenamento de dados do ASP.NET Core Identity é um repositório do Entity Framework fornecido no pacote `Microsoft.AspNetCore.Identity.EntityFrameworkCore`. No entanto, os repositórios personalizados ou outros pacotes de terceiros podem ser usados para armazenar informações de identidade no Armazenamento de Tabelas do Azure, no CosmosDB ou em outros locais.

> [!TIP]
> ASP.NET Core 2,1 e posterior fornece [ASP.NET Core identidade](/aspnet/core/security/authentication/identity) como uma [biblioteca de classes Razor](/aspnet/core/razor-pages/ui-class), de modo que você não verá grande parte do código necessário em seu projeto, como foi o caso de versões anteriores. Para obter detalhes sobre como personalizar o código de identidade para atender às suas necessidades, consulte [Scaffold Identity in ASP.NET Core Projects](/aspnet/core/security/authentication/scaffold-identity).

O código a seguir é obtido do modelo de projeto ASP.NET Core aplicativo Web MVC 3,1 com a autenticação de conta de usuário individual selecionada. Ele mostra como configurar ASP.NET Core identidade usando Entity Framework Core no `Startup.ConfigureServices` método.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    //...
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(
            Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<IdentityUser>(options => options.SignIn.RequireConfirmedAccount = true)
        .AddEntityFrameworkStores<ApplicationDbContext>();

    services.AddRazorPages();
    //...
}
```

Quando ASP.NET Core identidade estiver configurada, você a habilitará adicionando o `app.UseAuthentication()` e `endpoints.MapRazorPages()` conforme mostrado no código a seguir no método do serviço `Startup.Configure` :

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    //...
    app.UseRouting();

    app.UseAuthentication();
    app.UseAuthorization();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapRazorPages();
    });
    //...
}
```

> [!IMPORTANT]
> As linhas no código anterior **devem estar na ordem mostrada** para que a identidade funcione corretamente.

Usar o ASP.NET Core Identity permite vários cenários:

- Criar informações de novo usuário usando o tipo UserManager (userManager.CreateAsync).

- Autenticar usuários usando o tipo SignInManager. Você pode usar o `signInManager.SignInAsync` para entrar diretamente ou `signInManager.PasswordSignInAsync` para confirmar se a senha do usuário está correta e, em seguida, conectá-la.

- Identifique um usuário com base nas informações armazenadas em um cookie (que é lido por ASP.NET Core middleware de identidade) para que as solicitações subsequentes de um navegador incluam uma identidade e declarações do usuário conectado.

O ASP.NET Core Identity também dá suporte à [autenticação de dois fatores](/aspnet/core/security/authentication/2fa).

Para cenários de autenticação que usam um armazenamento de dados de usuário local e que persistem a identidade entre solicitações usando cookies (como é comum para aplicativos Web MVC), o ASP.NET Core Identity é uma solução recomendada.

### <a name="authenticate-with-external-providers"></a>Autenticar com provedores externos

O ASP.NET Core também dá suporte ao uso de [provedores de autenticação externos](/aspnet/core/security/authentication/social/) para permitir que os usuários entrem por meio de fluxos [OAuth 2.0](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2). Isso significa que os usuários podem entrar usando os processos de autenticação existentes de provedores como Microsoft, Google, Facebook ou Twitter e associar essas identidades a um ASP.NET Core Identity no seu aplicativo.

Para usar a autenticação externa, além de incluir o middleware de autenticação, conforme mencionado anteriormente, usando o `app.UseAuthentication()` método, você também precisa registrar o provedor externo no `Startup` , conforme mostrado no exemplo a seguir:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    //...
    services.AddDefaultIdentity<IdentityUser>(options => options.SignIn.RequireConfirmedAccount = true)
        .AddEntityFrameworkStores<ApplicationDbContext>();

    services.AddAuthentication()
        .AddMicrosoftAccount(microsoftOptions =>
        {
            microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ClientId"];
            microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:ClientSecret"];
        })
        .AddGoogle(googleOptions => { ... })
        .AddTwitter(twitterOptions => { ... })
        .AddFacebook(facebookOptions => { ... });
    //...
}
```

Os provedores de autenticação externos populares e seus pacotes NuGet associados são mostrados na tabela a seguir:

| **Provedor**  | **Pacote**                                          |
| ------------- | ---------------------------------------------------- |
| **Microsoft** | **Microsoft.AspNetCore.Authentication.MicrosoftAccount** |
| **Google**    | **Microsoft.AspNetCore.Authentication.Google**           |
| **Facebook**  | **Microsoft.AspNetCore.Authentication.Facebook**         |
| **Twitter**   | **Microsoft.AspNetCore.Authentication.Twitter**          |

Em todos os casos, você deve concluir um procedimento de registro de aplicativo que é dependente do fornecedor e que geralmente envolve:

1. Obtendo uma ID do aplicativo cliente.
2. Obtendo um segredo do aplicativo cliente.
3. Configurando uma URL de redirecionamento, que é tratada pelo middleware de autorização e pelo provedor registrado
4. Opcionalmente, a configuração de uma URL de saída para tratar corretamente a saída em um cenário de logon único (SSO).

Para obter detalhes sobre como configurar seu aplicativo para um provedor externo, consulte a [autenticação do provedor externo na documentação do ASP.NET Core](/aspnet/core/security/authentication/social/)).

>[!TIP]
>Todos os detalhes são tratados pelo middleware de autorização e pelos serviços mencionados anteriormente. Portanto, basta escolher a opção de autenticação de **conta de usuário individual** ao criar o projeto de aplicativo Web de código ASP.net no Visual Studio, como mostrado na Figura 9-3, além de registrar os provedores de autenticação mencionados anteriormente.

![Captura de tela da caixa de diálogo novo ASP.NET Core aplicativo Web.](./media/index/select-individual-user-account-authentication-option.png)

**Figura 9-3**. Selecionando a opção contas de usuário individuais, para usar a autenticação externa, ao criar um projeto de aplicativo Web no Visual Studio 2019.

Além dos provedores de autenticação externos listados anteriormente, estão disponíveis pacotes de terceiros que fornecem middleware para o uso de vários outros provedores de autenticação externos. Para obter uma lista, consulte o repositório [ASPNET. Security. OAuth. Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers/tree/dev/src) no github.

Você também pode criar seu próprio middleware de autenticação externa para resolver alguma necessidade especial.

### <a name="authenticate-with-bearer-tokens"></a>Autenticar com tokens de portador

Autenticar com o ASP.NET Core Identity (ou com Identity e também com provedores de autenticação externos) funciona bem para vários cenários de aplicativo Web nos quais é apropriado armazenar informações do usuário em um cookie. Em outros cenários, no entanto, os cookies não são uma maneira natural de persistir e transmitir dados.

Por exemplo, em uma API Web do ASP.NET Core que expõe pontos de extremidade RESTful que podem ser acessados por SPAs (Aplicativos de Única Página), por clientes nativos ou até mesmo por outras APIs Web, geralmente é melhor usar a autenticação de token de portador. Esses tipos de aplicativos não funcionam com cookies, mas podem facilmente recuperar um token de portador e incluí-lo no cabeçalho de autorização das próximas solicitações. Para habilitar a autenticação de token, o ASP.NET Core dá suporte a várias opções para usar [OAuth 2.0](https://oauth.net/2/) e [OpenID Connect](https://openid.net/connect/).

### <a name="authenticate-with-an-openid-connect-or-oauth-20-identity-provider"></a>Autenticar com um provedor de identidade OAuth 2.0 ou OpenID Connect

Se as informações de usuário estiverem armazenadas no Azure Active Directory ou em outra solução de identidade que dê suporte a OpenID Connect ou OAuth 2.0, você poderá usar o pacote **Microsoft.AspNetCore.Authentication.OpenIdConnect** para autenticar usando o fluxo de trabalho do OpenID Connect. Por exemplo, para autenticar no microsserviço Identity.Api em eShopOnContainers, um aplicativo Web do ASP.NET Core pode usar o middleware desse pacote, conforme mostrado no seguinte exemplo simplificado em `Startup.cs`:

```csharp
// Startup.cs

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    //…
    app.UseAuthentication();
    //…
    app.UseEndpoints(endpoints =>
    {
        //...
    });
}

public void ConfigureServices(IServiceCollection services)
{
    var identityUrl = Configuration.GetValue<string>("IdentityUrl");
    var callBackUrl = Configuration.GetValue<string>("CallBackUrl");
    var sessionCookieLifetime = Configuration.GetValue("SessionCookieLifetimeMinutes", 60);

    // Add Authentication services

    services.AddAuthentication(options =>
    {
        options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;
    })
    .AddCookie(setup => setup.ExpireTimeSpan = TimeSpan.FromMinutes(sessionCookieLifetime))
    .AddOpenIdConnect(options =>
    {
        options.SignInScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        options.Authority = identityUrl.ToString();
        options.SignedOutRedirectUri = callBackUrl.ToString();
        options.ClientId = useLoadTest ? "mvctest" : "mvc";
        options.ClientSecret = "secret";
        options.ResponseType = useLoadTest ? "code id_token token" : "code id_token";
        options.SaveTokens = true;
        options.GetClaimsFromUserInfoEndpoint = true;
        options.RequireHttpsMetadata = false;
        options.Scope.Add("openid");
        options.Scope.Add("profile");
        options.Scope.Add("orders");
        options.Scope.Add("basket");
        options.Scope.Add("marketing");
        options.Scope.Add("locations");
        options.Scope.Add("webshoppingagg");
        options.Scope.Add("orders.signalrhub");
    });
}
```

Observe que quando você usa este fluxo de trabalho, o middleware do ASP.NET Core Identity não é necessário, porque todo o armazenamento de informações de usuário e toda a autenticação são manipulados pelo serviço de identidade.

### <a name="issue-security-tokens-from-an-aspnet-core-service"></a>Emitir tokens de segurança de um serviço do ASP.NET Core

Se preferir emitir tokens de segurança para usuários do ASP.NET Core Identity local em vez de usar um provedor de identidade externo, você poderá usufruir de algumas boas bibliotecas de terceiros.

[IdentityServer4](https://github.com/IdentityServer/IdentityServer4) e [OpenIddict](https://github.com/openiddict/openiddict-core) são provedores do OpenID Connect que se integram facilmente ao ASP.NET Core Identity para permitir que você emita tokens de segurança de um serviço do ASP.NET Core. A [IdentityServer4 documentation](https://identityserver4.readthedocs.io/en/latest/) (Documentação do IdentityServer4) tem instruções detalhadas para usar a biblioteca. No entanto, as etapas básicas para usar o IdentityServer4 para emitir tokens são as seguintes.

1. Você chama o aplicativo. UseIdentityServer no método Startup.Configlhar para adicionar IdentityServer4 ao pipeline de processamento de solicitação HTTP do aplicativo. Isso permite que a biblioteca atenda solicitações dos pontos de extremidade do OpenID Connect e do OAuth2 como /connect/token.

2. Você pode configurar o IdentityServer4 no Startup.ConfigureServices fazendo uma chamada para services.AddIdentityServer.

3. Configure o servidor de identidade definindo os seguintes dados:

   - As [credenciais](https://identityserver4.readthedocs.io/en/latest/topics/crypto.html) a serem usadas para assinar.

   - Os [recursos de identidade e da API](https://identityserver4.readthedocs.io/en/latest/topics/resources.html) aos quais os usuários podem solicitar acesso:

      - Os recursos da API representam dados ou funcionalidades protegidos que o usuário pode acessar com um token de acesso. Um exemplo de um recurso da API seria uma API Web (ou conjunto de APIs) que exige autorização.

      - Os recursos de identidade representam informações (declarações) que são concedidas a um cliente para identificar um usuário. As declarações podem incluir o nome de usuário, o endereço de email e assim por diante.

   - Os [clientes](https://identityserver4.readthedocs.io/en/latest/topics/clients.html) que se conectarão para solicitar tokens.

   - O mecanismo de armazenamento de informações do usuário, como o [ASP.NET Core Identity](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) ou um outro.

Ao especificar os clientes e os recursos para o IdentityServer4 usar, você pode passar uma coleção <xref:System.Collections.Generic.IEnumerable%601> do tipo apropriado aos métodos que usam os repositórios de clientes ou de recursos na memória. Ou, para cenários mais complexos, você pode fornecer os tipos de provedor de clientes ou de recursos por meio de injeção de dependência.

Um exemplo de configuração para o IdentityServer4 usar recursos e clientes na memória fornecidos por um tipo IClientStore personalizado pode ser semelhante ao exemplo a seguir:

```csharp
public IServiceProvider ConfigureServices(IServiceCollection services)
{
    //...
    services.AddSingleton<IClientStore, CustomClientStore>();
    services.AddIdentityServer()
        .AddSigningCredential("CN=sts")
        .AddInMemoryApiResources(MyApiResourceProvider.GetAllResources())
        .AddAspNetIdentity<ApplicationUser>();
    //...
}
```

### <a name="consume-security-tokens"></a>Consumir tokens de segurança

Autenticar em relação a um ponto de extremidade do OpenID Connect ou emitir seus próprios tokens de segurança cobre alguns cenários. Mas, e um serviço que apenas precisa limitar o acesso a esses usuários que têm tokens de segurança válidos que foram fornecidos por um serviço diferente?

Para esse cenário, o middleware de autenticação que manipula os tokens JWT está disponível no pacote **Microsoft.AspNetCore.Authentication.JwtBearer**. JWT representa "[Token Web JSON](https://tools.ietf.org/html/rfc7519)" e é um formato de token de segurança comum (definido pelo RFC 7519) para comunicação de declarações de segurança. Um exemplo simplificado de como usar o middleware para consumir esses tokens pode parecer com este fragmento de código, tirado do microsserviço Ordering.API de eShopOnContainers.

```csharp
// Startup.cs

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    //…
    // Configure the pipeline to use authentication
    app.UseAuthentication();
    //…
    app.UseEndpoints(endpoints =>
    {
        //...
    });
}

public void ConfigureServices(IServiceCollection services)
{
    var identityUrl = Configuration.GetValue<string>("IdentityUrl");

    // Add Authentication services

    services.AddAuthentication(options =>
    {
        options.DefaultAuthenticateScheme = AspNetCore.Authentication.JwtBearer.JwtBearerDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme = AspNetCore.Authentication.JwtBearer.JwtBearerDefaults.AuthenticationScheme;

    }).AddJwtBearer(options =>
    {
        options.Authority = identityUrl;
        options.RequireHttpsMetadata = false;
        options.Audience = "orders";
    });
}
```

Os parâmetros nesse tipo de uso são:

- `Audience` representa o receptor do token ou do recurso de entrada ao qual o token concede acesso. Se o valor especificado nesse parâmetro não corresponder ao parâmetro no token, o token será rejeitado.

- `Authority` é o endereço do servidor de autenticação que emite o token. O middleware de portador do JWT usa esse URI para obter a chave pública que pode ser usada para validar a assinatura do token. O middleware também confirma se o parâmetro `iss` no token corresponde a esse URI.

Outro parâmetro, `RequireHttpsMetadata`, é útil para testes. Defina esse parâmetro como false para poder testar em ambientes nos quais você não tenha certificados. Em implantações reais, os tokens de portador do JWT sempre devem ser passados apenas por HTTPS.

Com este middleware em vigor, os tokens JWT são extraídos automaticamente dos cabeçalhos de autorização. Eles são desserializados, validados (usando os valores nos parâmetros `Audience` e `Authority`) e armazenados como informações de usuário a serem referenciadas posteriormente por ações de MVC ou filtros de autorização.

O middleware de autenticação de portador do JWT também pode dar suporte a cenários mais avançados, como o uso de um certificado local para validar um token se a autoridade não estiver disponível. Para este cenário, você pode especificar um objeto `TokenValidationParameters` no objeto `JwtBearerOptions`.

## <a name="additional-resources"></a>Recursos adicionais

- **Compartilhando cookies entre aplicativos** \
  [https://docs.microsoft.com/aspnet/core/security/cookie-sharing](/aspnet/core/security/cookie-sharing)

- **Introdução à identidade** \
  [https://docs.microsoft.com/aspnet/core/security/authentication/identity](/aspnet/core/security/authentication/identity)

- **Rick Anderson. Autenticação de dois fatores com SMS** \
  [https://docs.microsoft.com/aspnet/core/security/authentication/2fa](/aspnet/core/security/authentication/2fa)

- **Habilitando a autenticação usando o Facebook, o Google e outros provedores externos** \
  [https://docs.microsoft.com/aspnet/core/security/authentication/social/](/aspnet/core/security/authentication/social/)

- **Michell Anicas. Uma introdução ao OAuth 2** \
  <https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2>

- **AspNet.Security.OAuth.Providers** (repositório do GitHub para provedores do ASP.NET OAuth) \
  <https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers/tree/dev/src>

- **IdentityServer4. Documentação oficial** \
  <https://identityserver4.readthedocs.io/en/latest/>

>[!div class="step-by-step"]
>[Anterior](../implement-resilient-applications/monitor-app-health.md) 
> [Avançar](authorization-net-microservices-web-applications.md)
