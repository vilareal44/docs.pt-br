---
ms.openlocfilehash: afdf1e20db7dc564ddfb6028238604f97e00971a
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614295"
---
### <a name="serialization-of-control-characters-with-datacontractjsonserializer-is-now-compatible-with-ecmascript-v6-and-v8"></a>Serialização de caracteres de controle com DataContractJsonSerializer agora é compatível com ECMAScript V6 e V8

#### <a name="details"></a>Detalhes

No .NET Framework 4.6.2 e versões anteriores, o não <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer?displayProperty=fullName> serializava alguns caracteres de controle especiais, como \b, \f e \t, de forma que fosse compatível com os padrões ECMAScript V6 e V8. A partir do .NET Framework 4,7, a serialização desses caracteres de controle é compatível com ECMAScript V6 e V8.

#### <a name="suggestion"></a>Sugestão

Por padrão, esse novo layout é habilitado para aplicativos que se destinam ao .NET Framework 4.7. Se esse comportamento não for desejado, você poderá recusar esse recurso adicionando a seguinte linha na seção `<runtime>` do arquivo app.config ou web.config:

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.Runtime.Serialization.DoNotUseECMAScriptV6EscapeControlCharacter=false" />
</runtime>
```

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.7         |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer.WriteObject(System.IO.Stream,System.Object)?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer.WriteObject(System.Xml.XmlDictionaryWriter,System.Object)?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer.WriteObject(System.Xml.XmlWriter,System.Object)?displayProperty=nameWithType>
