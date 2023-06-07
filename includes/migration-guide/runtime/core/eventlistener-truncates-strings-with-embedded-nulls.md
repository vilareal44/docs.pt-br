---
ms.openlocfilehash: 6f5c1ecead4e2f74e621354058aab70bfa1cccb6
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619779"
---
### <a name="eventlistener-truncates-strings-with-embedded-nulls"></a>EventListener trunca cadeias de caracteres com nulos inseridos

#### <a name="details"></a>Detalhes

<xref:System.Diagnostics.Tracing.EventListener?displayProperty=fullName> trunca cadeias de caracteres com nulos inseridos. Os caracteres nulos não são compatíveis com a classe <xref:System.Diagnostics.Tracing.EventSource?displayProperty=fullName>. A alteração afeta apenas os aplicativos que usam <xref:System.Diagnostics.Tracing.EventListener?displayProperty=fullName> para ler dados <xref:System.Diagnostics.Tracing.EventSource?displayProperty=fullName> no processo e que usam caracteres nulos como delimitadores.

#### <a name="suggestion"></a>Sugestão

Os dados <xref:System.Diagnostics.Tracing.EventSource?displayProperty=fullName> devem ser atualizados, se possível, para não usar caracteres nulos inseridos.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5.1|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Diagnostics.Tracing.EventListener.%23ctor></li><li><xref:System.Diagnostics.Tracing.EventListener.EnableEvents(System.Diagnostics.Tracing.EventSource,System.Diagnostics.Tracing.EventLevel)?displayProperty=nameWithType></li><li><xref:System.Diagnostics.Tracing.EventListener.EnableEvents(System.Diagnostics.Tracing.EventSource,System.Diagnostics.Tracing.EventLevel,System.Diagnostics.Tracing.EventKeywords)?displayProperty=nameWithType></li><li><xref:System.Diagnostics.Tracing.EventListener.EnableEvents(System.Diagnostics.Tracing.EventSource,System.Diagnostics.Tracing.EventLevel,System.Diagnostics.Tracing.EventKeywords,System.Collections.Generic.IDictionary{System.String,System.String})?displayProperty=nameWithType></li></ul>|
