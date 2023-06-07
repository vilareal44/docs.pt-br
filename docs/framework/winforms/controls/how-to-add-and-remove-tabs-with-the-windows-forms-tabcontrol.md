---
title: Adicionar e remover guias com TabControl
description: Saiba como adicionar e remover guias com o Windows Forms controle TabControl, que contém dois controles TabPage. Acesse essas guias por meio da propriedade TabPages.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- tabs [Windows Forms], removing from pages
- TabPage control
- TabControl control [Windows Forms], adding and removing tabs
- tabs [Windows Forms], adding to pages
- tab pages
ms.assetid: 66d4dfca-41e8-44e3-9c80-fb7ac4cb1619
ms.openlocfilehash: 7e67d0bbc13bd7d9c8835dc6fb9b9c5c9333b8bf
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85618071"
---
# <a name="how-to-add-and-remove-tabs-with-the-windows-forms-tabcontrol"></a>Como adicionar e remover guias com o controle TabControl dos Windows Forms
Por padrão, um <xref:System.Windows.Forms.TabControl> controle contém dois <xref:System.Windows.Forms.TabPage> controles. Você pode acessar essas guias por meio da <xref:System.Windows.Forms.TabControl.TabPages%2A> propriedade.  
  
### <a name="to-add-a-tab-programmatically"></a>Para adicionar uma guia programaticamente  
  
- Use o <xref:System.Windows.Forms.TabControl.TabPageCollection.Add%2A> método da <xref:System.Windows.Forms.TabControl.TabPages%2A> propriedade.  
  
    ```vb  
    Dim myTabPage As New TabPage()  
    myTabPage.Text = "TabPage" & (TabControl1.TabPages.Count + 1)  
    TabControl1.TabPages.Add(myTabPage)  
    ```  
  
    ```csharp  
    string title = "TabPage " + (tabControl1.TabCount + 1).ToString();  
    TabPage myTabPage = new TabPage(title);  
    tabControl1.TabPages.Add(myTabPage);  
    ```  
  
    ```cpp  
    String^ title = String::Concat("TabPage ",  
       (tabControl1->TabCount + 1).ToString());  
    TabPage^ myTabPage = gcnew TabPage(title);  
    tabControl1->TabPages->Add(myTabPage);  
    ```  
  
### <a name="to-remove-a-tab-programmatically"></a>Para remover uma guia programaticamente  
  
- Para remover as guias selecionadas, use o <xref:System.Windows.Forms.TabControl.TabPageCollection.Remove%2A> método da <xref:System.Windows.Forms.TabControl.TabPages%2A> propriedade.  
  
     -ou-  
  
- Para remover todas as guias, use o <xref:System.Windows.Forms.TabControl.TabPageCollection.Clear%2A> método da <xref:System.Windows.Forms.TabControl.TabPages%2A> propriedade.  
  
    ```vb  
    ' Removes the selected tab:  
    TabControl1.TabPages.Remove(TabControl1.SelectedTab)  
    ' Removes all the tabs:  
    TabControl1.TabPages.Clear()  
    ```  
  
    ```csharp  
    // Removes the selected tab:  
    tabControl1.TabPages.Remove(tabControl1.SelectedTab);  
    // Removes all the tabs:  
    tabControl1.TabPages.Clear();  
    ```  
  
    ```cpp  
    // Removes the selected tab:  
    tabControl1->TabPages->Remove(tabControl1->SelectedTab);  
    // Removes all the tabs:  
    tabControl1->TabPages->Clear();  
    ```  
  
## <a name="see-also"></a>Consulte também

- [Visão geral do controle TabControl](tabcontrol-control-overview-windows-forms.md)
- [Como adicionar um controle a uma página da guia](how-to-add-a-control-to-a-tab-page.md)
- [Como desabilitar páginas de guia](how-to-disable-tab-pages.md)
- [Como alterar a aparência do TabControl dos Windows Forms](how-to-change-the-appearance-of-the-windows-forms-tabcontrol.md)
