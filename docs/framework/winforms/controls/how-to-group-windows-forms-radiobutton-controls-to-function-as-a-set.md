---
title: Agrupar controles de botão de opção para funcionar como um conjunto
description: Saiba como agrupar Windows Forms controles RadioButton para funcionar independentemente de outros conjuntos.
ms.date: 03/30/2017
helpviewer_keywords:
- radio buttons [Windows Forms], grouping
- controls [Windows Forms], grouping
- Windows Forms controls, grouping
- RadioButton control [Windows Forms], grouping
ms.assetid: 58f8fe34-50b7-49d8-a2be-c271be3c6b32
ms.openlocfilehash: 9f6795330922e739cca1f2b5c11bb2ec68bb4e84
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85325917"
---
# <a name="how-to-group-windows-forms-radiobutton-controls-to-function-as-a-set"></a>Como agrupar controles RadioButton dos Windows Forms para funcionarem como um conjunto
<xref:System.Windows.Forms.RadioButton>Os controles de Windows Forms são projetados para fornecer aos usuários uma opção entre duas ou mais configurações, das quais apenas uma pode ser atribuída a um procedimento ou objeto. Por exemplo, um grupo de <xref:System.Windows.Forms.RadioButton> controles pode exibir uma opção de operadoras de pacote para um pedido, mas apenas uma das operadoras será usada. Portanto, somente um de <xref:System.Windows.Forms.RadioButton> cada vez pode ser selecionado, mesmo que ele faça parte de um grupo funcional.  
  
 Agrupe os botões de opção desenhando-os dentro de um contêiner, como um <xref:System.Windows.Forms.Panel> controle, um <xref:System.Windows.Forms.GroupBox> controle ou um formulário. Todos os botões de opção que forem adicionados diretamente a um formulário se tornam um grupo. Para adicionar grupos separados, você deve inseri-los em painéis ou caixas de grupo. Para obter mais informações sobre painéis ou caixas de grupo, consulte [Visão geral do controle Panel](panel-control-overview-windows-forms.md) ou [Visão geral do controle GroupBox](groupbox-control-overview-windows-forms.md).  
  
### <a name="to-group-radiobutton-controls-as-a-set-to-function-independently-of-other-sets"></a>Para agrupar controles RadioButton como um conjunto para funcionar independentemente dos outros conjuntos  
  
1. Arraste um <xref:System.Windows.Forms.GroupBox> <xref:System.Windows.Forms.Panel> controle ou da guia **Windows Forms** na caixa de **ferramentas** para o formulário.  
  
2. Desenhe <xref:System.Windows.Forms.RadioButton> controles no <xref:System.Windows.Forms.GroupBox> controle ou <xref:System.Windows.Forms.Panel> .  
  
## <a name="see-also"></a>Veja também

- <xref:System.Windows.Forms.RadioButton>
- [Visão geral do controle RadioButton](radiobutton-control-overview-windows-forms.md)
- [Visão geral do controle Panel](panel-control-overview-windows-forms.md)
- [Visão geral do controle GroupBox](groupbox-control-overview-windows-forms.md)
- [Visão geral do controle CheckBox](checkbox-control-overview-windows-forms.md)
- [Controle RadioButton](radiobutton-control-windows-forms.md)
