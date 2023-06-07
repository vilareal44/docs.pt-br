---
title: Como posicionar os elementos filhos de uma grade
description: Saiba como usar os métodos get e set que são definidos em uma grade de Windows Presentation Foundation para posicionar elementos filho.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Grid control [WPF], positioning child elements
ms.assetid: 27b3ba9b-ad32-44e2-bcab-a79d573a463c
ms.openlocfilehash: 926d223097dd21dd0292c9523903b4be8aba8ba9
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87167392"
---
# <a name="how-to-position-the-child-elements-of-a-grid"></a>Como posicionar os elementos filhos de uma grade
Este exemplo mostra como usar os métodos get e set que são definidos no <xref:System.Windows.Controls.Grid> para posicionar elementos filho.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir define um <xref:System.Windows.Controls.Grid> elemento pai ( `grid1` ) que tem três colunas e três linhas. Um <xref:System.Windows.Shapes.Rectangle> elemento filho ( `rect1` ) é adicionado ao <xref:System.Windows.Controls.Grid> na coluna posição zero, posição de linha zero. <xref:System.Windows.Controls.Button>os elementos representam métodos que podem ser chamados para reposicionar o <xref:System.Windows.Shapes.Rectangle> elemento dentro do <xref:System.Windows.Controls.Grid> . Quando um usuário clica em um botão, o método relacionado é ativado.  
  
 [!code-xaml[gridGetSetMethods](~/samples/snippets/csharp/VS_Snippets_Wpf/gridGetSetMethods/CSharp/Window1.xaml)]  
  
 O exemplo code-behind a seguir manipula os métodos que os <xref:System.Windows.Controls.Primitives.ButtonBase.Click> eventos de botão geram. O exemplo grava essas chamadas de método em <xref:System.Windows.Controls.TextBlock> elementos que usam os métodos get relacionados para gerar os novos valores de propriedade como cadeias de caracteres.  
  
 [!code-csharp[gridGetSetMethods#2](~/samples/snippets/csharp/VS_Snippets_Wpf/gridGetSetMethods/CSharp/Window1.xaml.cs#2)]
 [!code-vb[gridGetSetMethods#2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/gridGetSetMethods/VisualBasic/Window1.xaml.vb#2)]  
 Este é o resultado concluído!

 ![uma captura de tela representa uma interface do usuário do WPF com duas colunas, o lado direito tem uma grade 3 x 3 e a esquerda tem botões para mover um retângulo colorido entre as colunas e as linhas da grade](././media/grid-methods-sample.png)
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.Controls.Grid>
- [Visão geral de painéis](panels-overview.md)
