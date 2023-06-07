---
ms.openlocfilehash: 7cb146d19486618a4cee9976abe2220ea4b72790
ms.sourcegitcommit: d337df55f83325918cbbd095eb573400bea49064
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88204031"
---
### <a name="binaryformatter-serialization-methods-are-obsolete-and-prohibited-in-aspnet-apps"></a>Os métodos de serialização BinaryFormatter são obsoletos e proibidos em aplicativos ASP.NET

`Serialize` e `Deserialize` métodos em <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> , <xref:System.Runtime.Serialization.Formatter> e <xref:System.Runtime.Serialization.IFormatter> agora são obsoletos. Além disso, <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> a serialização é proibida por padrão para aplicativos ASP.net.

#### <a name="change-description"></a>Descrição da alteração

Devido a [vulnerabilidades de segurança](../../../../docs/standard/serialization/binaryformatter-security-guide.md#binaryformatter-security-vulnerabilities) no <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> , os métodos a seguir agora são obsoletos. Além disso, no ASP.NET Core 5,0 e em aplicativos posteriores, eles lançarão um <xref:System.NotSupportedException> , a menos que o aplicativo Web tenha reativado a <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> funcionalidade.

- <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Serialize%2A?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize%2A?displayProperty=nameWithType>

Esses métodos são marcados como obsoletos como parte de um esforço para o uso do vento <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> no ecossistema do .net.

Os métodos de serialização a seguir também são obsoletos, mas não têm nenhuma alteração comportamental:

- <xref:System.Runtime.Serialization.Formatter.Serialize(System.IO.Stream,System.Object)?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.Formatter.Deserialize(System.IO.Stream)?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.IFormatter.Serialize(System.IO.Stream,System.Object)?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.IFormatter.Deserialize(System.IO.Stream)?displayProperty=nameWithType>

#### <a name="version-introduced"></a>Versão introduzida

5,0 Preview 8

#### <a name="recommended-action"></a>Ação recomendada

- Pare <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> de usar no seu código. Em vez disso, considere usar <xref:System.Text.Json.JsonSerializer> ou <xref:System.Xml.Serialization.XmlSerializer> . Para obter mais informações, consulte [Guia de segurança do BinaryFormatter](../../../../docs/standard/serialization/binaryformatter-security-guide.md).

- Você pode suprimir temporariamente o <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> aviso de tempo de compilação, que é `SYSLIB0011` . Recomendamos que você avalie minuciosamente seu código em busca de riscos antes de escolher essa opção. A maneira mais fácil de suprimir os avisos é envolver o site de chamada individual com `#pragma` diretivas.

  ```csharp
  // Now read the purchase order back from disk
  using (var readStream = new FileStream("myfile.bin", FileMode.Open))
  {
      var formatter = new BinaryFormatter();
  #pragma warning disable SYSLIB0011
      return (PurchaseOrder)formatter.Deserialize(readStream);
  #pragma warning restore SYSLIB0011
  }
  ```

  Você também pode suprimir o aviso no arquivo de projeto.

  ```xml
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net5.0</TargetFramework>
    <!-- Disable "BinaryFormatter is obsolete" warnings for entire project -->
    <NoWarn>$(NoWarn);SYSLIB0011</NoWarn>
  </PropertyGroup>
  ```

  Se você suprimir o aviso no arquivo de projeto, o aviso será suprimido para todos os arquivos de código no projeto. Suprimir SYSLIB0011 não suprime avisos causados por outras APIs obsoletas.

- Para continuar usando <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> o em aplicativos ASP.net, você pode reabilitá-lo no arquivo de projeto. No entanto, é altamente recomendável não fazer isso. Para obter mais informações, consulte [Guia de segurança do BinaryFormatter](../../../../docs/standard/serialization/binaryformatter-security-guide.md).

  ```xml
  <PropertyGroup>
    <TargetFramework>net5.0</TargetFramework>
    <!-- Warning: Setting the following switch is *NOT* recommended in web apps. -->
    <EnableUnsafeBinaryFormatterSerialization>true</EnableUnsafeBinaryFormatterSerialization>
  </PropertyGroup>
  ```

Para obter mais informações sobre as ações recomendadas, consulte [Resolvendo erros de BinaryFormatter obsoletion e de desativação](https://aka.ms/binaryformatter).

#### <a name="category"></a>Categoria

- Bibliotecas principais do .NET
- ASP.NET

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Serialize%2A?displayProperty=fullName>
- <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize%2A?displayProperty=fullName>
- <xref:System.Runtime.Serialization.Formatter.Serialize(System.IO.Stream,System.Object)?displayProperty=fullName>
- <xref:System.Runtime.Serialization.Formatter.Deserialize(System.IO.Stream)?displayProperty=fullName>
- <xref:System.Runtime.Serialization.IFormatter.Serialize(System.IO.Stream,System.Object)?displayProperty=fullName>
- <xref:System.Runtime.Serialization.IFormatter.Deserialize(System.IO.Stream)?displayProperty=fullName>

<!--

#### Affected APIs

- `Overload:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Serialize`
- `Overload:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize`
- `M:System.Runtime.Serialization.Formatter.Serialize(System.IO.Stream,System.Object)`
- `M:System.Runtime.Serialization.Formatter.Deserialize(System.IO.Stream)`
- `M:System.Runtime.Serialization.IFormatter.Serialize(System.IO.Stream,System.Object)`
- `M:System.Runtime.Serialization.IFormatter.Deserialize(System.IO.Stream)`

-->
