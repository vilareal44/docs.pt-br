---
title: associar objetos a controles DataGridView
description: Saiba como associar uma coleção de objetos a um controle Windows Forms DataGridView para que cada objeto seja exibido como uma linha separada.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DataGridView control [Windows Forms], object binding
- data grids [Windows Forms], object binding
- object binding [Windows Forms], DataGridView control
ms.assetid: cb8f29fa-577e-4e2b-883f-3a01c6189b9c
ms.openlocfilehash: add0047937b404dcec1ea12bac8053bb9bdfcf1c
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85325859"
---
# <a name="how-to-bind-objects-to-windows-forms-datagridview-controls"></a>Como associar objetos a controles DataGridView dos Windows Forms
O exemplo de código a seguir demonstra como associar uma coleção de objetos a um <xref:System.Windows.Forms.DataGridView> controle para que cada objeto seja exibido como uma linha separada. Este exemplo também ilustra como exibir uma propriedade com um tipo de enumeração em um <xref:System.Windows.Forms.DataGridViewComboBoxColumn> para que a lista suspensa caixa de combinação contenha os valores de enumeração.  
  
## <a name="example"></a>Exemplo  
 [!code-csharp[System.Windows.Forms.DataGridView._CollectionBound#00](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridView._CollectionBound/CS/collectionbound.cs#00)]
 [!code-vb[System.Windows.Forms.DataGridView._CollectionBound#00](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridView._CollectionBound/VB/collectionbound.vb#00)]  
  
## <a name="compiling-the-code"></a>Compilando o código  
 Este exemplo requer:  
  
- Referências aos assemblies System e System.Windows.Forms.  
  
## <a name="see-also"></a>Veja também

- <xref:System.Windows.Forms.DataGridView>
- [Exibindo dados no controle DataGridView dos Windows Forms](displaying-data-in-the-windows-forms-datagridview-control.md)
- [Como acessar objetos associados às linhas de DataGridView dos Windows Forms](how-to-access-objects-bound-to-windows-forms-datagridview-rows.md)
