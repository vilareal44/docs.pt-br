---
ms.openlocfilehash: 2aa6603e2ed77ffa94fbc6325cd5db50985bda6a
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619876"
---
### <a name="previewlostkeyboardfocus-is-called-repeatedly-if-its-handler-shows-a-windows-forms-message-box"></a>PreviewLostKeyboardFocus será chamado repetidamente se seu manipulador mostrar uma caixa de mensagem do Windows Forms

#### <a name="details"></a>Detalhes

A partir do .NET Framework 4.5, chamar <xref:System.Windows.Forms.MessageBox.Show%2A?displayProperty=nameWithType> de um manipulador <xref:System.Windows.UIElement.PreviewLostKeyboardFocus> fará com que o manipulador seja acionado novamente quando a caixa de mensagem for fechada, possivelmente resultando em um loop infinito de caixas de mensagem.

#### <a name="suggestion"></a>Sugestão

Há duas opções para contornar esse problema:<ol><li>Ele pode ser evitado chamando <xref:System.Windows.MessageBox.Show%2A?displayProperty=nameWithType> em vez de <xref:System.Windows.Forms.MessageBox.Show%2A?displayProperty=nameWithType>.</li><li>Ele pode ser evitado mostrando a caixa de mensagem de um manipulador de eventos <xref:System.Windows.UIElement.LostKeyboardFocus> (em oposição a um manipulador de eventos <xref:System.Windows.UIElement.PreviewLostKeyboardFocus?displayProperty=fullName>).</li></ol>

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Windows.ContentElement.PreviewLostKeyboardFocus?displayProperty=nameWithType></li><li><xref:System.Windows.IInputElement.PreviewLostKeyboardFocus?displayProperty=nameWithType></li><li><xref:System.Windows.UIElement.PreviewLostKeyboardFocus?displayProperty=nameWithType></li><li><xref:System.Windows.UIElement3D.PreviewLostKeyboardFocus?displayProperty=nameWithType></li></ul>|
