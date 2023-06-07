---
ms.openlocfilehash: 9659068304eb208fd6a0a753273453bc669fbc56
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621027"
---
### <a name="keytips-behavior-improved-in-wpf"></a>Melhoria no comportamento das dicas de tecla no WPF

#### <a name="details"></a>Detalhes

O comportamento das dicas de tecla foi modificado para manter a paridade com o comportamento no Microsoft Word e no Windows Explorer. Ao verificar se o estado da dica de tecla está habilitado ou não no caso de uma <xref:System.Windows.Input.KeyEventArgs.SystemKey> (especificamente, <xref:System.Windows.Input.Key> ou <xref:System.Windows.Input.Key.F11>) que está sendo pressionada, o WPF trata corretamente as teclas de dica de tecla. As dicas de tecla agora descartam um menu, mesmo quando ele é aberto pelo mouse.

#### <a name="suggestion"></a>Sugestão

N/D

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.7.2|
|Type|Runtime|
