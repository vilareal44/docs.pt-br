---
title: Novidades do .NET Core 2.1
description: Conheça os novos recursos encontrados no .NET Core 2.1.
dev_langs:
- csharp
- vb
ms.date: 10/10/2018
ms.openlocfilehash: 94f3db14046ad5d63975d0ca44425abed5d52062
ms.sourcegitcommit: 97ce5363efa88179dd76e09de0103a500ca9b659
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2020
ms.locfileid: "86281531"
---
# <a name="whats-new-in-net-core-21"></a>Novidades do .NET Core 2.1

O .NET Core 2.1 contém melhorias e novos recursos nas seguintes áreas:

- [Ferramentas](#tooling)
- [Rolar para frente](#roll-forward)
- [Implantação](#deployment)
- [Pacote de Compatibilidade do Windows](#windows-compatibility-pack)
- [Aprimoramentos da compilação JIT](#jit-compiler-improvements)
- [Alterações de API](#api-changes)

## <a name="tooling"></a>Ferramentas

O SDK do .NET Core 2.1 (v 2.1.300), o conjunto de ferramentas incluído no .NET Core 2.1, inclui as seguintes alterações e aprimoramentos:

### <a name="build-performance-improvements"></a>Melhorias de desempenho de build

Um dos principais focos do .NET Core 2.1 é melhorar o desempenho de tempo de build, especialmente para builds incrementais. Essas melhorias de desempenho se aplicam a ambos os builds de linha de comando que usam `dotnet build` e a builds no Visual Studio. Algumas áreas individuais de melhoria incluem:

- Na resolução de ativos de pacote, resolução somente de ativos usados por um build, em vez de todos os ativos.

- Armazenamento em cache de referências de assembly.

- Uso de servidores de build do SDK de longa execução, que são processos que se estendem por chamadas individuais de `dotnet build`. Eles eliminam a necessidade de compilação JIT de grandes blocos de código toda vez que `dotnet build` é executado. Os processos do servidor de build podem ser finalizados automaticamente com o seguinte comando:

   ```dotnetcli
   dotnet buildserver shutdown
   ```

### <a name="new-cli-commands"></a>Novos comandos da CLI

Várias ferramentas que estavam disponíveis apenas por projeto usando `DotnetCliToolReference` agora estão disponíveis como parte do SDK do .NET Core. Essas ferramentas incluem:

- `dotnet watch` fornece um observador do sistema de arquivos que aguarda que um arquivo seja alterado antes de executar um conjunto designado de comandos. Por exemplo, o comando a seguir recria automaticamente o projeto atual e gera uma saída detalhada sempre que um arquivo é alterado nele:

   ```dotnetcli
   dotnet watch -- --verbose build
   ```

   Observe a opção `--` que precede a opção `--verbose`. Isso delimita as opções passadas diretamente para o comando `dotnet watch` dos argumentos que são passados para o processo `dotnet` filho. Sem isso, a opção `--verbose` se aplica ao comando `dotnet watch`, não ao comando `dotnet build`.
  
   Para obter mais informações, consulte [desenvolver ASP.NET Core aplicativos usando o relógio dotnet](/aspnet/core/tutorials/dotnet-watch).

- `dotnet dev-certs` gera e gerencia os certificados usados durante o desenvolvimento de aplicativos do ASP.NET Core.

- `dotnet user-secrets` gerencia os segredos em um repositório de segredos do usuário em aplicativos do ASP.NET Core.

- `dotnet sql-cache` cria uma tabela e índices em um banco de dados do Microsoft SQL Server a serem usados para armazenamento em cache distribuído.

- `dotnet ef` é uma ferramenta para gerenciar bancos de dados, objetos <xref:Microsoft.EntityFrameworkCore.DbContext> e migrações em aplicativos Entity Framework Core. Para saber mais, confira [Ferramentas da linha de comando do .NET EF Core](/ef/core/miscellaneous/cli/dotnet).

### <a name="global-tools"></a>Ferramentas Globais

O .NET Core 2.1 oferece suporte a *Ferramentas Globais*, ou seja, ferramentas personalizadas que estão disponíveis globalmente a partir da linha de comando. O modelo de extensibilidade em versões anteriores do .NET Core disponibilizava ferramentas personalizadas apenas por projeto usando `DotnetCliToolReference`.

Para instalar uma Ferramenta Global, use o comando [dotnet tool install](../tools/dotnet-tool-install.md). Por exemplo:

```dotnetcli
dotnet tool install -g dotnetsay
```

Uma vez instalada, a ferramenta pode ser executada a partir da linha de comando, especificando o nome da ferramenta. Para saber mais, confira [Visão geral das Ferramentas Globais do .NET Core](../tools/global-tools.md).

### <a name="tool-management-with-the-dotnet-tool-command"></a>Gerenciamento de ferramentas com o comando `dotnet tool`

No SDK do .NET Core 2.1, todas as operações de ferramentas usam o comando `dotnet tool`. As seguintes opções estão disponíveis:

- [`dotnet tool install`](../tools/dotnet-tool-install.md)para instalar uma ferramenta.

- [`dotnet tool update`](../tools/dotnet-tool-update.md)para desinstalar e reinstalar uma ferramenta, que a atualiza efetivamente.

- [`dotnet tool list`](../tools/dotnet-tool-list.md)para listar as ferramentas atualmente instaladas.

- [`dotnet tool uninstall`](../tools/dotnet-tool-uninstall.md)para desinstalar as ferramentas atualmente instaladas.

## <a name="roll-forward"></a>Efetuar roll forward

Do .NET Core 2.0 em diante, todos os aplicativos .NET Core efetuam roll forward automaticamente para a última *versão secundária* instalada em um sistema.

A partir do .NET Core 2,0, se a versão do .NET Core com a qual um aplicativo foi criado não estiver presente no tempo de execução, o aplicativo será executado automaticamente na *versão secundária* instalada mais recente do .NET Core. Em outras palavras, se um aplicativo for criado com o .NET Core 2.0, e o .NET Core 2.0 não estiver presente no sistema do host, mas o .NET Core 2.1 estiver, o aplicativo será executado com o .NET Core 2.1.

> [!IMPORTANT]
> Esse comportamento de roll-forward não se aplica a versões prévias. Por padrão, ele também não se aplica a versões principais, mas isso pode ser alterado com as configurações abaixo.

Modifique esse comportamento alterando a configuração de roll forward em nenhuma estrutura compartilhada candidata. As configurações disponíveis são:

- `0` – desabilitar o comportamento de roll forward da versão secundária. Com essa configuração, um aplicativo criado para o .NET Core 2.0.0 efetuará roll forward para o .NET Core 2.0.1, mas não para o .NET Core 2.2.0 ou o .NET Core 3.0.0.
- `1` – habilitar o comportamento de roll forward da versão secundária. Esse é o valor padrão para a configuração. Com essa configuração, um aplicativo criado para o .NET Core 2.0.0 efetuará roll forward para o .NET Core 2.0.1 ou o .NET Core 2.2.0, dependendo de qual estiver instalado, mas não efetuará roll forward para o .NET Core 3.0.0.
- `2` – habilitar o comportamento de roll forward das versões secundária e principal. Se essa opção estiver definida, até mesmo versões principais diferentes serão consideradas e, portanto, um aplicativo criado para o .NET Core 2.0.0 efetuará roll forward para o .NET Core 3.0.0.

Modifique essa configuração de uma das três maneiras:

- Defina a variável de ambiente `DOTNET_ROLL_FORWARD_ON_NO_CANDIDATE_FX` com o valor desejado.

- Adicione a seguinte linha com o valor desejado ao arquivo *.runtimeconfig.json*:

   ```json
   "rollForwardOnNoCandidateFx" : 0
   ```

- Ao usar o [CLI do .NET Core](../tools/index.md), adicione a opção a seguir com o valor desejado a um comando do .NET Core, como `run` :

   ```dotnetcli
   dotnet run --rollForwardOnNoCandidateFx=0
   ```

O roll forward da versão de patch é independente dessa configuração e é feito após a aplicação de qualquer roll forward potencial da versão secundária ou principal.

## <a name="deployment"></a>Implantação

### <a name="self-contained-application-servicing"></a>Serviço de aplicativo autocontido

`dotnet publish` agora publica aplicativos autocontidos com uma versão de runtime atendido. Quando você publica um aplicativo autocontido com o SDK do .NET Core 2.1 (v 2.1.300), seu aplicativo inclui a versão mais recente de runtime atendido conhecida por esse SDK. Ao atualizar para o SDK mais recente, você publicará com a versão mais recente do tempo de execução do .NET Core. Isso se aplica aos runtimes do .NET Core 1.0 e posteriores.

A publicação independente depende das versões de tempo de execução no NuGet.org. Você não precisa ter o tempo de execução de serviço em seu computador.

Com o uso do SDK do .NET Core 2.0, os aplicativos autocontidos são publicados com o runtime do .NET Core 2.0.0, a menos que uma versão diferente seja especificada por meio da propriedade `RuntimeFrameworkVersion`. Com esse novo comportamento, você não precisará mais definir essa propriedade para selecionar uma versão de tempo de execução maior para um aplicativo independente. A abordagem mais fácil daqui para frente é sempre publicar com o SDK do .NET Core 2.1 (v 2.1.300).

Veja mais informações em [Efetuar roll forward de runtime de implantação autossuficiente](../deploying/runtime-patch-selection.md).
## <a name="windows-compatibility-pack"></a>Pacote de Compatibilidade do Windows

Ao transmitir código existente do .NET Framework para o .NET Core, você pode usar o [Pacote de Compatibilidade do Windows](https://www.nuget.org/packages/Microsoft.Windows.Compatibility). Ele fornece acesso 20.000 APIs a mais do que as disponíveis no .NET Core. Essas APIs incluem tipos no namespace <xref:System.Drawing?displayProperty=nameWithType>, a classe <xref:System.Diagnostics.EventLog>, WMI, contadores de desempenho, Windows Services e os tipos de registro e membros do Windows.

## <a name="jit-compiler-improvements"></a>Melhorias do compilador JIT

O .NET Core incorpora uma nova tecnologia de compilador JIT chamada de *compilação em camadas* (também conhecida como *otimização adaptativa*) que pode melhorar significativamente o desempenho. A compilação em camadas é uma configuração opcional.

Uma das tarefas importantes executadas pelo compilador JIT é otimizar a execução de código. No entanto, para caminhos de código pouco usados, o compilador pode gastar mais tempo otimizando o código do que o runtime gasta executando código não otimizado. A compilação em camadas introduz dois estágios na compilação JIT:

- Uma **primeira camada**, que gera código o mais rápido possível.

- Uma **segunda camada**, que gera código otimizado para os métodos executados com frequência. A segunda camada de compilação é executada em paralelo para melhorar o desempenho.

Você pode optar pela compilação em camadas de duas maneiras.

- Para usar a compilação em camadas em todos os projetos que usam o SDK do .NET Core 2.1, defina a seguinte variável de ambiente:

  ```console
  COMPlus_TieredCompilation="1"
  ```

- Para usar a compilação em camadas por projeto, adicione a propriedade `<TieredCompilation>` à seção `<PropertyGroup>` do arquivo de projeto MSBuild, como mostra o exemplo a seguir:

   ```xml
   <PropertyGroup>
      <!-- other property definitions -->

      <TieredCompilation>true</TieredCompilation>
   </PropertyGroup>
   ```

## <a name="api-changes"></a>Alterações de API

### <a name="spant-and-memoryt"></a>`Span<T>` e `Memory<T>`

O .NET Core 2.1 inclui alguns novos tipos de tornam o trabalho com matrizes e outros tipos de memória muito mais eficiente. Os novos tipos incluem:

- <xref:System.Span%601?displayProperty=nameWithType> e <xref:System.ReadOnlySpan%601?displayProperty=nameWithType>.

- <xref:System.Memory%601?displayProperty=nameWithType> e <xref:System.ReadOnlyMemory%601?displayProperty=nameWithType>.

Sem esses tipos, ao transmitir esses itens como parte de uma matriz ou seção de um buffer de memória, você precisará fazer uma cópia de uma parte dos dados antes de transmiti-los a um método. Esses tipos fornecem uma visão virtual desses dados, o que elimina a necessidade de alocação de memória adicional e operações de cópia.

O exemplo a seguir usa uma instância <xref:System.Span%601> e <xref:System.Memory%601> para fornecer uma visão virtual de 10 elementos de uma matriz.

[!code-csharp[Span\<T>](./snippets/dotnet-core-2-1/csharp/program.cs)]

[!code-vb[Memory\<T>](./snippets/dotnet-core-2-1/vb/program.vb)]

### <a name="brotli-compression"></a>Compactação Brotli

O .NET Core 2.1 adiciona suporte para compactação e descompactação Brotli. Brotli é um algoritmo de compactação sem perda, de uso geral, que é definido em [RFC 7932](https://www.ietf.org/rfc/rfc7932.txt) e é compatível com a maioria dos navegadores da Web e com os principais servidores Web. Você pode usar a classe <xref:System.IO.Compression.BrotliStream?displayProperty=nameWithType> baseada em fluxo ou as classes <xref:System.IO.Compression.BrotliEncoder?displayProperty=nameWithType> e <xref:System.IO.Compression.BrotliDecoder?displayProperty=nameWithType> baseadas em span de alto desempenho. O exemplo a seguir ilustra a compactação com a classe <xref:System.IO.Compression.BrotliStream>:

[!code-csharp[Brotli compression](./snippets/dotnet-core-2-1/csharp/brotli.cs#1)]

[!code-vb[Brotli compression](./snippets/dotnet-core-2-1/vb/brotli.vb#1)]

O comportamento <xref:System.IO.Compression.BrotliStream> é o mesmo de <xref:System.IO.Compression.DeflateStream> e <xref:System.IO.Compression.GZipStream>, o que facilita a conversão de código que chame essas APIs para <xref:System.IO.Compression.BrotliStream>.

### <a name="new-cryptography-apis-and-cryptography-improvements"></a>Novas APIs de criptografia e aprimoramentos de criptografia

O .NET Core 2.1 inclui vários aprimoramentos para a APIs de criptografia:

- <xref:System.Security.Cryptography.Pkcs.SignedCms?displayProperty=nameWithType> está disponível no pacote System.Security.Cryptography.Pkcs. A implementação é igual à classe <xref:System.Security.Cryptography.Pkcs.SignedCms> no .NET Framework.

- Novas sobrecargas dos métodos <xref:System.Security.Cryptography.X509Certificates.X509Certificate.GetCertHash%2A?displayProperty=nameWithType> e <xref:System.Security.Cryptography.X509Certificates.X509Certificate.GetCertHashString%2A?displayProperty=nameWithType> aceitam um identificador de algoritmo de hash para permitir que os chamadores obtenham valores de impressão digital do certificado usando algoritmos diferentes de SHA-1.

- Novas APIs de criptografia baseadas em <xref:System.Span%601> estão disponíveis para hash, HMAC, geração de número aleatório criptográfico, geração de assinatura assimétrica, processamento de assinatura assimétrica e criptografia RSA.

- O desempenho de <xref:System.Security.Cryptography.Rfc2898DeriveBytes?displayProperty=nameWithType> melhorou em cerca de 15% usando uma implementação baseada em <xref:System.Span%601>.

- A nova classe <xref:System.Security.Cryptography.CryptographicOperations?displayProperty=nameWithType> inclui dois novos métodos:

  - <xref:System.Security.Cryptography.CryptographicOperations.FixedTimeEquals%2A> leva um período fixo para retornar para duas entradas do mesmo tamanho, o que o torna adequado para uso em verificação criptográfica a fim de evitar contribuir com informações de temporização.

  - <xref:System.Security.Cryptography.CryptographicOperations.ZeroMemory%2A> é uma rotina de limpeza de memória que não pode ser otimizada.

- O método estático <xref:System.Security.Cryptography.RandomNumberGenerator.Fill%2A?displayProperty=nameWithType> preenche um <xref:System.Span%601> com valores aleatórios.

- O <xref:System.Security.Cryptography.Pkcs.EnvelopedCms?displayProperty=nameWithType> agora tem suporte no Linux e no MacOS.

- A curva elíptica Diffie-Hellman (ECDH) agora está disponível na família de classes <xref:System.Security.Cryptography.ECDiffieHellman?displayProperty=nameWithType>. A área de superfície é o mesmo que no .NET Framework.

- A instância retornada por <xref:System.Security.Cryptography.RSA.Create%2A?displayProperty=nameWithType> pode criptografar ou descriptografar com OAEP usando um resumo de SHA-2, bem como gerar ou validar assinaturas usando RSA-PSS.

### <a name="sockets-improvements"></a>Aperfeiçoamentos de soquetes

O .NET Core inclui um novo tipo, <xref:System.Net.Http.SocketsHttpHandler?displayProperty=nameWithType>, e um <xref:System.Net.Http.HttpMessageHandler?displayProperty=nameWithType> reescrito, que formam a base de APIs de rede de nível superior.  <xref:System.Net.Http.SocketsHttpHandler?displayProperty=nameWithType>, por exemplo, é a base da implementação <xref:System.Net.Http.HttpClient>. Nas versões anteriores do .NET Core, as APIs de nível superior eram baseadas em implementações de redes nativas.

A implementação de soquetes introduzida no .NET Core 2.1 tem várias vantagens:

- Uma melhoria de desempenho significativa quando comparada com a implementação anterior.

- Eliminação das dependências da plataforma, o que simplifica a implantação e a manutenção.

- Comportamento consistente em todas as plataformas .NET Core.

<xref:System.Net.Http.SocketsHttpHandler> é a implementação padrão do .NET Core 2.1. No entanto, você pode configurar seu aplicativo para usar a classe <xref:System.Net.Http.HttpClientHandler> mais antiga chamando o método <xref:System.AppContext.SetSwitch%2A?displayProperty=nameWithType>:

```csharp
AppContext.SetSwitch("System.Net.Http.UseSocketsHttpHandler", false);
```

```vb
AppContext.SetSwitch("System.Net.Http.UseSocketsHttpHandler", False)
```

Também é possível usar uma variável de ambiente para desativar o uso de implementações de soquetes com base em <xref:System.Net.Http.SocketsHttpHandler>. Para fazer isso, defina `DOTNET_SYSTEM_NET_HTTP_USESOCKETSHTTPHANDLER` para `false` ou 0.

No Windows, você também pode escolher usar <xref:System.Net.Http.WinHttpHandler?displayProperty=nameWithType>, que depende de uma implementação nativa, ou a classe <xref:System.Net.Http.SocketsHttpHandler> transmitindo uma instância da classe para o construtor <xref:System.Net.Http.HttpClient>.

No Linux e no macOS, só é possível configurar <xref:System.Net.Http.HttpClient> por processo. No Linux, você precisa implantar [libcurl](https://curl.haxx.se/libcurl/) se quiser usar a implementação <xref:System.Net.Http.HttpClient> antiga. (Ele é instalado com .NET Core 2.0.)

### <a name="breaking-changes"></a>Alterações da falha

Para obter informações sobre alterações significativas, consulte [alterações recentes de migração da versão 2,0 para 2,1](../compatibility/2.0-2.1.md).

## <a name="see-also"></a>Confira também

- [Novidades do .NET Core 3.1](dotnet-core-3-1.md)
- [Novos recursos no EF Core 2.1](/ef/core/what-is-new/ef-core-2.1)
- [Novidades do ASP.NET Core 2.1](/aspnet/core/aspnetcore-2.1)
