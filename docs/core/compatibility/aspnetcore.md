---
title: ASP.NET Core alterações significativas
titleSuffix: ''
description: Lista as alterações significativas em ASP.NET Core.
ms.date: 07/17/2020
author: scottaddie
ms.author: scaddie
ms.openlocfilehash: 7a07df5194d5dc220b61d55a4457d90881ac9ddf
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86474820"
---
# <a name="aspnet-core-breaking-changes"></a>ASP.NET Core alterações significativas

ASP.NET Core fornece os recursos de desenvolvimento de aplicativo Web usados pelo .NET Core.

As seguintes alterações significativas estão documentadas nesta página:

- [APIs antifalsificação obsoletas, CORS, diagnóstico, MVC e roteamento removidas](#obsolete-antiforgery-cors-diagnostics-mvc-and-routing-apis-removed)
- [Autenticação: Google + substituição](#authentication-google-deprecated-and-replaced)
- [Autenticação: Propriedade HttpContext. Authentication removida](#authentication-httpcontextauthentication-property-removed)
- [Autenticação: Newtonsoft.Jsem tipos substituídos](#authentication-newtonsoftjson-types-replaced)
- [Autenticação: assinatura OAuthHandler ExchangeCodeAsync alterada](#authentication-oauthhandler-exchangecodeasync-signature-changed)
- [Autorização: sobrecarga de addautoria movida para um assembly diferente](#authorization-addauthorization-overload-moved-to-different-assembly)
- [Autorização: IAllowAnonymous removido de AuthorizationFilterContext. Filters](#authorization-iallowanonymous-removed-from-authorizationfiltercontextfilters)
- [Autorização: implementações de IAuthorizationPolicyProvider exigem novo método](#authorization-iauthorizationpolicyprovider-implementations-require-new-method)
- [Autorização: o recurso no roteamento de ponto de extremidade é HttpContext](#authorization-resource-in-endpoint-routing-is-httpcontext)
- [Azure: pacotes de integração do Azure prefixados da Microsoft removidos](#azure-microsoft-prefixed-azure-integration-packages-removed)
- [Mais grande: espaço em branco insignificado cortado de componentes em tempo de compilação](#blazor-insignificant-whitespace-trimmed-from-components-at-compile-time)
- [Mais incrivelmente: estrutura de destino de pacotes NuGet alterada](#blazor-target-framework-of-nuget-packages-changed)
- [Caching: Propriedade CompactOnMemoryPressure removida](#caching-compactonmemorypressure-property-removed)
- [Caching: o Microsoft. Extensions. Caching. SqlServer usa o novo pacote SqlClient](#caching-microsoftextensionscachingsqlserver-uses-new-sqlclient-package)
- [Caching: tipos de ResponseCaching "pubternal" alterados para interno](#caching-responsecaching-pubternal-types-changed-to-internal)
- [Proteção de dados: dataprotection. AzureStorage usa novas APIs de armazenamento do Azure](#data-protection-dataprotectionazurestorage-uses-new-azure-storage-apis)
- [Extensões: alterações de referência de pacote que afetam alguns pacotes NuGet](#extensions-package-reference-changes-affecting-some-nuget-packages)
- [Hospedagem: AspNetCoreModule v1 removido do pacote de hospedagem do Windows](#hosting-aspnetcoremodule-v1-removed-from-windows-hosting-bundle)
- [Hospedagem: host genérico restringe injeção de construtor de inicialização](#hosting-generic-host-restricts-startup-constructor-injection)
- [Hospedagem: redirecionamento de HTTPS habilitado para aplicativos fora do processo do IIS](#hosting-https-redirection-enabled-for-iis-out-of-process-apps)
- [Hospedagem: tipos IHostingEnvironment e IApplicationLifetime substituídos](#hosting-ihostingenvironment-and-iapplicationlifetime-types-marked-obsolete-and-replaced)
- [Hospedagem: objectpoolprovider removido de dependências WebHostBuilder](#hosting-objectpoolprovider-removed-from-webhostbuilder-dependencies)
- [Tipos de BadHttpRequestException de HTTP: Kestrel e IIS marcados como obsoletos e substituídos](#http-kestrel-and-iis-badhttprequestexception-types-marked-obsolete-and-replaced)
- [HTTP: navegador SameSite alterações de impacto na autenticação](#http-browser-samesite-changes-impact-authentication)
- [HTTP: extensibilidade defaulthttpcontext removida](#http-defaulthttpcontext-extensibility-removed)
- [Campos HTTP: Headernames alterados para static readonly](#http-headernames-constants-changed-to-static-readonly)
- [HTTP: instâncias de HttpClient criadas por códigos de status de inteiro de log IHttpClientFactory](#http-httpclient-instances-created-by-ihttpclientfactory-log-integer-status-codes)
- [HTTP: alterações de infraestrutura de corpo de resposta](#http-response-body-infrastructure-changes)
- [HTTP: alguns SameSite de cookie foram alterados valores padrão](#http-some-cookie-samesite-defaults-changed-to-none)
- [HTTP: e/s síncrona desabilitada por padrão](#http-synchronous-io-disabled-in-all-servers)
- [HttpS: renegociação de certificado de cliente desabilitada por padrão](#httpsys-client-certificate-renegotiation-disabled-by-default)
- [Identidade: sobrecarga do método AddDefaultUI removida](#identity-adddefaultui-method-overload-removed)
- [Identidade: alteração da versão de inicialização da interface do usuário](#identity-default-bootstrap-version-of-ui-changed)
- [Identidade: o SignInAsync gera uma exceção para a identidade não autenticada](#identity-signinasync-throws-exception-for-unauthenticated-identity)
- [Identity: o Construtor SignInManager aceita o novo parâmetro](#identity-signinmanager-constructor-accepts-new-parameter)
- [Identidade: a interface do usuário usa o recurso de ativos da Web estáticos](#identity-ui-uses-static-web-assets-feature)
- [IIS: cadeias de consulta de middleware UrlRewrite são preservadas](#iis-urlrewrite-middleware-query-strings-are-preserved)
- [Kestrel: alterações de configuração em tempo de execução detectadas por padrão](#kestrel-configuration-changes-at-run-time-detected-by-default)
- [Kestrel: adaptadores de conexão removidos](#kestrel-connection-adapters-removed)
- [Kestrel: versões do protocolo TLS com suporte padrão alteradas](#kestrel-default-supported-tls-protocol-versions-changed)
- [Kestrel: assembly HTTPS vazio removido](#kestrel-empty-https-assembly-removed)
- [Kestrel: HTTP/2 desabilitado via TLS em versões incompatíveis do Windows](#kestrel-http2-disabled-over-tls-on-incompatible-windows-versions)
- [Kestrel: transporte Libuv marcado como obsoleto](#kestrel-libuv-transport-marked-as-obsolete)
- [Kestrel: Solicitar cabeçalhos de trailer movidos para nova coleção](#kestrel-request-trailer-headers-moved-to-new-collection)
- [Kestrel: alterações na camada de abstração de transporte](#kestrel-transport-abstractions-removed-and-made-public)
- [Localização: APIs marcadas como obsoletas](#localization-resourcemanagerwithculturestringlocalizer-and-withculture-marked-obsolete)
- [Localização: APIs "Pubternal" removidas](#localization-pubternal-apis-removed)
- [Localização: Construtor obsoleto removido no middleware de localização de solicitação](#localization-obsolete-constructor-removed-in-request-localization-middleware)
- [Localização: classe ResourceManagerWithCultureStringLocalizer e membro da interface WithCulture removidos](#localization-resourcemanagerwithculturestringlocalizer-class-and-withculture-interface-member-removed)
- [Log: classe DebugLogger criada internamente](#logging-debuglogger-class-made-internal)
- [MVC: sufixo assíncrono de ação do controlador removido](#mvc-async-suffix-trimmed-from-controller-action-names)
- [MVC: JsonResult movido para Microsoft. AspNetCore. Mvc. Core](#mvc-jsonresult-moved-to-microsoftaspnetcoremvccore)
- [MVC: ferramenta de pré-compilação preterida](#mvc-precompilation-tool-deprecated)
- [MVC: tipos alterados para interno](#mvc-pubternal-types-changed-to-internal)
- [MVC: Shim de compatibilidade da API Web removido](#mvc-web-api-compatibility-shim-removed)
- [Razor: compilação de tempo de execução movida para um pacote](#razor-runtime-compilation-moved-to-a-package)
- [Segurança: codificação de nome de cookie removida](#security-cookie-name-encoding-removed)
- [Segurança: versões do pacote NuGet do IdentityModel atualizadas](#security-identitymodel-nuget-package-versions-updated)
- [Estado da sessão: APIs obsoletas removidas](#session-state-obsolete-apis-removed)
- [Estrutura compartilhada: remoção de assembly do Microsoft. AspNetCore. app](#shared-framework-assemblies-removed-from-microsoftaspnetcoreapp)
- [Estrutura compartilhada: Microsoft. AspNetCore. All removidas](#shared-framework-removed-microsoftaspnetcoreall)
- [Signalr: HandshakeProtocol. SuccessHandshakeData substituído](#signalr-handshakeprotocolsuccesshandshakedata-replaced)
- [Signalr: métodos HubConnection removidos](#signalr-hubconnection-resetsendping-and-resettimeout-methods-removed)
- [Signalr: construtores HubConnectionContext alterados](#signalr-hubconnectioncontext-constructors-changed)
- [Signalr: alteração de nome de pacote de cliente JavaScript](#signalr-javascript-client-package-name-changed)
- [Signalr: protocolo Hub MessagePack movido para o pacote MessagePack 2. x](#signalr-messagepack-hub-protocol-moved-to-messagepack-2x-package)
- [Signalr: tipo de opções de protocolo de Hub MessagePack alterado](#signalr-messagepack-hub-protocol-options-type-changed)
- [Signalr: APIs obsoletas](#signalr-usesignalr-and-useconnections-methods-marked-obsolete)
- [Signalr: métodos UseSignalR e UseConnections removidos](#signalr-usesignalr-and-useconnections-methods-removed)
- [SPAs: SpaServices e Nodeservices console agente de fallback alterar padrão](#spas-spaservices-and-nodeservices-no-longer-fall-back-to-console-logger)
- [SPAs: SpaServices e Nodeservices marcados como obsoletos](#spas-spaservices-and-nodeservices-marked-obsolete)
- [Arquivos estáticos: tipo de conteúdo CSV alterado para compatível com padrões](#static-files-csv-content-type-changed-to-standards-compliant)
- [Estrutura de destino: .NET Framework sem suporte](#target-framework-net-framework-support-dropped)

## <a name="aspnet-core-50"></a>ASP.NET Core 5,0

[!INCLUDE[Authorization: Resource in endpoint routing is HttpContext](~/includes/core-changes/aspnetcore/5.0/authorization-resource-in-endpoint-routing.md)]

***

[!INCLUDE[Azure: Microsoft-prefixed Azure integration packages removed](~/includes/core-changes/aspnetcore/5.0/azure-integration-packages-removed.md)]

***

[!INCLUDE[Blazor: Insignificant whitespace trimmed from components at compile time](~/includes/core-changes/aspnetcore/5.0/blazor-components-trim-insignificant-whitespace.md)]

***

[!INCLUDE[Blazor: Target framework of NuGet packages changed](~/includes/core-changes/aspnetcore/5.0/blazor-packages-target-framework-changed.md)]

***

[!INCLUDE[Extensions: Package reference changes](~/includes/core-changes/aspnetcore/5.0/extensions-package-reference-changes.md)]

***

[!INCLUDE[HTTP: HttpClient instances created by IHttpClientFactory log integer status codes](~/includes/core-changes/aspnetcore/5.0/http-httpclient-instances-log-integer-status-codes.md)]

***

[!INCLUDE[HTTP: Kestrel and IIS BadHttpRequestException types marked obsolete and replaced](~/includes/core-changes/aspnetcore/5.0/http-badhttprequestexception-obsolete.md)]

***

[!INCLUDE[HttpSys: Client certificate renegotiation disabled by default](~/includes/core-changes/aspnetcore/5.0/httpsys-client-certificate-renegotiation-disabled-by-default.md)]

***

[!INCLUDE[IIS: UrlRewrite middleware query strings are preserved](~/includes/core-changes/aspnetcore/5.0/iis-urlrewrite-middleware-query-strings-are-preserved.md)]

***

[!INCLUDE[Kestrel: Configuration changes at run time detected by default](~/includes/core-changes/aspnetcore/5.0/kestrel-configuration-changes-at-run-time-detected-by-default.md)]

***
[!INCLUDE[Kestrel: Default supported TLS protocol versions changed](~/includes/core-changes/aspnetcore/5.0/kestrel-default-supported-tls-protocol-versions-changed.md)]

***

[!INCLUDE[Kestrel: HTTP/2 disabled over TLS on incompatible Windows versions](~/includes/core-changes/aspnetcore/5.0/kestrel-disables-http2-over-tls.md)]

***

[!INCLUDE[Kestrel: Libuv transport marked as obsolete](~/includes/core-changes/aspnetcore/5.0/kestrel-libuv-transport-obsolete.md)]

***

[!INCLUDE[Localization: "Pubternal" APIs removed](~/includes/core-changes/aspnetcore/5.0/localization-pubternal-apis-removed.md)]

***

[!INCLUDE[Localization: Obsolete constructor removed in request localization middleware](~/includes/core-changes/aspnetcore/5.0/localization-requestlocalizationmiddleware-constructor-removed.md)]

***

[!INCLUDE[Localization: ResourceManagerWithCultureStringLocalizer class and WithCulture interface member removed](~/includes/core-changes/aspnetcore/5.0/localization-members-removed.md)]

***

[!INCLUDE[Security: Cookie name encoding removed](~/includes/core-changes/aspnetcore/5.0/security-cookie-name-encoding-removed.md)]

***

[!INCLUDE[Security: IdentityModel NuGet package versions updated](~/includes/core-changes/aspnetcore/5.0/security-identitymodel-nuget-package-versions-updated.md)]

***

[!INCLUDE[SignalR: MessagePack Hub Protocol moved to MessagePack 2.x package](~/includes/core-changes/aspnetcore/5.0/signalr-messagepack-package.md)]

***

[!INCLUDE[SignalR: MessagePack Hub Protocol options type changed](~/includes/core-changes/aspnetcore/5.0/signalr-messagepack-hub-protocol-options-changed.md)]

***

[!INCLUDE[SignalR: UseSignalR and UseConnections methods removed](~/includes/core-changes/aspnetcore/5.0/signalr-usesignalr-useconnections-removed.md)]

***

[!INCLUDE[Static files: CSV content type changed to standards-compliant](~/includes/core-changes/aspnetcore/5.0/static-files-csv-content-type-changed.md)]

***

## <a name="aspnet-core-31"></a>ASP.NET Core 3,1

[!INCLUDE[HTTP: Browser SameSite changes impact authentication](~/includes/core-changes/aspnetcore/3.1/http-cookie-samesite-authn-impacts.md)]

***

## <a name="aspnet-core-30"></a>ASP.NET Core 3,0

[!INCLUDE[Obsolete Antiforgery, CORS, Diagnostics, MVC, and Routing APIs removed](~/includes/core-changes/aspnetcore/3.0/obsolete-apis-removed.md)]

***

[!INCLUDE[Authentication: Google+ deprecation](~/includes/core-changes/aspnetcore/3.0/authn-google-plus-authn-changes.md)]

***

[!INCLUDE[Authentication: HttpContext.Authentication property removed](~/includes/core-changes/aspnetcore/3.0/authn-httpcontext-property-removed.md)]

***

[!INCLUDE[Authentication: Newtonsoft.Json types replaced](~/includes/core-changes/aspnetcore/3.0/authn-apis-json-types.md)]

***

[!INCLUDE[Authentication: OAuthHandler ExchangeCodeAsync signature changed](~/includes/core-changes/aspnetcore/3.0/authn-exchangecodeasync-signature-change.md)]

***

[!INCLUDE[Authorization: AddAuthorization overload assembly change](~/includes/core-changes/aspnetcore/3.0/authz-assembly-change.md)]

***

[!INCLUDE[Authorization: IAllowAnonymous removed from AuthorizationFilterContext.Filters](~/includes/core-changes/aspnetcore/3.0/authz-iallowanonymous-removed-from-collection.md)]

***

[!INCLUDE[Authorization: IAuthorizationPolicyProvider implementations require new method](~/includes/core-changes/aspnetcore/3.0/authz-iauthzpolicyprovider-new-method.md)]

***

[!INCLUDE[Caching: CompactOnMemoryPressure property removed](~/includes/core-changes/aspnetcore/3.0/caching-memory-property-removed.md)]

***

[!INCLUDE[Caching: Microsoft.Extensions.Caching.SqlServer uses new SqlClient package](~/includes/core-changes/aspnetcore/3.0/caching-new-sqlclient-package.md)]

***

[!INCLUDE[Caching: ResponseCaching types changed to internal](~/includes/core-changes/aspnetcore/3.0/caching-response-pubternal-to-internal.md)]

***

[!INCLUDE[Data Protection: DataProtection.AzureStorage uses new Azure Storage APIs](~/includes/core-changes/aspnetcore/3.0/dataprotection-azstorage-using-azstorage-apis.md)]

***

[!INCLUDE[Hosting: ANCM version 1 removed from hosting bundle](~/includes/core-changes/aspnetcore/3.0/hosting-ancmv1-hosting-bundle-removal.md)]

***

[!INCLUDE[Hosting: Generic host restriction on Startup constructor injection](~/includes/core-changes/aspnetcore/3.0/hosting-generic-host-startup-ctor-restriction.md)]

***

[!INCLUDE[Hosting: HTTPS redirection enabled for IIS OutOfProcess](~/includes/core-changes/aspnetcore/3.0/hosting-https-redirection-iis-outofprocess.md)]

***

[!INCLUDE[Hosting: IHostingEnvironment and IApplicationLifetime types replaced](~/includes/core-changes/aspnetcore/3.0/hosting-ihostingenv-iapplifetime-types-replaced.md)]

***

[!INCLUDE[Hosting: ObjectPoolProvider removed from WebHostBuilder dependencies](~/includes/core-changes/aspnetcore/3.0/hosting-objectpoolprovider-webhostbuilder-dependencies.md)]

***

[!INCLUDE[HTTP: DefaultHttpContext extensibility removed](~/includes/core-changes/aspnetcore/3.0/http-defaulthttpcontext-extensibility-removed.md)]

***

[!INCLUDE[HTTP: HeaderNames fields changed to static readonly](~/includes/core-changes/aspnetcore/3.0/http-headernames-constants-staticreadonly.md)]

***

[!INCLUDE[HTTP: Response body infrastructure changes](~/includes/core-changes/aspnetcore/3.0/http-response-body-changes.md)]

***

[!INCLUDE[HTTP: Some cookie SameSite default values changed](~/includes/core-changes/aspnetcore/3.0/http-cookie-samesite-defaults-change.md)]

***

[!INCLUDE[HTTP: Synchronous IO disabled by default](~/includes/core-changes/aspnetcore/3.0/http-synchronous-io-disabled.md)]

***

[!INCLUDE[Identity: AddDefaultUI method overload removed](~/includes/core-changes/aspnetcore/3.0/identity-ui-adddefaultui-overload-removed.md)]

***

[!INCLUDE[Identity: UI Bootstrap version change](~/includes/core-changes/aspnetcore/3.0/identity-ui-bootstrap-version.md)]

***

[!INCLUDE[Identity: SignInAsync throws exception for unauthenticated identity](~/includes/core-changes/aspnetcore/3.0/identity-signinasync-throws-exception.md)]

***

[!INCLUDE[Identity: SignInManager constructor accepts new parameter](~/includes/core-changes/aspnetcore/3.0/identity-signinmanager-ctor-parameter.md)]

***

[!INCLUDE[Identity: UI uses static web assets feature](~/includes/core-changes/aspnetcore/3.0/identity-ui-static-web-assets.md)]

***

[!INCLUDE[Kestrel: Connection adapters removed](~/includes/core-changes/aspnetcore/3.0/kestrel-connection-adapters-removed.md)]

***

[!INCLUDE[Kestrel: Empty HTTPS assembly removed](~/includes/core-changes/aspnetcore/3.0/kestrel-empty-assembly-removed.md)]

***

[!INCLUDE[Kestrel: Request trailer headers moved to new collection](~/includes/core-changes/aspnetcore/3.0/kestrel-request-trailer-headers.md)]

***

[!INCLUDE[Kestrel: Transport abstraction layer changes](~/includes/core-changes/aspnetcore/3.0/kestrel-transport-abstractions.md)]

***

[!INCLUDE[Localization: APIs marked obsolete](~/includes/core-changes/aspnetcore/3.0/localization-apis-marked-obsolete.md)]

***

[!INCLUDE[Logging: DebugLogger class made internal](~/includes/core-changes/aspnetcore/3.0/logging-debuglogger-to-internal.md)]

***

[!INCLUDE[MVC: Controller action Async suffix removed](~/includes/core-changes/aspnetcore/3.0/mvc-action-async-suffix-trimmed.md)]

***

[!INCLUDE[MVC: JsonResult moved to Microsoft.AspNetCore.Mvc.Core](~/includes/core-changes/aspnetcore/3.0/mvc-jsonresult-moved.md)]

***

[!INCLUDE[MVC: Precompilation tool deprecated](~/includes/core-changes/aspnetcore/3.0/mvc-precompilation-tool-deprecated.md)]

***

[!INCLUDE[MVC: Types changed to internal](~/includes/core-changes/aspnetcore/3.0/mvc-pubternal-to-internal.md)]

***

[!INCLUDE[MVC: Web API compatibility shim removed](~/includes/core-changes/aspnetcore/3.0/mvc-webapi-compat-shim-removed.md)]

***

[!INCLUDE[Razor: Runtime compilation moved to a package](~/includes/core-changes/aspnetcore/3.0/razor-runtime-compilation-package.md)]

***

[!INCLUDE[Session state: Obsolete APIs removed](~/includes/core-changes/aspnetcore/3.0/session-obsolete-apis-removed.md)]

***

[!INCLUDE[Shared framework: Assembly removal from Microsoft.AspNetCore.App](~/includes/core-changes/aspnetcore/3.0/sharedfx-app-shared-framework-assemblies.md)]

***

[!INCLUDE[Shared framework: Microsoft.AspNetCore.All removed](~/includes/core-changes/aspnetcore/3.0/sharedfx-all-framework-removed.md)]

***

[!INCLUDE[SignalR: HandshakeProtocol.SuccessHandshakeData replaced](~/includes/core-changes/aspnetcore/3.0/signalr-successhandshakedata-replaced.md)]

***

[!INCLUDE[SignalR: HubConnection methods removed](~/includes/core-changes/aspnetcore/3.0/signalr-hubconnection-methods-removed.md)]

***

[!INCLUDE[SignalR: HubConnectionContext constructors changed](~/includes/core-changes/aspnetcore/3.0/signalr-hubconnectioncontext-ctors.md)]

***

[!INCLUDE[SignalR: JavaScript client package name change](~/includes/core-changes/aspnetcore/3.0/signalr-js-client-package-name.md)]

***

[!INCLUDE[SignalR: Obsolete APIs](~/includes/core-changes/aspnetcore/3.0/signalr-obsolete-apis.md)]

***

[!INCLUDE[SPAs: SpaServices and NodeServices marked obsolete](~/includes/core-changes/aspnetcore/3.0/spas-spaservices-nodeservices-obsolete.md)]

***

[!INCLUDE[SPAs: SpaServices and NodeServices console logger fallback default change](~/includes/core-changes/aspnetcore/3.0/spas-spaservices-nodeservices-fallback.md)]

***

[!INCLUDE[Target framework: .NET Framework not supported](~/includes/core-changes/aspnetcore/3.0/targetfx-netfx-tfm-support.md)]

***
