---
title: Como criar uma caixa de texto somente leitura
description: Saiba como transformar uma caixa de texto editável Windows Forms em uma caixa de texto de Windows Forms somente leitura.
ms.date: 03/30/2017
helpviewer_keywords:
- TextBox control [Windows Forms], read-only
- read-only text boxes
- text boxes [Windows Forms], read-only
ms.assetid: 60baa9ab-fa57-44ad-bb7c-61b05aa64296
ms.openlocfilehash: 5baa7c66d5f16560a4ea23861d563b099592957f
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619358"
---
# <a name="how-to-create-a-read-only-text-box-windows-forms"></a>Como criar uma caixa de texto somente leitura (Windows Forms)

Você pode transformar uma caixa de texto Windows Forms editável em um controle somente leitura. Por exemplo, a caixa de texto pode exibir um valor que geralmente é editado, mas pode não estar no momento, devido ao estado do aplicativo.

## <a name="to-create-a-read-only-text-box"></a>Para criar uma caixa de texto somente leitura

1. Defina a <xref:System.Windows.Forms.TextBox> Propriedade do controle <xref:System.Windows.Forms.TextBoxBase.ReadOnly%2A> como `true` . Com a propriedade definida como `true` , os usuários ainda podem rolar e realçar o texto em uma caixa de texto sem permitir alterações. Um comando de **cópia** é funcional em uma caixa de texto, mas os comandos **recortar** e **colar** não são.

    > [!NOTE]
    > A <xref:System.Windows.Forms.TextBoxBase.ReadOnly%2A> propriedade afeta apenas a interação do usuário em tempo de execução. Você ainda pode alterar o conteúdo da caixa de texto programaticamente em tempo de execução alterando a <xref:System.Windows.Forms.TextBox.Text%2A> propriedade da caixa de texto.

## <a name="see-also"></a>Consulte também

- <xref:System.Windows.Forms.TextBox>
- [Visão geral do controle TextBox](textbox-control-overview-windows-forms.md)
- [Como controlar o ponto de inserção em um controle TextBox dos Windows Forms](how-to-control-the-insertion-point-in-a-windows-forms-textbox-control.md)
- [Como criar uma caixa de texto de senha com o controle TextBox dos Windows Forms](how-to-create-a-password-text-box-with-the-windows-forms-textbox-control.md)
- [Como inserir aspas em uma cadeia de caracteres](how-to-put-quotation-marks-in-a-string-windows-forms.md)
- [Como selecionar texto no controle TextBox dos Windows Forms](how-to-select-text-in-the-windows-forms-textbox-control.md)
- [Como exibir várias linhas no controle TextBox dos Windows Forms](how-to-view-multiple-lines-in-the-windows-forms-textbox-control.md)
- [Controle TextBox](textbox-control-windows-forms.md)
