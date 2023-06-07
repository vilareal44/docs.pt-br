---
title: IdentityServer para aplicativos nativos de nuvem
description: Arquitetando aplicativos .NET nativos da nuvem para o Azure | IdentityServer
ms.date: 05/13/2020
ms.openlocfilehash: 2128001f0d25b1edd795dd9676e0d76018c1fa3a
ms.sourcegitcommit: ee5b798427f81237a3c23d1fd81fff7fdc21e8d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84144364"
---
# <a name="identityserver-for-cloud-native-applications"></a>IdentityServer para aplicativos nativos de nuvem

IdentityServer é um servidor de autenticação de software livre que implementa os padrões do OIDC (OpenID Connect) e do OAuth 2,0 para ASP.NET Core. Ele foi projetado para fornecer uma maneira comum de autenticar solicitações para todos os seus aplicativos, sejam eles Web, nativos, móveis ou pontos de extremidade de API. O IdentityServer pode ser usado para implementar o SSO (logon único) para vários aplicativos e tipos de aplicativos. Ele pode ser usado para autenticar usuários reais por meio de formulários de entrada e interfaces de usuário semelhantes, bem como a autenticação baseada em serviço que normalmente envolve a emissão de tokens, a verificação e a renovação sem qualquer interface do usuário. O IdentityServer foi projetado para ser uma solução personalizável. Cada instância é normalmente personalizada para atender a uma organização individual e/ou a um conjunto de necessidades de aplicativos.

## <a name="common-web-app-scenarios"></a>Cenários comuns de aplicativos Web

Normalmente, os aplicativos precisam dar suporte a alguns ou todos os seguintes cenários:

- Usuários humanos que acessam aplicativos Web com um navegador.
- Usuários humanos que acessam APIs Web de back-end de aplicativos baseados em navegador.
- Usuários humanos em clientes móveis/nativos que acessam APIs Web de back-end.
- Outros aplicativos que acessam APIs Web de back-end (sem uma interface do usuário ou usuário ativo).
- Qualquer aplicativo pode precisar interagir com outras APIs da Web, usando sua própria identidade ou delegando para a identidade do usuário.

![Tipos e cenários de aplicativos](./media/application-types.png)

**Figura 8-1**. Tipos de aplicativos e cenários.

Em cada um desses cenários, a funcionalidade exposta precisa ser protegida contra uso não autorizado. No mínimo, isso normalmente requer a autenticação do usuário ou entidade de segurança que faz uma solicitação para um recurso. Essa autenticação pode usar um dos vários protocolos comuns, como SAML2p, WS-enalimentado ou OpenID Connect. A comunicação com APIs normalmente usa o protocolo OAuth2 e seu suporte para tokens de segurança. Separar essas preocupações críticas de segurança abrangentes e seus detalhes de implementação dos próprios aplicativos garante a consistência e melhora a segurança e a facilidade de manutenção. Terceirizar essas preocupações com um produto dedicado, como o IdentityServer, ajuda o requisito para cada aplicativo resolver esses problemas em si.

