---
title: O que há de novo no .NET Framework
description: Veja o que há de novo em várias versões do .NET Framework. Leia um resumo dos principais novos recursos e aprimoramentos em cada versão.
ms.date: 04/18/2019
dev_langs:
- csharp
- vb
helpviewer_keywords:
- what's new [.NET Framework]
ms.assetid: 1d971dd7-10fc-4692-8dac-30ca308fc0fa
ms.openlocfilehash: 3eba676a4134415f0e2981d5094c0b0f6a1651be
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88559018"
---
# <a name="whats-new-in-net-framework"></a>O que há de novo no .NET Framework

Este artigo resume os novos recursos-chave e melhorias nas seguintes versões do .NET Framework:

- [.NET Framework 4,8](#v48)
- [.NET Framework 4.7.2](#v472)
- [.NET Framework 4.7.1](#v471)
- [.NET Framework 4,7](#v47)
- [.NET Framework 4.6.2](#v462)
- [.NET Framework 4.6.1](#v461)
- [.NET 2015 e .NET Framework 4.6](#v46)
- [.NET Framework 4.5.2](#v452)
- [.NET Framework 4.5.1](#v451)
- [.NET Framework 4.5](#v45)

Este artigo não fornece informações abrangentes sobre cada recurso novo e está sujeito a alterações. Para obter informações gerais sobre o .NET Framework, confira [Introdução](../get-started/index.md). Para conhecer as plataformas compatíveis, confira [Requisitos do sistema](../get-started/system-requirements.md). Para obter links de download e instruções de instalação, confira [Guia de instalação](../install/guide-for-developers.md).

> [!NOTE]
> A equipe do .NET Framework também libera recursos fora de banda com o NuGet para expandir o suporte à plataforma e introduzir novas funcionalidades, como coleções imutáveis e tipos de vetor habilitados para SIMD. Para saber mais, confira [Bibliotecas de classes e APIs adicionais](../additional-apis/index.md) e [O .NET Framework e lançamentos fora da banda](../get-started/the-net-framework-and-out-of-band-releases.md).
> Veja uma [lista completa de pacotes do NuGet](https://www.nuget.org/profiles/dotnetframework) para o .NET Framework.

<a name="v48"></a>

## <a name="introducing-net-framework-48"></a>Introdução ao .NET Framework 4.8

O .NET Framework 4.8 baseia-se nas versões anteriores do .NET Framework 4.x, adicionando muitas correções e vários recursos novos, mas permanecendo um produto muito estável.

### <a name="download-and-install-net-framework-48"></a>Baixar e instalar o .NET Framework 4,8

Você pode baixar o .NET Framework 4.8 nos seguintes locais:

- [Instalador Web do .NET Framework 4.8](https://dotnet.microsoft.com/download/dotnet-framework/net48)

- [Instalador Offline do .NET Framework 4.8](https://dotnet.microsoft.com/download/dotnet-framework/net48)

O .NET Framework 4.8 pode ser instalado nos sistemas operacionais Windows 10, Windows 8.1, Windows 7 SP1 e nas plataformas de servidor correspondentes, a partir do Windows Server 2008 R2 SP1. É possível instalar o .NET Framework 4.8 usando o instalador Web ou o instalador offline. A maneira recomendada para a maioria dos usuários é usar o instalador da Web.

Para direcionar para o .NET Framework 4.8 no Visual Studio 2012 ou posterior, instale o [Pacote do Desenvolvedor do .NET Framework 4.8](https://go.microsoft.com/fwlink/?LinkId=2085167).

### <a name="whats-new-in-net-framework-48"></a>Novidades no .NET Framework 4.8

O .NET Framework 4.8 apresenta novos recursos nas seguintes áreas:

- [Classes base](#core48)
- [Windows Communication Foundation (WCF)](#wcf48)
- [WPF (Windows Presentation Foundation)](#wpf48)
- [Common Language Runtime](#clr48)

A melhoria na acessibilidade, que permite a um aplicativo proporcionar uma experiência adequada para os usuários da Tecnologia Assistencial, continua sendo um ponto importante do .NET Framework 4.8. Para saber mais sobre melhorias de acessibilidade no .NET Framework 4.8, consulte [Novidades de acessibilidade no .NET Framework](whats-new-in-accessibility.md).

<a name="core48"></a>

#### <a name="base-classes"></a>Classes base

**Redução do impacto do FIPS na criptografia**. Nas versões anteriores do .NET Framework, as classes de provedor criptográfico gerenciadas, como <xref:System.Security.Cryptography.SHA256Managed> o throw a, <xref:System.Security.Cryptography.CryptographicException> quando as bibliotecas criptográficas do sistema são configuradas no "modo FIPS". Essas exceções ocorrem porque as versões gerenciadas das classes do provedor criptográfico, ao contrário das bibliotecas criptográficas do sistema, não receberam a certificação 140-2 do padrão FIPS (Federal Information Processing Standards). Como os computadores de desenvolvimento de alguns desenvolvedores estão no modo FIPS, as exceções geralmente ocorrem em sistemas de produção.

Por padrão, em aplicativos destinados ao .NET Framework 4.8, as seguintes classes de criptografia gerenciadas não geram mais uma <xref:System.Security.Cryptography.CryptographicException>:

- <xref:System.Security.Cryptography.MD5Cng>
- <xref:System.Security.Cryptography.MD5CryptoServiceProvider>
- <xref:System.Security.Cryptography.RC2CryptoServiceProvider>
- <xref:System.Security.Cryptography.RijndaelManaged>
- <xref:System.Security.Cryptography.RIPEMD160Managed>
- <xref:System.Security.Cryptography.SHA256Managed>

Em vez disso, essas classes redirecionam as operações criptográficas para uma biblioteca de criptografia do sistema. Essa alteração elimina efetivamente uma diferença potencialmente confusa entre os ambientes do desenvolvedor e os ambientes de produção, fazendo com que componentes nativos e componentes gerenciados operem sob a mesma política criptográfica. Os aplicativos que dependem dessas exceções podem restaurar o comportamento anterior configurando o comutador `Switch.System.Security.Cryptography.UseLegacyFipsThrow` para `true`. Para saber mais, consulte [Classes de criptografia gerenciadas não geram uma CryptographyException no modo FIPS](../migration-guide/retargeting/4.7.2-4.8.md#managed-cryptography-classes-do-not-throw-a-cryptographyexception-in-fips-mode).

**Uso da versão atualizada do ZLib**

Começando no .NET Framework 4.5, o assembly clrcompression.dll usa [ZLib](https://www.zlib.net), uma biblioteca externa nativa para a compactação de dados, a fim de fornecer uma implementação para o algoritmo deflate. No .NET Framework 4.8, o clrcompression.dll foi atualizado para usar a versão 1.2.11 do ZLib, que inclui várias melhorias e correções importantes.

<a name="wcf48"></a>

#### <a name="windows-communication-foundation-wcf"></a>Windows Communication Foundation (WCF)

**Introdução do ServiceHealthBehavior**

Os pontos de extremidade de integridade são amplamente usados pelas ferramentas de orquestração para gerenciar os serviços com base em seu status de integridade. As verificações de integridade também podem ser usadas pelas ferramentas de monitoramento para rastrear e fornecer notificações sobre a disponibilidade e o desempenho de um serviço.

O **ServiceHealthBehavior** é um comportamento de serviço de aplicativo do Windows Communication Foundation que estende o <xref:System.ServiceModel.Description.IServiceBehavior>.  Quando adicionado à coleção <xref:System.ServiceModel.Description.ServiceDescription.Behaviors?displayProperty=nameWithType>, um comportamento de serviço executa estas tarefas:

- Retorna um status de integridade do serviço com códigos de resposta HTTP. Em uma cadeia de consulta, você pode especificar o código de status HTTP para uma solicitação de investigação de integridade HTTP/GET.

- Publica informações sobre a integridade do serviço. Os detalhes específicos do serviço, incluindo o estado do serviço, contagens de aceleração e capacidade, podem ser exibidos usando uma solicitação HTTP/GET com a cadeia de consulta `?health`. A facilidade de acesso a essas informações é importante ao solucionar problemas de um serviço WCF com comportamento inadequado.

Há duas maneiras de expor o ponto de extremidade da integridade e publicar informações de integridade do serviço WCF:

- Percorrer o código. Por exemplo:

  ```csharp
  ServiceHost host = new ServiceHost(typeof(Service1),
                     new Uri("http://contoso:81/Service1"));
  ServiceHealthBehavior healthBehavior =
      host.Description.Behaviors.Find<ServiceHealthBehavior>();
  healthBehavior ??= new ServiceHealthBehavior();
  host.Description.Behaviors.Add(healthBehavior);
  ```

  ```vb
  Dim host As New ServiceHost(GetType(Service1),
              New Uri("http://contoso:81/Service1"))
  Dim healthBehavior As ServiceHealthBehavior =
     host.Description.Behaviors.Find(Of ServiceHealthBehavior)()
  If healthBehavior Is Nothing Then
     healthBehavior = New ServiceHealthBehavior()
  End If
  host.Description.Behaviors.Add(healthBehavior)
  ```

- Usando um arquivo de configuração. Por exemplo:

  ```xml
  <behaviors>
    <serviceBehaviors>
      <behavior name="DefaultBehavior">
        <serviceHealth httpsGetEnabled="true"/>
      </behavior>
    </serviceBehaviors>
  </behaviors>
  ```

O status de integridade de um serviço pode ser consultado usando parâmetros de consulta como `OnServiceFailure`, `OnDispatcherFailure`, `OnListenerFailure`, `OnThrottlePercentExceeded`, e um código de resposta HTTP pode ser especificado para cada parâmetro de consulta. Se o código de resposta HTTP for omitido para um parâmetro de consulta, será usado um código de resposta HTTP 503 por padrão. Por exemplo:

- OnServiceFailure: `https://contoso:81/Service1?health&OnServiceFailure=450`

  Retorna um código de status de resposta HTTP 450 quando [ServiceHost.State](xref:System.ServiceModel.Channels.CommunicationObject.State) é maior que <xref:System.ServiceModel.CommunicationState.Opened?displayProperty=nameWithType>.
Parâmetros de consulta e exemplos:

- OnDispatcherFailure: `https://contoso:81/Service1?health&OnDispatcherFailure=455`

  Retorna um código de status de resposta HTTP 455 quando o estado de qualquer um dos dispatchers de canal é maior que <xref:System.ServiceModel.CommunicationState.Opened?displayProperty=nameWithType>.

- OnListenerFailure: `https://contoso:81/Service1?health&OnListenerFailure=465`

  Retorna um código de status de resposta HTTP 465 quando o estado de qualquer um dos ouvintes de canais é maior que <xref:System.ServiceModel.CommunicationState.Opened?displayProperty=nameWithType>.

- OnThrottlePercentExceeded: `https://contoso:81/Service1?health&OnThrottlePercentExceeded= 70:350,95:500`

  Especifica a porcentagem {1 a 100} que dispara a resposta e seu código de resposta HTTP {200 a 599}. Neste exemplo:

  - Se a porcentagem é maior que 95, retorna um código de resposta HTTP 500.

  - Se a porcentagem está entre 70 e 95, retorna 350.

  - Caso contrário, retorna 200.

O status da integridade do serviço pode ser exibido em HTML com a especificação de uma cadeia de consulta como `https://contoso:81/Service1?health` ou em XML com a especificação de uma cadeia de consulta como `https://contoso:81/Service1?health&Xml`. Uma cadeia de consulta como `https://contoso:81/Service1?health&NoContent` retorna uma página HTML vazia.

<a name="wpf48"></a>

#### <a name="windows-presentation-foundation-wpf"></a>Windows Presentation Foundation (WPF)

**Aprimoramentos de alto DPI**

No .NET Framework 4.8, o WPF adiciona suporte para Reconhecimento de DPI V2 por Monitor e ajuste de DPI no Modo Misto. Consulte [Desenvolvimento de aplicativo de área de trabalho com alto DPI no Windows](/windows/win32/hidpi/high-dpi-desktop-application-development-on-windows) para saber mais sobre o desenvolvimento com alto DPI.

O .NET Framework 4.8 melhora o suporte para interoperação hospedada de HWNDs e do Windows Forms em aplicativos WPF com alto DPI em plataformas compatíveis com o ajuste do DPI no Modo Misto (começando com a Atualização de abril de 2018 do Windows 10). Quando os controles hospedados de HWNDs ou do Windows Forms são criados como janelas em escala de DPI no Modo Misto chamando [SetThreadDpiHostingBehavior](/windows/desktop/api/winuser/nf-winuser-setthreaddpihostingbehavior) e [SetThreadDpiAwarenessContext](/windows/desktop/api/winuser/nf-winuser-setthreaddpiawarenesscontext), eles podem ser hospedados em um aplicativo WPF V2 por Monitor e são dimensionados adequadamente. Tal conteúdo hospedado não é processado no DPI nativo; em vez disso, o sistema operacional ajusta o conteúdo hospedado para o tamanho apropriado. O suporte para o modo de reconhecimento de DPI V2 por Monitor também permite que os controles WPF sejam hospedados (isto é, tenham pai) em uma janela nativa em um aplicativo com alto DPI.

Para habilitar o suporte para ajuste de Alto DPI no Modo Misto, você pode definir os seguintes parâmetros [AppContext](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) no arquivo de configuração de aplicativo:

```xml
<runtime>
   <AppContextSwitchOverrides value = "Switch.System.Windows.DoNotScaleForDpiChanges=false; Switch.System.Windows.DoNotUsePresentationDpiCapabilityTier2OrGreater=false"/>
</runtime>
```

<a name="clr48"></a>

#### <a name="common-language-runtime"></a>Common Language Runtime

O runtime no .NET Framework 4.8 inclui as seguintes mudanças e melhorias:

**Melhorias no compilador JIT**. O compilador JIT (Just-in-time) no .NET Framework 4.8 baseia-se no compilador JIT do .NET Core 2.1. Muitas das otimizações e todas as correções de bugs feitas no compilador JIT do .NET Core 2.1 estão inclusas no compilador JIT do .NET Framework 4.8.

**Aprimoramentos do NGEN**. O runtime melhorou o gerenciamento de memória para imagens NGEN ([Gerador de Imagem Nativa](../tools/ngen-exe-native-image-generator.md)) para que os dados mapeados das imagens NGEN não residam na memória. Isso reduz a área de superfície disponível para ataques que tentam executar código arbitrário modificando a memória a ser executada.

**Verificação antimalware em todos os assemblies**. Em versões anteriores do .NET Framework, o runtime examina todos os assemblies carregados do disco usando o Windows Defender ou software antimalware de terceiros. No entanto, os assemblies carregados de outras fontes, como pelo método <xref:System.Reflection.Assembly.Load(System.Byte[])?displayProperty=nameWithType>, não são examinados e podem conter malware não detectado. A partir do .NET Framework 4.8 em execução no Windows 10, o runtime dispara uma verificação por soluções antimalware que implementam a [AMSI (Interface de Verificação Antimalware)](/windows/desktop/AMSI/antimalware-scan-interface-portal).

<a name="v472"></a>

## <a name="whats-new-in-net-framework-472"></a>Novidades no .NET Framework 4.7.2

O .NET Framework 4.7.2 inclui novos recursos nas seguintes áreas:

- [Classes base](#core-472)
- [ASP.NET](#asp-net472)
- [Rede](#net472)
- [SQL](#sql472)
- [WPF](#wpf472)
- [ClickOnce](#clickonce)

Um dos principais objetivos do .NET Framework 4.7.2 é a melhoria de acessibilidade, que permite a um aplicativo fornecer uma experiência apropriada para os usuários da Tecnologia Assistencial. Para saber mais sobre melhorias de acessibilidade no .NET Framework 4.7.2,consulte [Novidades de acessibilidade no .NET Framework](whats-new-in-accessibility.md).

<a name="core-472"></a>

#### <a name="base-classes"></a>Classes base

O .NET Framework 4.7.2 apresenta um grande número de aperfeiçoamentos de criptografia, melhor suporte à descompactação de arquivos ZIP e APIs de coleção adicionais.

**Novas sobrecargas de RSA.Create e DSA.Create**

Os métodos <xref:System.Security.Cryptography.DSA.Create(System.Security.Cryptography.DSAParameters)?displayProperty=nameWithType> e <xref:System.Security.Cryptography.RSA.Create(System.Security.Cryptography.RSAParameters)?displayProperty=nameWithType> permitem fornecer parâmetros de chave ao instanciar uma nova chave <xref:System.Security.Cryptography.DSA> ou <xref:System.Security.Cryptography.RSA>. Elas permitem substituir o código desta forma:

```csharp
// Before .NET Framework 4.7.2
using (RSA rsa = RSA.Create())
{
   rsa.ImportParameters(rsaParameters);
   // Other code to execute using the RSA instance.
}
```

```vb
' Before .NET Framework 4.7.2
Using rsa = RSA.Create()
   rsa.ImportParameters(rsaParameters)
   ' Other code to execute using the rsa instance.
End Using
```

com este código:

```csharp
// Starting with .NET Framework 4.7.2
using (RSA rsa = RSA.Create(rsaParameters))
{
   // Other code to execute using the rsa instance.
}
```

```vb
' Starting with .NET Framework 4.7.2
Using rsa = RSA.Create(rsaParameters)
   ' Other code to execute using the rsa instance.
End Using
```

Os métodos <xref:System.Security.Cryptography.DSA.Create(System.Int32)?displayProperty=nameWithType> e <xref:System.Security.Cryptography.RSA.Create(System.Int32)?displayProperty=nameWithType> permitem gerar chaves <xref:System.Security.Cryptography.DSA> ou <xref:System.Security.Cryptography.RSA> novas, com um tamanho específico. Por exemplo:

```csharp
using (DSA dsa = DSA.Create(2048))
{
   // Other code to execute using the dsa instance.
}
```

```vb
Using dsa = DSA.Create(2048)
   ' Other code to execute using the dsa instance.
End Using
```

**Os construtores Rfc2898DeriveBytes aceitam um nome de algoritmo de hash**

A classe <xref:System.Security.Cryptography.Rfc2898DeriveBytes> tem três novos construtores com um parâmetro <xref:System.Security.Cryptography.HashAlgorithmName> que identifica o algoritmo HMAC a ser usado na derivação de chaves. Em vez de usar o SHA-1, os desenvolvedores devem usar um HMAC baseado em SHA-2, como o SHA-256, conforme mostrado no exemplo a seguir:

```csharp
private static byte[] DeriveKey(string password, out int iterations, out byte[] salt,
                                out HashAlgorithmName algorithm)
{
   iterations = 100000;
   algorithm = HashAlgorithmName.SHA256;

   const int SaltSize = 32;
   const int DerivedValueSize = 32;

   using (Rfc2898DeriveBytes pbkdf2 = new Rfc2898DeriveBytes(password, SaltSize,
                                                             iterations, algorithm))
   {
      salt = pbkdf2.Salt;
      return pbkdf2.GetBytes(DerivedValueSize);
   }
}
```

```vb
Private Shared Function DeriveKey(password As String, ByRef iterations As Integer,
                                  ByRef salt AS Byte(), ByRef algorithm As HashAlgorithmName) As Byte()
   iterations = 100000
   algorithm = HashAlgorithmName.SHA256

   Const SaltSize As Integer = 32
   Const  DerivedValueSize As Integer = 32

   Using pbkdf2 = New Rfc2898DeriveBytes(password, SaltSize, iterations, algorithm)
      salt = pbkdf2.Salt
      Return pbkdf2.GetBytes(DerivedValueSize)
   End Using
End Function
```

**Compatibilidade com chaves efêmeras**

Opcionalmente, a importação PFX pode carregar chave privada direto da memória, ignorando o disco rígido.Quando o novo sinalizador <xref:System.Security.Cryptography.X509Certificates.X509KeyStorageFlags.EphemeralKeySet?displayProperty=nameWithType> é especificado em um construtor <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> ou uma das sobrecargas do método <xref:System.Security.Cryptography.X509Certificates.X509Certificate2.Import%2A?displayProperty=nameWithType>, as chaves privadas serão carregadas como chaves efêmeras. Isso impede as chaves de ficarem visíveis no disco. No entanto:

- Como as chaves não são persistidas no disco, os certificados carregados com esse sinalizador não são bons candidatos para serem adicionados a um X509Store.

- As chaves carregadas dessa maneira são quase sempre carregadas pelo CNG do Windows. Portanto, os chamadores precisam acessar a chave privada chamando métodos de extensão, como [cert.GetRSAPrivateKey()](xref:System.Security.Cryptography.X509Certificates.RSACertificateExtensions.GetRSAPrivateKey%2A). A propriedade <xref:System.Security.Cryptography.X509Certificates.X509Certificate2.PrivateKey?displayProperty=nameWithType> não funciona.

- Como a propriedade herdada <xref:System.Security.Cryptography.X509Certificates.X509Certificate2.PrivateKey?displayProperty=nameWithType> não funciona com certificados, os desenvolvedores precisam executar testes rigorosos antes de alternar para chaves efêmeras.

**Criação programática de solicitações de assinatura de certificado PKCS#10 e certificados de chave pública X.509**

A partir do .NET Framework 4.7.2, as cargas de trabalho podem gerar CSRs (solicitações de assinatura de certificado), o que permite que a geração de solicitação de certificados seja realizada nas ferramentas existentes. Frequentemente, isso é útil em cenários de teste.

Para saber mais e obter exemplos de código, veja "Criação programática de solicitações de assinatura de certificado PKCS#10 e certificados de chave pública X.509" no [Blog do .NET](https://devblogs.microsoft.com/dotnet/net-framework-4-7-2-developer-pack-early-access-build-3056-is-available/).

**Novos membros do SignerInfo**

A partir do .NET Framework 4.7.2, a classe <xref:System.Security.Cryptography.Pkcs.SignerInfo> expõe mais informações sobre a assinatura. É possível recuperar o valor da propriedade <xref:System.Security.Cryptography.Pkcs.SignerInfo.SignatureAlgorithm?displayProperty=fullName> para determinar o algoritmo de assinatura usado pelo signatário. <xref:System.Security.Cryptography.Pkcs.SignerInfo.GetSignature%2A?displayProperty=nameWithType> pode ser chamado para obter uma cópia da assinatura de criptografia desse signatário.

**Deixar um fluxo encapsulado aberto depois de descartar CryptoStream**

A partir do .NET Framework 4.7.2, a classe <xref:System.Security.Cryptography.CryptoStream> tem um construtor adicional que permite que <xref:System.Security.Cryptography.CryptoStream.Dispose%2A> não feche o fluxo encapsulado.Para manter o fluxo encapsulado aberto após o descarte da instância <xref:System.Security.Cryptography.CryptoStream>, chame o novo construtor <xref:System.Security.Cryptography.CryptoStream> da seguinte maneira:

```csharp
var cStream = new CryptoStream(stream, transform, mode, leaveOpen: true);
```

```vb
Dim cStream = New CryptoStream(stream, transform, mode, leaveOpen:=true)
```

**Alterações de descompactação em DeflateStream**

A partir do .NET Framework 4.7.2, a implementação de operações de descompactação na classe <xref:System.IO.Compression.DeflateStream> foi alterada para usar APIs nativas do Windows por padrão. Normalmente, isso resulta em uma melhoria significativa de desempenho.

A compatibilidade para descompactação por meio das APIs do Windows está habilitada por padrão em aplicativos direcionados ao .NET Framework 4.7.2. Os aplicativos direcionados a versões anteriores do .NET Framework, mas que são executados no .NET Framework 4.7.2 podem aceitar esse comportamento adicionando a [opção AppContext](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) ao arquivo de configuração de aplicativo:

```xml
<AppContextSwitchOverrides value="Switch.System.IO.Compression.DoNotUseNativeZipLibraryForDecompression=false" />
```

**APIs de coleção adicionais**

O .NET Framework 4.7.2 adiciona uma série de novas APIs aos tipos <xref:System.Collections.Generic.SortedSet%601> e <xref:System.Collections.Generic.HashSet%601>. Eles incluem:

- métodos `TryGetValue`, o que estende o padrão try usado em outros tipos de coleção. Os métodos são:

  - [public bool HashSet\<T>.TryGetValue(T equalValue, out T actualValue)](xref:System.Collections.Generic.SortedSet%601.TryGetValue%2A)
  - [public bool SortedSet\<T>.TryGetValue(T equalValue, out T actualValue)](xref:System.Collections.Generic.SortedSet%601.TryGetValue%2A)

- Métodos de extensão `Enumerable.To*`, o que converte uma coleção em <xref:System.Collections.Generic.HashSet%601>:

  - [HashSet público estático\<TSource> ToHashSet\<TSource>(esta origem IEnumerable\<TSource>)](xref:System.Linq.Enumerable.ToHashSet%2A)
  - [HashSet estático público\<TSource> ToHashSet\<TSource>(esta origem IEnumerable\<TSource>, comparador IEqualityComparer\<TSource>)](xref:System.Linq.Enumerable.ToHashSet%2A)

- Novos construtores <xref:System.Collections.Generic.HashSet%601> que permitem definir a capacidade da coleção, o que gera um benefício de desempenho quando o tamanho de <xref:System.Collections.Generic.HashSet%601> é conhecido com antecedência:

  - [HashSet(int capacity) público](xref:System.Collections.Generic.HashSet%601.%23ctor(System.Int32))
  - [HashSet(int capacity público, comparador IEqualityComparer\<T>)](xref:System.Collections.Generic.HashSet%601.%23ctor(System.Int32,System.Collections.Generic.IEqualityComparer%7B%600%7D))

A classe <xref:System.Collections.Concurrent.ConcurrentDictionary%602> inclui novas sobrecargas dos métodos <xref:System.Collections.Concurrent.ConcurrentDictionary%602.AddOrUpdate%2A> e <xref:System.Collections.Concurrent.ConcurrentDictionary%602.GetOrAdd%2A> para recuperar um valor do dicionário ou adicioná-lo caso ele não seja encontrado e para adicionar um valor ao dicionário ou atualizá-lo caso ele já exista.

```csharp
public TValue AddOrUpdate<TArg>(TKey key, Func<TKey, TArg, TValue> addValueFactory, Func<TKey, TValue, TArg, TValue> updateValueFactory, TArg factoryArgument)

public TValue GetOrAdd<TArg>(TKey key, Func<TKey, TArg, TValue> valueFactory, TArg factoryArgument)
```

```vb
Public AddOrUpdate(Of TArg)(key As TKey, addValueFactory As Func(Of TKey, TArg, TValue), updateValueFactory As Func(Of TKey, TValue, TArg, TValue), factoryArgument As TArg) As TValue

Public GetOrAdd(Of TArg)(key As TKey, valueFactory As Func(Of TKey, TArg, TValue), factoryArgument As TArg) As TValue
```

<a name="asp-net472"></a>

#### <a name="aspnet"></a>ASP.NET

**Suporte para injeção de dependência no Web Forms**

A [DI (injeção de dependência)](/aspnet/core/fundamentals/dependency-injection#overview-of-dependency-injection) separa os objetos de suas dependências para que o código do objeto não precise mais ser alterado somente porque uma dependência foi modificada. Ao desenvolver aplicativos ASP.NET direcionados para o .NET Framework 4.7.2, é possível:

- Use uma injeção baseada em setter, interfaces e construtores nos [manipuladores e módulos](https://docs.microsoft.com/previous-versions/aspnet/bb398986(v=vs.100)), [instâncias Página](xref:System.Web.UI.Page) e [controles de usuário](https://docs.microsoft.com/previous-versions/aspnet/y6wb1a0e(v=vs.100)) de projetos de aplicativo Web ASP.NET.

- Use uma injeção baseada em setter e interface nos [manipuladores e módulos](https://docs.microsoft.com/previous-versions/aspnet/bb398986(v=vs.100)), [instâncias Página](xref:System.Web.UI.Page) e [controles de usuário](https://docs.microsoft.com/previous-versions/aspnet/y6wb1a0e(v=vs.100)) de projetos de site da Web do ASP.NET.

- Conecte estruturas de injeção de dependência diferentes.

**Compatibilidade com cookies do mesmo site**

O [SameSite](https://tools.ietf.org/html/draft-west-first-party-cookies-07) impede que o navegador envie um cookie junto com a solicitação entre sites. O .NET Framework 4.7.2 adiciona uma propriedade <xref:System.Web.HttpCookie.SameSite?displayProperty=nameWithType> cujo valor é um membro da enumeração <xref:System.Web.SameSiteMode?displayProperty=nameWithType>. Se o valor for <xref:System.Web.SameSiteMode.Strict?displayProperty=nameWithType> ou <xref:System.Web.SameSiteMode.Lax?displayProperty=nameWithType>, o ASP.NET adicionará o atributo `SameSite` ao cabeçalho set-cookie. A compatibilidade com o SameSite se aplica aos objetos <xref:System.Web.HttpCookie>, bem como aos cookies <xref:System.Web.Security.FormsAuthentication> e <xref:System.Web.SessionState>.

É possível definir o SameSite para um objeto <xref:System.Web.HttpCookie> da seguinte maneira:

```csharp
var c = new HttpCookie("secureCookie", "same origin");
c.SameSite = SameSiteMode.Lax;
```

```vb
Dim c As New HttpCookie("secureCookie", "same origin")
c.SameSite = SameSiteMode.Lax
```

Também é possível configurar cookies SameSite no nível do aplicativo modificando o arquivo web.config:

```xml
<system.web>
   <httpCookies sameSite="Strict" />
</system.web>
```

É possível adicionar SameSite a cookies <xref:System.Web.Security.FormsAuthentication> e <xref:System.Web.SessionState> modificando o arquivo de configuração Web:

```xml
<system.web>
   <authentication mode="Forms">
      <forms cookieSameSite="Lax">
         <!-- ...   -->
      </forms>
   </authentication>
   <sessionState cookieSameSite="Lax"></sessionState>
</system.web>
```

<a name="net472"></a>

#### <a name="networking"></a>Rede

**Implementação de propriedades HttpClientHandler**

O .NET Framework 4.7.1 adicionou oito propriedades à classe <xref:System.Net.Http.HttpClientHandler?displayProperty=nameWithType>. No entanto, duas geraram um <xref:System.PlatformNotSupportedException>. Agora, o .NET Framework 4.7.2 fornece uma implementação dessas propriedades. As propriedades são:

- <xref:System.Net.Http.HttpClientHandler.CheckCertificateRevocationList>
- <xref:System.Net.Http.HttpClientHandler.SslProtocols>

<a name="sql472"></a>

#### <a name="sqlclient"></a>SQLClient

**Compatibilidade com a Autenticação Universal e Autenticação Multifator do Azure Active Directory**

Demandas crescente de conformidade e segurança exigem que muitos clientes usem a MFA (autenticação multifator). Além disso, as melhores práticas atuais não incentivam a inclusão de senhas do usuário diretamente em cadeias de conexão. Para dar suporte a essas alterações, o .NET Framework 4.7.2 estende as [cadeias de conexão SQLClient](xref:System.Data.SqlClient.SqlConnection.ConnectionString) adicionando um novo valor, "Active Directory Interactive", à palavra-chave "Authentication" existente, para ser compatível com a MFA e a [Autenticação do Azure Active Directory](/azure/sql-database/sql-database-aad-authentication-configure). O novo método interativo é compatível com usuários nativos e federados do Azure AD, bem como os usuários convidados do Azure AD. Quando esse método é usado, a MFA imposta pelo Azure AD é compatível com bancos de dados SQL. Além disso, o processo de autenticação solicita uma senha de usuário para aderir às melhores práticas de segurança.

Em versões anteriores do .NET Framework, conectividade SQL é compatível apenas com as opções <xref:System.Data.SqlClient.SqlAuthenticationMethod.ActiveDirectoryPassword?displayProperty=nameWithType> e <xref:System.Data.SqlClient.SqlAuthenticationMethod.ActiveDirectoryIntegrated?displayProperty=nameWithType>. Ambos são parte do [protocolo ADAL](/azure/active-directory/develop/active-directory-authentication-libraries) não interativo, que não é compatível com o MFA. Com a nova opção <xref:System.Data.SqlClient.SqlAuthenticationMethod.ActiveDirectoryInteractive?displayProperty=nameWithType>, a conectividade SQL é compatível com a MFA, bem como com métodos de autenticação existentes (senha e autenticação integrada), o que permite que os usuários insiram senhas de forma interativa, sem persistir senhas na cadeia de conexão.

Para obter mais informações e um exemplo, veja "Suporte Universal e de Autenticação Multifator para SQL – Azure AD", no [Blog do .NET](https://devblogs.microsoft.com/dotnet/net-framework-4-7-2-developer-pack-early-access-build-3056-is-available/).

**Compatibilidade com a versão 2 do Always Encrypted**

O NET Framework 4.7.2 acrescenta suporte para o Always Encrypted com base em enclave. A versão original do Always Encrypted é uma tecnologia de criptografia do lado do cliente em que as chaves de criptografia nunca deixam o cliente. No Always Encrypted com base em enclave, o cliente tem a opção de enviar as chaves de criptografia para um enclave seguro, que é uma entidade computacional segura que pode ser considerada parte do SQL Server, mas que o código do SQL Server não pode modificar. Para ser compatível com o Always Encrypted com base em enclave, o .NET Framework 4.7.2 acrescenta os seguintes tipos e membros ao namespace <xref:System.Data.SqlClient>:

- <xref:System.Data.SqlClient.SqlConnectionStringBuilder.EnclaveAttestationUrl?displayProperty=nameWithType>, que especifica o URI para o Always Encrypted com base em enclave.

- <xref:System.Data.SqlClient.SqlColumnEncryptionEnclaveProvider>, que é uma classe abstrata da qual todos os provedores de enclave são derivados.

- <xref:System.Data.SqlClient.SqlEnclaveSession>, que encapsula o estado de uma determinada sessão de enclave.

- <xref:System.Data.SqlClient.SqlEnclaveAttestationParameters>, que fornece os parâmetros de atestado usados pelo SQL Server para obter as informações necessárias para executar um determinado Protocolo de Atestado.

O arquivo de configuração de aplicativo, em seguida, especifica uma implementação concreta da classe abstrata <xref:System.Data.SqlClient.SqlColumnEncryptionEnclaveProvider?displayProperty=nameWithType> que fornece a funcionalidade para o provedor de enclave. Por exemplo:

```xml
<configuration>
  <configSections>
    <section name="SqlColumnEncryptionEnclaveProviders" type="System.Data.SqlClient.SqlColumnEncryptionEnclaveProviderConfigurationSection,System.Data,Version=4.0.0.0,Culture=neutral,PublicKeyToken=b77a5c561934e089"/>
  </configSections>
  <SqlColumnEncryptionEnclaveProviders>
    <providers>
      <add name="Azure" type="Microsoft.SqlServer.Management.AlwaysEncrypted.AzureEnclaveProvider,MyApp"/>
      <add name="HGS" type="Microsoft.SqlServer.Management.AlwaysEncrypted.HGSEnclaveProvider,MyApp" />
    </providers>
  </SqlColumnEncryptionEnclaveProviders >
</configuration>
```

O fluxo básico do Always Encrypted com base em enclave é:

1. O usuário cria uma conexão do AlwaysEncrypted com o SQL Server compatível com o Always Encrypted com base em enclave. O driver entra em contato com o serviço de atestado para garantir que ele está se conectando ao enclave correto.

1. Depois da confirmação do enclave, o driver estabelece um canal seguro com o enclave seguro hospedado no SQL Server.

1. O driver compartilha as chaves de criptografia autorizadas pelo cliente com o enclave seguro durante a conexão do SQL.

<a name="wpf472"></a>

#### <a name="windows-presentation-foundation"></a>Windows Presentation Foundation

**Localizar ResourceDictionaries por origem**

A partir do .NET Framework 4.7.2, um assistente de diagnóstico pode localizar o  <xref:System.Windows.Xps.Packaging.IXpsFixedPageReader.ResourceDictionaries> criado de um determinado URI de origem.(Esse recurso é para uso pelos assistentes de diagnóstico, não por aplicativos de produção.) Um assistente de diagnóstico como o recurso "Editar e continuar" do Visual Studio permite que seu usuário edite um ResourceDictionary com a intenção de que as alterações sejam aplicadas ao aplicativo em execução. Uma etapa para conseguir isso é encontrar todos os ResourceDictionaries que o aplicativo em execução criou a partir do dicionário que está sendo editado. Por exemplo, um aplicativo pode declarar um ResourceDictionary cujo conteúdo é copiado de um determinado URI de origem:

```xml
<ResourceDictionary Source="MyRD.xaml" />
```

Um assistente de diagnóstico que edita a marcação original em *MyRD. XAML*   pode usar o novo recurso para localizar o dicionário.O recurso é implementado por um novo método estático, <xref:System.Windows.Diagnostics.ResourceDictionaryDiagnostics.GetResourceDictionariesForSource%2A?displayProperty=nameWithType>. O assistente de diagnóstico chama o novo método usando um URI absoluto que identifica a marcação original, conforme ilustrado pelo código a seguir:

```csharp
IEnumerable<ResourceDictionary> dictionaries = ResourceDictionaryDiagnostics.GetResourceDictionariesForSource(new Uri("pack://application:,,,/MyApp;component/MyRD.xaml"));
```

```vb
Dim dictionaries As IEnumerable(Of ResourceDictionary) = ResourceDictionaryDiagnostics.GetResourceDictionariesForSource(New Uri("pack://application:,,,/MyApp;component/MyRD.xaml"))
```

O método retorna um Enumerable vazio, a menos que  <xref:System.Windows.Diagnostics.VisualDiagnostics> esteja habilitado e a [`ENABLE_XAML_DIAGNOSTICS_SOURCE_INFO`](xref:System.Windows.Diagnostics.VisualDiagnostics.GetXamlSourceInfo%2A)   variável de ambiente esteja definida.

**Localizar os proprietários de ResourceDictionary**

A partir do .NET Framework 4.7.2, um assistente de diagnóstico pode localizar os proprietários de um determinado <xref:Windows.UI.Xaml.ResourceDictionary>.(O recurso é para uso por assistentes de diagnóstico e não por aplicativos de produção.) Sempre que uma alteração é feita em um <xref:Windows.UI.Xaml.ResourceDictionary> , o WPF localiza automaticamente todas as referências [DynamicResource](../wpf/advanced/dynamicresource-markup-extension.md) que podem ser afetadas pela alteração.

Um assistente de diagnóstico como a facilidade de "Editar e continuar" do Visual Studio pode querer estendê-lo para manipular referências [StaticResource](../wpf/advanced/staticresource-markup-extension.md) . A primeira etapa nesse processo é localizar os proprietários do dicionário; ou seja, para localizar todos os objetos cuja propriedade `Resources` se refere ao dicionário (direta ou indiretamente por meio da propriedade <xref:System.Windows.ResourceDictionary.MergedDictionaries?displayProperty=nameWithType>). Três novos métodos estáticos implementados na classe <xref:System.Windows.Diagnostics.ResourceDictionaryDiagnostics?displayProperty=nameWithType>, um para cada um dos tipos base que tem uma propriedade `Resources`, compatível com esta etapa:

- [`public static IEnumerable<FrameworkElement> GetFrameworkElementOwners(ResourceDictionary dictionary);`](xref:System.Windows.Diagnostics.ResourceDictionaryDiagnostics.GetFrameworkElementOwners%2A)

- [`public static IEnumerable<FrameworkContentElement> GetFrameworkContentElementOwners(ResourceDictionary dictionary);`](xref:System.Windows.Diagnostics.ResourceDictionaryDiagnostics.GetFrameworkContentElementOwners%2A)

- [`public static IEnumerable<Application> GetApplicationOwners(ResourceDictionary dictionary);`](xref:System.Windows.Diagnostics.ResourceDictionaryDiagnostics.GetApplicationOwners%2A)

Esses métodos retornam um Enumerable vazio, a menos que  <xref:System.Windows.Diagnostics.VisualDiagnostics> esteja habilitado e a [`ENABLE_XAML_DIAGNOSTICS_SOURCE_INFO`](xref:System.Windows.Diagnostics.VisualDiagnostics.GetXamlSourceInfo%2A)   variável de ambiente esteja definida.

**Localizar referências StaticResource**

Agora, um assistente de diagnóstico pode receber uma notificação sempre que uma referência [StaticResource](../wpf/advanced/staticresource-markup-extension.md) for resolvida.(O recurso é para uso pelos assistentes de diagnóstico, não por aplicativos de produção.) Um assistente de diagnóstico como o recurso "Editar e continuar" do Visual Studio pode querer atualizar todos os usos de um recurso quando seu valor em uma <xref:Windows.UI.Xaml.ResourceDictionary> alteração. O WPF faz isso automaticamente para referências [DynamicResource](../wpf/advanced/dynamicresource-markup-extension.md), mas ele intencionalmente não o faz para referências [StaticResource](../wpf/advanced/staticresource-markup-extension.md). A partir do .NET Framework 4.7.2, o assistente de diagnóstico pode usar essas notificações para localizar os usos do recurso estático.

A notificação é implementada pelo novo evento <xref:System.Windows.Diagnostics.ResourceDictionaryDiagnostics.StaticResourceResolved?displayProperty=nameWithType>:

```csharp
public static event EventHandler<StaticResourceResolvedEventArgs> StaticResourceResolved;
```

```vb
Public Shared Event StaticResourceResolved As EventHandler(Of StaticResourceResolvedEventArgs)
```

Esse evento é gerado sempre que o runtime resolve uma referência [StaticResource](../wpf/advanced/staticresource-markup-extension.md).Os argumentos <xref:System.Windows.Diagnostics.StaticResourceResolvedEventArgs> descrevem a resolução e indicam o objeto e a propriedade que hospedam a referência [StaticResource](../wpf/advanced/staticresource-markup-extension.md), o  <xref:Windows.UI.Xaml.ResourceDictionary> e a chave usada para a resolução:

```csharp
public class StaticResourceResolvedEventArgs : EventArgs
{
   public Object TargetObject { get; }

   public Object TargetProperty { get; }

   public ResourceDictionary ResourceDictionary { get; }

   public object ResourceKey { get; }
}
```

```vb
Public Class StaticResourceResolvedEventArgs : Inherits EventArgs
   Public ReadOnly Property TargetObject As Object
   Public ReadOnly Property TargetProperty As Object
   Public ReadOnly Property ResourceDictionary As ResourceDictionary
   Public ReadOnly Property ResourceKey As Object
End Class
```

O evento não é gerado (e seu `add` acessador é ignorado), a menos que  <xref:System.Windows.Diagnostics.VisualDiagnostics> esteja habilitado e a [`ENABLE_XAML_DIAGNOSTICS_SOURCE_INFO`](xref:System.Windows.Diagnostics.VisualDiagnostics.GetXamlSourceInfo%2A)   variável de ambiente esteja definida.

#### <a name="clickonce"></a>ClickOnce

Todos os aplicativos com reconhecimento de HDPI para Windows Forms, WPF (Windows Presentation Foundation) e VSTO (Visual Studio Tools para Office) podem ser implantados usando o ClickOnce. Se a entrada a seguir for encontrada no manifesto do aplicativo, a implantação será bem-sucedida no .NET Framework 4.7.2:

```xml
<windowsSettings>
   <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
</windowsSettings>
```

Para o aplicativo do Windows Forms, a solução anterior de definir o reconhecimento de DPI no arquivo de configuração de aplicativo em vez de no manifesto do aplicativo não é mais necessária para a implantação pelo ClickOnce ser bem-sucedida.

<a name="v471"></a>

## <a name="whats-new-in-net-framework-471"></a>Novidades no .NET Framework 4.7.1

O .NET Framework 4.7.1 inclui novos recursos nas seguintes áreas:

- [Classes base](#core471)
- [CLR (Common Language Runtime)](#clr)
- [Rede](#net471)
- [ASP.NET](#asp-net471)

Além disso, um dos objetivos principais do .NET Framework 4.7.1 é a melhoria de acessibilidade, que permite a um aplicativo fornecer uma experiência apropriada para os usuários da Tecnologia Assistencial. Para saber mais sobre melhorias de acessibilidade no .NET Framework 4.7.1,consulte [Novidades de acessibilidade no .NET Framework](whats-new-in-accessibility.md).

<a name="core471"></a>

#### <a name="base-classes"></a>Classes base

**Suporte para .NET Standard 2.0**

O [.NET Standard](../../standard/net-standard.md) define um conjunto das APIs que precisam estar disponíveis em todas as implementações do .NET compatíveis com a versão do standard. O .NET Framework 4.7.1 dá suporte total ao .NET Standard 2.0 e adiciona [cerca de 200 APIs](https://github.com/dotnet/standard/blob/master/src/netstandard/src/ApiCompatBaseline.net461.txt) que são definidas no .NET Standard 2.0 e não estão presentes nas versões 4.6.1, 4.6.2 e 4.7 do .NET Framework. (Observe que essas versões do .NET Framework oferecem suporte a .NET Standard 2,0 somente se os arquivos de suporte .NET Standard adicionais também forem implantados no sistema de destino.) Para obter mais informações, consulte "suporte a BCL-.NET Standard 2,0" na postagem do blog [.NET Framework 4.7.1 tempo de execução e recursos do compilador](https://devblogs.microsoft.com/dotnet/net-framework-4-7-1-runtime-and-compiler-features/) .

**Suporte para construtores de configuração**

Construtores de configuração permitem aos desenvolvedores injetar e compilar definições de configuração para aplicativos dinamicamente no tempo de execução. Construtores de configuração personalizada podem ser usados para modificar os dados existentes em uma seção de configuração ou criar uma seção de configuração do zero. Sem construtores de configuração, arquivos .config são estáticos e suas configurações são definidas algum tempo antes de um aplicativo ser lançado.

Para criar um construtor de configuração personalizado, derive seu construtor da classe abstrata <xref:System.Configuration.ConfigurationBuilder> e substitua os <xref:System.Configuration.ConfigurationBuilder.ProcessConfigurationSection%2A?displayProperty=nameWithType> e <xref:System.Configuration.ConfigurationBuilder.ProcessRawXml%2A?displayProperty=nameWithType>. Você também pode definir seus construtores no arquivo .config. Para obter mais informações, consulte a seção “Construtores de configuração” na postagem de blog [ASP.NET e recursos de configuração do .NET Framework 4.7.1](https://devblogs.microsoft.com/dotnet/net-framework-4-7-1-asp-net-and-configuration-features/).

**Detecção de recurso de tempo de execução**

A classe <xref:System.Runtime.CompilerServices.RuntimeFeature?displayProperty=nameWithType> fornece um mecanismo para determinar se há suporte para um recurso predefinido em determinada implementação .NET no tempo de compilação ou no tempo de execução. No tempo de compilação, um compilador pode verificar se um campo especificado existe para determinar se há suporte para o recurso; se houver, ele pode emitir um código que aproveita tal recurso. No runtime, um aplicativo pode chamar o método <xref:System.Runtime.CompilerServices.RuntimeFeature.IsSupported%2A?displayProperty=nameWithType> antes de emitir o código no runtime. Para obter mais informações, consulte [Adicionar método auxiliar para descrever os recursos com suporte no runtime](https://github.com/dotnet/corefx/issues/17116).

**Tipos de tupla de valor são serializáveis**

A partir do .NET Framework 4.7.1, <xref:System.ValueTuple?displayProperty=nameWithType> e seus tipos genéricos associados são marcados como [Serializable](xref:System.SerializableAttribute), o que permite a serialização binária. Isso deve facilitar bastante a migração de tipos de Tupla, como <xref:System.Tuple%603> e <xref:System.Tuple%604>, em tipos de tupla de valor. Para obter mais informações, consulte “Compilador -- ValueTuple é serializável” na postagem de blog [Runtime e recursos do compilador do .NET Framework 4.7.1](https://devblogs.microsoft.com/dotnet/net-framework-4-7-1-runtime-and-compiler-features/).

**Suporte para referências somente leitura**

O .NET Framework 4.7.1 adiciona o <xref:System.Runtime.CompilerServices.IsReadOnlyAttribute?displayProperty=nameWithType>. Este atributo é usado por compiladores de linguagem para marcar membros que têm parâmetros ou tipos de retorno somente leitura de referência. Para obter mais informações, consulte “Compilador -- Suporte a ReadOnlyReferences” na postagem de blog [Runtime e recursos do compilador do .NET Framework 4.7.1](https://devblogs.microsoft.com/dotnet/net-framework-4-7-1-runtime-and-compiler-features/). Para saber mais sobre valores retornados de referência, consulte [Valores retornados de referência e locais de referência (Guia de C#)](../../csharp/programming-guide/classes-and-structs/ref-returns.md) e [Valores retornados de referência (Visual Basic)](../../visual-basic/programming-guide/language-features/procedures/ref-return-values.md).

<a name="clr"></a>

#### <a name="common-language-runtime-clr"></a>CLR (Common Language Runtime)

**Melhorias de desempenho de coleta de lixo**

Alterações na coleta de lixo (GC) no .NET Framework 4.7.1 melhoram o desempenho geral, especialmente para alocações de LOH (Large Object heap). No .NET Framework 4.7.1, bloqueios separados são usados para a SOH (heap de objeto pequeno) e as alocações de LOH, o que permite que as alocações de LOH ocorram quando o GC em segundo plano está varrendo a SOH. Como resultado, os aplicativos que compõem um grande número de alocações de LOH devem observar uma redução na contenção de bloqueio de alocação e melhoria no desempenho. Para obter mais informações, consulte a seção “Runtime -- Melhorias de desempenho do GC” na postagem de blog [Runtime e recursos do compilador do .NET Framework 4.7.1](https://devblogs.microsoft.com/dotnet/net-framework-4-7-1-runtime-and-compiler-features/).

<a name="net471"/>

#### <a name="networking"></a>Rede

**Suporte a SHA-2 Message.HashAlgorithm**

No .NET Framework 4.7 e versões anteriores, a propriedade <xref:System.Messaging.Message.HashAlgorithm%2A?displayProperty=nameWithType> era compatível somente com os valores <xref:System.Messaging.HashAlgorithm.Md5?displayProperty=nameWithType> e <xref:System.Messaging.HashAlgorithm.Sha?displayProperty=nameWithType>. A partir do .NET Framework 4.7.1, <xref:System.Messaging.HashAlgorithm.Sha256?displayProperty=nameWithType>, <xref:System.Messaging.HashAlgorithm.Sha384?displayProperty=nameWithType> e <xref:System.Messaging.HashAlgorithm.Sha512?displayProperty=nameWithType> também são compatíveis. Se esse valor será realmente usado depende de MSMQ, visto que a própria instância <xref:System.Messaging.Message> não faz nenhum hash, passando simplesmente os valores para o MSMQ. Para obter mais informações, consulte a seção “Suporte de SHA-2 para Message.HashAlgorithm” na postagem de blog [ASP.NET e recursos de configuração do .NET Framework 4.7.1](https://devblogs.microsoft.com/dotnet/net-framework-4-7-1-asp-net-and-configuration-features/).

<a name="asp-net471"></a>

#### <a name="aspnet"></a>ASP.NET

**Etapas de execução em aplicativos ASP.NET**

O ASP.NET processa as solicitações em um pipeline predefinido que inclui 23 eventos. O ASP.NET executa cada manipulador de eventos como uma etapa de execução. Em versões do ASP.NET até o .NET Framework 4.7, o ASP.NET não podia dar vazão ao fluxo do contexto de execução devido à alternância entre os threads nativos e gerenciados. Em vez disso, o ASP.NET dá vazão seletivamente somente ao fluxo do <xref:System.Web.HttpContext>. A partir do .NET Framework 4.7.1, o método <xref:System.Web.HttpApplication.OnExecuteRequestStep(System.Action{System.Web.HttpContextBase,System.Action})?displayProperty=nameWithType> também permite que os módulos restaurem dados do ambiente. Esse recurso é destinado a bibliotecas preocupadas com o rastreamento, criação de perfil, diagnóstico ou transações, como por exemplo, aquelas que se preocupam com o fluxo de execução do aplicativo. Para obter mais informações, consulte “Recurso de etapa de execução do ASP.NET” na postagem de blog [ASP.NET e recursos de configuração do .NET Framework 4.7.1](https://devblogs.microsoft.com/dotnet/net-framework-4-7-1-asp-net-and-configuration-features/).

**Análise de HttpCookie do ASP.NET**

O .NET Framework 4.7.1 inclui um novo método, <xref:System.Web.HttpCookie.TryParse%2A?displayProperty=nameWithType>, que fornece uma maneira padronizada para criar um objeto <xref:System.Web.HttpCookie> com base em uma cadeia de caracteres e atribuir com precisão valores de cookie como data de validade e caminho. Para obter mais informações, consulte “Análise do ASP.NET HttpCookie” na postagem de blog [ASP.NET e recursos de configuração do .NET Framework 4.7.1](https://devblogs.microsoft.com/dotnet/net-framework-4-7-1-asp-net-and-configuration-features/).

**Opções de hash SHA-2 para credenciais de autenticação de formulários do ASP.NET**

No .NET Framework 4.7 e versões anteriores, o ASP.NET permitia aos desenvolvedores armazenar credenciais de usuário com as senhas com hash nos arquivos de configuração usando MD5 ou SHA1. A partir do .NET Framework 4.7.1, o ASP.NET também dá suporte a novas opções de hash SHA-2 seguras como SHA256, SHA384 e SHA512. O SHA1 continua sendo o padrão e um algoritmo de hash não padrão pode ser definido no arquivo de configuração da Web. Por exemplo:

```xml
<system.web>
    <authentication mode="Forms">
        <forms loginUrl="~/login.aspx">
          <credentials passwordFormat="SHA512">
            <user name="jdoe" password="6D003E98EA1C7F04ABF8FCB375388907B7F3EE06F278DB966BE960E7CBBD103DF30CA6D61F7E7FD981B2E4E3A64D43C836A4BEDCA165C33B163E6BCDC538A664" />
          </credentials>
        </forms>
    </authentication>
</system.web>
```

<a name="v47"></a>

## <a name="whats-new-in-net-framework-47"></a>Novidades no .NET Framework 4.7

O .NET Framework 4.7 inclui novos recursos nas seguintes áreas:

- [Classes base](#Core47)
- [Rede](#net47)
- [ASP.NET](#ASP-NET47)
- [Windows Communication Foundation (WCF)](#wcf47)
- [Windows Forms](#wf47)
- [WPF (Windows Presentation Foundation)](#WPF47)

Para obter uma lista das novas APIs adicionadas ao .NET Framework 4.7, consulte [Alterações na API do .NET Framework 4.7](https://github.com/Microsoft/dotnet/blob/master/releases/net47/dotnet47-api-changes.md) no GitHub. Para obter uma lista de aprimoramentos de recursos e correções de bugs no .NET Framework 4.7, consulte [Lista de alterações do .NET Framework 4.7](https://github.com/Microsoft/dotnet/blob/master/releases/net47/dotnet47-changes.md) no GitHub. Para obter mais informações, consulte [anunciando o .NET Framework 4,7](https://devblogs.microsoft.com/dotnet/announcing-the-net-framework-4-7/) no blog do .net.

<a name="Core47"></a>

#### <a name="base-classes"></a>Classes base

O .NET Framework 4.7 melhora a serialização pelo <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer>:

**Funcionalidade aprimorada com ECC (criptografia de curva elíptica)***

No .NET Framework 4.7, os métodos `ImportParameters(ECParameters)` foram adicionados às classes <xref:System.Security.Cryptography.ECDsa> e <xref:System.Security.Cryptography.ECDiffieHellman> para permitir que um objeto representasse uma chave já estabelecida. Também foi adicionado um método `ExportParameters(Boolean)` à exportação da chave usando parâmetros de curva explícita.

O .NET Framework 4.7 também adiciona suporte para curvas adicionais (incluindo o conjunto de curvas Brainpool) e adicionou definições predefinidas para facilitar a criação por meio dos novos métodos de fábrica <xref:System.Security.Cryptography.ECDsa.Create%2A> e <xref:System.Security.Cryptography.ECDiffieHellman.Create%2A>.

Veja um [exemplo dos aprimoramentos de criptografia do .NET Framework 4.7](https://gist.github.com/richlander/5a182899895a87a296c21ada97f7a54e) no GitHub.

**Suporte aprimorado para caracteres de controle do DataContractJsonSerializer**

No .NET Framework 4,7, a <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> classe serializa os caracteres de controle em conformidade com o padrão ECMAScript 6. Esse comportamento é habilitado por padrão para aplicativos direcionados .NET Framework 4,7 e é um recurso de aceitação para aplicativos que estão sendo executados sob .NET Framework 4,7, mas que têm como alvo uma versão anterior do .NET Framework. Para obter mais informações, consulte a seção [compatibilidade de aplicativos](../migration-guide/application-compatibility.md) .

<a name="net47"></a>

#### <a name="networking"></a>Rede

O .NET Framework 4.7 adiciona os seguintes recursos de rede:

**Suporte do sistema operacional padrão para protocolos TLS***

A pilha de TLS, que é usada por <xref:System.Net.Security.SslStream?displayProperty=nameWithType> e por componentes de início de pilha, como HTTP, FTP e SMTP, permite que os desenvolvedores usem protocolos TLS padrão com suporte do sistema operacional. Os desenvolvedores não precisam mais codificar uma versão de TLS.

<a name="ASP-NET47"></a>

#### <a name="aspnet"></a>ASP.NET

No .NET Framework 4.7, o ASP.NET inclui os seguintes recursos novos:

**Extensibilidade de cache do objeto**

A partir do .NET Framework 4.7, o ASP.NET adiciona um novo conjunto de APIs que permitem aos desenvolvedores substituir as implementações padrão de ASP.NET para o armazenamento em cache de objeto na memória e o monitoramento da memória. Agora, os desenvolvedores podem substituir qualquer um dos três componentes a seguir se a implementação do ASP.NET não for adequada:

- **Armazenamento de cache de objeto**. Na nova seção de configuração de provedores de cache, os desenvolvedores podem conectar novas implementações de um cache de objeto para um aplicativo ASP.NET usando a nova interface **ICacheStoreProvider**.

- **Monitoramento de memória**. O monitor de memória padrão no ASP.NET notifica os aplicativos quando eles estiverem se aproximando do limite configurado de bytes particulares para o processo, ou quando o computador estiver com pouca RAM física total disponível. Quando esses limites estiverem próximos, as notificações serão disparadas. Para alguns aplicativos, as notificações são disparadas muito próximas aos limites configurados a fim de permitir reações úteis. Agora, os desenvolvedores podem criar seus próprios monitores de memória para substituir o padrão usando a propriedade <xref:System.Web.Hosting.ApplicationMonitors.MemoryMonitor%2A?displayProperty=nameWithType>.

- **Reações de limite de memória**. Por padrão, o ASP.NET tenta aparar o cache de objetos e chamar periodicamente <xref:System.GC.Collect%2A?displayProperty=nameWithType> quando o limite do processo de bytes particulares está próximo. Para alguns aplicativos, a frequência de chamadas para <xref:System.GC.Collect%2A?displayProperty=nameWithType> ou a quantidade aparada do cache é ineficaz. Agora, os desenvolvedores podem substituir ou complementar o comportamento padrão incluindo implementações do **IObserver** no monitor de memória do aplicativo.

<a name="wcf47"></a>

#### <a name="windows-communication-foundation-wcf"></a>Windows Communication Foundation (WCF)

O Windows Communication Foundation (WCF) adiciona os seguintes recursos e alterações:

**Capacidade de definir configurações de segurança de mensagem padrão para TLS 1.1 ou TLS 1.2**

A partir do .NET Framework 4.7, o WCF permite que você configure o TSL 1.1 ou o TLS 1.2, além do SSL 3.0 e TSL 1.0, como o protocolo de segurança de mensagem padrão. Esta é uma configuração baseada no consentimento; para habilitá-la, você deve adicionar a seguinte entrada ao arquivo de configuração do aplicativo:

```xml
<runtime>
   <AppContextSwitchOverrides value="Switch.System.ServiceModel.DisableUsingServicePointManagerSecurityProtocols=false;Switch.System.Net.DontEnableSchUseStrongCrypto=false" />
</runtime>
```

**Confiabilidade aprimorada de aplicativos WCF e serialização do WCF**

O WCF inclui diversas alterações de código que eliminam as condições de corrida, melhorando o desempenho e a confiabilidade das opções de serialização. Eles incluem:

- Suporte aprimorado para combinação de código síncrono e assíncrono em chamadas para **SocketConnection.BeginRead** e **SocketConnection.Read**.
- Confiabilidade aprimorada ao anular uma conexão com **SharedConnectionListener** e **DuplexChannelBinder**.
- Aumento da confiabilidade das operações de serialização ao chamar o método <xref:System.Runtime.Serialization.FormatterServices.GetSerializableMembers%28System.Type%29?displayProperty=nameWithType>.
- Confiabilidade aprimorada ao remover um waiter chamando o método **ChannelSynchronizer.RemoveWaiter**.

<a name="wf47"></a>

#### <a name="windows-forms"></a>Windows Forms

No .NET Framework 4.7, o Windows Forms melhora o suporte para monitores com alto DPI.

**Suporte a DPI alto**

A partir dos aplicativos direcionados ao .NET Framework 4.7, o .NET Framework apresenta suporte para alto DPI e DPI dinâmico em aplicativos do Windows Forms. O suporte para DPI alto melhora o layout e a aparência de formulários e controles em monitores com alto DPI. O DPI dinâmico altera o layout e a aparência de formulários e controles quando o usuário altera o DPI ou o fator de escala de exibição de um aplicativo em execução.

O suporte a DPI alto é um recurso opcional que você configura definindo uma [\<System.Windows.Forms.ConfigurationSection>](../configure-apps/file-schema/winforms/index.md) seção no arquivo de configuração do aplicativo. Para saber mais sobre como adicionar suporte ao DPI alto e suporte ao DPI dinâmico para seu aplicativo Windows Forms, confira [Suporte a DPI alto no Windows Forms](../winforms/high-dpi-support-in-windows-forms.md).

<a name="WPF47"></a>

#### <a name="windows-presentation-foundation-wpf"></a>Windows Presentation Foundation (WPF)

No .NET Framework 4.7, o WPF inclui os seguintes aprimoramentos:

**Suporte para uma pilha de toque/caneta com base em mensagens WM_POINTER do Windows**

Agora você tem a opção de usar uma pilha de toque/caneta com base em [mensagens WM_POINTER](https://docs.microsoft.com/previous-versions/windows/desktop/InputMsg/messages) em vez de WISP (Plataforma de Serviços do Windows Ink). Esse é um recurso opcional no .NET Framework. Para obter mais informações, consulte a seção [compatibilidade de aplicativos](../migration-guide/application-compatibility.md) .

**Nova implementação para APIs de impressão do WPF**

As APIs de impressão do WPF na classe <xref:System.Printing.PrintQueue?displayProperty=nameWithType> chamam a [Print Document Package API (API de Impressão pacote de documentos)](/windows/desktop/printdocs/tailored-app-printing-api) do Windows em vez da [XPS Print API (API de impressão XPS)](/windows/desktop/printdocs/xps-printing), que foi preterida. Para o impacto dessa alteração na compatibilidade de aplicativos, consulte a seção [compatibilidade de aplicativos](../migration-guide/application-compatibility.md) .

<a name="v462"></a>

## <a name="whats-new-in-net-framework-462"></a>Novidades no .NET Framework 4.6.2

O .NET Framework 4.6.2 inclui novos recursos nas seguintes áreas:

- [ASP.NET](#ASPNET462)

- [Categorias de caractere](#Strings)

- [Criptografia](#Crypto462)

- [SqlClient](#SQLClient)

- [Windows Communication Foundation](#WCF)

- [WPF (Windows Presentation Foundation)](#WPF462)

- [Windows Workflow Foundation (WF)](#WF462)

- [ClickOnce](#clickonce-1)

- [Convertendo aplicativos Windows Forms e WPF em aplicativos UWP](#UWPConvert)

- [Melhorias na depuração](#Debug462)

Para obter uma lista das novas APIs adicionadas ao .NET Framework 4.6.2, consulte [Alterações na API do .NET Framework 4.6.2](https://github.com/Microsoft/dotnet/blob/master/releases/net462/dotnet462-api-changes.md) no GitHub. Para obter uma lista de aprimoramentos de recursos e correções de bugs no .NET Framework 4.6.2, consulte o artigo [Lista de alterações do .NET Framework 4.6.2](https://github.com/Microsoft/dotnet/blob/master/releases/net462/dotnet462-changes.md) no GitHub. Para obter mais informações, consulte [anunciando .NET Framework 4.6.2](https://devblogs.microsoft.com/dotnet/announcing-net-framework-4-6-2/) no blog do .net.

<a name="ASPNET462"></a>

### <a name="aspnet"></a>ASP.NET

No .NET Framework 4.6.2, o ASP.NET inclui os seguintes aprimoramentos:

**Suporte aprimorado para mensagens de erro localizadas em validadores de anotação de dados**

Os validadores de anotação de dados permitem que você execute a validação adicionando um ou mais atributos a uma propriedade de classe. O elemento <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.ErrorMessage%2A?displayProperty=nameWithType> do atributo define o texto da mensagem de erro se a validação falhar. A partir do .NET Framework 4.6.2, o ASP.NET facilita a localização de mensagens de erro. Mensagens de erro serão localizadas se:

1. O <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.ErrorMessage%2A?displayProperty=nameWithType> é fornecido no atributo de validação.

2. O arquivo de recurso é armazenado na pasta App_LocalResources.

3. O nome do arquivo de recursos localizado tem o nome do formulário `DataAnnotation.Localization.{` *name* `}.resx` , em que *Name* é um nome de cultura no formato *languageCode* `-` *Country/regionCode* ou *languageCode*.

4. O nome da chave do recurso é a cadeia de caracteres atribuída ao atributo <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.ErrorMessage%2A?displayProperty=nameWithType> e seu valor é a mensagem de erro localizada.

Por exemplo, o atributo de anotação de dados a seguir define a mensagem de erro da cultura padrão para uma classificação inválida.

```csharp
public class RatingInfo
{
   [Required(ErrorMessage = "The rating must be between 1 and 10.")]
   [Display(Name = "Your Rating")]
   public int Rating { get; set; }
}
```

```vb
Public Class RatingInfo
   <Required(ErrorMessage = "The rating must be between 1 and 10.")>
   <Display(Name = "Your Rating")>
   Public Property Rating As Integer = 1
End Class
```

Você pode criar um arquivo de recurso, DataAnnotation.Localization.fr.resx, cuja chave é a cadeia de caracteres da mensagem de erro e cujo valor é a mensagem de erro localizada. O arquivo deve ser encontrado na pasta `App.LocalResources`. Por exemplo, veja a seguir a chave e seu valor em uma mensagem de erro localizada no idioma francês (fr):

| Nome                                 | Valor                                     |
| ------------------------------------ | ----------------------------------------- |
| A classificação deve estar entre 1 e 10. | La note doit être comprise entre 1 et 10. |

 Além disso, a localização de anotação de dados é extensível. Os desenvolvedores podem conectar seu próprio provedor de localizador de cadeia de caracteres implementando a interface <xref:System.Web.Globalization.IStringLocalizerProvider> para armazenar a cadeia de localização em algum lugar diferente de um arquivo de recurso.

 **Suporte assíncrono com provedores de armazenamento de estado de sessão**

 Agora, o ASP.NET permite o uso de métodos de retorno de tarefas com provedores de armazenamento de estado da sessão, permitindo assim que os aplicativos ASP.NET obtenham os benefícios de escalabilidade do modo assíncrono. Para dar suporte às operações assíncronas com provedores de armazenamento de estado de sessão, o ASP.NET inclui uma nova interface, <xref:System.Web.SessionState.ISessionStateModule?displayProperty=nameWithType>, que herda de <xref:System.Web.IHttpModule> e permite aos desenvolvedores implementar seus próprios módulo de estado de sessão e provedores de estado de sessão assíncrona. A interface é definida assim:

```csharp
public interface ISessionStateModule : IHttpModule {
    void ReleaseSessionState(HttpContext context);
    Task ReleaseSessionStateAsync(HttpContext context);
}
```

```vb
Public Interface ISessionStateModule : Inherits IHttpModule
   Sub ReleaseSessionState(context As HttpContext)
   Function ReleaseSessionStateAsync(context As HttpContext) As Task
End Interface
```

 Além disso, a classe <xref:System.Web.SessionState.SessionStateUtility> inclui dois métodos novos, <xref:System.Web.SessionState.SessionStateUtility.IsSessionStateReadOnly%2A> e <xref:System.Web.SessionState.SessionStateUtility.IsSessionStateRequired%2A>, que podem ser usados para oferecer suporte a operações assíncronas.

 **Suporte assíncrono para provedores de cache de saída**

 A partir do .NET Framework 4.6.2, os métodos de retorno de tarefa podem ser usados com provedores de cache de saída a fim de oferecer os benefícios de escalabilidade do modo assíncrono.  Provedores que implementam esses métodos reduzem o bloqueio de thread em um servidor Web e melhoram a escalabilidade de um serviço ASP.NET.

 As seguintes APIs foram adicionadas para oferecer suporte a provedores de cache de saída assíncronos:

- A classe <xref:System.Web.Caching.OutputCacheProviderAsync?displayProperty=nameWithType>, que herda de <xref:System.Web.Caching.OutputCacheProvider?displayProperty=nameWithType> e permite aos desenvolvedores implementar um provedor de cache de saída assíncrono.

- A classe <xref:System.Web.Caching.OutputCacheUtility>, que fornece métodos auxiliares para configuração do cache de saída.

- 18 métodos novos na classe <xref:System.Web.HttpCachePolicy?displayProperty=nameWithType>. Entre eles <xref:System.Web.HttpCachePolicy.GetCacheability%2A>, <xref:System.Web.HttpCachePolicy.GetCacheExtensions%2A>, <xref:System.Web.HttpCachePolicy.GetETag%2A>, <xref:System.Web.HttpCachePolicy.GetETagFromFileDependencies%2A>, <xref:System.Web.HttpCachePolicy.GetMaxAge%2A>, <xref:System.Web.HttpCachePolicy.GetMaxAge%2A>, <xref:System.Web.HttpCachePolicy.GetNoStore%2A>, <xref:System.Web.HttpCachePolicy.GetNoTransforms%2A>, <xref:System.Web.HttpCachePolicy.GetOmitVaryStar%2A>, <xref:System.Web.HttpCachePolicy.GetProxyMaxAge%2A>, <xref:System.Web.HttpCachePolicy.GetRevalidation%2A>, <xref:System.Web.HttpCachePolicy.GetUtcLastModified%2A>, <xref:System.Web.HttpCachePolicy.GetVaryByCustom%2A>, <xref:System.Web.HttpCachePolicy.HasSlidingExpiration%2A> e <xref:System.Web.HttpCachePolicy.IsValidUntilExpires%2A>.

- 2 métodos novos na classe <xref:System.Web.HttpCacheVaryByContentEncodings?displayProperty=nameWithType>: <xref:System.Web.HttpCacheVaryByContentEncodings.GetContentEncodings%2A> e <xref:System.Web.HttpCacheVaryByContentEncodings.SetContentEncodings%2A>.

- 2 métodos novos na classe <xref:System.Web.HttpCacheVaryByHeaders?displayProperty=nameWithType>: <xref:System.Web.HttpCacheVaryByHeaders.GetHeaders%2A> e <xref:System.Web.HttpCacheVaryByHeaders.SetHeaders%2A>.

- 2 métodos novos na classe <xref:System.Web.HttpCacheVaryByParams?displayProperty=nameWithType>: <xref:System.Web.HttpCacheVaryByParams.GetParams%2A> e <xref:System.Web.HttpCacheVaryByParams.SetParams%2A>.

- Na classe <xref:System.Web.Caching.AggregateCacheDependency?displayProperty=nameWithType>, o método <xref:System.Web.Caching.AggregateCacheDependency.GetFileDependencies%2A>.

- Na <xref:System.Web.Caching.CacheDependency>, o método <xref:System.Web.Caching.CacheDependency.GetFileDependencies%2A>.

<a name="Strings"></a>

### <a name="character-categories"></a>Categorias de caractere

Os caracteres no .NET Framework 4.6.2 são classificados com base no [padrão Unicode, versão 8.0.0](https://www.unicode.org/versions/Unicode8.0.0/). No .NET Framework 4.6 e no .NET Framework 4.6.1, os caracteres foram classificados com base nas categorias de caracteres do Unicode 6.3.

O suporte para o Unicode 8.0 é limitado à classificação de caracteres pela classe <xref:System.Globalization.CharUnicodeInfo> e para tipos e métodos que dependem dela. Entre elas está a classe <xref:System.Globalization.StringInfo>, o método <xref:System.Char.GetUnicodeCategory%2A?displayProperty=nameWithType> sobrecarregado e as [classes de caracteres](../../standard/base-types/character-classes-in-regular-expressions.md) reconhecidas pelo mecanismo de expressões regulares do .NET Framework.  A comparação e a classificação de caracteres e cadeia de caracteres não são afetadas por essa alteração e continuam a depender do sistema operacional subjacente ou, em sistemas com Windows 7, em dados de caracteres fornecidos pelo .NET Framework.

Para alterações nas categorias de caracteres do Unicode 6.0 para Unicode 7.0, confira [O padrão Unicode, versão 7.0.0](https://www.unicode.org/versions/Unicode7.0.0/) no site The Unicode Consortium. Para alterações do Unicode 7.0 para Unicode 8.0, confira [O padrão Unicode, versão 8.0.0](https://www.unicode.org/versions/Unicode8.0.0/) no site The Unicode Consortium.

<a name="Crypto462"></a>

### <a name="cryptography"></a>Criptografia

**Suporte para certificados X509 contendo um DSA FIPS 186-3**

O .NET Framework 4.6.2 adiciona suporte a certificados DSA (Algoritmo de Assinatura Digital) X509 cujas chaves ultrapassam o limite de 1024 bits do FIPS 186-2.

Além de oferecer suporte a chaves maiores de FIPS 186-3, o .NET Framework 4.6.2 permite assinaturas de computação com a família SHA-2 de algoritmos de hash (SHA256, SHA384 e SHA512). O suporte a FIPS 186-3 é fornecido pela nova classe <xref:System.Security.Cryptography.DSACng?displayProperty=nameWithType>.

Para acompanhar as alterações recentes na classe <xref:System.Security.Cryptography.RSA> do .NET Framework 4.6 e na classe <xref:System.Security.Cryptography.ECDsa> do .NET Framework 4.6.1, a classe base abstrata <xref:System.Security.Cryptography.DSA> do .NET Framework 4.6.2 tem métodos adicionais que permitem aos chamadores usar essa funcionalidade sem conversão. Você pode chamar o método de extensão <xref:System.Security.Cryptography.X509Certificates.DSACertificateExtensions.GetDSAPrivateKey%2A?displayProperty=nameWithType> para assinar dados, como mostra o exemplo a seguir.

```csharp
public static byte[] SignDataDsaSha384(byte[] data, X509Certificate2 cert)
{
    using (DSA dsa = cert.GetDSAPrivateKey())
    {
        return dsa.SignData(data, HashAlgorithmName.SHA384);
    }
}
```

```vb
Public Shared Function SignDataDsaSha384(data As Byte(), cert As X509Certificate2) As Byte()
    Using DSA As DSA = cert.GetDSAPrivateKey()
        Return DSA.SignData(data, HashAlgorithmName.SHA384)
    End Using
End Function
```

E você pode chamar o método de extensão <xref:System.Security.Cryptography.X509Certificates.DSACertificateExtensions.GetDSAPublicKey%2A?displayProperty=nameWithType> para verificar os dados assinados, como mostra o exemplo a seguir.

```csharp
public static bool VerifyDataDsaSha384(byte[] data, byte[] signature, X509Certificate2 cert)
{
    using (DSA dsa = cert.GetDSAPublicKey())
    {
        return dsa.VerifyData(data, signature, HashAlgorithmName.SHA384);
    }
}
```

```vb
 Public Shared Function VerifyDataDsaSha384(data As Byte(), signature As Byte(), cert As X509Certificate2) As Boolean
    Using dsa As DSA = cert.GetDSAPublicKey()
        Return dsa.VerifyData(data, signature, HashAlgorithmName.SHA384)
    End Using
End Function
```

**Maior clareza para as entradas nas rotinas de derivação de chaves ECDiffieHellman**

O .NET Framework 3.5 adicionou suporte para o Contrato de chave Diffie-Hellman de curva elíptica com três rotinas KDF (Função de Derivação de Chaves) diferentes. As entradas para as rotinas, e as próprias rotinas, foram configuradas por meio de propriedades no objeto <xref:System.Security.Cryptography.ECDiffieHellmanCng>. Mas como nem toda rotina lia cada propriedade de entrada, havia muita margem para confusão do desenvolvedor.

Para lidar com isso no .NET Framework 4.6.2, os três métodos a seguir foram adicionados à classe base <xref:System.Security.Cryptography.ECDiffieHellman> para representar mais claramente essas rotinas KDF e suas entradas:

|Método ECDiffieHellman|Descrição|
|----------------------------|-----------------|
|<xref:System.Security.Cryptography.ECDiffieHellman.DeriveKeyFromHash%28System.Security.Cryptography.ECDiffieHellmanPublicKey%2CSystem.Security.Cryptography.HashAlgorithmName%2CSystem.Byte%5B%5D%2CSystem.Byte%5B%5D%29>|Deriva o material da chave usando a fórmula<br /><br /> HASH(secretPrepend &#124;&#124; *x* &#124;&#124; secretAppend)<br /><br /> HASH(secretPrepend OrElse *x* OrElse secretAppend)<br /><br /> em que *x* é o resultado calculado do algoritmo EC Diffie-Hellman.|
|<xref:System.Security.Cryptography.ECDiffieHellman.DeriveKeyFromHmac%28System.Security.Cryptography.ECDiffieHellmanPublicKey%2CSystem.Security.Cryptography.HashAlgorithmName%2CSystem.Byte%5B%5D%2CSystem.Byte%5B%5D%2CSystem.Byte%5B%5D%29>|Deriva o material da chave usando a fórmula<br /><br /> HMAC(hmacKey, secretPrepend &#124;&#124; *x* &#124;&#124; secretAppend)<br /><br /> HMAC(hmacKey, secretPrepend OrElse *x* OrElse secretAppend)<br /><br /> em que *x* é o resultado calculado do algoritmo EC Diffie-Hellman.|
|<xref:System.Security.Cryptography.ECDiffieHellman.DeriveKeyTls%28System.Security.Cryptography.ECDiffieHellmanPublicKey%2CSystem.Byte%5B%5D%2CSystem.Byte%5B%5D%29>|Deriva o material da chave usando o algoritmo de derivação PRF (Função pseudoaleatória) TLS.|

**Suporte para criptografia simétrica de chave persistida**

A biblioteca de criptografia do Windows (CNG) adicionou suporte para o armazenamento de chaves simétricas persistidas e para o uso de chaves simétricas armazenadas em hardware, e o .NET Framework 4.6.2 possibilita aos desenvolvedores o uso desse recurso.  Como a noção de nomes e provedores de chave é específica à implementação, o uso desse recurso exige a utilização do construtor dos tipos de implementação concreta em vez da abordagem preferencial de fábrica (por exemplo, chamar `Aes.Create`).

O suporte à criptografia simétrica de chave persistente existe para os algoritmos AES (<xref:System.Security.Cryptography.AesCng>) e 3DES (<xref:System.Security.Cryptography.TripleDESCng>). Por exemplo:

```csharp
public static byte[] EncryptDataWithPersistedKey(byte[] data, byte[] iv)
{
    using (Aes aes = new AesCng("AesDemoKey", CngProvider.MicrosoftSoftwareKeyStorageProvider))
    {
        aes.IV = iv;

        // Using the zero-argument overload is required to make use of the persisted key
        using (ICryptoTransform encryptor = aes.CreateEncryptor())
        {
            if (!encryptor.CanTransformMultipleBlocks)
            {
                throw new InvalidOperationException("This is a sample, this case wasn't handled...");
            }

            return encryptor.TransformFinalBlock(data, 0, data.Length);
        }
    }
}
```

```vb
Public Shared Function EncryptDataWithPersistedKey(data As Byte(), iv As Byte()) As Byte()
    Using Aes As Aes = New AesCng("AesDemoKey", CngProvider.MicrosoftSoftwareKeyStorageProvider)
        Aes.IV = iv

        ' Using the zero-argument overload Is required to make use of the persisted key
        Using encryptor As ICryptoTransform = Aes.CreateEncryptor()
            If Not encryptor.CanTransformMultipleBlocks Then
                Throw New InvalidOperationException("This is a sample, this case wasn't handled...")
            End If
            Return encryptor.TransformFinalBlock(data, 0, data.Length)
        End Using
    End Using
End Function
```

**Suporte a SignedXml para hash SHA-2**

O .NET Framework 4.6.2 adiciona o suporte à classe <xref:System.Security.Cryptography.Xml.SignedXml> para os métodos de assinatura PKCS#1 RSA-SHA256, RSA-SHA384 e RSA-SHA512, e os algoritmos de resumo de referência SHA256, SHA384 e SHA512.

As constantes de URI são expostas em <xref:System.Security.Cryptography.Xml.SignedXml>:

|Campo SignedXml|Constante|
|---------------------|--------------|
|<xref:System.Security.Cryptography.Xml.SignedXml.XmlDsigSHA256Url>|"http://www.w3.org/2001/04/xmlenc#sha256"|
|<xref:System.Security.Cryptography.Xml.SignedXml.XmlDsigRSASHA256Url>|"http://www.w3.org/2001/04/xmldsig-more#rsa-sha256"|
|<xref:System.Security.Cryptography.Xml.SignedXml.XmlDsigSHA384Url>|"http://www.w3.org/2001/04/xmldsig-more#sha384"|
|<xref:System.Security.Cryptography.Xml.SignedXml.XmlDsigRSASHA384Url>|"http://www.w3.org/2001/04/xmldsig-more#rsa-sha384"|
|<xref:System.Security.Cryptography.Xml.SignedXml.XmlDsigSHA512Url>|"http://www.w3.org/2001/04/xmlenc#sha512"|
|<xref:System.Security.Cryptography.Xml.SignedXml.XmlDsigRSASHA512Url>|"http://www.w3.org/2001/04/xmldsig-more#rsa-sha512"|

 Todos os programas que registraram um manipulador <xref:System.Security.Cryptography.SignatureDescription> personalizado em <xref:System.Security.Cryptography.CryptoConfig> para adicionar suporte a esses algoritmos continuarão a funcionar como antes, mas como agora há padrões de plataforma, o registro de <xref:System.Security.Cryptography.CryptoConfig> não é mais necessário.

<a name="SQLClient"></a>

### <a name="sqlclient"></a>SqlClient

O provedor de dados do .NET Framework para SQL Server (<xref:System.Data.SqlClient?displayProperty=nameWithType>) inclui os seguintes recursos novos no .NET Framework 4.6.2:

**Pooling de conexão e tempos limite com bancos de dados SQL do Azure**

Quando o pool de conexões está habilitado e ocorre um tempo limite ou outro erro de logon, uma exceção é armazenada em cache e a exceção armazenada em cache é gerada em qualquer tentativa de conexão subsequente para os próximos 5 segundos a 1 minuto. Para obter mais informações, consulte [Pool de Conexões do SQL Server (ADO.NET)](../data/adonet/sql-server-connection-pooling.md).

Esse comportamento não é desejável ao se conectar a Bancos de Dados SQL do Azure, uma vez que as tentativas de conexão podem falhar com erros transitórios que normalmente são recuperados rapidamente. Para otimizar melhor a experiência de repetição de conexão, o comportamento do período de bloqueio do pool conexão é removido quando as conexões com os Bancos de Dados SQL do Azure falham.

A adição da nova `PoolBlockingPeriod` palavra-chave permite selecionar o período de bloqueio mais adequado para seu aplicativo. Os valores são:

<xref:System.Data.SqlClient.PoolBlockingPeriod.Auto>

O período de bloqueio do pool de conexão de um aplicativo que se conecta a um Banco de Dados SQL do Azure está desabilitado, e período de bloqueio do pool de conexão de um aplicativo que se conecta a qualquer outra instância do SQL Server está habilitado. Este é o valor padrão. Se o nome de ponto de extremidade do Servidor terminar com qualquer uma das seguintes opções, será considerado um Banco de Dado SQL do Azure:

- .database.windows.net

- .database.chinacloudapi.cn

- .database.usgovcloudapi.net

- .database.cloudapi.de

<xref:System.Data.SqlClient.PoolBlockingPeriod.AlwaysBlock>

O período de bloqueio do pool de conexão está sempre habilitado.

<xref:System.Data.SqlClient.PoolBlockingPeriod.NeverBlock>

O período de bloqueio do pool de conexão está sempre desabilitado.

**Aprimoramentos para Always Encrypted**

O SQLClient apresenta dois aprimoramentos para Always Encrypted:

- Agora, para melhorar o desempenho de consultas parametrizadas em colunas de banco de dados criptografadas, os metadados de criptografia para parâmetros de consulta são armazenado em cache. Com a propriedade <xref:System.Data.SqlClient.SqlConnection.ColumnEncryptionQueryMetadataCacheEnabled%2A?displayProperty=nameWithType> definida como `true` (que é o valor padrão), se a mesma consulta for chamada várias vezes, o cliente recuperará metadados de parâmetro do servidor somente uma vez.

- As entradas de chave de criptografia da coluna no cache de chave são removidas após um intervalo de tempo configurável, definido usando a propriedade <xref:System.Data.SqlClient.SqlConnection.ColumnEncryptionKeyCacheTtl%2A?displayProperty=nameWithType>.

<a name="WCF"></a>

### <a name="windows-communication-foundation"></a>Windows Communication Foundation

No .NET Framework 4.6.2, o Windows Communication Foundation foi aprimorado nas seguintes áreas:

**Suporte à segurança de transporte do WCF para certificados armazenados usando CNG**

A segurança de transporte do WCF dá suporte a certificados armazenados usando a biblioteca de criptografia do Windows (CNG). No .NET Framework 4.6.2, esse suporte é limitado ao uso de certificados com uma chave pública que tenha um expoente não superior a 32 bits de comprimento. Quando um aplicativo é direcionado ao .NET Framework 4.6.2, esse recurso é ativado por padrão.

Para aplicativos direcionados ao .NET Framework 4.6.1 e anteriores, mas que estão em execução no .NET Framework 4.6.2, esse recurso pode ser habilitado adicionando a linha a seguir à [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) seção do arquivo app.config ou web.config.

```xml
<AppContextSwitchOverrides
    value="Switch.System.ServiceModel.DisableCngCertificates=false"
/>
```

Isso também pode ser feito de modo programático com um código como o seguinte:

```csharp
private const string DisableCngCertificates = @"Switch.System.ServiceModel.DisableCngCertificates";
AppContext.SetSwitch(disableCngCertificates, false);
```

```vb
Const DisableCngCertificates As String = "Switch.System.ServiceModel.DisableCngCertificates"
AppContext.SetSwitch(disableCngCertificates, False)
```

**Melhor suporte para várias regras de ajuste de horário de verão pela classe DataContractJsonSerializer**

Os clientes podem usar uma configuração de aplicativo para determinar se a classe<xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> dá suporte a várias regras de ajuste para um único fuso horário. Esse é um recurso de opção de aceitação. Para habilitá-lo, adicione a seguinte configuração ao arquivo app.config:

```xml
<runtime>
     <AppContextSwitchOverrides value="Switch.System.Runtime.Serialization.DoNotUseTimeZoneInfo=false" />
</runtime>
```

Quando esse recurso estiver habilitado, um objeto <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> usará o tipo <xref:System.TimeZoneInfo> em vez do <xref:System.TimeZone> para desserializar dados de data e hora. O <xref:System.TimeZoneInfo> oferece suporte a várias regras de ajuste, o que possibilita o trabalho com dados históricos fuso horário; o <xref:System.TimeZone> não oferece.

Para saber mais sobre a estrutura <xref:System.TimeZoneInfo> e ajustes de fuso horário, confira [Visão geral do fuso horário](../../standard/datetime/time-zone-overview.md).

**Melhor correspondência de NetNamedPipeBinding**

O WCF possui uma nova configuração de aplicativo que pode ser definida em aplicativos cliente para garantir que eles sempre se conectem ao serviço escutando no URI mais próximo ao solicitado. Com essa configuração de aplicativo definida como `false` (padrão), os clientes podem usar <xref:System.ServiceModel.NetNamedPipeBinding> para tentar se conectar a um serviço escutando em um URI que é uma subcadeia de caracteres do URI solicitado.

Por exemplo, um cliente tenta se conectar a um serviço escutando em `net.pipe://localhost/Service1`, mas um serviço diferente nessa máquina que está executando com privilégios de administrador está escutando em `net.pipe://localhost`. Com essa configuração de aplicativo definida como `false`, o cliente tentaria se conectar ao serviço errado. Depois de definir a configuração do aplicativo como `true`, o cliente sempre se conectará ao serviço com correspondência mais próxima.

> [!NOTE]
> Os clientes que usam <xref:System.ServiceModel.NetNamedPipeBinding> localizam serviços com base no endereço base do serviço (se ele existir) em vez do endereço de ponto de extremidade completo. Para garantir sempre o funcionamento dessa configuração o serviço deve usar um endereço base exclusivo.

Para habilitar essa alteração, adicione a seguinte configuração de aplicativo ao arquivo app.config ou web.config de seu aplicativo cliente:

```xml
<configuration>
    <appSettings>
        <add key="wcf:useBestMatchNamedPipeUri" value="true" />
    </appSettings>
</configuration>
```

**SSL 3.0 não é um protocolo padrão**

Ao usar NetTcp com segurança de transporte e um tipo de credencial de certificado, o SSL 3.0 não é mais um protocolo padrão usado para negociar uma conexão segura. Na maioria dos casos, não deve haver impacto nos aplicativos existentes, pois o TLS 1.0 está incluído na lista de protocolos para NetTcp. Todos os clientes existentes devem ser capazes de negociar uma conexão usando no mínimo o TLS 1.0. Se Ssl3 for exigido, use um dos seguintes mecanismos de configuração para adicioná-lo à lista de protocolos negociados.

- A propriedade <xref:System.ServiceModel.Channels.SslStreamSecurityBindingElement.SslProtocols%2A?displayProperty=nameWithType>

- A propriedade <xref:System.ServiceModel.TcpTransportSecurity.SslProtocols%2A?displayProperty=nameWithType>

- A [\<transport>](../configure-apps/file-schema/wcf/transport-of-nettcpbinding.md) seção da [\<netTcpBinding>](../configure-apps/file-schema/wcf/nettcpbinding.md) seção

- A [\<sslStreamSecurity>](../configure-apps/file-schema/wcf/sslstreamsecurity.md) seção da [\<customBinding>](../configure-apps/file-schema/wcf/custombinding.md) seção

<a name="WPF462"></a>

### <a name="windows-presentation-foundation-wpf"></a>Windows Presentation Foundation (WPF)

No .NET Framework 4.6.2, o Windows Presentation Foundation foi aprimorado nas seguintes áreas:

**Classificação de grupo**

Um aplicativo que usa um objeto <xref:System.Windows.Data.CollectionView> para agrupar dados agora pode declarar explicitamente como classificar os grupos. A classificação explícita resolve o problema de ordens não intuitivas que ocorrem quando um aplicativo adiciona ou remove grupos dinamicamente, ou quando ele altera o valor das propriedades do item envolvidas no agrupamento. Ela também pode melhorar o desempenho do processo de criação de grupo mudando comparações das propriedades de agrupamento da classificação da coleção inteira para a classificação dos grupos.

Para oferecer suporte à classificação de grupo, as novas propriedades <xref:System.ComponentModel.GroupDescription.SortDescriptions%2A?displayProperty=nameWithType> e <xref:System.ComponentModel.GroupDescription.CustomSort%2A?displayProperty=nameWithType> descrevem como classificar a coleção de grupos produzida pelo objeto <xref:System.ComponentModel.GroupDescription>. Isso é semelhante à forma como as propriedades <xref:System.Windows.Data.ListCollectionView> de nome idêntico descrevem como classificar os itens de dados.

As duas novas propriedades estáticas da classe <xref:System.Windows.Data.PropertyGroupDescription>, <xref:System.Windows.Data.PropertyGroupDescription.CompareNameAscending%2A> e <xref:System.Windows.Data.PropertyGroupDescription.CompareNameDescending%2A>, podem ser usadas para os casos mais comuns.

Por exemplo, os seguintes dados de grupos XAML por idade, classificam as faixas etárias em ordem crescente, e agrupam os itens dentro de cada faixa etária por sobrenome.

```xaml
<GroupDescriptions>
     <PropertyGroupDescription
         PropertyName="Age"
         CustomSort=
              "{x:Static PropertyGroupDescription.CompareNamesAscending}"/>
     </PropertyGroupDescription>
</GroupDescriptions>

<SortDescriptions>
     <SortDescription PropertyName="LastName"/>
</SortDescriptions>
```

**Suporte ao teclado de toque**

O suporte ao teclado de toque permite o rastreamento de foco em aplicativos do WPF invocando e ignorando automaticamente o teclado de toque no Windows 10 quando a entrada por toque é recebida por um controle que pode receber entrada textual.

Em versões anteriores do .NET Framework, os aplicativos do WPF não podem optar pelo controle de foco sem desabilitar o suporte ao gesto de caneta/toque do WPF. Como resultado, os aplicativos WPF devem escolher entre o suporte total a toque do WPF ou depender da promoção de mouse do Windows.

**DPI por monitor**

Para dar suporte à recente proliferação de ambientes com alto DPI e DPI híbrido para aplicativos WPF, o WPF no .NET Framework 4.6.2 permite o reconhecimento por monitor. Confira [Exemplos e guia do desenvolvedor](https://github.com/Microsoft/WPF-Samples/tree/master/PerMonitorDPI) no GitHub para saber mais sobre como habilitar seu aplicativo WPF para ter o reconhecimento do DPI por monitor.

Nas versões anteriores do .NET Framework, os aplicativos WPF tinha reconhecimento de DPI do sistema. Em outras palavras, a interface do usuário do aplicativo é dimensionado adequadamente pelo sistema operacional, dependendo do DPI do monitor no qual o aplicativo é renderizado.

Para aplicativos em execução no .NET Framework 4.6.2, você pode desabilitar as alterações de DPI por monitor em aplicativos do WPF adicionando uma instrução de configuração à [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) seção do arquivo de configuração do aplicativo, da seguinte maneira:

```xml
<runtime>
   <AppContextSwitchOverrides value="Switch.System.Windows.DoNotScaleForDpiChanges=false"/>
</runtime>
```

<a name="WF462"></a>

### <a name="windows-workflow-foundation-wf"></a>Windows Workflow Foundation (WF)

No .NET Framework 4.6.2, o Windows Workflow Foundation foi aprimorado na seguinte área:

**Suporte para expressões C# e IntelliSense no designer do WF rehospedado**

A partir do .NET Framework 4,5, o WF dá suporte a expressões C# no Visual Studio Designer e em fluxos de trabalho de código. A Designer de Fluxo de Trabalho rehospedada é um recurso-chave do WF que permite que o Designer de Fluxo de Trabalho esteja em um aplicativo fora do Visual Studio (por exemplo, no WPF).  Windows Workflow Foundation fornece a capacidade de dar suporte a expressões C# e IntelliSense no Designer de Fluxo de Trabalho rehospedado. Para saber mais, confira o [blog do Windows Workflow Foundation](https://docs.microsoft.com/archive/blogs/workflowteam/building-c-expressions-support-and-intellisense-in-the-rehosted-workflow-designer).

`Availability of IntelliSense when a customer rebuilds a workflow project from Visual Studio` Em versões do .NET Framework antes do 4.6.2, o WF designer IntelliSense é interrompido quando um cliente recria um projeto de fluxo de trabalho do Visual Studio. Embora a compilação do projeto seja bem-sucedida, os tipos de fluxo de trabalho não são encontrados no designer, e surgem avisos do IntelliSense para os tipos de fluxo de trabalho ausentes na janela **Lista de Erros**. .NET Framework 4.6.2 resolve esse problema e disponibiliza o IntelliSense.

**Agora, os aplicativos do Fluxo de Trabalho V1 com Acompanhamento de Fluxo de Trabalho ativado são executados no modo FIPS**

Máquinas com o Modo de Conformidade FIPS habilitado podem executar um aplicativo no estilo Fluxo de trabalho versão 1 com o acompanhamento de Fluxo de trabalho ativado. Para habilitar esse cenário, faça a seguinte alteração em seu arquivo app.config:

```xml
<add key="microsoft:WorkflowRuntime:FIPSRequired" value="true" />
```

Se esse cenário não estiver habilitado, a execução do aplicativo continuará a gerar uma exceção com a mensagem "Esta implementação não faz parte dos algoritmos criptográficos validados por FIPS da Plataforma Windows".

**Aprimoramentos de fluxo de trabalho ao usar a Atualização Dinâmica com o Designer de Fluxo de Trabalho do Visual Studio**

Agora, o Designer de Fluxo de Trabalho, o Designer de Atividade do Fluxograma e outros Designers de Atividade de Fluxo de Trabalho carregam e exibem com êxito os fluxos de trabalho que foram salvos depois de chamar o método <xref:System.Activities.DynamicUpdate.DynamicUpdateServices.PrepareForUpdate%2A?displayProperty=nameWithType>. Nas versões do .NET Framework anteriores ao .NET Framework 4.6.2, carregar um arquivo XAML no Visual Studio para um fluxo de trabalho que foi salvo após chamar <xref:System.Activities.DynamicUpdate.DynamicUpdateServices.PrepareForUpdate%2A?displayProperty=nameWithType> poderá resultar nos seguintes problemas:

- O Designer de Fluxo de Trabalho não consegue carregar o arquivo XAML corretamente (quando <xref:System.Activities.Presentation.ViewState.ViewStateData.Id%2A?displayProperty=nameWithType> está no final da linha).

- O Designer de Atividades do Fluxograma ou outros Designers de Atividade de Fluxo de Trabalho podem exibir todos os objetos em seus locais padrão em vez de valores de propriedade anexados.

<a name="clickonce-1"></a>

### <a name="clickonce"></a>ClickOnce

O ClickOnce foi atualizado para oferecer suporte a TLS 1.1 e o TLS 1.2, além do protocolo 1.0, para o qual ele já oferece suporte. O ClickOnce detecta automaticamente qual protocolo é necessário; não é necessário executar nenhuma etapa adicional dentro do aplicativo ClickOnce para habilitar o suporte ao TLS 1.1 e 1.2.

<a name="UWPConvert"></a>

### <a name="converting-windows-forms-and-wpf-apps-to--uwp-apps"></a>Converter aplicativos do Windows Forms e do WPF em aplicativos UWP

Agora, o Windows oferece recursos para levar aplicativos existentes da área de trabalho do Windows, incluindo aplicativos do Windows Forms e WPF, para a plataforma Universal do Windows (UWP). Essa tecnologia age como uma ponte, permitindo que você migre gradualmente sua base de código existente para a UWP, levando seu aplicativo para todos os dispositivos com Windows 10.

Os aplicativos da área de trabalho convertidos ganham uma identidade de aplicativo semelhante à identidade de aplicativo de aplicativos da UWP, o que torna as APIs da UWP acessíveis para habilitar recursos como Blocos Dinâmicos e notificações. O aplicativo continua a se comportar como antes e é executado como um aplicativo de confiança total. Após a conversão do aplicativo, um processo de contêiner do aplicativo pode ser adicionado ao processo de confiança total existente a fim de adicionar uma interface do usuário adaptável. Quando toda a funcionalidade for movida para o processo de contêiner do aplicativo, o processo de confiança total poderá ser removido, e o novo aplicativo UWP poderá ser disponibilizado para todos os dispositivos com Windows 10.

<a name="Debug462"></a>

### <a name="debugging-improvements"></a>Melhorias na depuração

A *API de depuração não gerenciada* foi aprimorada no .NET Framework 4.6.2 para executar análises adicionais quando um <xref:System.NullReferenceException> for gerado, para que seja possível determinar qual variável em uma única linha do código-fonte é `null`.   Para oferecer suporte a esse cenário, as seguintes APIs foram adicionadas à API de depuração não gerenciada.

- As interfaces [ICorDebugCode4](../unmanaged-api/debugging/icordebugcode4-interface.md), [ICorDebugVariableHome](../unmanaged-api/debugging/icordebugvariablehome-interface.md) e [ICorDebugVariableHomeEnum](../unmanaged-api/debugging/icordebugvariablehomeenum-interface.md), que expõem os locais nativos das variáveis gerenciadas. Isso permite que os depuradores façam algumas análises de fluxo de código quando uma <xref:System.NullReferenceException> ocorrer e trabalhem com versões anteriores para determinar a variável gerenciada que corresponde ao local nativo no qual estava `null`.

- O método [ICorDebugType2::GetTypeID](../unmanaged-api/debugging/icordebugtype2-gettypeid-method.md) fornece um mapeamento para ICorDebugType para [COR_TYPEID](../unmanaged-api/debugging/cor-typeid-structure.md), que permite que o depurador obtenha um [COR_TYPEID](../unmanaged-api/debugging/cor-typeid-structure.md) sem uma instância do ICorDebugType. APIs existentes em [COR_TYPEID](../unmanaged-api/debugging/cor-typeid-structure.md) podem ser usadas para determinar o layout de classe do tipo.

<a name="v461"></a>

## <a name="whats-new-in-net-framework-461"></a>Novidades no .NET Framework 4.6.1

O .NET Framework 4.6.1 inclui novos recursos nas seguintes áreas:

- [Criptografia](#Crypto)

- [ADO.NET](#ADO.NET461)

- [WPF (Windows Presentation Foundation)](#WPF461)

- [Windows Workflow Foundation](#WWF461)

- [Criação de perfil](#Profile461)

- [NGen](#NGEN461)

Para saber mais sobre o .NET Framework 4.6.1, consulte os seguintes tópicos:

- [Lista de alterações do .NET Framework 4.6.1](https://github.com/Microsoft/dotnet/blob/master/releases/net461/dotnet461-changes.md)

- [Compatibilidade de aplicativos na versão 4.6.1](../migration-guide/application-compatibility.md)

- [Comparação da API do .NET Framework](https://github.com/Microsoft/dotnet/blob/master/releases/net461/dotnet461-api-changes.md) (no GitHub)

<a name="Crypto"></a>

### <a name="cryptography-support-for-x509-certificates-containing-ecdsa"></a>Criptografia: suporte para certificados X509 contendo ECDSA

O .NET Framework 4.6 adicionou suporte a RSACng para certificados X509. O .NET Framework 4.6.1 adiciona suporte para certificados X509 ECDSA (Algoritmo de Assinatura Digital de Curva Elíptica).

O ECDSA oferece melhor desempenho e é um algoritmo de criptografia mais seguro do que o RSA, fornecendo uma excelente opção quando o desempenho e a escalabilidade de TLS (Transport Layer Security) forem uma preocupação. A implementação do .NET Framework envolve chamadas para funcionalidades existentes do Windows.

O exemplo de código a seguir mostra como é fácil gerar uma assinatura para um fluxo de bytes usando o novo suporte para certificados X509 ECDSA incluídos no .NET Framework 4.6.1.

[!code-csharp[whatsnew.461.crypto#1](~/samples/snippets/csharp/VS_Snippets_CLR/whatsnew.461.crypto/cs/Code46.cs#1)]
[!code-vb[whatsnew.461.crypto#1](~/samples/snippets/visualbasic/VS_Snippets_CLR/whatsnew.461.crypto/vb/Code461.vb#1)]

Isso oferece um contraste evidente ao código necessário para gerar uma assinatura no .NET Framework 4.6.

[!code-csharp[whatsnew.461.crypto#2](~/samples/snippets/csharp/VS_Snippets_CLR/whatsnew.461.crypto/cs/Code46.cs#2)]
[!code-vb[whatsnew.461.crypto#2](~/samples/snippets/visualbasic/VS_Snippets_CLR/whatsnew.461.crypto/vb/Code46.vb#2)]

<a name="ADO.NET461"></a>

### <a name="adonet"></a>ADO.NET

Os itens a seguir foram adicionados ao ADO.NET:

**Suporte a Always Encrypted para chaves protegidas por hardware**

Agora, o ADO.NET oferece suporte ao armazenamento nativo de chaves mestras de coluna do Always Encrypted em HSMs (Módulos de segurança de Hardware). Com esse suporte, os clientes podem aproveitar as chaves assimétricas armazenadas em HSMs sem ter que escrever provedores de armazenamento de chave mestra de coluna personalizados e registrá-los nos aplicativos.

Os clientes precisam instalar o provedor de CSP fornecido pelo fornecedor de HSM ou provedores de armazenamento de chaves CNG em servidores de aplicativos ou computadores cliente para acessar dados do Always Encrypted protegidos com chaves mestras de coluna armazenadas em um HSM.

**<xref:System.Data.SqlClient.SqlConnectionStringBuilder.MultiSubnetFailover%2A>Comportamento de conexão aprimorado para AlwaysOn**

Agora, o SqlClient fornece automaticamente uma conexão mais rápida para um AG (Grupo de Disponibilidade) do AlwaysOn. Ele detecta de forma transparente se o seu aplicativo está se conectando a um grupo de disponibilidade (AG) do AlwaysOn em uma sub-rede diferente e descobre rapidamente o servidor ativo atual e fornece uma conexão ao servidor. Antes dessa versão, um aplicativo tinha que definir a cadeia de conexão para incluir `"MultisubnetFailover=true"` a fim de indicar que ele estava se conectando a um Grupo de disponibilidade do AlwaysOn. Sem definir a palavra-chave de conexão como `true`, um aplicativo pode enfrentar um tempo limite ao se conectar a um Grupo de disponibilidade AlwaysOn. Com esta versão, um aplicativo *não* precisa mais definir <xref:System.Data.SqlClient.SqlConnectionStringBuilder.MultiSubnetFailover%2A> como `true`. Para saber mais sobre o suporte ao SqlClient para Grupos de Disponibilidade AlwaysOn, confira [Suporte do SqlClient para alta disponibilidade e recuperação de desastres](../data/adonet/sql/sqlclient-support-for-high-availability-disaster-recovery.md).

<a name="WPF461"></a>

### <a name="windows-presentation-foundation-wpf"></a>Windows Presentation Foundation (WPF)

Windows Presentation Foundation inclui diversos aprimoramentos e alterações.

**Desempenho aprimorado**

O atraso em disparar eventos de toque foi corrigido no .NET Framework 4.6.1. Além disso, a digitação de controle <xref:System.Windows.Controls.RichTextBox> não ocupa mais o thread de renderização durante a entrada rápida.

**Aprimoramentos na verificação ortográfica**

O verificador ortográfico do WPF foi atualizado no Windows 8.1 e em versões posteriores para aproveitar o suporte ao sistema operacional para verificação ortográfica de outros idiomas.  Não há nenhuma alteração na funcionalidade em versões do Windows anteriores ao Windows 8.1.

Como nas versões anteriores do .NET Framework, o idioma para um controle <xref:System.Windows.Controls.TextBox> ou um bloco <xref:System.Windows.Controls.RichTextBox> é detectado procurando por informações na seguinte ordem:

- `xml:lang`, se estiver presente.

- Idioma de entrada atual.

- Cultura do thread atual.

Para obter mais informações sobre o suporte a idiomas no WPF, consulte a [postagem no blog do WPF em .NET Framework recursos do 4.6.1](https://devblogs.microsoft.com/wpf/wpf-in-net-4-6-1/).

**Suporte adicional para dicionários personalizados por usuário**

No .NET Framework 4.6.1, o WPF reconhece os dicionários personalizados registrados globalmente. Esse recurso está disponível além da capacidade de registrá-los por controle.

Nas versões anteriores do WPF, os dicionários personalizados não reconheciam as listas de Palavras Excluídas e de Autocorreção. Elas têm suporte no Windows 8.1 e no Windows 10 com o uso de arquivos que podem ser colocados no diretório `%AppData%\Microsoft\Spelling\<language tag>`.  As regras a seguir se aplicam a estes arquivos:

- Os arquivos devem ter extensões .dic (para palavras adicionadas), .exc (para palavras excluídas) ou .acl (para Autocorreção).

- Os arquivos devem ser em texto sem formatação UTF-16 LE que começa com a marca de ordem de Byte (BOM).

- Cada linha deve conter uma palavra (nas listas de palavras adicionadas e excluídas) ou um par de autocorreção com as palavras separadas por uma barra vertical ("&#124;") (na lista de palavras de Autocorreção).

- Esses arquivos são considerados somente leitura e não são modificados pelo sistema.

> [!NOTE]
> Esses novos formatos de arquivo não recebem suporte diretamente das APIs de verificação ortográfica do WPF, e os dicionários personalizados fornecidos ao WPF nos aplicativos devem continuar usando arquivos .lex.

**Amostras**

Há um número de amostras WPF no repositório do GitHub [de amostras Microsoft/WPF](https://github.com/Microsoft/WPF-Samples). Ajude-nos a melhorar nossos exemplos enviando-nos uma solicitação pull ou abrindo um [problema no GitHub](https://github.com/Microsoft/WPF-Samples/issues).

**Extensões do DirectX**

O WPF inclui um [pacote do NuGet](https://www.nuget.org/packages/Microsoft.Wpf.Interop.DirectX-x86/) que fornece novas implementações de <xref:System.Windows.Interop.D3DImage> que facilitam a interoperação com o conteúdo do DX10 e Dx11. O código para esse pacote foi aberto e está disponível [no GitHub](https://github.com/Microsoft/WPFDXInterop).

<a name="WWF461"></a>

### <a name="windows-workflow-foundation-transactions"></a>Windows Workflow Foundation: Transações

Agora, o método <xref:System.Transactions.Transaction.EnlistPromotableSinglePhase%2A?displayProperty=nameWithType> pode usar um gerenciador de transação distribuída diferente do MSDTC para promover a transação. Você pode fazer isso especificando um identificador de promotor de transação de GUID para a nova sobrecarga <xref:System.Transactions.Transaction.EnlistPromotableSinglePhase%28System.Transactions.IPromotableSinglePhaseNotification%2CSystem.Guid%29?displayProperty=nameWithType>. Se essa operação for bem-sucedida, haverá limitações nos recursos da transação. Após a inscrição do um promotor de transação não MSDTC, os métodos a seguir geram uma <xref:System.Transactions.TransactionPromotionException>, pois esses métodos exigem promoção para MSDTC:

- <xref:System.Transactions.Transaction.EnlistDurable%2A?displayProperty=nameWithType>

- <xref:System.Transactions.TransactionInterop.GetDtcTransaction%2A?displayProperty=nameWithType>

- <xref:System.Transactions.TransactionInterop.GetExportCookie%2A?displayProperty=nameWithType>

- <xref:System.Transactions.TransactionInterop.GetTransmitterPropagationToken%2A?displayProperty=nameWithType>

Após a inscrição do um promotor de transação não MSDTC, ele deverá ser usado para futuras inscrições duráveis usando os protocolos que definidos por ele. O <xref:System.Guid> do promotor de transação pode ser obtido usando a propriedade <xref:System.Transactions.Transaction.PromoterType%2A>. Quando transação promove, o promotor da transação fornece uma matriz <xref:System.Byte> que representa o token promovido. Um aplicativo pode obter o token promovido para uma transação promovida não MSDTC com o método <xref:System.Transactions.Transaction.GetPromotedToken%2A>.

Os usuários da nova sobrecarga <xref:System.Transactions.Transaction.EnlistPromotableSinglePhase%28System.Transactions.IPromotableSinglePhaseNotification%2CSystem.Guid%29?displayProperty=nameWithType> devem seguir uma sequência de chamadas específica para que a operação de promoção seja concluída com êxito. Essas regras estão na documentação do método.

<a name="Profile461"></a>

### <a name="profiling"></a>Criação de perfil

A API de criação de perfil não gerenciado foi aprimorada da seguinte forma:

- Melhor suporte para acessar PDBs na interface [ICorProfilerInfo7](../unmanaged-api/profiling/icorprofilerinfo7-interface.md) .

  No ASP.NET Core, está se tornando cada vez mais comum a compilação de assemblies na memória pelo Roslyn. Para os desenvolvedores que estão criando ferramentas de criação de perfil, isso significa que os PDBs que eram serializados historicamente no disco talvez não estejam mais presentes. Normalmente, as ferramentas de criação de perfil usam PDBs para mapear o código de volta para as linhas de origem para tarefas como cobertura de código ou análise de desempenho de linha por linha. A interface [ICorProfilerInfo7](../unmanaged-api/profiling/icorprofilerinfo7-interface.md) agora inclui dois métodos novos, [ICorProfilerInfo7::GetInMemorySymbolsLength](../unmanaged-api/profiling/icorprofilerinfo7-getinmemorysymbolslength-method.md) e [ICorProfilerInfo7::ReadInMemorySymbols](../unmanaged-api/profiling/icorprofilerinfo7-readinmemorysymbols.md), a fim de fornecer essas ferramentas de criação de perfil com acesso aos dados de PDB na memória. Com as novas APIs, um criador de perfil pode obter o conteúdo de um PDB na memória como uma matriz de bytes e, em seguida, processá-lo ou serializá-lo no disco.

- Instrumentação com mais qualidade com a interface ICorProfiler.

  Agora, os criadores de perfil que usam a funcionalidade ReJit da API `ICorProfiler` para instrumentação dinâmica podem modificar alguns metadados. Anteriormente, essas ferramentas podiam instrumentar IL a qualquer momento, mas os metadados só podiam ser modificados no tempo de carregamento do módulo. Como IL faz referência aos metadados, isso limitou os tipos de instrumentação possíveis. Nós levantamos alguns desses limites adicionando o método [ICorProfilerInfo7:: ApplyMetaData](../unmanaged-api/profiling/icorprofilerinfo7-applymetadata-method.md) para dar suporte a um subconjunto de edições de metadados depois que o módulo é carregado, particularmente adicionando novos `AssemblyRef` registros,,,, `TypeRef` `TypeSpec` `MemberRef` `MemberSpec` e `UserString` . Essa alteração possibilita um intervalo mais amplo de instrumentação dinâmica possível.

<a name="NGEN461"></a>

### <a name="native-image-generator-ngen-pdbs"></a>PDBs do NGEN (Gerador de Imagens Nativas)

O rastreamento de eventos entre máquinas permite aos clientes criar o perfil de um programa na Máquina A e examinar os dados de criação de perfil com mapeamento de linha de origem na Máquina B. Com as versões anteriores do .NET Framework, o usuário copiaria todos os módulos e imagens nativas da máquina com perfil para a máquina de análise que contém o PDB IL para criar o mapeamento da fonte para a nativa. Embora esse processo funcione bem quando os arquivos são relativamente pequenos, por exemplo, para aplicativos de telefone, os arquivos podem ser muito grandes em sistemas do desktop e a cópia exige um tempo considerável.

Com PDBs do Ngen, o NGen pode criar um PDB que contém o mapeamento do IL para a nativa sem uma dependência do PDB do IL. Em nosso cenário de rastreamento de eventos entre máquinas, basta copiar o PDB da imagem nativa gerado pela Máquina A para a Máquina B e usar as [APIs de acesso à interface de depuração](/visualstudio/debugger/debug-interface-access/debug-interface-access-sdk-reference) para ler o mapeamento da fonte ao IL do PDB de IL e o mapeamento de IL para nativa do PDB da imagem nativa. A combinação dos dois mapeamentos fornece um mapeamento da fonte para nativa. Como o PDB da imagem nativa é muito menor do que todos os módulos e imagens nativas, o processo de cópia da Máquina A para a Máquina B é muito mais rápido.

<a name="v46"></a>

## <a name="whats-new-in-net-2015"></a>Novidades do .NET 2015

O .NET 2015 apresenta o .NET Framework 4.6 e o .NET Core. Alguns recursos novos se aplicam aos dois, enquanto outros recursos são específicos ao .NET Framework 4.6 ou .NET Core.

- **ASP.NET Core**

  O .NET 2015 inclui o ASP.NET Core, que é uma implementação enxuta do .NET para a compilação de modernos aplicativos baseados em nuvem. O ASP.NET Core é modular para que você possa incluir somente os recursos necessários em seu aplicativo. Pode ser hospedada no IIS ou auto-hospedada em um processo personalizado, e você pode executar aplicativos com diferentes versões do .NET Framework no mesmo servidor. Ela inclui um novo sistema de configuração do ambiente projetado para implantação na nuvem.

  MVC, API da Web e Páginas da Web são unificados em uma única estrutura chamada MVC 6. Você cria aplicativos ASP.NET Core por meio de ferramentas no Visual Studio 2015 ou posterior. Os aplicativos existentes funcionarão no novo .NET Framework; no entanto, para compilar um aplicativo que usa o MVC 6 ou SignalR 3, use o sistema de projetos no Visual Studio 2015 ou posterior.

  Para obter informações, veja [ASP.NET Core](/aspnet/core/).

- **Atualizações do ASP.NET**

  - **API baseada em tarefas para a liberação de resposta assíncrona**

    Agora, o ASP.NET fornece uma API simples baseada em tarefas para liberação de resposta assíncrona, <xref:System.Web.HttpResponse.FlushAsync%2A?displayProperty=nameWithType>, que permite a liberação das respostas de modo assíncrono usando o suporte a `async/await` de sua linguagem.

  - **O model binding dá suporte a métodos de retorno de tarefa**

    No .NET Framework 4.5, o ASP.NET adicionou o recurso de Model Binding, que permitiu uma abordagem extensível e focada no código para operações de dados com base em CRUD em páginas de Web Forms e controles de usuário. Agora, o sistema de Model Binding oferece suporte a métodos de model binding que retornam <xref:System.Threading.Tasks.Task>. Esse recurso permite aos desenvolvedores de Web Forms obter os benefícios de escalabilidade do modo assíncrono com a facilidade do sistema de associação de dados ao usar versões mais recentes dos ORMs, incluindo o Entity Framework.

    O model binding assíncrono é controlado pela configuração `aspnet:EnableAsyncModelBinding`.

    ```xml
    <appSettings>
        <add key=" aspnet:EnableAsyncModelBinding" value="true|false" />
    </appSettings>
    ```

    Em aplicativos direcionados ao .NET Framework 4.6, o padrão é `true`. Em aplicativos em execução no .NET Framework 4.6 direcionados a uma versão anterior do .NET Framework, o padrão é `false`. Isso pode ser habilitado ao definir a configuração como `true`.

  - **Suporte a HTTP/2 (Windows 10)**

    [HTTP/2](https://www.wikipedia.org/wiki/HTTP/2) é uma nova versão do protocolo HTTP que fornece uma utilização de conexão muito melhor (menos viagens entre cliente e servidor), resultando em um carregamento de página da Web com menor latência para os usuários.  As páginas da Web (em vez de serviços) são as mais beneficiados com o HTTP/2, pois o protocolo otimiza para solicitação de vários artefatos como parte de uma experiência única. O suporte a HTTP/2 foi adicionado ao ASP.NET no .NET Framework 4.6. Como a funcionalidade de rede existe em várias camadas, havia a necessidade de novos recursos no Windows, no IIS e no ASP.NET para habilitar o HTTP/2. Você deve estar executando o Windows 10 para usar o HTTP/2 com o ASP.NET.

    O HTTP/2 também é compatível e ativado por padrão em aplicativos UWP (Plataforma Universal do Windows) do Windows 10 que usam a API <xref:System.Net.Http.HttpClient?displayProperty=nameWithType>.

    Para fornecer uma maneira de usar o recurso [PUSH_PROMISE](https://http2.github.io/http2-spec/#PUSH_PROMISE) em aplicativos ASP.NET, um novo método com duas sobrecargas, <xref:System.Web.HttpResponse.PushPromise%28System.String%29> e <xref:System.Web.HttpResponse.PushPromise%28System.String%2CSystem.String%2CSystem.Collections.Specialized.NameValueCollection%29>, foi adicionado à classe <xref:System.Web.HttpResponse>.

    > [!NOTE]
    > Embora o ASP.NET Core dê suporte ao HTTP/2, o suporte para o recurso PUSH PROMISE ainda não foi adicionado.

    O navegador e o servidor Web (IIS no Windows) fazem todo o trabalho. Você não precisa fazer qualquer trabalho pesado para seus usuários.

    A maioria dos [principais navegadores oferece suporte a HTTP/2](https://www.wikipedia.org/wiki/HTTP/2), portanto, é provável que seus usuários se beneficiem do suporte a HTTP/2 se o servidor oferecer suporte a ele.

  - **Suporte para o protocolo de associação de token**

    A Microsoft e o Google colaboraram em uma nova abordagem para autenticação chamada de [Protocolo de associação de token](https://github.com/TokenBinding/Internet-Drafts). A premissa é que os tokens de autenticação (no cache do navegador) podem ser roubados e usados por criminosos para acessar recursos de outra forma seguros (por exemplo, sua conta bancária) sem exigir sua senha ou qualquer outro conhecimento privilegiado. O novo protocolo tem como objetivo atenuar esse problema.

    O Protocolo de associação de token será implementado no Windows 10 como um recurso do navegador. Aplicativos ASP.NET participarão do protocolo, para que os tokens de autenticação sejam validados como legítimos. As implementações do cliente e do servidor estabelecem a proteção de ponta a ponta especificada pelo protocolo.

  - **Algoritmos de hash da cadeia de caracteres aleatória**

    O .NET Framework 4.5 introduziu um [algoritmo de hash de cadeia de caracteres aleatória](../configure-apps/file-schema/runtime/userandomizedstringhashalgorithm-element.md). No entanto, ele não tinha suporte do ASP.NET porque alguns recursos do ASP.NET dependem de um código hash estável. No .NET Framework 4.6, já há suporte para os algoritmos de hash da cadeia de caracteres aleatória. Para habilitar esse recurso, use a configuração `aspnet:UseRandomizedStringHashAlgorithm`.

    ```xml
    <appSettings>
        <add key="aspnet:UseRandomizedStringHashAlgorithm" value="true|false" />
    </appSettings>
    ```

- **ADO.NET**

  O ADO.NET agora oferece suporte ao recurso Always Encrypted disponível no SQL Server 2016 Community Technology Preview 2 (CTP2). Com o Always Encrypted, SQL Server pode executar operações em dados criptografados e o melhor de toda a chave de criptografia reside com o aplicativo dentro do ambiente confiável do cliente e não no servidor. O Sempre Criptografado protege os dados do cliente para que DBAs não tenham acesso a dados de texto sem formatação. A criptografia e a descriptografia de dados ocorre de forma transparente no nível do driver, minimizando as alterações que precisam ser feitas nos aplicativos atuais. Para obter detalhes, confira [Always Encrypted (Mecanismo de banco de dados)](/sql/relational-databases/security/encryption/always-encrypted-database-engine) e [Always Encrypted (desenvolvimento do cliente)](/sql/relational-databases/security/encryption/always-encrypted-client-development).

- **Compilador JIT de 64 bits para código gerenciado**

  O .NET Framework 4.6 apresenta uma nova versão do compilador JIT de 64 bits (cujo codinome original é RyuJIT). O novo compilador de 64 bits fornece melhorias consideráveis de desempenho em relação ao compilador JIT de 64 bits mais antigo. O novo compilador de 64 bits está habilitado para processos de 64 bits em execução sobre o .NET Framework 4.6. Seu aplicativo será executado em um processo de 64 bits se for compilado como 64 bits ou AnyCPU e será executado em um sistema operacional de 64 bits. Apesar de nossos esforços para fazer a transição para o novo compilador a mais transparente possível, ainda é possível perceber mudanças no comportamento.

  O novo compilador JIT de 64 bits também inclui recursos de aceleração de hardware SIMD quando combinado com tipos habilitados para SIMD no namespace <xref:System.Numerics>, o que pode suspender aprimoramentos no desempenho.

- **Aperfeiçoamentos no carregador de assembly**

  Agora, o carregador de assembly usa a memória de forma mais eficiente ao descarregar assemblies de nível de integridade após uma imagem NGEN correspondente ser carregada. Essa mudança reduz a memória virtual, o que é particularmente benéfico para aplicativos de 32 bits grandes (como o Visual Studio) e também poupa memória física.

- **Mudanças na biblioteca de classes base**

  Várias APIs novas foram adicionadas ao .NET Framework 4.6 para habilitar cenários-chave. As seguintes alterações e adições foram incluídas:

  - **\<T>Implementações de IReadOnlyCollection**

    As coleções adicionais implementam <xref:System.Collections.Generic.IReadOnlyCollection%601> como <xref:System.Collections.Generic.Queue%601> e <xref:System.Collections.Generic.Stack%601>.

  - **CultureInfo.CurrentCulture e CultureInfo.CurrentUICulture**

    Agora, as propriedades <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> e <xref:System.Globalization.CultureInfo.CurrentUICulture%2A?displayProperty=nameWithType> são leitura / gravação em vez de somente leitura. Se você atribuir um novo objeto <xref:System.Globalization.CultureInfo> a essas propriedades, a cultura do thread atual definida pela propriedade `Thread.CurrentThread.CurrentCulture`, e a cultura do thread de interface do usuário atual definida pela propriedade `Thread.CurrentThread.CurrentUICulture` também mudarão.

  - **Aprimoramentos na coleta de lixo (GC)**

    Agora, a classe <xref:System.GC> inclui os métodos <xref:System.GC.TryStartNoGCRegion%2A> e <xref:System.GC.EndNoGCRegion%2A> que permitem o cancelamento da permissão da coleta de lixo durante a execução de um caminho crítico.

    Uma nova sobrecarga do método <xref:System.GC.Collect%28System.Int32%2CSystem.GCCollectionMode%2CSystem.Boolean%2CSystem.Boolean%29?displayProperty=nameWithType> lhe permite controlar se o heap de objetos pequeno e o heap de objetos grande são limpos e compactadas ou somente limpos.

  - **Tipos habilitados para SIMD**

    O namespace <xref:System.Numerics> agora inclui uma série de tipos habilitados para SIMD, como <xref:System.Numerics.Matrix3x2>, <xref:System.Numerics.Matrix4x4>, <xref:System.Numerics.Plane>, <xref:System.Numerics.Quaternion>, <xref:System.Numerics.Vector2>, <xref:System.Numerics.Vector3> e <xref:System.Numerics.Vector4>.

    Como o novo compilador JIT de 64 bits também inclui recursos de aceleração de hardware SIMD, há melhorias de desempenho consideráveis, especialmente ao usar os tipos habilitados para SIMD com o novo compilador JIT de 64 bits.

  - **Atualizações de criptografia**

    A API <xref:System.Security.Cryptography?displayProperty=nameWithType> está sendo atualizada para oferecer suporte às [APIs de criptografia CNG do Windows](/windows/desktop/SecCNG/cng-reference). As versões anteriores do .NET Framework dependiam inteiramente de uma [versão anterior das APIs de criptografia do Windows](/windows/desktop/SecCrypto/cryptography-portal) como a base para a implementação de <xref:System.Security.Cryptography?displayProperty=nameWithType>. Recebemos solicitações para o suporte a API de CNG, uma vez que ela dá suporte a [algoritmos de criptografia modernos](/windows/desktop/SecCNG/cng-features#suite-b-support), que são importantes para determinadas categorias de aplicativos.

    O .NET Framework 4.6 inclui os seguintes aprimoramentos novos para compatibilidade com as APIs de criptografia CNG do Windows:

    - Um conjunto de métodos de extensão para Certificados X509, `System.Security.Cryptography.X509Certificates.RSACertificateExtensions.GetRSAPublicKey(System.Security.Cryptography.X509Certificates.X509Certificate2)` e `System.Security.Cryptography.X509Certificates.RSACertificateExtensions.GetRSAPrivateKey(System.Security.Cryptography.X509Certificates.X509Certificate2)`, que retornam uma implementação baseada em CNG, em vez de uma implementação baseada em CAPI, quando for possível. (Alguns cartões inteligentes etc., ainda exigirão CAPI, e as APIs tratam do fallback).

    - A classe <xref:System.Security.Cryptography.RSACng?displayProperty=nameWithType>, que fornece uma implementação CNG do algoritmo RSA.

    - Aprimoramentos à API do RSA para que as ações comuns não exijam mais a conversão. Por exemplo, a criptografia de dados usando um objeto <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> exige um código semelhante ao seguinte em versões anteriores do .NET Framework.

      [!code-csharp[WhatsNew.Casting#1](~/samples/snippets/csharp/VS_Snippets_CLR/whatsnew.casting/cs/program.cs#1)]
      [!code-vb[WhatsNew.Casting#1](~/samples/snippets/visualbasic/VS_Snippets_CLR/whatsnew.casting/vb/module1.vb#1)]

      O código que usa as novas APIs de criptografia no .NET Framework 4.6 pode ser reescrito como se segue para evitar a conversão.

      [!code-csharp[WhatsNew.Casting#2](~/samples/snippets/csharp/VS_Snippets_CLR/whatsnew.casting/cs/program.cs#2)]
      [!code-vb[WhatsNew.Casting#2](~/samples/snippets/visualbasic/VS_Snippets_CLR/whatsnew.casting/vb/module1.vb#2)]

  - **Suporte para conversão de datas e horas para ou do horário Unix**

    Os novos métodos a seguir foram adicionados à estrutura <xref:System.DateTimeOffset> para oferecer suporte à conversão de valores de data e hora para ou de horário Unix:

    - <xref:System.DateTimeOffset.FromUnixTimeSeconds%2A?displayProperty=nameWithType>

    - <xref:System.DateTimeOffset.FromUnixTimeMilliseconds%2A?displayProperty=nameWithType>

    - <xref:System.DateTimeOffset.ToUnixTimeSeconds%2A?displayProperty=nameWithType>

    - <xref:System.DateTimeOffset.ToUnixTimeMilliseconds%2A?displayProperty=nameWithType>

  - **Opções de compatibilidade**

    A <xref:System.AppContext> classe adiciona um novo recurso de compatibilidade que permite que os gravadores de biblioteca forneçam um mecanismo de aceitação uniforme para a nova funcionalidade para seus usuários. Ele estabelece um contrato livremente acoplado entre componentes para comunicar uma solicitação de recusa. Normalmente, essa funcionalidade é importante quando uma alteração é feita na funcionalidade existente. Por outro lado, já existe uma aceitação implícita da nova funcionalidade.

    Com o <xref:System.AppContext>, as bibliotecas definem e expõem as opções de compatibilidade, enquanto o código que depende delas podem definir essas opções a fim de afetar o comportamento da biblioteca. Por padrão, as bibliotecas fornecem a nova funcionalidade, e apenas a alteram (ou seja, eles fornecem a funcionalidade anterior) se a opção for definida.

    Um aplicativo (ou uma biblioteca) pode declarar o valor de uma opção (que é sempre um valor <xref:System.Boolean>) definida pela biblioteca dependente. A opção é sempre implicitamente `false`. A definição da opção como `true` a habilita. A definição explícita da opção `false` fornece o novo comportamento.

    ```csharp
    AppContext.SetSwitch("Switch.AmazingLib.ThrowOnException", true);
    ```

    ```vb
    AppContext.SetSwitch("Switch.AmazingLib.ThrowOnException", True)
    ```

    A biblioteca deve verificar se um consumidor declarou o valor da opção e, em seguida, agir adequadamente.

    ```csharp
    if (!AppContext.TryGetSwitch("Switch.AmazingLib.ThrowOnException", out shouldThrow))
    {
        // This is the case where the switch value was not set by the application.
        // The library can choose to get the value of shouldThrow by other means.
        // If no overrides nor default values are specified, the value should be 'false'.
        // A false value implies the latest behavior.
    }

    // The library can use the value of shouldThrow to throw exceptions or not.
    if (shouldThrow)
    {
        // old code
    }
    else
    {
        // new code
    }
    ```

    ```vb
    If Not AppContext.TryGetSwitch("Switch.AmazingLib.ThrowOnException", shouldThrow) Then
        ' This is the case where the switch value was not set by the application.
        ' The library can choose to get the value of shouldThrow by other means.
        ' If no overrides nor default values are specified, the value should be 'false'.
        ' A false value implies the latest behavior.
    End If

    ' The library can use the value of shouldThrow to throw exceptions or not.
    If shouldThrow Then
        ' old code
    Else
        ' new code
    End If
    ```

    É útil usar um formato consistente para opções, já que elas são um contrato formal exposto por uma biblioteca. Veja a seguir dois formatos óbvios.

    - *Opção*.*namespace*.*nomedaopção*

    - *Opção*.*biblioteca*.*nomedaopção*

  - **Mudanças no TAP (padrão assíncrono baseado em tarefas)**

    Para aplicativos direcionados aos objetos <xref:System.Threading.Tasks.Task> e <xref:System.Threading.Tasks.Task%601> do NET Framework 4.6 herdam a cultura e a cultura da interface do usuário do thread de chamada. O comportamento de aplicativos direcionados a versões anteriores do .NET Framework, ou que não são direcionados a uma versão específica do .NET Framework, não é afetado. Para saber mais, confira a seção "Cultura e operações assíncronas baseadas em tarefas" do tópico da classe <xref:System.Globalization.CultureInfo>.

    A classe <xref:System.Threading.AsyncLocal%601?displayProperty=nameWithType> permite a representação dos dados do ambiente que sejam locais a um fluxo determinado de controle assíncrono, como um método `async`. Ele pode ser usado para persistir dados entre threads. Você também pode definir um método de retorno de chamada que será notificado sempre que os dados do ambiente mudarem, pois a propriedade <xref:System.Threading.AsyncLocal%601.Value%2A?displayProperty=nameWithType> foi alterada explicitamente, ou porque o thread encontrou uma transição de contexto.

    Os três métodos de conveniência, <xref:System.Threading.Tasks.Task.CompletedTask%2A?displayProperty=nameWithType>, <xref:System.Threading.Tasks.Task.FromCanceled%2A?displayProperty=nameWithType> e <xref:System.Threading.Tasks.Task.FromException%2A?displayProperty=nameWithType>, foram adicionados ao TAP (padrão assíncrono baseado em tarefa) para retornar as tarefas concluídas em um estado específico.

    Agora, a classe <xref:System.IO.Pipes.NamedPipeClientStream> dá suporte à comunicação assíncrona com seu novo <xref:System.IO.Pipes.NamedPipeClientStream.ConnectAsync%2A>. método.

  - **O EventSource agora oferece suporte à gravação no Log de eventos**

    Agora, você pode usar a classe <xref:System.Diagnostics.Tracing.EventSource> para registrar mensagens administrativas ou operacionais no log de eventos, além de quaisquer sessões ETW existentes criadas na máquina. No passado, era necessário usar o pacote Microsoft.Diagnostics.Tracing.EventSource NuGet para essa funcionalidade. Agora, essa funcionalidade está integrada ao .NET Framework 4.6.

    O pacote do NuGet e o .NET Framework 4.6 foram atualizados com os seguintes recursos:

    - **Eventos dinâmicos**

      Permite eventos definidos como "dinâmicos" sem a criação de métodos de evento.

    - **Cargas avançadas**

      Permite que classes atribuídas especialmente e matrizes, bem como tipos primitivos, sejam passados como uma carga

    - **Acompanhamento de atividades**

      Faz com que os eventos Iniciar e Parar marquem eventos entre eles com uma ID que representa todas as atividades ativas no momento.

    Para oferecer suporte a esses recursos, o método sobrecarregado <xref:System.Diagnostics.Tracing.EventSource.Write%2A> foi adicionado à classe <xref:System.Diagnostics.Tracing.EventSource>.

- **WPF (Windows Presentation Foundation)**

  - **Aprimoramentos de HDPI**

    Agora, o suporte ao HDPI no WPF é melhor no .NET Framework 4.6. Foram feitas alterações no layout para reduzir as ocorrências de distorção em controles com bordas. Por padrão, esse recurso será habilitado somente se <xref:System.Runtime.Versioning.TargetFrameworkAttribute> estiver definido para o .NET 4.6.  Os aplicativos destinados a versões anteriores do Framework, mas que estão em execução no .NET Framework 4,6, podem aceitar o novo comportamento adicionando a seguinte linha à [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) seção do arquivo app.config:

    ```xml
    <AppContextSwitchOverrides
    value="Switch.MS.Internal.DoNotApplyLayoutRoundingToMarginsAndBorderThickness=false"
    />
    ```

    Janelas do WPF invadindo vários monitores com configurações de DPI diferentes (configuração de DPI múltiplo) agora são renderizadas completamente sem regiões escurecidas. Você pode desativar esse comportamento adicionando a seguinte linha à seção `<appSettings>` do arquivo app.config:

    ```xml
    <add key="EnableMultiMonitorDisplayClipping" value="true"/>
    ```

    O suporte ao carregamento automático do cursor à direita com base na configuração de DPI foi adicionado a <xref:System.Windows.Input.Cursor?displayProperty=nameWithType>.

  - **O toque está melhor**

    Os comentários de clientes no [Connect](https://connect.microsoft.com/VisualStudio/feedback/details/903760/) informando que o toque produz um comportamento imprevisível foram resolvidos no .NET Framework 4.6. O limite de toque duplo para aplicativos da Windows Store e aplicativos do WPF agora é o mesmo no Windows 8.1 e em versões posteriores.

  - **Suporte para janela filho transparente**

    O WPF no .NET Framework 4.6 oferece suporte a janelas filho transparente no Windows 8.1 e versões posteriores. Isso permite a criação de janelas filho não retangulares e janelas filho transparente em suas janelas de nível superior. Você pode habilitar esse recurso configurando a propriedade <xref:System.Windows.Interop.HwndSourceParameters.UsesPerPixelTransparency%2A?displayProperty=nameWithType> como `true`.

- **Windows Communication Foundation (WCF)**

  - **Suporte a SSL**

    Agora, o WCF oferece suporte ao TLS 1.1 e TLS 1.2 versão SSL, além do SSL 3.0 e TLS 1.0, ao usar o NetTcp com autenticação de cliente e segurança de transporte. Agora é possível selecionar qual protocolo usar, ou desabilitar protocolos antigos e menos seguros. Isso pode ser feito definindo a propriedade <xref:System.ServiceModel.TcpTransportSecurity.SslProtocols%2A> ou adicionando o seguinte ao arquivo de configuração.

    ```xml
    <netTcpBinding>
        <binding>
          <security mode= "None|Transport|Message|TransportWithMessageCredential" >
              <transport clientCredentialType="None|Windows|Certificate"
                        protectionLevel="None|Sign|EncryptAndSign"
                        sslProtocols="Ssl3|Tls1|Tls11|Tls12">
                </transport>
          </security>
        </binding>
    </netTcpBinding>
    ```

  - **Enviar mensagens usando conexões HTTP diferentes**

    Agora, o WCF permite que os usuários garantam o envio de determinadas mensagens usando conexões HTTP subjacentes diferentes. Há duas maneiras de fazer isso:

    - **Usar um prefixo de nome de grupo de conexão**

      Os usuários podem especificar uma cadeia de caracteres que o WCF usará como obtenção de prefixo para o nome do grupo de conexão. Duas mensagens com prefixos diferentes são enviadas usando conexões HTTP subjacentes diferentes. Defina o prefixo adicionando um par de chave/valor à propriedade <xref:System.ServiceModel.Channels.Message.Properties%2A?displayProperty=nameWithType> da mensagem. A chave é "HttpTransportConnectionGroupNamePrefix"; o valor é o prefixo desejado.

    - **Usar fábricas de canal diferente**

      Os usuários também podem habilitar um recurso que garante que as mensagens enviadas usando canais criados por fábricas de canal diferentes usarão conexões HTTP subjacentes diferentes. Para habilitar esse recurso, os usuários devem definir o seguinte `appSetting` como `true`:

      ```xml
      <appSettings>
          <add key="wcf:httpTransportBinding:useUniqueConnectionPoolPerFactory" value="true" />
      </appSettings>
      ```

- **Windows Workflow Foundation (WWF)**

  Agora você pode especificar o número de segundos que um serviço de fluxo de trabalho manterá uma solicitação de operação fora de ordem quando houver um indicador de "não protocolo" pendentes antes do tempo limite da solicitação. Um indicador de "não protocolo" é um indicador que não está relacionado a atividades de Recebimento pendentes. Algumas atividades criam indicadores de não protocolo dentro de sua implementação, portanto, talvez a existência de um indicador não protocolo não seja óbvia. Entre elas estão Estado e Seleção. Então, se você tiver um serviço de fluxo de trabalho implementado com uma máquina de estado, ou que contenha uma atividade de Seleção, provavelmente você terá indicadores de não protocolo. Especifique o intervalo adicionando uma linha semelhante à seguinte à seção `appSettings` do arquivo app.config:

  ```xml
  <add key="microsoft:WorkflowServices:FilterResumeTimeoutInSeconds" value="60"/>
  ```

  O valor padrão é 60 segundos. Se `value` estiver definido como 0, as solicitações de fora de ordem serão imediatamente rejeitadas com uma falha e o seguinte texto:

  ```console
  Operation 'Request3|{http://tempuri.org/}IService' on service instance with identifier '2b0667b6-09c8-4093-9d02-f6c67d534292' cannot be performed at this time. Please ensure that the operations are performed in the correct order and that the binding in use provides ordered delivery guarantees.
  ```

  Essa é a mesma mensagem recebida se uma mensagem de operação fora de ordem for recebida e não houver nenhum indicador de não protocolo.

  Se o valor do elemento `FilterResumeTimeoutInSeconds` for diferente de zero, houver indicadores de não protocolo e o intervalo de tempo limite expirar, a operação falhará com uma mensagem de tempo limite.

- **Transações**

  Agora você pode incluir o identificador de transação distribuída para a transação que causou uma exceção derivada de <xref:System.Transactions.TransactionException>. Faça isso adicionando a seguinte chave à seção `appSettings` do arquivo app.config:

  ```xml
  <add key="Transactions:IncludeDistributedTransactionIdInExceptionMessage" value="true"/>
  ```

  O valor padrão é `false`.

- **Rede**

  - **Reutilização de soquete**

    O Windows 10 inclui um novo algoritmo de rede de alta escalabilidade que utiliza melhor os recursos da máquina reutilizando portas locais para conexões TCP de saída. O .NET Framework 4.6 oferece suporte ao novo algoritmo, permitindo que aplicativos .NET aproveitem o novo comportamento. Em versões anteriores do Windows, havia um limite de conexão simultânea artificial (normalmente 16.384, o tamanho padrão do intervalo de porta dinâmica), que pode limitar a escalabilidade de um serviço causando o esgotamento de porta sob carga.

    No .NET Framework 4,6, duas APIs foram adicionadas para habilitar a reutilização de porta, o que efetivamente remove o limite de 64 KB em conexões simultâneas:

    - O valor de enumeração <xref:System.Net.Sockets.SocketOptionName?displayProperty=nameWithType>.

    - A propriedade de <xref:System.Net.ServicePointManager.ReusePort%2A?displayProperty=nameWithType> .

    Por padrão, a propriedade <xref:System.Net.ServicePointManager.ReusePort%2A?displayProperty=nameWithType> é `false`, a menos que o valor `HWRPortReuseOnSocketBind` da chave do Registro `HKLM\SOFTWARE\Microsoft\.NETFramework\v4.0.30319` seja definido como 0x1. Para habilitar a reutilização de porta local em conexões HTTP, defina a propriedade <xref:System.Net.ServicePointManager.ReusePort%2A?displayProperty=nameWithType> como `true`. Isso faz com que todas as conexões de soquete TCP externa do <xref:System.Net.Http.HttpClient> e <xref:System.Net.HttpWebRequest> usem uma nova opção de soquete do Windows 10 [SO_REUSE_UNICASTPORT](/windows/desktop/WinSock/sol-socket-socket-options), que permite a reutilização de porta local.

    Os desenvolvedores que criam um aplicativo somente para soquetes podem especificar a opção <xref:System.Net.Sockets.SocketOptionName?displayProperty=nameWithType> ao chamar um método, como <xref:System.Net.Sockets.Socket.SetSocketOption%2A?displayProperty=nameWithType>, para que os soquetes de saídas reutilizem portas locais durante a associação.

  - **Suporte para nomes de domínio internacionais e PunyCode**

    Uma nova propriedade, <xref:System.Uri.IdnHost%2A>, foi adicionada à classe <xref:System.Uri> para oferecer melhor suporte a nomes de domínio internacionais e PunyCode.

- **Redimensionamento em controles do Windows Forms.**

  Esse recurso foi expandido no NET Framework 4.6 para incluir os tipos <xref:System.Windows.Forms.DomainUpDown>, <xref:System.Windows.Forms.NumericUpDown>, <xref:System.Windows.Forms.DataGridViewComboBoxColumn>, <xref:System.Windows.Forms.DataGridViewColumn> e <xref:System.Windows.Forms.ToolStripSplitButton>, e o retângulo especificado pela propriedade <xref:System.Drawing.Design.PaintValueEventArgs.Bounds%2A> usada ao desenhar um <xref:System.Drawing.Design.UITypeEditor>.

  Esse é um recurso de opção de aceitação. Para habilitá-lo, defina o elemento `EnableWindowsFormsHighDpiAutoResizing` como `true` no arquivo de configuração do aplicativo (app.config):

  ```xml
  <appSettings>
      <add key="EnableWindowsFormsHighDpiAutoResizing" value="true" />
  </appSettings>
  ```

- **Suporte para codificações de página de código**

  O .NET Core oferece suporte principalmente a codificações Unicode, e por padrão fornece suporte limitado a codificações de página de código. É possível adicionar suporte a codificações de página de código disponíveis no .NET Framework, mas que não sejam compatíveis ao .NET Core ao registrar codificações de página de código com o método <xref:System.Text.Encoding.RegisterProvider%2A?displayProperty=nameWithType>. Para obter mais informações, consulte <xref:System.Text.CodePagesEncodingProvider?displayProperty=nameWithType>.

- **.NET Nativo**

  Aplicativos do Windows para Windows 10 direcionados ao .NET Core e escritos em C# ou Visual Basic podem tirar proveito de uma nova tecnologia que compila aplicativos para código nativo em vez de IL. Ela produz aplicativos caracterizados por inicialização e tempos de execução mais rápidos. Para saber mais, confira [Compilação de aplicativos com .NET Nativo](../net-native/index.md). Para obter uma visão geral do .NET Nativo, examinando qual é a diferente dele para a compilação JIT e o NGEN e o que isso significa para seu código, confira [.NET Nativo e compilação](../net-native/net-native-and-compilation.md).

  Seus aplicativos são compilados por padrão para código nativo quando você os compila com o Visual Studio 2015 ou posterior. Para saber mais, confira [Introdução ao .NET Nativo](../net-native/getting-started-with-net-native.md).

  Para oferecer suporte à depuração de aplicativos do .NET Nativo, foram adicionadas diversas interfaces e enumerações novas à API de depuração não gerenciada. Para saber mais, confira o tópico [Depuração (Referência de API não gerenciada)](../unmanaged-api/debugging/index.md).

- **Pacotes do código-fonte aberto do .NET Framework**

  Pacotes do .NET Core, como as coleções imutáveis, as [APIs SIMD](https://www.nuget.org/packages/Microsoft.Bcl.Simd)e as APIs de rede, como as encontradas no <xref:System.Net.Http> namespace, agora estão disponíveis como pacotes de software livre no [GitHub](https://github.com/). Para acessar o código, consulte [.net no GitHub](https://github.com/dotnet/runtime). Para saber mais e saber como contribuir com esses pacotes, confira [.NET Core e código-fonte aberto](../get-started/net-core-and-open-source.md), [Home Page do .NET no GitHub](https://github.com/dotnet/home).

<a name="v452"></a>

## <a name="whats-new-in-net-framework-452"></a>Novidades no .NET Framework 4.5.2

- **Novas APIs para aplicativos do ASP.NET.** Os novos métodos <xref:System.Web.HttpResponse.AddOnSendingHeaders%2A?displayProperty=nameWithType> e <xref:System.Web.HttpResponseBase.AddOnSendingHeaders%2A?displayProperty=nameWithType> permitem inspecionar e modificar os cabeçalhos de resposta e o código de status à medida que a resposta é liberada para o aplicativo do cliente. Sugerimos usar esses métodos ao invés dos eventos <xref:System.Web.HttpApplication.PreSendRequestHeaders> e <xref:System.Web.HttpApplication.PreSendRequestContent>; eles são mais eficientes e confiáveis.

  O método <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem%2A?displayProperty=nameWithType> permite agendar pequenos itens de trabalho em segundo plano. ASP.NET rastreia esses itens e evita que IIS termine abruptamente o processo de trabalho até que todos os itens de trabalho em segundo plano tenham sido concluídos. Esse método não pode ser chamado fora de um domínio de aplicativo gerenciado pelo ASP.NET.

  As novas propriedades <xref:System.Web.HttpResponse.HeadersWritten?displayProperty=nameWithType> e <xref:System.Web.HttpResponseBase.HeadersWritten?displayProperty=nameWithType> retornam valores boolianos que indicam se os cabeçalhos de resposta foram gravados. É possível usar essas propriedades para certificar-se de que as chamadas para APIs como <xref:System.Web.HttpResponse.StatusCode%2A?displayProperty=nameWithType> (que geram exceções se os cabeçalhos foram gravados) tiveram êxito.

- **Redimensionamento em controles do Windows Forms.** Esse recurso foi expandido. É possível usar a configuração do sistema DPI para redimensionar os componentes dos seguintes controles adicionais (por exemplo, a seta suspensa nas caixas de combinação):

  - <xref:System.Windows.Forms.ComboBox>
  - <xref:System.Windows.Forms.ToolStripComboBox>
  - <xref:System.Windows.Forms.ToolStripMenuItem>
  - <xref:System.Windows.Forms.Cursor>
  - <xref:System.Windows.Forms.DataGridView>
  - <xref:System.Windows.Forms.DataGridViewComboBoxColumn>

  Esse é um recurso de opção de aceitação. Para habilitá-lo, defina o elemento `EnableWindowsFormsHighDpiAutoResizing` como `true` no arquivo de configuração do aplicativo (app.config):

  ```xml
  <appSettings>
      <add key="EnableWindowsFormsHighDpiAutoResizing" value="true" />
  </appSettings>
  ```

- **Novo recurso de fluxo de trabalho.** Um gerenciador de recursos que usa o método <xref:System.Transactions.Transaction.EnlistPromotableSinglePhase%2A> (e, portanto, implementa a interface <xref:System.Transactions.IPromotableSinglePhaseNotification>) pode usar o novo método <xref:System.Transactions.Transaction.PromoteAndEnlistDurable%2A?displayProperty=nameWithType> para requisitar o seguinte:

  - Promover a transação para um Coordenador de transações distribuídas da Microsoft (MSDTC).

  - Substitua <xref:System.Transactions.IPromotableSinglePhaseNotification> por <xref:System.Transactions.ISinglePhaseNotification>, que é uma inscrição durável que é compatível com a confirmação de uma única fase.

  Isso pode ser feito dentro do mesmo domínio do aplicativo e não requer nenhum código extra não gerado para interagir com o MSDTC para executar a promoção. O novo método pode ser chamado apenas quando há uma chamada pendente do método <xref:System.Transactions?displayProperty=nameWithType> para o <xref:System.Transactions.IPromotableSinglePhaseNotification>`Promote` implementado por uma inscrição que pode ser promovida.

- **Aprimoramentos na criação de perfil.** As seguintes novas APIs não gerenciadas de criação de perfis fornecem uma criação de perfil mais robusta:

  - [Estrutura COR_PRF_ASSEMBLY_REFERENCE_INFO](../unmanaged-api/profiling/cor-prf-assembly-reference-info-structure.md)
  - [Enumeração COR_PRF_HIGH_MONITOR](../unmanaged-api/profiling/cor-prf-high-monitor-enumeration.md)
  - [Método GetAssemblyReferences](../unmanaged-api/profiling/icorprofilercallback6-getassemblyreferences-method.md)
  - [Método GetEventMask2](../unmanaged-api/profiling/icorprofilerinfo5-geteventmask2-method.md)
  - [Método SetEventMask2](../unmanaged-api/profiling/icorprofilerinfo5-seteventmask2-method.md)
  - [Método AddAssemblyReference](../unmanaged-api/profiling/icorprofilerassemblyreferenceprovider-addassemblyreference-method.md)

  Implementações anteriores de `ICorProfiler` compatíveis com carregamento lento de montagens pendentes. As novas APIs de criação de perfis requerem montagens dependentes que são injetadas pelo criador de perfil a ser carregado imediatamente, em vez de ser carregado depois que o aplicativo for totalmente inicializado. Essa mudança não afeta os usuários das APIs `ICorProfiler` existentes.

- **Aprimoramentos de depuração.** As seguintes APIs de depuração novas não gerenciadas fornecem uma melhor integração com um criador de perfil. Agora é possível acessar metadados inseridos por um criador de perfil bem como variáveis locais e código produzido por pedidos do ReJIT compilador quando despejo do depurador.

  - [Método SetWriteableMetadataUpdateMode](../unmanaged-api/debugging/icordebugprocess7-setwriteablemetadataupdatemode-method.md)
  - [Método EnumerateLocalVariablesEx](../unmanaged-api/debugging/icordebugilframe4-enumeratelocalvariablesex-method.md)
  - [Método GetLocalVariableEx](../unmanaged-api/debugging/icordebugilframe4-getlocalvariableex-method.md)
  - [Método GetCodeEx](../unmanaged-api/debugging/icordebugilframe4-getcodeex-method.md)
  - [Método GetActiveReJitRequestILCode](../unmanaged-api/debugging/icordebugfunction3-getactiverejitrequestilcode-method.md)
  - [Método GetInstrumentedILMap](../unmanaged-api/debugging/icordebugilcode2-getinstrumentedilmap-method.md)

- **Mudanças no rastreamento de eventos.** O .NET Framework 4.5.2 permite o rastreamento fora do processo para atividades baseadas no ETW (Rastreamento de Eventos para Windows) em uma área de superfície maior. Isso permite que fornecedores de Gerenciamento Avançado de Energia (APM) forneçam ferramentas que rastreiam com precisão os custos de pedidos individuais e atividades entre threads.  Esses eventos são gerados apenas quando os controladores ETW permitem e, portanto, as mudanças não afetam o código ETW gravado anteriormente ou o código executado com o ETW desabilitado.

- **Promover uma transação e convertê-la em uma inscrição durável**

  <xref:System.Transactions.Transaction.PromoteAndEnlistDurable%2A?displayProperty=nameWithType> é uma nova API adicionada ao .NET Framework 4.5.2 e 4.6:

  ```csharp
  [System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name = "FullTrust")]
  public Enlistment PromoteAndEnlistDurable(Guid resourceManagerIdentifier,
                                            IPromotableSinglePhaseNotification promotableNotification,
                                            ISinglePhaseNotification enlistmentNotification,
                                            EnlistmentOptions enlistmentOptions)
  ```

  ```vb
  <System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name:="FullTrust")>
  public Function PromoteAndEnlistDurable(resourceManagerIdentifier As Guid,
                                          promotableNotification As IPromotableSinglePhaseNotification,
                                          enlistmentNotification As ISinglePhaseNotification,
                                          enlistmentOptions As EnlistmentOptions) As Enlistment
  ```

  O método pode ser usado por uma inscrição criada anteriormente por <xref:System.Transactions.Transaction.EnlistPromotableSinglePhase%2A?displayProperty=nameWithType> em resposta ao método <xref:System.Transactions.ITransactionPromoter.Promote%2A?displayProperty=nameWithType>. Ele pede que `System.Transactions` promova a transação a uma transação MSDTC e "converta" a inscrição passível de promoção em uma inscrição durável. Após a conclusão desse método, a interface <xref:System.Transactions.IPromotableSinglePhaseNotification> não será mais referenciada por `System.Transactions`, e quaisquer notificações futuras chegarão na interface <xref:System.Transactions.ISinglePhaseNotification> fornecida. A inscrição em questão deve agir como uma inscrição durável, oferecendo suporte à ao registro em log e recuperação da transação. Veja <xref:System.Transactions.Transaction.EnlistDurable%2A?displayProperty=nameWithType> para obter detalhes. Além disso, a inscrição deve oferecer suporte a <xref:System.Transactions.ISinglePhaseNotification>.  Esse método *só* pode ser chamado durante o processamento de uma chamada <xref:System.Transactions.ITransactionPromoter.Promote%2A?displayProperty=nameWithType>. Se esse não for o caso, uma exceção <xref:System.Transactions.TransactionException> será gerada.

<a name="v451"></a>

## <a name="whats-new-in-net-framework-451"></a>Novidades no .NET Framework 4.5.1

**Atualizações de abril de 2014**:

- [Atualização 2 do Visual Studio 2013](https://go.microsoft.com/fwlink/p/?LinkId=393658) inclui atualizações para modelos de Biblioteca de Classes Portátil para suporte destes cenários:

  - É possível usar as APIs do Windows Runtime em bibliotecas portáteis que tenham como destino Windows 8.1, Windows Phone 8.1 e Windows Phone Silverlight 8.1.

  - Você pode incluir XAML (Windows.UI.XAML types) em bibliotecas portáteis quando o destino for Windows 8.1 ou Windows Phone 8.1. Os seguintes modelos XAML são compatíveis: Blank Page, Resource Dictionary, Templated Control e User Control.

  - Você pode criar um componente do Windows Runtime portátil (.winmd file) para uso em Store apps cujo destino seja Windows 8.1 e Windows Phone 8.1.

  - É possível redirecionar uma biblioteca de classes Windows Store ou Windows Phone Store novamente como uma Biblioteca de Classes Portátil.

  Para saber mais sobre essas mudanças, confira [Biblioteca de classes portátil](../../standard/cross-platform/cross-platform-development-with-the-portable-class-library.md).

- O conjunto de conteúdo do .NET Framework agora inclui documentação para .NET Native, que é uma tecnologia de pré-compilação para construção e implantação dos aplicativos do Windows. Para melhorar o desempenho, o .NET Native compila seus aplicativos diretamente para o código nativo, ao invés de usar uma linguagem intermediária (IL). Para obter detalhes, confira [Compilação de aplicativos com o .NET Nativo](../net-native/index.md).

- O [.NET Framework Reference Source](https://referencesource.microsoft.com/) fornece experiências de navegação e de funcionalidade, novas e aprimoradas. É possível navegar através do código-fonte online do .NET Framework, [baixar a referência](https://referencesource.microsoft.com/download.html) para visualização offline e percorrer as fontes (incluindo correções e atualizações) durante a depuração. Para saber mais, confira a postagem no blog [A new look for .NET Reference Source](https://devblogs.microsoft.com/dotnet/a-new-look-for-net-reference-source/) (Um novo olhar sobre a fonte de referência do .NET).

Os novos recursos e aprimoramentos nas classes base do .NET Framework 4.5.1 incluem:

- Redirecionamento de associação automático de assemblies. Desde o Visual Studio 2013, quando você compilar um aplicativo destinado ao .NET Framework 4.5.1, os redirecionamentos de associação poderão ser adicionados ao arquivo de configuração do aplicativo se o aplicativo ou seus componentes referenciarem várias versões do mesmo assembly. Você também pode habilitar esse recurso para projetos que se destinam a versões anteriores do .NET Framework. Para saber mais, confira [Como habilitar e desabilitar o redirecionamento automático de associações](../configure-apps/how-to-enable-and-disable-automatic-binding-redirection.md).

- Capacidade de coletar informações de diagnóstico para ajudar desenvolvedores na melhoria do desempenho dos aplicativos de servidor e de nuvem. Para obter mais informações, consulte os métodos <xref:System.Diagnostics.Tracing.EventSource.WriteEventWithRelatedActivityId%2A> e <xref:System.Diagnostics.Tracing.EventSource.WriteEventWithRelatedActivityIdCore%2A> na classe <xref:System.Diagnostics.Tracing.EventSource>.

- Capacidade de compactar explicitamente a LOH (pilha de objetos grandes) durante a coleta de lixo. Para obter mais informações, consulte a propriedade <xref:System.Runtime.GCSettings.LargeObjectHeapCompactionMode%2A?displayProperty=nameWithType>.

- Aperfeiçoamentos de desempenho adicionais como a suspensão do aplicativo ASP.NET, aperfeiçoamentos JIT multicore e inicialização mais rápida do aplicativo após uma atualização do .NET Framework. Para obter detalhes, confira o [Comunicado do .NET Framework 4.5.1](https://devblogs.microsoft.com/dotnet/announcing-the-net-framework-4-5-1-preview/) e a postagem de blog [Suspensão de aplicativos ASP.NET](https://devblogs.microsoft.com/dotnet/asp-net-app-suspend-responsive-shared-net-web-hosting/).

Os aprimoramentos para Windows Forms incluem:

- Redimensionamento em controles do Windows Forms. É possível usar a configuração do sistema DPI para redimensionar os componentes de controle (por exemplo, os ícones que aparecem na grade de propriedade) optando por uma entrada no arquivo de configuração de aplicativo (app.config) do seu aplicativo. Atualmente, esse recurso é compatível com os seguintes controles do Windows Forms:

  - <xref:System.Windows.Forms.PropertyGrid>
  - <xref:System.Windows.Forms.TreeView>
  -     Alguns aspectos do <xref:System.Windows.Forms.DataGridView> (consulte [novos recursos em 4.5.2](#v452) para obter os controles adicionais compatíveis)

  Para habilitar esse recurso, adicione um novo \<appSettings> elemento ao arquivo de configuração (app.config) e defina o `EnableWindowsFormsHighDpiAutoResizing` elemento como `true` :

  ```xml
  <appSettings>
      <add key="EnableWindowsFormsHighDpiAutoResizing" value="true" />
  </appSettings>
  ```

Entre os aperfeiçoamentos durante a depuração de seus aplicativos do .NET Framework no Visual Studio 2013 estão:

- Valores de retorno no depurador do Visual Studio. Quando você depura um aplicativo gerenciado no Visual Studio 2013, a janela Autos exibe valores e tipos de retorno para os métodos. Essas informações estão disponíveis para aplicativos de área de trabalho, Windows Store e Windows Phone. Confira mais informações em [Examinar valores de retorno de chamadas de método](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/dn323257(v=vs.120)).

- Editar e continuar para aplicativos 64 bits. O Visual Studio 2013 dá suporte ao recurso Editar e continuar para aplicativos gerenciados de 64 bits para área de trabalho, Windows Store e Windows Phone. As limitações existentes permanecem em vigor para aplicativos 32 e 64 bits (confira a última seção do artigo [Alterações de código compatíveis (C#)](/visualstudio/debugger/supported-code-changes-csharp)).

- Depuração async-aware. Para facilitar a depuração de aplicativos assíncronos no Visual Studio 2013, a pilha de chamadas oculta o código de infraestrutura fornecido por compiladores para dar suporte à programação assíncrona e também as cadeias em quadros pai lógicos para que você possa acompanhar a execução lógica do programa com mais clareza. Uma janela Tarefas substitui a janela Tarefas Paralelas e exibe tarefas relacionadas a um ponto de interrupção específico e também exibe outras tarefas que estão ativas ou agendadas no aplicativo. Leia sobre esse recurso na seção "Depuração async-aware" do [Comunicado do .NET Framework 4.5.1](https://devblogs.microsoft.com/dotnet/announcing-the-net-framework-4-5-1-preview/).

- Melhor suporte à exceção para componentes do Windows Runtime. No Windows 8.1, as exceções surgidas de aplicativos da Windows Store preservam as informações sobre o erro que causou a exceção, mesmo entre os limites de linguagem. Leia sobre esse recurso na seção "Desenvolvimento de aplicativos para a Windows Store" do [Comunicado do .NET Framework 4.5.1](https://devblogs.microsoft.com/dotnet/announcing-the-net-framework-4-5-1-preview/).

A partir do Visual Studio 2013, você pode usar a [Ferramenta de Otimização Orientada de Perfil Gerenciado (Mpgo.exe)](../tools/mpgo-exe-managed-profile-guided-optimization-tool.md) para otimizar aplicativos Windows 8.x Store, bem como aplicativos da área de trabalho.

Para novos recursos no ASP.NET 4.5.1, confira [Notas sobre a versão do ASP.NET and Web Tools para Visual Studio 2013](/aspnet/visual-studio/overview/2013/release-notes).

<a name="v45"></a>

## <a name="whats-new-in-net-framework-45"></a>Novidades no .NET Framework 4.5

### <a name="base-classes"></a>Classes base

- Capacidade de reduzir as reinicializações do sistema detectando e fechando-se os aplicativos do .NET Framework 4 durante a implantação. Confira [Redução de reinicializações do sistema durante instalações do .NET Framework 4.5](../deployment/reducing-system-restarts.md).

- Suporte para matrizes maiores que 2 gigabytes (GB) em plataformas 64 bits. Esse recurso pode ser habilitado no arquivo de configuração do aplicativo. Consulte o [ \<gcAllowVeryLargeObjects> elemento](../configure-apps/file-schema/runtime/gcallowverylargeobjects-element.md), que também lista outras restrições sobre tamanho do objeto e tamanho da matriz.

- Melhor desempenho por meio de coleta de lixo em segundo plano para servidores. Quando você usa a coleta de lixo do servidor no .NET Framework 4.5, a coleta de lixo em segundo plano é automaticamente ativada. Confira a seção Coleta de lixo de servidor em segundo plano do tópico [Noções básicas da coleta de lixo](../../standard/garbage-collection/fundamentals.md).

- Compilação JIT (just-in-time) em segundo plano, disponível como opção em vários processadores multicore para melhorar o desempenho do aplicativo. Consulte <xref:System.Runtime.ProfileOptimization>.

- Capacidade de limitar quanto tempo o mecanismo de expressão regular tentará resolver uma expressão regular antes de atingir o tempo limite. Consulte a <xref:System.Text.RegularExpressions.Regex.MatchTimeout%2A?displayProperty=nameWithType> propriedade.

- Capacidade de definir a cultura padrão para um domínio de aplicativo. Consulte a classe <xref:System.Globalization.CultureInfo>.

- Suporte de console para a codificação Unicode (UTF-16). Consulte a classe <xref:System.Console>.

- Suporte para controle de versão da ordenação de cadeia de caracteres culturais e dados de comparação. Consulte a classe <xref:System.Globalization.SortVersion>.

- Melhor desempenho durante a recuperação de recursos. Confira [Empacotar e implantar recursos](../resources/packaging-and-deploying-resources-in-desktop-apps.md).

- Aperfeiçoamentos na compactação para reduzir o tamanho de um arquivo compactado. Consulte o namespace <xref:System.IO.Compression?displayProperty=nameWithType>.

- Capacidade de personalizar um contexto de reflexão para substituir o comportamento de reflexão padrão por meio da classe <xref:System.Reflection.Context.CustomReflectionContext>.

- Suporte para a versão 2008 dos nomes de domínio internacionalizados no padrão de aplicativos (IDNA) quando a <xref:System.Globalization.IdnMapping?displayProperty=nameWithType> classe é usada no Windows 8.

- A delegação da comparação de cadeia de caracteres para o sistema operacional, que implementa o Unicode 6.0, quando o .NET Framework é usado no Windows 8. Ao executar em outras plataformas, o .NET Framework inclui seus próprios dados de comparação da cadeia de caracteres, o que implementa o Unicode 5.x. Consulte a classe <xref:System.String> e a seção Comentários da classe <xref:System.Globalization.SortVersion>.

- Capacidade de computar os códigos de hash para cadeias de caracteres com base no domínio do aplicativo. Consulte o [ \<UseRandomizedStringHashAlgorithm> elemento](../configure-apps/file-schema/runtime/userandomizedstringhashalgorithm-element.md).

- Suporte à reflexão de tipo dividido entre as classes <xref:System.Type> e <xref:System.Reflection.TypeInfo>. Confira [Reflexão no .NET Framework para aplicativos da Windows Store](../reflection-and-codedom/reflection-for-windows-store-apps.md).

### <a name="managed-extensibility-framework-mef"></a>MEF (Managed Extensibility Framework)

No .NET Framework 4.5, a MEF (Managed Extensibility Framework) fornece os seguintes recursos novos:

- Suporte para tipos genéricos.

- O modelo de programação baseado na convenção permite criar partes com base em convenções de nomenclatura, em vez de atributos.

- Vários escopos.

- Um subconjunto da MEF que você pode usar ao criar aplicativos Windows 8.x Store. Esse subconjunto está disponível como um [pacote baixável](https://www.nuget.org/packages/Microsoft.Composition) da Galeria NuGet. Para instalar o pacote, abra o projeto no Visual Studio, escolha **Gerenciar Pacotes NuGet** no menu **Projeto** e procure o pacote `Microsoft.Composition` online.

Para saber mais, confira [Managed Extensibility Framework (MEF)](../mef/index.md).

### <a name="asynchronous-file-operations"></a>Operações de arquivo assíncronas

No .NET Framework 4.5, os novos recursos assíncronos foram adicionados às linguagens C# e Visual Basic. Esses recursos adicionam um modelo com base na tarefa para executar operações assíncronas. Para usar esse novo modelo, use os métodos assíncronos nas classes de E/S. Confira [E/S de arquivo assíncrona](../../standard/io/asynchronous-file-i-o.md).

<a name="tools"></a>

### <a name="tools"></a>Ferramentas

No .NET Framework 4.5, o Gerador de Arquivos de Recurso (Resgen.exe) permite criar um arquivo .resw a ser usado em aplicativos Windows 8.x Store com base em um arquivo .resources inserido em um assembly do .NET Framework. Para saber mais, confira [Resgen.exe (Gerador de arquivo de recurso)](../tools/resgen-exe-resource-file-generator.md).

A Ferramenta de Otimização Orientada de Perfil Gerenciado (Mpgo.exe) permite melhorar o tempo de inicialização do aplicativo, a utilização da memória (tamanho do conjunto de trabalho) e a produtividade otimizando-se assemblies de imagem nativa. A ferramenta de linha de comando gera dados de perfil para assemblies de aplicativo de imagem nativa. Confira [Mpgo.exe (Ferramenta de Otimização Guiada por Perfil Gerenciado)](../tools/mpgo-exe-managed-profile-guided-optimization-tool.md). A partir do Visual Studio 2013, você pode usar a Mpgo.exe para otimizar aplicativos Windows 8.x Store, bem como aplicativos da área de trabalho.

<a name="parallel"></a>

### <a name="parallel-computing"></a>Computação paralela

O .NET Framework 4.5 fornece vários recursos e aperfeiçoamentos novos para computação paralela. Entre eles estão melhor desempenho, mais controle, maior suporte para programação assíncrona, uma nova biblioteca de fluxo de dados e o melhor suporte para a depuração paralela e a análise de desempenho. Veja a entrada [novidades para paralelismo no .net 4,5](https://devblogs.microsoft.com/pfxteam/whats-new-for-parallelism-in-net-4-5/) no blog programação paralela com o .net.

<a name="web"></a>

### <a name="web"></a>Web

ASP.NET 4.5 e 4.5.1 adicionam model binding para formulários da Web, suporte WebSocket, manipuladores assíncronos, aperfeiçoamentos de desempenho e muitos outros recursos. Para saber mais, consulte os recursos a seguir:

- [ASP.NET 4.5 e Visual Studio 2012](https://docs.microsoft.com/previous-versions/aspnet/hh420390(v=vs.110))

- [Notas de versão do ASP.NET and Web Tools para Visual Studio 2013](/aspnet/visual-studio/overview/2013/release-notes)

### <a name="networking"></a>Rede <a name="networking"></a>

O .NET Framework 4.5 fornece uma nova interface de programação para aplicativos HTTP. Para obter mais informações, consulte os novos namespaces <xref:System.Net.Http?displayProperty=nameWithType> e <xref:System.Net.Http.Headers?displayProperty=nameWithType>.

O suporte também está incluído para uma nova interface de programação para aceitar e interagir com uma conexão WebSocket usando-se a classe <xref:System.Net.HttpListener> existente e as classes relacionadas. Para obter mais informações, consulte o novo namespace <xref:System.Net.WebSockets> e a classe <xref:System.Net.HttpListener>.

Além disso, o .NET Framework 4.5 inclui os seguintes aperfeiçoamentos de rede:

- Suporte ao URI compatível com RFC. Para obter mais informações, consulte <xref:System.Uri> e as classes relacionadas.

- Suporte para análise IDN. Para obter mais informações, consulte <xref:System.Uri> e as classes relacionadas.

- Suporte para EAI (Email Address Internationalization). Para obter mais informações, consulte o namespace de <xref:System.Net.Mail>.

- Suporte a IPv6 melhorado. Para obter mais informações, consulte o namespace de <xref:System.Net.NetworkInformation>.

- Suporte a soquete de modo duplo. Para obter mais informações, consulte as classes <xref:System.Net.Sockets.Socket> e <xref:System.Net.Sockets.TcpListener>.

<a name="client"></a>

### <a name="windows-presentation-foundation-wpf"></a>Windows Presentation Foundation (WPF)

No .NET Framework 4.5, o Windows Presentation Foundation (WPF) contém modificações e aperfeiçoamentos nas seguintes áreas:

- O novo controle <xref:System.Windows.Controls.Ribbon.Ribbon>, que permite que você implemente uma interface de usuário da faixa de opções que hospeda uma Barra de Ferramentas de Acesso Rápido, um menu do aplicativo e guias.

- A nova interface <xref:System.ComponentModel.INotifyDataErrorInfo>, que dá suporte à validação de dados síncronos e assíncronos.

- Novos recursos para as classes <xref:System.Windows.Controls.VirtualizingPanel> e <xref:System.Windows.Threading.Dispatcher>.

- Desempenho melhorado durante a exibição de grandes conjuntos de dados agrupados e o acesso a coleções em threads que não sejam de IU.

- Associação de dados a propriedades estáticas, associação de dados a tipos personalizados que implementam a interface <xref:System.Reflection.ICustomTypeProvider> e a recuperação de informações de associação de dados de uma expressão de associação.

- Reposicionamento de dados à medida que os valores mudam (shaping dinâmico).

- Capacidade de verificar se o contexto de dados de um contêiner de itens está desconectado.

- Capacidade de definir o tempo que deve decorrer entre as alterações de propriedade e as atualizações da fonte de dados.

- Suporte melhorado para implementar padrões de evento fracos. Além disso, os eventos agora podem aceitar extensões de marcação.

<a name="windows_communication_foundation"></a>

### <a name="windows-communication-foundation-wcf"></a>Windows Communication Foundation (WCF)

No .NET Framework 4.5, os seguintes recursos foram adicionados para simplificar a gravação e a manutenção de aplicativos do Windows Communication Foundation (WCF):

- Simplificação de arquivos de configuração gerados.

- Suporte para desenvolvimento de primeiro contrato.

- Capacidade de configurar o modo de compatibilidade do ASP.NET mais facilmente.

- Alterações em valores de propriedade de transporte padrão para reduzir a probabilidade de você defini-los.

- Atualizações feitas na classe <xref:System.Xml.XmlDictionaryReaderQuotas> reduzem a probabilidade de você precisar configurar manualmente as cotas para leitores de dicionário XML.

- Validação de arquivos de configuração WCF pelo Visual Studio como parte do processo de compilação, de forma que você possa detectar erros de configuração antes de executar o aplicativo.

- Novo suporte de streaming assíncrono.

- Novo mapeamento de protocolo HTTPS para facilitar a exposição de um ponto de extremidade em HTTPS com o IIS (Serviços de Informações da Internet).

- Capacidade de gerar metadados em um único documento WSDL acrescentando `?singleWSDL` à URL de serviço.

- Suporte de Websockets para habilitar comunicação bidirecional real em portas 80 e 443 com as características de desempenho semelhantes ao transporte TCP.

- Suporte para configurar serviços no código.

- Dicas de ferramenta do Editor de XML

- Suporte ao armazenamento em cache <xref:System.ServiceModel.ChannelFactory>.

- Suporte à compactação de codificador binário.

- Suporte para um transporte UDP que permite que os desenvolvedores gravem serviços que usem o sistema de mensagens "acionar e esquecer". Um cliente envia uma mensagem para um serviço e não espera nenhuma resposta do serviço.

- Capacidade de dar suporte a vários modos de autenticação em um único ponto de extremidade WCF durante o uso do transporte HTTP e da segurança de transporte.

- Suporte para serviços WCF que usam IDNs (nomes de domínio internacionalizados).

Para saber mais, confira [Novidades no Windows Communication Foundation](../wcf/whats-new.md).

<a name="windows_workflow_foundation"></a>

### <a name="windows-workflow-foundation-wf"></a>Windows Workflow Foundation (WF)

No .NET Framework 4.5, vários recursos novos foram adicionados ao Windows Workflow Foundation (WF), incluindo:

- Os fluxos de trabalho da máquina de estado, que foram introduzidos pela primeira vez como parte do .NET Framework 4.0.1 ([Atualização 1 da Plataforma .NET Framework 4](https://docs.microsoft.com/archive/blogs/endpoint/microsoft-net-framework-4-platform-update-1)). Essa atualização incluiu várias classes e atividades novas que permitiram que os desenvolvedores criassem fluxos de trabalho de computador do estado. Essas classes e atividades foram atualizadas para que o .NET Framework 4.5 inclua:

  - A capacidade de definir pontos de interrupção em estados.

  - A capacidade de copiar e colar transições no designer de fluxo de trabalho.

  - Suporte de designer para criação de transição do disparador compartilhado.

  - Atividades para criar fluxos de trabalho de computador de estado, incluindo: <xref:System.Activities.Statements.StateMachine>, <xref:System.Activities.Statements.State> e <xref:System.Activities.Statements.Transition>.

- Recursos do Designer de Fluxo de Trabalho aperfeiçoados, como o seguinte:

  - Capacidades de pesquisa do fluxo de trabalho aperfeiçoado no Visual Studio, incluindo **Localização Rápida** e **Localizar nos Arquivos**.

  - Capacidade de criar automaticamente uma atividade de Sequência quando uma segunda atividade filho é adicionada a uma atividade de contêiner e de incluir ambas as atividades na atividade de Sequência.

  - Suporte a movimento panorâmico, o que permite que a parte visível de um fluxo de trabalho seja alterada sem o uso das barras de rolagem.

  - Uma nova exibição **Estrutura de Tópicos de Documentos** mostra os componentes de um fluxo de trabalho em uma exibição de contorno em estilo árvore e permite selecionar um componente na exibição **Estrutura de Tópicos de Documentos**.

  - Capacidade de adicionar anotações a atividades.

  - Capacidade de definir e consumir representantes de atividade usando-se o designer de fluxo de trabalho.

  - Conexão e inserção automática para atividades e transições na máquina de estado e fluxos de trabalho do fluxograma.

- Armazenamento das informações do estado de exibição para um fluxo de trabalho em um único elemento no arquivo XAML, logo, você pode localizar e editar facilmente as informações de estado de exibição.

- Uma atividade de contêiner NoPersistScope para impedir que as atividades filho persistam.

- Suporte para expressões do C#:

  - Os projetos de fluxo de trabalho que usam o Visual Basic usarão expressões do Visual Basic e os projetos de fluxo de trabalho do C# usarão as expressões do C#.

  - Projetos de fluxo de trabalho do C# criados no Visual Studio 2010 e com expressões do Visual Basic são compatíveis com projetos do fluxo de trabalho do C# que usem expressões do C#.

- Aperfeiçoamentos do controle de versão:

  - A nova classe <xref:System.Activities.WorkflowIdentity>, que fornece um mapeamento entre uma instância mantida de fluxo de trabalho e sua definição de fluxo de trabalho.

  - Execução lado a lado de várias versões de fluxo de trabalho no mesmo host, incluindo <xref:System.ServiceModel.Activities.WorkflowServiceHost>.

  - Na Atualização Dinâmica, a capacidade de modificar a definição de uma instância do fluxo de trabalho persistente.

- Implantação do serviço de fluxo de trabalho de primeiro contrato, que fornece suporte para gerar automaticamente atividades para corresponder um contrato de serviço existente.

Para saber mais, confira [Novidades no Windows Workflow Foundation](../windows-workflow-foundation/whats-new-in-wf-in-dotnet.md).

<a name="tailored"></a>

### <a name="net-for-windows-8x-store-apps"></a>.NET para aplicativos da Windows 8.x Store

Os aplicativos Windows 8.x Store foram projetados para fatores forma específicos e aproveitam a capacidade do sistema operacional Windows. Um subconjunto do .NET Framework 4.5 ou 4.5.1 está disponível para compilar aplicativos Windows 8.x Store para o Windows usando o C# ou o Visual Basic. Esse subconjunto é chamado .NET para aplicativos da loja do Windows 8. x e é discutido em uma [visão geral](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140)).

### <a name="portable-class-libraries"></a>Bibliotecas de Classe Portáteis <a name="portable"></a>

O projeto Biblioteca de Classes Portátil no Visual Studio 2012 (e em versões posteriores) permite gravar e compilar assemblies gerenciados que funcionem em várias plataformas do .NET Framework. Usando um projeto de biblioteca de classes portátil, você escolhe as plataformas (como Windows Phone e .NET para aplicativos da loja do Windows 8. x) para o destino. Os tipos e membros disponíveis em seu projeto são restritos automaticamente aos tipos e membros comuns através dessas plataformas. Para saber mais, veja [Biblioteca de Classes Portátil](../../standard/cross-platform/cross-platform-development-with-the-portable-class-library.md).

## <a name="see-also"></a>Confira também

- [O .NET Framework e lançamentos fora da banda](../get-started/the-net-framework-and-out-of-band-releases.md)
- [Novidades na acessibilidade do .NET Framework](whats-new-in-accessibility.md)
- [O que há de novo no Visual Studio 2017](/visualstudio/ide/whats-new-visual-studio-2017)
- [O que há de novo no Visual Studio 2019](/visualstudio/ide/whats-new-visual-studio-2019)
- [ASP.NET](/aspnet)
- [O que há de novo para C++ no Visual Studio](/cpp/what-s-new-for-visual-cpp-in-visual-studio)
