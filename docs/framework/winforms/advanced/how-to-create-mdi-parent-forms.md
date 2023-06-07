---
title: Como criar formulários pai MDI
description: Saiba como criar um formulário pai MDI programaticamente e usando o Designer de Formulários do Windows.
ms.date: 03/30/2017
helpviewer_keywords:
- parent forms
- MDI [Windows Forms], creating forms
ms.assetid: 12c71221-2377-4bb6-b10b-7b4b300fd462
ms.openlocfilehash: d387837a565ca247f62828c19f353990b35117c7
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174699"
---
# <a name="how-to-create-mdi-parent-forms"></a>Como criar formulários pai MDI

> [!IMPORTANT]
> Este tópico usa o controle <xref:System.Windows.Forms.MainMenu>, que foi substituído pelo controle <xref:System.Windows.Forms.MenuStrip>. O controle <xref:System.Windows.Forms.MainMenu> é mantido para compatibilidade com versões anteriores e uso futuro, se você desejar. Para obter informações sobre como criar um formulário pai MDI usando um <xref:System.Windows.Forms.MenuStrip> , consulte [como: criar uma lista de janelas MDI com MenuStrip](../controls/how-to-create-an-mdi-window-list-with-menustrip-windows-forms.md).

A base de um aplicativo de interface MDI é o formulário MDI pai. Este é o formulário que contém as janelas MDI filhas, que as subjanelas nas quais o usuário interage com o aplicativo MDI. Criar um formulário MDI pai é fácil, tanto no Designer de Formulários do Windows e por meio de programação.

## <a name="create-an-mdi-parent-form-at-design-time"></a>Criar um formulário pai MDI em tempo de design

1. Crie um projeto de aplicativo do Windows no Visual Studio.

2. Na janela **Propriedades** , defina a <xref:System.Windows.Forms.Form.IsMdiContainer%2A> propriedade como **true**.

     Isso designa o formulário como um recipiente MDI para janelas filho.

    > [!NOTE]
    > Ao configurar propriedades na janela **Propriedades**, você também pode definir a propriedade `WindowState` para **Maximized**, se desejar, pois é mais fácil manipular janelas filho MDI quando o formulário pai está maximizado. Além disso, esteja ciente de que a borda do formulário MDI pai selecionará a cor do sistema (definida no Painel de Controle do Sistema do Windows), em vez da cor de fundo definida usando a propriedade <xref:System.Windows.Forms.Control.BackColor%2A?displayProperty=nameWithType>.

3. Na **Caixa de Ferramentas**, arraste um controle **MenuStrip** para o formulário. Crie um item de menu de nível superior com a propriedade **Text** definida para **&File** com itens de submenu chamados **&New** e **&Close**. Crie também um item de menu de nível superior chamado **&Window**.

     O primeiro menu criará e ocultará itens do menu no tempo de execução e o segundo menu irá controlar as janelas MDI filhas abertas. Neste ponto, você já terá criado uma janela MDI pai.

4. Pressione **F5** para executar o aplicativo. Para obter informações sobre como criar janelas filho MDI que operam no formulário pai MDI, confira [Como criar formulários filho MDI](how-to-create-mdi-child-forms.md).

## <a name="see-also"></a>Veja também

- [Aplicativos da interface MDI (Interface de Vários Documentos)](multiple-document-interface-mdi-applications.md)
- [Como criar formulários filho MDI](how-to-create-mdi-child-forms.md)
- [Como determinar o filho MDI ativo](how-to-determine-the-active-mdi-child.md)
- [Como enviar dados para o filhos MDI ativos](how-to-send-data-to-the-active-mdi-child.md)
- [Como Organizar Formulários Filho MDI](how-to-arrange-mdi-child-forms.md)
