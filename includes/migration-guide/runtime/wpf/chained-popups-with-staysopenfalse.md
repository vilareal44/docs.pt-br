---
ms.openlocfilehash: 171b7a3a962f8259e64b88f1ae893e649b5f24bb
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621024"
---
### <a name="chained-popups-with-staysopenfalse"></a>Pop-ups encadeados com StaysOpen=False

#### <a name="details"></a>Detalhes

Um pop-up com StaysOpen=False deve fechar quando você clica fora dele. Quando dois ou mais pop-ups do tipo estão encadeados (por exemplo, quando um contém o outro), há muitos problemas, incluindo:<ul><li>Abra dois níveis, clique fora do P2, mas dentro do P1.  Nada acontecerá.</li><li>Abra dois níveis, clique fora do P1.  Ambos pop-ups serão fechados.</li><li>Abra e feche dois níveis.  Em seguida, tente abrir novamente o P2.  Nada acontecerá.</li><li>Tente abrir três níveis.  Você não pode.  (Nada acontece ou os dois primeiros níveis fecham, dependendo de onde você clica.) Esses casos (e outras variantes) agora funcionam conforme o esperado.</li></ul>

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.7.1|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Windows.Controls.Primitives.Popup.StaysOpen?displayProperty=nameWithType></li></ul>|
