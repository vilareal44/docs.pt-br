---
title: Como editar colunas e linhas em um controle TableLayoutPanel
description: Saiba como usar a caixa de diálogo estilos de coluna e linha para editar as linhas e colunas de seus controles de Windows Forms.
ms.date: 03/30/2017
f1_keywords:
- net.ComponentModel.StyleCollectionEditor
helpviewer_keywords:
- columns [Windows Forms], editing
- TableLayoutPanel control [Windows Forms], editing
- rows [Windows Forms], editing
ms.assetid: c367ed43-40dc-49eb-9e0f-ba70e83dfec0
ms.openlocfilehash: cfd2ca4be5d5a2658a9a129d911f1dba9670ccfd
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619345"
---
# <a name="how-to-edit-columns-and-rows-in-a-tablelayoutpanel-control"></a>Como editar colunas e linhas em um controle TableLayoutPanel

Você pode usar o editor de coleção do <xref:System.Windows.Forms.TableLayoutPanel> controle, chamado de **coluna e** caixa de diálogo estilos de linha, para editar as linhas e colunas de seus controles.

> [!NOTE]
> Se você quiser que um controle ocupe várias linhas ou colunas, defina as propriedades `RowSpan` e `ColumnSpan` do controle. Para obter mais informações, consulte [Passo a passo: organizando controles nos Windows Forms usando um TableLayoutPanel](walkthrough-arranging-controls-on-windows-forms-using-a-tablelayoutpanel.md).
>
> Se você quiser alinhar um controle dentro de uma célula ou se desejar que um controle seja ampliado dentro de uma célula, use a <xref:System.Windows.Forms.Control.Anchor%2A> Propriedade do controle. Para obter mais informações, consulte [Passo a passo: organizando controles nos Windows Forms usando um TableLayoutPanel](walkthrough-arranging-controls-on-windows-forms-using-a-tablelayoutpanel.md).

## <a name="to-edit-rows-and-columns"></a>Editar linhas e colunas

1. Arraste um <xref:System.Windows.Forms.TableLayoutPanel> controle da **caixa de ferramentas** para seu formulário.

2. Clique no <xref:System.Windows.Forms.TableLayoutPanel> glifo ações do designer do controle ( ![ seta preta pequena ](./media/designer-actions-glyph.gif) ) e selecione **Editar linhas e colunas** para abrir a caixa de diálogo estilos de **coluna e linha** . Você também pode clicar com o botão direito do mouse no <xref:System.Windows.Forms.TableLayoutPanel> controle e selecionar **Editar linhas e colunas** no menu de atalho.

3. Para adicionar ou remover colunas, selecione **Colunas** da caixa de listagem suspensa **Tipo de membro**.

4. Para adicionar ou remover linhas, selecione **Linhas** da caixa de listagem suspensa **Tipo de membro**.

5. Clique no botão **Adicionar** para adicionar uma linha ou coluna ao final da linha **Membro**.

6. Clique no botão **Inserir** para adicionar uma linha ou coluna antes do item atualmente selecionado na lista.

7. Se você estiver adicionando uma linha ou coluna, selecione o **Tipo de tamanho** para a nova linha ou coluna. Para obter mais informações, consulte <xref:System.Windows.Forms.SizeType>.

8. Para remover uma linha ou coluna, clique no botão **Remover** para excluir o item selecionado no momento na lista **Membro**.

## <a name="see-also"></a>Consulte também

- <xref:System.Windows.Forms.SizeType>
- [Controle TableLayoutPanel](tablelayoutpanel-control-windows-forms.md)
