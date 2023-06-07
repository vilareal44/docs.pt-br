---
title: Modo de preenchimento de coluna no controle DataGridView
description: Saiba como o controle DataGridView Windows Forms no modo de preenchimento de coluna redimensiona suas colunas automaticamente para que elas preencham a largura da área de exibição disponível.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data grids [Windows Forms], automatically resizing columns
- DataGridView control [Windows Forms], column fill mode
- data grids [Windows Forms], column fill mode
ms.assetid: b4ef7411-ebf4-4e26-bb33-aecec90de80c
ms.openlocfilehash: 766a58954250d78ce6e44404730332b3158e1fad
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85622816"
---
# <a name="column-fill-mode-in-the-windows-forms-datagridview-control"></a>Modo de preenchimento da coluna no controle DataGridView dos Windows Forms
No modo de preenchimento de coluna, o <xref:System.Windows.Forms.DataGridView> controle redimensiona suas colunas automaticamente para que elas preencham a largura da área de exibição disponível. O controle não exibe a barra de rolagem horizontal, exceto quando é necessário manter a largura de cada coluna igual ou maior que o <xref:System.Windows.Forms.DataGridViewColumn.MinimumWidth%2A> valor da propriedade.  
  
 O comportamento de dimensionamento de cada coluna depende de sua <xref:System.Windows.Forms.DataGridViewColumn.InheritedAutoSizeMode%2A> propriedade. O valor dessa propriedade será herdado da propriedade da coluna <xref:System.Windows.Forms.DataGridViewColumn.AutoSizeMode%2A> ou da Propriedade do controle <xref:System.Windows.Forms.DataGridView.AutoSizeColumnsMode%2A> se o valor da coluna for <xref:System.Windows.Forms.DataGridViewAutoSizeColumnMode.NotSet> (o valor padrão).  
  
 Cada coluna pode ter um modo de tamanho diferente, mas todas as colunas com um modo de tamanho de <xref:System.Windows.Forms.DataGridViewAutoSizeColumnMode.Fill> irão compartilhar a largura da área de exibição que não é usada pelas outras colunas. Essa largura é dividida entre as colunas de modo de preenchimento em proporções relativas aos seus <xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A> valores de propriedade. Por exemplo, se duas colunas tiverem <xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A> valores de 100 e 200, a primeira coluna será metade de largura da segunda coluna.  
  
## <a name="user-resizing-in-fill-mode"></a>Redimensionamento de usuário no modo de preenchimento  
 Ao contrário dos modos de dimensionamento que redimensionam com base no conteúdo da célula, o modo de preenchimento não impede que os usuários redimensionem colunas com <xref:System.Windows.Forms.DataGridViewColumn.Resizable%2A> valores de propriedade de `true` . Quando um usuário redimensiona uma coluna de modo de preenchimento, todas as colunas de modo de preenchimento após a coluna redimensionada (à direita, se <xref:System.Windows.Forms.Control.RightToLeft%2A> for `false` ; caso contrário, à esquerda) também são redimensionadas para compensar a alteração na largura disponível. Se não houver nenhuma coluna do modo de preenchimento depois da coluna redimensionada, todas as outras colunas do modo de preenchimento no controle serão redimensionadas para compensar. Se não houver nenhuma outra coluna do modo de preenchimento no controle, o redimensionamento será ignorado. Se uma coluna que não estiver no modo de preenchimento for redimensionada, todas as colunas do modo de preenchimento no controle terão os tamanhos alterados para compensar.  
  
 Depois de redimensionar uma coluna de modo de preenchimento, os <xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A> valores de todas as colunas alteradas são ajustados proporcionalmente. Por exemplo, se quatro colunas de modo de preenchimento tiverem <xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A> valores de 100, redimensionar a segunda coluna para metade da largura original resultará em <xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A> valores de 100, 50, 125 e 125. O redimensionamento de uma coluna que não está no modo de preenchimento não alterará nenhum <xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A> valor porque as colunas de modo de preenchimento simplesmente serão redimensionadas para compensar enquanto retém as mesmas proporções.  
  
## <a name="content-based-fillweight-adjustment"></a>Ajuste de FillWeight baseado em conteúdo  
 Você pode inicializar <xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A> valores para colunas de modo de preenchimento usando os <xref:System.Windows.Forms.DataGridView> métodos de redimensionamento automático, como o <xref:System.Windows.Forms.DataGridView.AutoResizeColumns%2A> método. Esse método primeiro calcula as larguras necessárias por colunas para exibir seu conteúdo. Em seguida, o controle ajusta os <xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A> valores para todas as colunas de modo de preenchimento para que suas proporções correspondam às proporções das larguras calculadas. Por fim, o controle redimensiona as colunas de modo de preenchimento usando as novas <xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A> proporções para que todas as colunas no controle preencham o espaço horizontal disponível.  
  
