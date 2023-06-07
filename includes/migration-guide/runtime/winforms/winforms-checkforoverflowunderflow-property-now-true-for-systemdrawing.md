---
ms.openlocfilehash: 4cd06fd02fadbaa9f74e40f850e688ee883454ed
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619887"
---
### <a name="winforms-checkforoverflowunderflow-property-is-now-true-for-systemdrawing"></a>A propriedade CheckForOverflowUnderflow do WinForm agora é true para System.Drawing

#### <a name="details"></a>Detalhes

A propriedade CheckForOverflowUnderflow do o assembly System.Drawing.dll está definida como true.

#### <a name="suggestion"></a>Sugestão

Anteriormente, quando os estouros ocorriam, o resultado teria sido truncado silenciosamente. Agora, uma exceção <xref:System.OverflowException?displayProperty=fullName> é gerada.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5|
|Type|Runtime|
