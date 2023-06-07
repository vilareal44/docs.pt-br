---
title: Adicionar e remover itens do controle ComboBox, ListBox ou CheckedListBox
ms.date: 03/30/2017
description: Saiba como adicionar e remover um Windows Forms controles ComboBox, ListBox e CheckedListBox simplesmente e sem Associação de dados.
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- combo boxes [Windows Forms], adding items
- list boxes [Windows Forms], removing items
- ComboBox control [Windows Forms], adding and removing items
- ListBox control [Windows Forms], adding and removing items
- list boxes [Windows Forms], adding items
- combo boxes [Windows Forms], removing items
- CheckedListBox control [Windows Forms], adding and removing items
ms.assetid: 7224c8d2-4118-443e-ae1e-d7c17d1e69ee
ms.openlocfilehash: f3701257bbe410bf03c4c21700705e87b581bf2e
ms.sourcegitcommit: 3824ff187947572b274b9715b60c11269335c181
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84904436"
---
# <a name="how-to-add-and-remove-items-from-a-windows-forms-combobox-listbox-or-checkedlistbox-control"></a>Como adicionar e remover itens de um controle ComboBox, ListBox ou CheckedListBox dos Windows Forms
Itens podem ser adicionados a uma caixa de combinação dos Windows Forms, caixa de listagem ou caixa de listagem marcada de várias maneiras, porque esses controles podem ser vinculados a uma variedade de fontes de dados. No entanto, este tópico demonstra o método mais simples e não requer nenhuma vinculação de dados. Normalmente, os itens exibidos são cadeias de caracteres; No entanto, qualquer objeto pode ser usado. O texto que é exibido no controle é o valor retornado pelo método do objeto `ToString` .  
  
### <a name="to-add-items"></a>Para adicionar Itens  
  
1. Adicione a cadeia de caracteres ou objeto à lista usando o método `Add` da classe `ObjectCollection`. A coleção é referenciada usando a `Items` Propriedade:  
  
    ```vb  
    ComboBox1.Items.Add("Tokyo")  
    ```  
  
    ```csharp  
    comboBox1.Items.Add("Tokyo");  
    ```  
  
    ```cpp  
    comboBox1->Items->Add("Tokyo");  
    ```  
  
     - ou –  
  
2. Insira a cadeia de caracteres ou o objeto no ponto desejado na lista com o `Insert` método:  
  
    ```vb  
    CheckedListBox1.Items.Insert(0, "Copenhagen")  
    ```  
  
    ```csharp  
    checkedListBox1.Items.Insert(0, "Copenhagen");  
    ```  
  
    ```cpp  
    checkedListBox1->Items->Insert(0, "Copenhagen");  
    ```  
  
     - ou –  
  
3. Atribua uma matriz inteira à `Items` coleção:  
  
    ```vb  
    Dim ItemObject(9) As System.Object  
    Dim i As Integer  
       For i = 0 To 9  
       ItemObject(i) = "Item" & i  
    Next i  
    ListBox1.Items.AddRange(ItemObject)  
    ```  
  
    ```csharp  
    System.Object[] ItemObject = new System.Object[10];  
    for (int i = 0; i <= 9; i++)  
    {  
       ItemObject[i] = "Item" + i;  
    }  
    listBox1.Items.AddRange(ItemObject);  
    ```  
  
    ```cpp  
    Array<System::Object^>^ ItemObject = gcnew Array<System::Object^>(10);  
    for (int i = 0; i <= 9; i++)  
    {  
       ItemObject[i] = String::Concat("Item", i.ToString());  
    }  
    listBox1->Items->AddRange(ItemObject);  
    ```  
  
### <a name="to-remove-an-item"></a>Para remover um item  
  
1. Chame o `Remove` `RemoveAt` método ou para excluir itens.  
  
     O `Remove` tem um argumento que especifica o item a ser removido.`RemoveAt` remove o item com o número de índice especificado.  
  
    ```vb  
    ' To remove item with index 0:  
    ComboBox1.Items.RemoveAt(0)  
    ' To remove currently selected item:  
    ComboBox1.Items.Remove(ComboBox1.SelectedItem)  
    ' To remove "Tokyo" item:  
    ComboBox1.Items.Remove("Tokyo")  
    ```  
  
    ```csharp  
    // To remove item with index 0:  
    comboBox1.Items.RemoveAt(0);  
    // To remove currently selected item:  
    comboBox1.Items.Remove(comboBox1.SelectedItem);  
    // To remove "Tokyo" item:  
    comboBox1.Items.Remove("Tokyo");  
    ```  
  
    ```cpp  
    // To remove item with index 0:  
    comboBox1->Items->RemoveAt(0);  
    // To remove currently selected item:  
    comboBox1->Items->Remove(comboBox1->SelectedItem);  
    // To remove "Tokyo" item:  
    comboBox1->Items->Remove("Tokyo");  
    ```  
  
### <a name="to-remove-all-items"></a>Para remover todos os itens  
  
1. Chame o método `Clear` para remover todos os itens da coleção:  
  
    ```vb  
    ListBox1.Items.Clear()  
    ```  
  
    ```csharp  
    listBox1.Items.Clear();  
    ```  
  
    ```cpp  
    listBox1->Items->Clear();  
    ```  
  
## <a name="see-also"></a>Veja também

- <xref:System.Windows.Forms.ComboBox>
- <xref:System.Windows.Forms.ListBox>
- <xref:System.Windows.Forms.CheckedListBox>
- [Como classificar o conteúdo de um controle ComboBox, ListBox ou CheckedListBox dos Windows Forms](sort-the-contents-of-a-wf-combobox-listbox-or-checkedlistbox-control.md)
- [Quando usar um ComboBox dos Windows Forms em vez de um ListBox](when-to-use-a-windows-forms-combobox-instead-of-a-listbox.md)
- [Controles dos Windows Forms usados para listar opções](windows-forms-controls-used-to-list-options.md)
