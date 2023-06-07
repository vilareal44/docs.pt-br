---
title: Adicionar colunas ao controle ListView
description: Saiba como adicionar colunas ao controle Windows Forms ListView para exibir vários tipos de informações sobre cada item de lista.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- ListView control [Windows Forms], adding column headers
- columns [Windows Forms], adding to ListView controls
- list views [Windows Forms], adding columns
ms.assetid: 79174274-12ee-4a5d-80db-6ec02976d010
ms.openlocfilehash: 7ca4e86ca3a9679876f2525c49596534f175096a
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174556"
---
# <a name="how-to-add-columns-to-the-windows-forms-listview-control"></a>Como adicionar colunas ao controle ListView dos Windows Forms
Na exibição de detalhes, o <xref:System.Windows.Forms.ListView> controle pode exibir várias colunas para cada item de lista. Você pode usar as colunas a serem exibidas para o usuário vários tipos de informações sobre cada item de lista. Por exemplo, uma lista de arquivos pode exibir o nome do arquivo, tipo de arquivo, tamanho e data da última modificação do arquivo. Para obter informações sobre como preencher as colunas depois que elas são criadas, consulte [como Exibir subitens em colunas com o controle ListView Windows Forms](how-to-display-subitems-in-columns-with-the-windows-forms-listview-control.md).  
  
### <a name="to-add-columns-programmatically"></a>Para adicionar colunas programaticamente  
  
1. Defina a propriedade do controle <xref:System.Windows.Forms.ListView.View%2A> como <xref:System.Windows.Forms.View.Details> .  
  
2. Use o <xref:System.Windows.Forms.ListView.ColumnHeaderCollection.Add%2A> método da propriedade da exibição de lista <xref:System.Windows.Forms.ListView.Columns%2A> .  
  
     [!code-csharp[System.Windows.Forms.ListViewLegacyTopics#31](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.ListViewLegacyTopics/CS/Class1.cs#31)]
     [!code-vb[System.Windows.Forms.ListViewLegacyTopics#31](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.ListViewLegacyTopics/VB/Class1.vb#31)]  
  
## <a name="see-also"></a>Veja também

- <xref:System.Windows.Forms.ListView>
- [Controle ListView](listview-control-windows-forms.md)
- [Visão geral do controle ListView](listview-control-overview-windows-forms.md)
