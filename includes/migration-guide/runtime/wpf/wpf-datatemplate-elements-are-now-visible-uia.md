---
ms.openlocfilehash: 06c699281c8890ac65be1d282b72b54774acc280
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619896"
---
### <a name="wpf-datatemplate-elements-are-now-visible-to-uia"></a>Elementos DataTemplate do WPF agora são visíveis à UIA

#### <a name="details"></a>Detalhes

Anteriormente, os elementos <xref:System.Windows.DataTemplate?displayProperty=fullName> eram invisíveis à Automação da Interface do Usuário. A partir da versão 4.5, a Automação da Interface do Usuário detectar esses elementos. Isso é útil em muitos casos, mas pode arruinar os testes que dependem das árvores UIA que não contêm elementos <xref:System.Windows.DataTemplate?displayProperty=fullName>.

#### <a name="suggestion"></a>Sugestão

Os testes de Automação da Interface do Usuário para esse aplicativo talvez precisem ser atualizados para contabilizar a árvore UIA agora, incluindo elementos <xref:System.Windows.DataTemplate?displayProperty=fullName> anteriormente invisíveis. Por exemplo, os testes que esperam que alguns elementos fiquem próximos uns dos outros agora podem precisar esperar elementos UIA anteriormente invisíveis entre eles. Ou os testes que dependem de determinadas contagens ou de índices para elementos UIA talvez precisem ser atualizados com novos valores.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Windows.DataTemplate.%23ctor></li><li><xref:System.Windows.DataTemplate.%23ctor(System.Object)></li></ul>|
