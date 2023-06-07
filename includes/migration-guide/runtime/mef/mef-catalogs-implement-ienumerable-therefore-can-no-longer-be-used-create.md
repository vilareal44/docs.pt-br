---
ms.openlocfilehash: 598df2121b480d411dac9c5571772a4a8d22b5ff
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619802"
---
### <a name="mef-catalogs-implement-ienumerable-and-therefore-can-no-longer-be-used-to-create-a-serializer"></a>Os catálogos do MEF implementam IEnumerable e, portanto, não podem mais ser usados para criar um serializador

#### <a name="details"></a>Detalhes

A partir do .NET Framework 4.5, os catálogos do MEF implementam IEnumerable e, portanto, não podem mais ser usados para criar um serializador (objeto <xref:System.Xml.Serialization.XmlSerializer?displayProperty=fullName>). Tentar serializar um catálogo de MEF gera uma exceção.

#### <a name="suggestion"></a>Sugestão

Não é mais possível usar o MEF para criar um serializador

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Principal|
|Versão|4.5|
|Type|Runtime|