O IdentityServer fornece middleware que é executado em um aplicativo ASP.NET Core e adiciona suporte para OpenID Connect e OAuth2 (consulte [especificações com suporte](https://docs.identityserver.io/en/latest/intro/specs.html)). As organizações criarão seu próprio aplicativo de ASP.NET Core usando o middleware IdentityServer para atuar como o STS para todos os seus protocolos de segurança baseados em token. O middleware IdentityServer expõe pontos de extremidade para dar suporte à funcionalidade padrão, incluindo:

- Autorizar (autenticar o usuário final)
- Token (solicitar um token programaticamente)
- Descoberta (metadados sobre o servidor)
- Informações do usuário (obter informações do usuário com um token de acesso válido)
- Autorização do dispositivo (usada para iniciar a autorização do fluxo do dispositivo)
- Introspecção (validação de token)
- Revogação (revogação de token)
- Encerrar sessão (disparar saída única em todos os aplicativos)

## <a name="getting-started"></a>Introdução

O IdentityServer4 é de código-fonte aberto e gratuito para uso. Você pode adicioná-lo aos seus aplicativos usando seus pacotes NuGet. O pacote principal é [IdentityServer4](https://www.nuget.org/packages/IdentityServer4/) que foi baixado em mais de 4 milhões vezes. O pacote base não inclui nenhum código de interface do usuário e só dá suporte à configuração da memória. Para usá-lo com um banco de dados, você também desejará um provedor de data como o [IdentityServer4. EntityFramework](https://www.nuget.org/packages/IdentityServer4.EntityFramework) que usa Entity Framework Core para armazenar a configuração e os dados operacionais do IdentityServer. Para a interface do usuário, você pode copiar arquivos do [repositório de IU de início rápido](https://github.com/IdentityServer/IdentityServer4.Quickstart.UI) para seu aplicativo ASP.NET Core MVC para adicionar suporte para entrar e sair usando o middleware IdentityServer.

## <a name="configuration"></a>Configuração

O IdentityServer dá suporte a diferentes tipos de protocolos e provedores de autenticação social que podem ser configurados como parte de cada instalação personalizada. Isso normalmente é feito na classe do aplicativo ASP.NET Core `Startup` no `ConfigureServices` método. A configuração envolve especificar os protocolos com suporte e os caminhos para os servidores e pontos de extremidade que serão usados. A Figura 8-2 mostra uma configuração de exemplo obtida do projeto de interface do usuário do IdentityServer4 QuickStart:

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddMvc();

        // some details omitted
        services.AddIdentityServer();

          services.AddAuthentication()
            .AddGoogle("Google", options =>
            {
                options.SignInScheme = IdentityServerConstants.ExternalCookieAuthenticationScheme;

                options.ClientId = "<insert here>";
                options.ClientSecret = "<insert here>";
            })
            .AddOpenIdConnect("demoidsrv", "IdentityServer", options =>
            {
                options.SignInScheme = IdentityServerConstants.ExternalCookieAuthenticationScheme;
                options.SignOutScheme = IdentityServerConstants.SignoutScheme;

                options.Authority = "https://demo.identityserver.io/";
                options.ClientId = "implicit";
                options.ResponseType = "id_token";
                options.SaveTokens = true;
                options.CallbackPath = new PathString("/signin-idsrv");
                options.SignedOutCallbackPath = new PathString("/signout-callback-idsrv");
                options.RemoteSignOutPath = new PathString("/signout-idsrv");

                options.TokenValidationParameters = new TokenValidationParameters
                {
                    NameClaimType = "name",
                    RoleClaimType = "role"
                };
            });
    }
}
```

**Figura 8-2**. Configurando o IdentityServer.

O IdentityServer também hospeda um site de demonstração pública que pode ser usado para testar vários protocolos e configurações. Ele está localizado em [https://demo.identityserver.io/](https://demo.identityserver.io/) e inclui informações sobre como configurar seu comportamento com base no `client_id` fornecido a ele.

## <a name="javascript-clients"></a>Clientes JavaScript

Muitos aplicativos nativos de nuvem aproveitam as APIs do lado do servidor e os aplicativos de página única (SPAs) de cliente avançado no front-end. O IdentityServer envia um [cliente JavaScript](https://docs.identityserver.io/en/latest/quickstarts/4_javascript_client.html) ( `oidc-client.js` ) por meio de NPM que pode ser adicionado ao spas para permitir que eles usem o IdentityServer para entrar, sair e autenticação baseada em token de APIs da Web.

## <a name="references"></a>Referências

- [Documentação do IdentityServer](https://docs.identityserver.io/en/latest/)
- [Tipos de aplicativos](https://docs.microsoft.com/azure/active-directory/develop/app-types)
- [Cliente OIDC do JavaScript](https://docs.identityserver.io/en/latest/quickstarts/4_javascript_client.html)

>[!div class="step-by-step"]
>[Anterior](azure-active-directory.md) 
> [Avançar](security.md)