## <a name="example"></a>Exemplo  
  
### <a name="description"></a>Descrição  
 Usando os valores apropriados para as <xref:System.Windows.Forms.DataGridViewColumn.AutoSizeMode%2A> <xref:System.Windows.Forms.DataGridViewColumn.MinimumWidth%2A> Propriedades,, e <xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A> <xref:System.Windows.Forms.DataGridViewColumn.Resizable%2A> , você pode personalizar os comportamentos de dimensionamento de coluna para vários cenários diferentes.  
  
 O código de demonstração a seguir permite que você experimente valores diferentes para <xref:System.Windows.Forms.DataGridViewColumn.AutoSizeMode%2A> as <xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A> Propriedades, e <xref:System.Windows.Forms.DataGridViewColumn.MinimumWidth%2A> de colunas diferentes. Neste exemplo, um <xref:System.Windows.Forms.DataGridView> controle é associado a sua própria <xref:System.Windows.Forms.DataGridView.Columns%2A> coleção e uma coluna é associada a cada uma das <xref:System.Windows.Forms.DataGridViewColumn.HeaderText%2A> <xref:System.Windows.Forms.DataGridViewColumn.AutoSizeMode%2A> Propriedades,, <xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A> , <xref:System.Windows.Forms.DataGridViewColumn.MinimumWidth%2A> e <xref:System.Windows.Forms.DataGridViewColumn.Width%2A> . Cada uma das colunas também é representada por uma linha no controle e a alteração dos valores em uma linha atualizará as propriedades da coluna correspondente para que você possa ver como os valores interagem.  
  
### <a name="code"></a>Código  
 [!code-csharp[System.Windows.Forms.DataGridViewFillColumnsDemo#00](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewFillColumnsDemo/CS/fillcolumns.cs#00)]
 [!code-vb[System.Windows.Forms.DataGridViewFillColumnsDemo#00](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewFillColumnsDemo/vb/fillcolumns.vb#00)]  
  
### <a name="comments"></a>Comentários  
 Para usar esse aplicativo de demonstração:  
  
- Altere o tamanho do formulário. Observe como as colunas alteram suas larguras enquanto retém as proporções indicadas pelos <xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A> valores de propriedade.  
  
- Altere os tamanhos das colunas, arrastando os divisores de coluna com o mouse. Observe como os <xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A> valores são alterados.  
  
- Altere o <xref:System.Windows.Forms.DataGridViewColumn.MinimumWidth%2A> valor de uma coluna e arraste para redimensionar o formulário. Observe como, quando você torna o formulário pequeno o suficiente, os <xref:System.Windows.Forms.DataGridViewColumn.Width%2A> valores não ficam abaixo dos <xref:System.Windows.Forms.DataGridViewColumn.MinimumWidth%2A> valores.  
  
- Altere os <xref:System.Windows.Forms.DataGridViewColumn.MinimumWidth%2A> valores de todas as colunas para números grandes para que os valores combinados excedam a largura do controle. Observe como a barra de rolagem horizontal é exibida.  
  
- Altere os <xref:System.Windows.Forms.DataGridViewColumn.AutoSizeMode%2A> valores de algumas colunas. Observe o efeito quando você redimensiona as colunas ou o formulário.  
  
## <a name="compiling-the-code"></a>Compilando o código  
 Este exemplo requer:  
  
- Referências aos assemblies System, System.Drawing e System.Windows.Forms.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridView.AutoResizeColumns%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.AutoSizeColumnsMode%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewAutoSizeColumnsMode>
- <xref:System.Windows.Forms.DataGridViewColumn>
- <xref:System.Windows.Forms.DataGridViewColumn.InheritedAutoSizeMode%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewColumn.AutoSizeMode%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewAutoSizeColumnMode>
- <xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewColumn.MinimumWidth%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewColumn.Width%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewColumn.Resizable%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.Control.RightToLeft%2A?displayProperty=nameWithType>
- [Redimensionando colunas e linhas no controle DataGridView dos Windows Forms](resizing-columns-and-rows-in-the-windows-forms-datagridview-control.md)
