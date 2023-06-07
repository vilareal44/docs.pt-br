---
ms.openlocfilehash: 87013a04f7ff975e40a3c49c41c1c5acc2374066
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619875"
---
### <a name="wf-serializes-expressionsliterallttgt-datetimes-differently-now-breaks-custom-xaml-parsers"></a>O WF agora serializa Expressions.Literal&lt;T&gt; DateTimes de modo diferente (interrompe analisadores XAML personalizados)

#### <a name="details"></a>Detalhes

O objeto <xref:System.Windows.Markup.ValueSerializer> associado converterá um objeto <xref:System.DateTime?displayProperty=fullName> ou <xref:System.DateTimeOffset?displayProperty=fullName> cujos componentes Second e <xref:System.DateTime.Millisecond?displayProperty=fullName> são diferentes de zero e (para um valor de <xref:System.DateTime?displayProperty=fullName>) cuja propriedade <xref:System.DateTime.Kind> não é Unspecified para a sintaxe de elemento de propriedade em vez de uma cadeia de caracteres. Essa alteração permite que os valores <xref:System.DateTime?displayProperty=fullName> e <xref:System.DateTimeOffset?displayProperty=fullName> completem um ciclo. Os analisadores XAML personalizados que assumem que a entrada XAML está na sintaxe de atributo não funcionará corretamente.

#### <a name="suggestion"></a>Sugestão

Essa alteração permite que os valores <xref:System.DateTime?displayProperty=fullName> e <xref:System.DateTimeOffset?displayProperty=fullName> completem um ciclo. Os analisadores XAML personalizados que assumem que a entrada XAML está na sintaxe de atributo não funcionará corretamente.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5|
|Type|Runtime|
