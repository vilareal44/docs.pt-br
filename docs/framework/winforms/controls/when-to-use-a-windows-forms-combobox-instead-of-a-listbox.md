---
title: ComboBox vs. ListBox
description: Saiba como usar Windows Forms caixa de combinação e Windows Forms caixa de listagem e saiba como saber quando um ou outro é mais apropriado para uma tarefa.
ms.date: 03/30/2017
helpviewer_keywords:
- ListBox control [Windows Forms], adding and removing items
- ListBox control [Windows Forms], vs. ComboBox
- bound controls [Windows Forms], combo boxes
- Windows Forms controls, data binding
- ComboBox control [Windows Forms], compared to ListBox
- combo boxes [Windows Forms], compared to list boxes
- ListBox control [Windows Forms], accessing items
- ListCount property
ms.assetid: 7bcaea58-1cfa-46db-9baf-b75a69d8f9ec
ms.openlocfilehash: ca6ad6bec2dbc30128ea09808af2806687b17a8c
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174413"
---
# <a name="when-to-use-a-windows-forms-combobox-instead-of-a-listbox"></a>Quando usar um ComboBox dos Windows Forms em vez de um ListBox
O <xref:System.Windows.Forms.ComboBox> e os <xref:System.Windows.Forms.ListBox> controles têm comportamentos semelhantes e, em alguns casos, podem ser intercambiáveis. No entanto, há vezes, em que um ou outro é mais apropriado para uma tarefa.  
  
 Em geral, uma caixa de combinação é apropriada quando há uma lista das opções sugeridas e uma caixa de listagem é apropriada quando você deseja limitar a entrada para o que está na lista. Uma caixa de combinação contém um campo de caixa de texto, portanto, as opções que não estão na lista podem ser digitadas. A exceção é quando a <xref:System.Windows.Forms.ComboBox.DropDownStyle%2A> propriedade é definida como <xref:System.Windows.Forms.ComboBoxStyle.DropDownList> . Nesse caso, o controle selecionará um item se você digitar sua primeira letra.  
  
 Além disso, caixas de combinação economizam espaço em um formulário. Como a lista completa não é exibida até que o usuário clique na seta para baixo, uma caixa de combinação pode se encaixar facilmente em um pequeno espaço em que uma caixa de listagem não caberia. Uma exceção é quando a <xref:System.Windows.Forms.ComboBox.DropDownStyle%2A> propriedade é definida como <xref:System.Windows.Forms.ComboBoxStyle.Simple> : a lista completa é exibida e a caixa de combinação ocupa mais espaço do que uma caixa de listagem.  
  
## <a name="see-also"></a>Veja também

- <xref:System.Windows.Forms.ComboBox>
- <xref:System.Windows.Forms.ListBox>
- [Como adicionar e remover itens de um controle ComboBox, ListBox ou CheckedListBox dos Windows Forms](add-and-remove-items-from-a-wf-combobox.md)
- [Como classificar o conteúdo de um controle ComboBox, ListBox ou CheckedListBox dos Windows Forms](sort-the-contents-of-a-wf-combobox-listbox-or-checkedlistbox-control.md)
- [Controles dos Windows Forms usados para listar opções](windows-forms-controls-used-to-list-options.md)
