---
title: 'Como: manipular a entrada do teclado no nível do formulário'
description: Saiba como lidar com a entrada de teclado para sua Windows Forms no nível de formulário, antes que as mensagens cheguem a um controle.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- keyboard input [Windows Forms], at form level
- Windows Forms, handling keyboard input
- keyboards [Windows Forms], form-level input
ms.assetid: d7f8b390-dc91-42d2-ae0f-2ffa388127ad
ms.openlocfilehash: 6f0695b6f665a613e0e4e001a4f9bbfc09dd45ed
ms.sourcegitcommit: 3824ff187947572b274b9715b60c11269335c181
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84904150"
---
# <a name="how-to-handle-keyboard-input-at-the-form-level"></a>Como: manipular a entrada do teclado no nível do formulário
Os Windows Forms proporcionam a capacidade de manipular mensagens de teclado no nível do formulário, antes que as mensagens cheguem a um controle. Este tópico mostra como realizar essa tarefa.  
  
### <a name="to-handle-a-keyboard-message-at-the-form-level"></a>Como manipular uma mensagem do teclado no nível do formulário  
  
- Manipule o <xref:System.Windows.Forms.Control.KeyPress> <xref:System.Windows.Forms.Control.KeyDown> evento ou do formulário de inicialização e defina a <xref:System.Windows.Forms.Form.KeyPreview%2A> Propriedade do formulário como para `true` que as mensagens do teclado sejam recebidas pelo formulário antes de alcançarem controles no formulário. O exemplo de código a seguir trata o <xref:System.Windows.Forms.Control.KeyPress> evento detectando todas as chaves de número e consumindo ' 1 ', ' 4 ' e ' 7 '.  
  
     [!code-cpp[System.Windows.Forms.KeyboardInputForm#10](~/samples/snippets/cpp/VS_Snippets_Winforms/System.Windows.Forms.KeyboardInputForm/cpp/form1.cpp#10)]
     [!code-csharp[System.Windows.Forms.KeyboardInputForm#10](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.KeyboardInputForm/CS/form1.cs#10)]
     [!code-vb[System.Windows.Forms.KeyboardInputForm#10](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.KeyboardInputForm/VB/form1.vb#10)]  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir é o aplicativo completo para o exemplo acima. O aplicativo inclui um <xref:System.Windows.Forms.TextBox> juntamente com vários outros controles que permitem que você mova o foco do <xref:System.Windows.Forms.TextBox> . O <xref:System.Windows.Forms.Control.KeyPress> evento da principal <xref:System.Windows.Forms.Form> consome ' 1 ', ' 4 ' e ' 7 ', e o <xref:System.Windows.Forms.Control.KeyPress> evento de <xref:System.Windows.Forms.TextBox> consome ' 2 ', ' 5 ' e ' 8 ' ao exibir as chaves restantes. Compare a <xref:System.Windows.Forms.MessageBox> saída quando você pressiona uma tecla numérica enquanto o <xref:System.Windows.Forms.TextBox> foco <xref:System.Windows.Forms.MessageBox> está na saída quando você pressiona uma tecla numérica enquanto o foco está em um dos outros controles.  
  
 [!code-cpp[System.Windows.Forms.KeyBoardInputForm#0](~/samples/snippets/cpp/VS_Snippets_Winforms/System.Windows.Forms.KeyboardInputForm/cpp/form1.cpp#0)]
 [!code-csharp[System.Windows.Forms.KeyBoardInputForm#0](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.KeyboardInputForm/CS/form1.cs#0)]
 [!code-vb[System.Windows.Forms.KeyBoardInputForm#0](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.KeyboardInputForm/VB/form1.vb#0)]  
  
## <a name="compiling-the-code"></a>Compilando o código  
 Este exemplo requer:  
  
- Referências aos assemblies System, System.Drawing e System.Windows.Forms.  
  
## <a name="see-also"></a>Veja também

- [Entrada do teclado em um aplicativo do Windows Forms](keyboard-input-in-a-windows-forms-application.md)
