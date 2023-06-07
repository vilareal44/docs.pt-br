---
ms.openlocfilehash: 3244a36808fb687663241e704d08775ea5c96720
ms.sourcegitcommit: 1eae045421d9ea2bfc82aaccfa5b1ff1b8c9e0e4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84803238"
---
### <a name="kestrel-default-supported-tls-protocol-versions-changed"></a>Kestrel: versões do protocolo TLS com suporte padrão alteradas

O Kestrel agora usa as versões do protocolo TLS padrão do sistema em vez de restringir as conexões com os protocolos TLS 1,1 e TLS 1,2 como fazia anteriormente.

Essa alteração permite:

* O TLS 1,3 a ser usado por padrão em ambientes que dão suporte a ele.
* O TLS 1,0 a ser usado em alguns ambientes (como o Windows Server 2016 por padrão), que geralmente [não é desejável](/security/engineering/solving-tls1-problem).

Para obter uma discussão, veja Issue [dotnet/aspnetcore # 22563](https://github.com/dotnet/aspnetcore/issues/22563).

#### <a name="version-introduced"></a>Versão introduzida

5,0 Preview 6

#### <a name="old-behavior"></a>Comportamento antigo

O Kestrel exigiu que as conexões usem TLS 1,1 ou TLS 1,2 por padrão.

#### <a name="new-behavior"></a>Novo comportamento

O Kestrel permite que o sistema operacional escolha o melhor protocolo a ser usado e bloqueie protocolos inseguros. <xref:Microsoft.AspNetCore.Server.Kestrel.Https.HttpsConnectionAdapterOptions.SslProtocols%2A?displayProperty=nameWithType>Agora, o padrão é `SslProtocols.None` em vez de `SslProtocols.Tls12 | SslProtocols.Tls11` .

#### <a name="reason-for-change"></a>Motivo da alteração

A alteração foi feita para dar suporte a TLS 1,3 e futuras versões de TLS por padrão, pois elas se tornam disponíveis.

#### <a name="recommended-action"></a>Ação recomendada

A menos que seu aplicativo tenha um motivo específico para não, você deve usar os novos padrões. Verifique se o sistema está configurado para permitir apenas protocolos seguros.

Para desabilitar protocolos mais antigos, execute uma das seguintes ações:

* Desabilite os protocolos mais antigos, como o TLS 1,0, em todo o sistema com as [instruções do Windows](/dotnet/framework/network-programming/tls#configuring-schannel-protocols-in-the-windows-registry). Atualmente, ele está habilitado por padrão em todas as versões do Windows.
* Selecione manualmente os protocolos aos quais você deseja dar suporte no código da seguinte maneira:

    ```csharp
    using System.Security.Authentication;
    using Microsoft.AspNetCore.Hosting;
    using Microsoft.Extensions.Hosting;

    public class Program
    {
        public static void Main(string[] args) =>
            CreateHostBuilder(args).Build().Run();

        public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
                .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseKestrel(kestrelOptions =>
                    {
                        kestrelOptions.ConfigureHttpsDefaults(httpsOptions =>
                        {
                            httpsOptions.SslProtocols = SslProtocols.Tls12 | SslProtocols.Tls13;
                        });
                    });

                    webBuilder.UseStartup<Startup>();
                });
    }
    ```

Infelizmente, não há API para excluir protocolos específicos.

#### <a name="category"></a>Categoria

ASP.NET Core

#### <a name="affected-apis"></a>APIs afetadas

<xref:Microsoft.AspNetCore.Server.Kestrel.Https.HttpsConnectionAdapterOptions.SslProtocols%2A?displayProperty=nameWithType>

<!-- 

#### Affected APIs

`P:Microsoft.AspNetCore.Server.Kestrel.Https.HttpsConnectionAdapterOptions.SslProtocols`

-->
