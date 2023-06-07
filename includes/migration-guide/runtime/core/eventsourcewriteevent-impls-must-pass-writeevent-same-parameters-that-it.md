---
ms.openlocfilehash: 662c140f019add66ff6605d47ad1f32c3f50d711
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619782"
---
### <a name="eventsourcewriteevent-impls-must-pass-writeevent-the-same-parameters-that-it-received-plus-id"></a>EventSource.WriteEvent deve passar a WriteEvent os mesmos parâmetros que recebeu (mais ID)

#### <a name="details"></a>Detalhes

O runtime agora impõe o contrato que especifica o seguinte: uma classe derivada de <xref:System.Diagnostics.Tracing.EventSource?displayProperty=fullName> que define um método de evento ETW deve chamar o método de classe base <code>EventSource.WriteEvent</code> com a ID do evento seguido dos mesmos argumentos que o método de evento ETW passou.

#### <a name="suggestion"></a>Sugestão

Uma exceção <xref:System.IndexOutOfRangeException?displayProperty=fullName> será acionada se um <xref:System.Diagnostics.Tracing.EventListener?displayProperty=fullName> ler dados <xref:System.Diagnostics.Tracing.EventSource?displayProperty=fullName> no processo de uma origem de evento que viola esse contrato.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.5.1|
|Type|Runtime|
