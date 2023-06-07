---
ms.openlocfilehash: 2e9267b35c9389da017927aca2346190348265c9
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87919377"
---
### <a name="binaryformatterdeserialize-rewraps-some-exceptions-in-serializationexception"></a>BinaryFormatter. desserializate reencapsula algumas exceções na Serializaexception

O <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize%2A?displayProperty=nameWithType> método agora reencapsula alguns objetos de exceção dentro de um <xref:System.Runtime.Serialization.SerializationException> antes de propagar a exceção de volta para o chamador.

#### <a name="change-description"></a>Descrição da alteração

Anteriormente, o <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize%2A?displayProperty=nameWithType> método permitia algumas exceções arbitrárias, como <xref:System.ArgumentNullException> , para propagar a pilha para seus chamadores.

No .NET 5,0 e posterior, o <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize%2A?displayProperty=nameWithType> método captura de forma mais agressiva as exceções que ocorrem devido a operações de desserialização inválidas e as encapsula em um <xref:System.Runtime.Serialization.SerializationException> .

#### <a name="version-introduced"></a>Versão introduzida

5,0 visualização 1

#### <a name="recommended-action"></a>Ação recomendada

Na maioria dos casos, você não precisa realizar nenhuma ação. No entanto, se seu site de chamada depende de uma determinada exceção sendo gerada, você pode desencapsular a exceção da externa <xref:System.Runtime.Serialization.SerializationException> , conforme mostrado no exemplo a seguir.

```csharp
Stream inputStream = GetInputStream();
var formatter = new BinaryFormatter();

try
{
    object deserialized = formatter.Deserialize(inputStream);
}
catch (MyException myEx)
{
    // Handle 'myEx' here in case it was thrown directly.
}
catch (SerializationException serEx) when (serEx.InnerException is MyException myEx)
{
    // Handle 'myEx' here in case it was wrapped in SerializationException.
}
```

#### <a name="category"></a>Categoria

Serialização

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize%2A?displayProperty=fullName>

<!--

#### Affected APIs

- `Overload:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize`

-->
