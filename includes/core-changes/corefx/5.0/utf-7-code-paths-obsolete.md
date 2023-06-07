---
ms.openlocfilehash: 1cb6e953aa57c72ee6a2665c521bada10e0786cf
ms.sourcegitcommit: 7499bdb428d63ed0e19e97f54d3d576c41598659
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87455779"
---
### <a name="utf-7-code-paths-are-obsolete"></a>Os caminhos de código UTF-7 estão obsoletos

A codificação UTF-7 não está mais em uso amplo entre aplicativos, e muitas especificações agora [proíbem seu uso](https://security.stackexchange.com/a/68609/3573) no intercâmbio. Ele também é [usado ocasionalmente como um vetor de ataque](https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=utf-7) em aplicativos que não antecipam a ocorrência de dados codificados em UTF-7. A Microsoft avisa sobre o uso do <xref:System.Text.UTF7Encoding?displayProperty=nameWithType> porque não fornece detecção de erros.

Consequentemente, a <xref:System.Text.Encoding.UTF7?displayProperty=nameWithType> propriedade e os <xref:System.Text.UTF7Encoding.%23ctor%2A> construtores agora são obsoletos. Além disso, <xref:System.Text.Encoding.GetEncoding%2A?displayProperty=nameWithType> e <xref:System.Text.Encoding.GetEncodings%2A?displayProperty=nameWithType> não permite mais que você especifique `UTF-7` .

#### <a name="change-description"></a>Descrição da alteração

Anteriormente, você poderia criar uma instância da codificação UTF-7 usando as <xref:System.Text.Encoding.GetEncoding%2A?displayProperty=nameWithType> APIs. Por exemplo:

```csharp
Encoding enc1 = Encoding.GetEncoding("utf-7"); // By name.
Encoding enc2 = Encoding.GetEncoding(65000); // By code page.
```

Além disso, uma instância que representa a codificação UTF-7 foi enumerada pelo <xref:System.Text.Encoding.GetEncodings?displayProperty=nameWithType> método, que enumera todas as <xref:System.Text.Encoding> instâncias registradas no sistema.

A partir do .NET 5,0, a <xref:System.Text.Encoding.UTF7?displayProperty=nameWithType> propriedade e os <xref:System.Text.UTF7Encoding.%23ctor%2A> construtores são obsoletos e produzem um aviso `SYSLIB0001` (ou `MSLIB0001` em versões de visualização). No entanto, para reduzir o número de avisos que os chamadores recebem ao usar a <xref:System.Text.UTF7Encoding> classe, o <xref:System.Text.UTF7Encoding> tipo em si não é marcado como obsoleto.

```csharp
// The next line generates warning SYSLIB0001.
UTF7Encoding enc = new UTF7Encoding();
// The next line does not generate a warning.
byte[] bytes = enc.GetBytes("Hello world!");
```

Além disso, os <xref:System.Text.Encoding.GetEncoding%2A?displayProperty=nameWithType> métodos tratam o nome de codificação `utf-7` e a página de código `65000` como `unknown` . Tratar a codificação como `unknown` faz com que o método gere um <xref:System.ArgumentException> .

```csharp
// Throws ArgumentException, same as calling Encoding.GetEncoding("unknown").
Encoding enc = Encoding.GetEncoding("utf-7");
```

Por fim, o <xref:System.Text.Encoding.GetEncodings?displayProperty=nameWithType> método não inclui a codificação UTF-7 na <xref:System.Text.EncodingInfo> matriz que ela retorna. A codificação é excluída porque não pode ser instanciada.

```csharp
foreach (EncodingInfo encInfo in Encoding.GetEncodings())
{
    // The next line would throw if GetEncodings included UTF-7.
    Encoding enc = Encoding.GetEncoding(encInfo.Name);
}
```

#### <a name="reason-for-change"></a>Motivo da alteração

Muitos aplicativos chamam `Encoding.GetEncoding("encoding-name")` um valor de nome de codificação fornecido por uma fonte não confiável. Por exemplo, um cliente ou servidor Web pode pegar a `charset` parte do `Content-Type` cabeçalho e passar o valor diretamente para `Encoding.GetEncoding` sem validá-lo. Isso pode permitir que um ponto de extremidade mal-intencionado seja especificado `Content-Type: ...; charset=utf-7` , o que poderia causar um comportamento incorreto do aplicativo de recebimento.

Além disso, a desabilitação de caminhos de código UTF-7 permite otimizar compiladores, como aqueles usados pelo mais alto, para remover totalmente esses caminhos de código do aplicativo resultante. Como resultado, os aplicativos compilados são executados com mais eficiência e ocupam menos espaço em disco.

#### <a name="version-introduced"></a>Versão introduzida

5,0 Preview 8

#### <a name="recommended-action"></a>Ação recomendada

Na maioria dos casos, você não precisa realizar nenhuma ação. No entanto, para aplicativos que já ativaram caminhos de código relacionados ao UTF-7, considere as diretrizes a seguir.

- Se seu aplicativo chama <xref:System.Text.Encoding.GetEncoding%2A?displayProperty=nameWithType> nomes de codificação desconhecidos fornecidos por uma fonte não confiável:

  Em vez disso, compare os nomes de codificação com uma lista de permissão configurável. A lista de permissões configuráveis deve, no mínimo, incluir o "UTF-8" padrão do setor. Dependendo de seus clientes e requisitos regulatórios, você também pode precisar permitir codificações específicas de região, como "GB18030".

  Se você não implementar uma lista de permissões, o <xref:System.Text.Encoding.GetEncoding%2A?displayProperty=nameWithType> retornará qualquer um <xref:System.Text.Encoding> que esteja embutido no sistema ou que seja registrado por meio de um personalizado <xref:System.Text.EncodingProvider> . Auditore os requisitos do serviço para validar que esse é o comportamento desejado. O UTF-7 continua desabilitado por padrão, a menos que o aplicativo habilite novamente a opção de compatibilidade mencionada posteriormente neste artigo.

- Se você estiver usando <xref:System.Text.Encoding.UTF7?displayProperty=nameWithType> ou <xref:System.Text.UTF7Encoding> dentro de seu próprio protocolo ou formato de arquivo:

  Alterne para o usando o <xref:System.Text.Encoding.UTF8?displayProperty=nameWithType> ou o <xref:System.Text.UTF8Encoding> . O UTF-8 é um padrão da indústria e tem amplo suporte em linguagens, sistemas operacionais e tempos de execução. O uso de UTF-8 facilita a manutenção futura de seu código e o torna mais interoperável com o restante do ecossistema.

- Se você estiver comparando uma <xref:System.Text.Encoding> instância do <xref:System.Text.Encoding.UTF7?displayProperty=nameWithType> :

  Em vez disso, considere executar uma verificação na página de código UTF-7 conhecida, que é `65000` . Ao comparar com a página de código, você evita o aviso e também lida com alguns casos de borda, como se alguém chamou `new UTF7Encoding()` ou subclasse o tipo.

  ```csharp
  void DoSomething(Encoding enc)
  {
      // Don't perform the check this way.
      // It produces a warning and misses some edge cases.
      if (enc == Encoding.UTF7)
      {
          // Encoding is UTF-7.
      }

      // Instead, perform the check this way.
      if (enc != null && enc.CodePage == 65000)
      {
          // Encoding is UTF-7.
      }
  }
  ```

- Se você precisar usar <xref:System.Text.Encoding.UTF7?displayProperty=nameWithType> ou <xref:System.Text.UTF7Encoding> :

  Você pode suprimir o `SYSLIB0001` aviso no código ou no arquivo *. csproj* do seu projeto.

  ```csharp
  #pragma warning disable SYSLIB0001 // Disable the warning.
  Encoding enc = Encoding.UTF7;
  #pragma warning restore SYSLIB0001 // Re-enable the warning.
  ```

  ```xml
  <Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
     <TargetFramework>net5.0</TargetFramework>
     <!-- NoWarn below suppresses SYSLIB0001 project-wide -->
     <NoWarn>$(NoWarn);SYSLIB0001</NoWarn>
    </PropertyGroup>
  </Project>
  ```

  > [!NOTE]
  > Suprimir `SYSLIB0001` apenas desabilita os <xref:System.Text.Encoding.UTF7?displayProperty=nameWithType> <xref:System.Text.UTF7Encoding> avisos e obsoletion. Ele não desabilita nenhum outro aviso nem altera o comportamento de APIs como <xref:System.Text.Encoding.GetEncoding%2A?displayProperty=nameWithType> .

- Se você precisar dar suporte a `Encoding.GetEncoding("utf-7", ...)` :

  Você pode reabilitar o suporte para isso por meio de uma opção de compatibilidade. Essa opção de compatibilidade pode ser especificada no arquivo *. csproj* do aplicativo ou em um [arquivo de configuração de tempo de execução](../../../../docs/core/run-time-config/index.md), conforme mostrado nos exemplos a seguir.

  No arquivo *. csproj* do aplicativo:

  ```xml
  <Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
     <TargetFramework>net5.0</TargetFramework>
     <!-- Re-enable support for UTF-7 -->
     <EnableUnsafeUTF7Encoding>true</EnableUnsafeUTF7Encoding>
    </PropertyGroup>
  </Project>
  ```

  Naruntimeconfig.template.jsdo aplicativo *no* arquivo:

  ```json
  {
    "configProperties": {
      "System.Text.Encoding.EnableUnsafeUTF7Encoding": true
    }
  }
  ```

  > [!TIP]
  > Se você reabilitar o suporte para UTF-7, deverá executar uma revisão de segurança do código que chama <xref:System.Text.Encoding.GetEncoding%2A?displayProperty=nameWithType> .

#### <a name="category"></a>Categoria

- Bibliotecas principais do .NET
- Segurança

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Text.Encoding.UTF7?displayProperty=fullName>
- <xref:System.Text.UTF7Encoding.%23ctor>
- <xref:System.Text.UTF7Encoding.%23ctor(System.Boolean)>
- <xref:System.Text.Encoding.GetEncoding(System.Int32)?displayProperty=fullName>
- <xref:System.Text.Encoding.GetEncoding(System.String)?displayProperty=fullName>
- <xref:System.Text.Encoding.GetEncoding(System.Int32,System.Text.EncoderFallback,System.Text.DecoderFallback)?displayProperty=fullName>
- <xref:System.Text.Encoding.GetEncoding(System.String,System.Text.EncoderFallback,System.Text.DecoderFallback)?displayProperty=fullName>
- <xref:System.Text.Encoding.GetEncodings?displayProperty=fullName>

<!--

#### Affected APIs

- `System.Text.Encoding.UTF7`
- `System.Text.UTF7Encoding.#ctor`
- `System.Text.UTF7Encoding.#ctor(System.Boolean)`
- `System.Text.Encoding.GetEncoding(System.Int32)`
- `System.Text.Encoding.GetEncoding(System.String)`
- `System.Text.Encoding.GetEncoding(System.Int32,System.Text.EncoderFallback,System.Text.DecoderFallback)`
- `System.Text.Encoding.GetEncoding(System.String,System.Text.EncoderFallback,System.Text.DecoderFallback)`
- `System.Text.Encoding.GetEncodings`

-->
