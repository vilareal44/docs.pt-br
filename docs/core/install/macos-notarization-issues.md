---
title: Trabalhando com o notarization Catalina do macOS
description: Como lidar com problemas de notarization e certificado com o macOS ao instalar o .NET Core Runtime, o SDK e os aplicativos criados com o .NET Core.
author: adegeo
ms.author: adegeo
ms.date: 02/14/2020
ms.openlocfilehash: a7741727ad46216ebd9936515d8af29b6d7049c2
ms.sourcegitcommit: c4a15c6c4ecbb8a46ad4e67d9b3ab9b8b031d849
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88656521"
---
# <a name="macos-catalina-notarization-and-the-impact-on-net-core-downloads-and-projects"></a>macOS Catalina notarization e o impacto sobre os downloads e projetos do .NET Core

A partir do macOS Catalina (versão 10,15), todo software criado após 1º de junho de 2019 e distribuído com a ID do desenvolvedor deve ser notarized. Esse requisito se aplica ao tempo de execução do .NET Core, SDK do .NET Core e software criado com o .NET Core. Este artigo descreve os cenários comuns que você pode enfrentar com o .NET Core e o macOS notarization.

## <a name="installing-net-core"></a>Instalando o .NET Core

Os instaladores do .NET Core (Runtime e SDK) versões 3,1, 3,0 e 2,1, foram notarizeddos desde 18 de fevereiro de 2020. Versões anteriores liberadas não são notarized. Você pode instalar manualmente uma versão não notarized do .NET Core baixando primeiro o instalador e, em seguida, usando o `sudo installer` comando. Para obter mais informações, consulte [baixar e instalar manualmente para o MacOS](sdk.md?pivots=os-macos#download-and-manually-install).

A partir das seguintes versões, os instaladores do .NET Core são notarized:

- Runtime do .NET Core
  - 2.1.16
  - 3.0.3
  - 3.1.2

- SDK do .Net Core
  - 2.1.512
  - 3.0.103
  - 3.1.102

## <a name="apphost-is-disabled-by-default"></a>appHost está desabilitado por padrão

Por padrão, as versões não notarized do SDK do .NET Core 3,0 e superior produzem um executável de Mach-O nativo (conhecido como **appHost**) quando seu projeto é compilado, publicado ou é executado. Esse executável é uma maneira conveniente de executar seu aplicativo. Caso contrário, seu aplicativo deve ser iniciado executando `dotnet <filename.dll>` . Quando o appHost está habilitado, o `dotnet run` comando é invocado no contexto do appHost. Para obter mais informações, consulte [contexto do appHost](#context-of-the-apphost).

A partir das versões notarized do SDK do .NET Core 3,0 e posterior, o executável appHost não é gerado por padrão. Você pode ativar a geração de appHost com a [`UseAppHost`](../project-sdk/msbuild-props.md#useapphost) configuração booliana no arquivo de projeto. Você também pode alternar o appHost com o `-p:UseAppHost` parâmetro na linha de comando para o `dotnet` comando específico executado:

- Arquivo de projeto

  ```xml
  <PropertyGroup>
    <UseAppHost>true</UseAppHost>
  </PropertyGroup>
  ```

- Parâmetro de linha de comando

  ```dotnetcli
  dotnet run -p:UseAppHost=true
  ```

Um appHost é sempre criado quando você publica [seu aplicativo independente](../deploying/index.md#publish-self-contained).

Para obter mais informações sobre a `UseAppHost` configuração, consulte [Propriedades do MSBuild para Microsoft. net. SDK](../project-sdk/msbuild-props.md#useapphost).

### <a name="context-of-the-apphost"></a>Contexto do appHost

Quando o appHost está habilitado em seu projeto e você usa o `dotnet run` comando para executar seu aplicativo, o aplicativo é invocado no contexto do appHost e não no host padrão (o host padrão é o `dotnet` comando). Se o appHost estiver desabilitado em seu projeto, o `dotnet run` comando executará seu aplicativo no contexto do host padrão. Mesmo que o appHost esteja desabilitado, a publicação de seu aplicativo como independente gera um executável appHost e os usuários usam esse executável para executar seu aplicativo. Executar seu aplicativo com `dotnet <filename.dll>` invoca o aplicativo com o host padrão, o tempo de execução compartilhado.

Quando um aplicativo que usa o appHost é invocado, a partição de certificado acessada pelo aplicativo é diferente do host padrão notarized. Se seu aplicativo precisar acessar os certificados instalados por meio do host padrão, use o `dotnet run` comando para executar seu aplicativo de seu arquivo de projeto ou use o `dotnet <filename.dll>` comando para iniciar o aplicativo diretamente.

Mais informações sobre esse cenário são fornecidas na seção [ASP.NET Core e MacOS e certificados](#aspnet-core-and-macos-and-certificates) .

## <a name="aspnet-core-and-macos-and-certificates"></a>ASP.NET Core e macOS e certificados

O .NET Core fornece a capacidade de gerenciar certificados no conjunto de chaves do macOS com a <xref:System.Security.Cryptography.X509Certificates> classe. O acesso ao conjunto de chaves do macOS usa a identidade de aplicativos como a chave primária ao decidir qual partição deve ser considerada. Por exemplo, aplicativos não assinados armazenam segredos na partição não assinada, mas os aplicativos assinados armazenam seus segredos em partições somente que podem acessar. A origem da execução que invoca seu aplicativo decide qual partição usar.

O .NET Core fornece três fontes de execução: [appHost](#apphost-is-disabled-by-default), host padrão (o `dotnet` comando) e um host personalizado. Cada modelo de execução pode ter identidades diferentes, assinadas ou não, e tem acesso a diferentes partições dentro do conjunto de chaves. Os certificados importados por um modo podem não estar acessíveis a partir de outro. Por exemplo, as versões notarized do .NET Core têm um host padrão assinado. Os certificados são importados para uma partição segura com base em sua identidade. Esses certificados não estão acessíveis de um appHost gerado, pois o appHost não está assinado.

Outro exemplo, por padrão, ASP.NET Core importa um certificado SSL padrão por meio do host padrão. ASP.NET Core aplicativos que usam um appHost não terão acesso a esse certificado e receberão um erro quando o .NET Core detectar que o certificado não está acessível. A mensagem de erro fornece instruções sobre como corrigir esse problema.

Se o compartilhamento de certificados for necessário, o macOS fornecerá opções de configuração com o `security` utilitário.

Para obter mais informações sobre como solucionar problemas de ASP.NET Core de certificado, consulte [impor HTTPS no ASP.NET Core](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.1&tabs=visual-studio#troubleshoot-certificate-problems).

## <a name="default-entitlements"></a>Direitos padrão

O host padrão do .NET Core (o `dotnet` comando) tem um conjunto de direitos padrão. Esses direitos são necessários para a operação adequada do .NET Core. É possível que seu aplicativo possa precisar de direitos adicionais. nesse caso, você precisará gerar e usar um [appHost](#apphost-is-disabled-by-default) e, em seguida, adicionar os direitos necessários localmente.

Conjunto padrão de direitos para o .NET Core:

- `com.apple.security.cs.allow-jit`
- `com.apple.security.cs.allow-unsigned-executable-memory`
- `com.apple.security.cs.allow-dyld-environment-variables`
- `com.apple.security.cs.disable-library-validation`

## <a name="notarize-a-net-core-app"></a>Notarize um aplicativo .NET Core

Se você quiser que seu aplicativo seja executado no macOS (versão 10,15) ou superior, convém notarize seu aplicativo. O appHost que você envia com seu aplicativo para notarization deve ser usado com pelo menos os mesmos [direitos padrão](#default-entitlements) para o .NET Core.

## <a name="next-steps"></a>Próximas etapas

- [Dependências e requisitos do .NET Core](macos.md#dependencies).
- [Instale o SDK e o tempo de execução do .NET Core](macos.md).
