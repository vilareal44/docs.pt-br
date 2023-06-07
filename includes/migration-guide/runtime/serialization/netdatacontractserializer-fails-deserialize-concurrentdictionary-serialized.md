---
ms.openlocfilehash: eef5633ec8566f6d5216b7dca4387766cacb600d
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619811"
---
### <a name="netdatacontractserializer-fails-to-deserialize-a-concurrentdictionary-serialized-with-a-different-net-version"></a>NetDataContractSerializer falha ao desserializar um ConcurrentDictionary serializado com uma versão diferente do .NET

#### <a name="details"></a>Detalhes

Por design, o <xref:System.Runtime.Serialization.NetDataContractSerializer?displayProperty=fullName> poderá ser usado somente se as extremidades de serialização e desserialização compartilharem os mesmos tipos CLR. Portanto, não há garantia de que um objeto serializado com uma versão do .NET Framework poderá ser desserializado com uma versão diferente.<xref:System.Collections.Concurrent.ConcurrentDictionary%602?displayProperty=fullName> é um tipo que não será desserializado corretamente se for serializado com o .NET Framework 4.5 ou anterior e for desserializado com o .NET Framework 4.5.1 ou posterior.

#### <a name="suggestion"></a>Sugestão

Há uma série de possíveis soluções alternativas para esse problema:<ul><li>Atualizar o computador de serialização para usar o .NET Framework 4.5.1 também.</li><li>Usar <xref:System.Runtime.Serialization.DataContractSerializer?displayProperty=fullName> em vez de <xref:System.Runtime.Serialization.NetDataContractSerializer?displayProperty=fullName>, pois ele não espera os mesmos tipos CLR nas extremidades de serialização e desserialização.</li><li>Usar <xref:System.Collections.Generic.Dictionary%602?displayProperty=fullName> em vez de <xref:System.Collections.Concurrent.ConcurrentDictionary%602?displayProperty=fullName>, uma vez que ele não apresenta essa quebra específica entre 4.5 e &gt;4.5.1.</li></ul>

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.5.1|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Runtime.Serialization.NetDataContractSerializer.Deserialize(System.IO.Stream)?displayProperty=nameWithType></li></ul>|
