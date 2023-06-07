---
ms.openlocfilehash: ae0f68a19d6eae53998d61e924cfef3aaaec1784
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619780"
---
### <a name="a-concurrentdictionary-serialized-in-net-framework-45-with-netdatacontractserializer-cannot-be-deserialized-by-net-framework-451-or-452"></a>Um ConcurrentDictionary serializado no .NET Framework 4.5 com NetDataContractSerializer não pode ser desserializado pelo .NET Framework 4.5.1 ou 4.5.2

#### <a name="details"></a>Detalhes

Devido a alterações internas no tipo, objetos <xref:System.Collections.Concurrent.ConcurrentDictionary%602> serializados com o .NET Framework 4.5 usando o <xref:System.Runtime.Serialization.NetDataContractSerializer?displayProperty=fullName> não podem ser desserializados no .NET Framework 4.5.1 ou no .NET Framework 4.5.2. Observe que mover-se no outro sentido (serializar com o .NET Framework 4.5.x e desserializar com o .NET Framework 4.5) funciona. Da mesma forma, todas as serializações entre versões 4. x funcionam com o .NET Framework 4.6. A possibilidade de serializar e desserializar com uma única versão do .NET Framework não é afetada.

#### <a name="suggestion"></a>Sugestão

Se for necessário serializar e desserializar um <xref:System.Collections.Concurrent.ConcurrentDictionary%602?displayProperty=fullName> entre o .NET Framework 4.5 e o .NET Framework 4.5.1/4.5.2, um serializador alternativo como o serializador <xref:System.Runtime.Serialization.DataContractSerializer?displayProperty=fullName> ou <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter?displayProperty=fullName> deve ser usado em vez do <xref:System.Runtime.Serialization.NetDataContractSerializer?displayProperty=fullName>. Como alternativa, uma vez que esse problema é resolvido no .NET Framework 4.6, isso pode ser solucionado com a atualização para essa versão do .NET Framework.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.5.1|
|Type|Runtime|
