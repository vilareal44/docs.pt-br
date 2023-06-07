---
ms.openlocfilehash: 5a3370e71488e4f9d8d933b504d1d771c78e0385
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619810"
---
### <a name="soapformatter-cannot-deserialize-hashtable-and-similar-ordered-collection-objects"></a>SoapFormatter não pode desserializar Hashtable e objetos de coleção ordenados semelhantes

#### <a name="details"></a>Detalhes

O <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter?displayProperty=fullName> não garante que objetos serializados em uma versão do .NET Framework serão desserializados com êxito em uma versão diferente. Especificamente, algumas coleções ordenadas (como <xref:System.Collections.Hashtable?displayProperty=fullName>) tiveram membros adicionados entre a 4.0 e a 4.5, portanto, os objetos desses tipos não poderão ser desserializados com o .NET Framework 4.0 se tiverem sido serializados com o .NET Framework 4.5. Observe que se os dados serializados forem serializados e desserializados com a mesma versão do .NET Framework, nenhum problema ocorrerá.

#### <a name="suggestion"></a>Sugestão

A serialização <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter?displayProperty=fullName> deve ser substituída pela serialização <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter?displayProperty=fullName> ou <xref:System.Runtime.Serialization.NetDataContractSerializer?displayProperty=fullName> para resistir às alterações do .NET Framework.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter.Serialize(System.IO.Stream,System.Object)?displayProperty=nameWithType></li><li><xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter.Serialize(System.IO.Stream,System.Object,System.Runtime.Remoting.Messaging.Header[])?displayProperty=nameWithType></li><li><xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter.Deserialize(System.IO.Stream)?displayProperty=nameWithType></li><li><xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter.Deserialize(System.IO.Stream,System.Runtime.Remoting.Messaging.HeaderHandler)?displayProperty=nameWithType></li></ul>|
