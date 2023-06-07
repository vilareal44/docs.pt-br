---
title: Adicionar e remover itens com controle ListView
description: Saiba como adicionar e remover um item com o controle Windows Forms ListView especificando o item e atribuindo Propriedades a ele.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- ListView control [Windows Forms], populating
- list views [Windows Forms], adding list items
- ListView control [Windows Forms], adding list items
ms.assetid: 1b35a80a-edd8-495f-a807-a28c4aae52c6
ms.openlocfilehash: db374ded69bcbd93527381d75a8ff751a1c9abe6
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85618084"
---
# <a name="how-to-add-and-remove-items-with-the-windows-forms-listview-control"></a>Como adicionar e remover itens com o controle ListView dos Windows Forms
O processo de adicionar um item a um controle de Windows Forms <xref:System.Windows.Forms.ListView> consiste principalmente em especificar o item e atribuir propriedades a ele. A adição ou remoção de itens de lista pode ser feita a qualquer momento.  
  
### <a name="to-add-items-programmatically"></a>Para adicionar itens programaticamente  
  
1. Use o <xref:System.Windows.Forms.ListView.ListViewItemCollection.Add%2A> método da <xref:System.Windows.Forms.ListView.Items%2A> propriedade.  
  
     [!code-csharp[System.Windows.Forms.ListViewLegacyTopics#11](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.ListViewLegacyTopics/CS/Class1.cs#11)]
     [!code-vb[System.Windows.Forms.ListViewLegacyTopics#11](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.ListViewLegacyTopics/VB/Class1.vb#11)]  
  
### <a name="to-remove-items-programmatically"></a>Para remover itens programaticamente  
  
1. Use o <xref:System.Windows.Forms.ListView.ListViewItemCollection.RemoveAt%2A> <xref:System.Windows.Forms.ListView.ListViewItemCollection.Clear%2A> método ou da <xref:System.Windows.Forms.ListView.Items%2A> propriedade. O <xref:System.Windows.Forms.ListView.ListViewItemCollection.RemoveAt%2A> método Remove um único item; o <xref:System.Windows.Forms.ListView.ListViewItemCollection.Clear%2A> método remove todos os itens da lista.  
  
     [!code-csharp[System.Windows.Forms.ListViewLegacyTopics#12](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.ListViewLegacyTopics/CS/Class1.cs#12)]
     [!code-vb[System.Windows.Forms.ListViewLegacyTopics#12](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.ListViewLegacyTopics/VB/Class1.vb#12)]  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Windows.Forms.ListView>
- [Controle ListView](listview-control-windows-forms.md)
- [Visão geral do controle ListView](listview-control-overview-windows-forms.md)
