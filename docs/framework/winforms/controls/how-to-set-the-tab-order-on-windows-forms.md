---
title: Definir a ordem de tabulação dos controles
description: Saiba como definir a ordem de tabulação dos controles em seu Windows Forms. Defina a ordem de tabulação com o Visual Studio ou usando a propriedade TabIndex na janela Propriedades.
ms.date: 03/30/2017
f1_keywords:
- TabStop
- TabIndex
helpviewer_keywords:
- tab order [Windows Forms], controls on Windows forms
- Windows Forms controls, setting tab order
- controls [Windows Forms], setting tab order
- Windows Forms, setting tab order
ms.assetid: 71fa8e76-0472-414b-ad3c-0f90166e0ad7
author: jillre
ms.author: jillfra
manager: jillfra
ms.openlocfilehash: 3d16da1ac73cc030b92bb36c4bfa3a79985339bf
ms.sourcegitcommit: 3824ff187947572b274b9715b60c11269335c181
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84903345"
---
# <a name="how-to-set-the-tab-order-on-windows-forms"></a>Como definir a ordem de tabulação em Windows Forms

A ordem de tabulação é a ordem na qual um usuário move o foco de um controle para outro pressionando a tecla Tab. Cada formulário tem sua própria ordem de tabulação. Por padrão, a ordem de tabulação é igual à ordem em que você criou os controles. A numeração da ordem de tabulação começa com zero.

## <a name="to-set-the-tab-order-of-a-control"></a>Para definir a ordem de tabulação de um controle

1. No Visual Studio, no menu **Exibir** , selecione **ordem de tabulação**.

   Isso ativa o modo de seleção de ordem de tabulação do formulário. Um número (representando a <xref:System.Windows.Forms.Control.TabIndex%2A> Propriedade) aparece no canto superior esquerdo de cada controle.

2. Clique nos controles sequencialmente para estabelecer a ordem de tabulação desejada.

   > [!NOTE]
   > Local do controle na ordem de tabulação pode ser definido como qualquer valor maior ou igual a 0. Quando ocorrem duplicatas, a ordem z dos dois controles é avaliada e o controle na parte superior é tabulado primeiro. (A ordem z consiste na disposição em camadas visuais de controles em um formulário ao longo do eixo z do formulário [profundidade]. A ordem z determina quais controles estão na frente de outros controles.) Para obter mais informações sobre a ordem z, consulte [camadas de objetos em Windows Forms](how-to-layer-objects-on-windows-forms.md).

3. Quando terminar, selecione a **ordem de tabulação** no menu **Exibir** novamente para sair do modo de ordem de tabulação.

   > [!NOTE]
   > Controles que não podem obter o foco, bem como controles desabilitados e invisíveis, não têm uma <xref:System.Windows.Forms.Control.TabIndex%2A> propriedade e não são incluídos na ordem de tabulação. Como um usuário pressiona a tecla Tab, esses controles são ignorados.

Como alternativa, a ordem de Tabulação pode ser definida no janela Propriedades usando a <xref:System.Windows.Forms.Control.TabIndex%2A> propriedade. A <xref:System.Windows.Forms.Control.TabIndex%2A> propriedade de um controle determina onde ele é posicionado na ordem de tabulação. Por padrão, o primeiro controle desenhado tem um <xref:System.Windows.Forms.Control.TabIndex%2A> valor de 0, o segundo tem um <xref:System.Windows.Forms.Control.TabIndex%2A> de 1 e assim por diante.

Além disso, por padrão, um <xref:System.Windows.Forms.GroupBox> controle tem seu próprio <xref:System.Windows.Forms.Control.TabIndex%2A> valor, que é um número inteiro. Um <xref:System.Windows.Forms.GroupBox> controle em si não pode ter o foco em tempo de execução. Assim, cada controle dentro de um <xref:System.Windows.Forms.GroupBox> tem seu próprio <xref:System.Windows.Forms.Control.TabIndex%2A> valor decimal, começando com. 0. Naturalmente, como o <xref:System.Windows.Forms.Control.TabIndex%2A> de um <xref:System.Windows.Forms.GroupBox> controle é incrementado, os controles dentro dele serão incrementados de acordo. Se você tiver alterado um <xref:System.Windows.Forms.Control.TabIndex%2A> valor de 5 para 6, o <xref:System.Windows.Forms.Control.TabIndex%2A> valor do primeiro controle em seu grupo será alterado automaticamente para 6,0 e assim por diante.

Por fim, qualquer controle dos muitos no seu formulário podem ser ignorados na ordem de tabulação. Normalmente, pressionar Tab sucessivamente em tempo de execução seleciona cada controle na ordem de tabulação. Ao desativar a <xref:System.Windows.Forms.Control.TabStop%2A> propriedade, você pode fazer com que um controle seja passado na ordem de tabulação do formulário.

## <a name="to-remove-a-control-from-the-tab-order"></a>Para remover um controle da ordem de tabulação

Defina a propriedade do controle <xref:System.Windows.Forms.Control.TabStop%2A> como **false** na janela **Propriedades** .

Um controle cuja <xref:System.Windows.Forms.Control.TabStop%2A> propriedade foi definida como `false` ainda mantém sua posição na ordem de tabulação, embora o controle seja ignorado quando você percorre os controles com a tecla Tab.

> [!NOTE]
> Um grupo de botões de opção tem uma única parada de tabulação no tempo de execução. O botão selecionado (ou seja, o botão com sua <xref:System.Windows.Forms.RadioButton.Checked%2A> propriedade definida como `true` ) tem sua <xref:System.Windows.Forms.Control.TabStop%2A> propriedade definida automaticamente como `true` , enquanto os outros botões têm sua <xref:System.Windows.Forms.Control.TabStop%2A> propriedade definida como `false` . Para obter mais informações sobre como agrupar <xref:System.Windows.Forms.RadioButton> controles, consulte [agrupando Windows Forms controles RadioButton para funcionar como um conjunto](how-to-group-windows-forms-radiobutton-controls-to-function-as-a-set.md).

## <a name="see-also"></a>Veja também

- [Controles de Windows Forms](index.md)
- [Controles a serem usados em Windows Forms](controls-to-use-on-windows-forms.md)
- [Controles dos Windows Forms por função](windows-forms-controls-by-function.md)
