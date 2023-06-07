---
title: Como exibir a hora com o controle DateTimePicker
description: Saiba como usar o controle DateTimePicker Windows Forms para permitir que os usuários selecionem uma data e hora e exibam essa data e hora no formato especificado.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- time [Windows Forms], displaying in DateTimePicker control
- examples [Windows Forms], DateTimePicker control
- DateTimePicker control [Windows Forms], displaying time
ms.assetid: 0c1c8b40-1b50-4301-a90c-39516775ccb1
ms.openlocfilehash: ab584367a189d05e567bb57d386c6bf629201102
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85325579"
---
# <a name="how-to-display-time-with-the-datetimepicker-control"></a>Como exibir a hora com o controle DateTimePicker
Se você quiser que o aplicativo permita que os usuários selecionem uma data e hora e exibam essa data e hora no formato especificado, use o <xref:System.Windows.Forms.DateTimePicker> controle. O procedimento a seguir mostra como usar o <xref:System.Windows.Forms.DateTimePicker> controle para exibir a hora.  
  
### <a name="to-display-the-time-with-the-datetimepicker-control"></a>Para exibir a hora com o controle DateTimePicker  
  
1. Defina a propriedade <xref:System.Windows.Forms.DateTimePicker.Format%2A> como <xref:System.Windows.Forms.DateTimePickerFormat.Time>  
  
     [!code-csharp[System.Windows.Forms.DateTimePickerTimeOnly#2](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DateTimePickerTimeOnly/CS/Form1.cs#2)]
     [!code-vb[System.Windows.Forms.DateTimePickerTimeOnly#2](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DateTimePickerTimeOnly/VB/Form1.vb#2)]  
  
2. Defina a <xref:System.Windows.Forms.DateTimePicker.ShowUpDown%2A> propriedade para o <xref:System.Windows.Forms.DateTimePicker> como `true` .  
  
     [!code-csharp[System.Windows.Forms.DateTimePickerTimeOnly#3](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DateTimePickerTimeOnly/CS/Form1.cs#3)]
     [!code-vb[System.Windows.Forms.DateTimePickerTimeOnly#3](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DateTimePickerTimeOnly/VB/Form1.vb#3)]  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir mostra como criar um <xref:System.Windows.Forms.DateTimePicker> que permite que os usuários escolham apenas um tempo.  
  
 [!code-csharp[System.Windows.Forms.DateTimePickerTimeOnly#1](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DateTimePickerTimeOnly/CS/Form1.cs#1)]
 [!code-vb[System.Windows.Forms.DateTimePickerTimeOnly#1](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DateTimePickerTimeOnly/VB/Form1.vb#1)]  
  
## <a name="compiling-the-code"></a>Compilando o código  
 Este exemplo requer:  
  
- Referências aos assemblies System, System.Data, System.Drawing e System.Windows.Forms.  
  
## <a name="see-also"></a>Veja também

- [Controle DateTimePicker](datetimepicker-control-windows-forms.md)
