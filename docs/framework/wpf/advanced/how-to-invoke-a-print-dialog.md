---
title: Como invocar uma caixa de diálogo Imprimir
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- invoking print dialogs [WPF]
- print dialogs [WPF], invoking
ms.assetid: e3a2c84c-74fe-45a4-8501-5813f9dbfed2
ms.openlocfilehash: 75a4160366766efbd19d6e8ae26fbec102e7f95b
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86924494"
---
# <a name="how-to-invoke-a-print-dialog"></a>Como invocar uma caixa de diálogo Imprimir
Para fornecer a capacidade de imprimir a partir do seu aplicativo, você pode simplesmente criar e abrir um <xref:System.Windows.Controls.PrintDialog> objeto.  
  
## <a name="example"></a>Exemplo  
 O <xref:System.Windows.Controls.PrintDialog> controle fornece um único ponto de entrada para o [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] , configuração e envio de trabalho XPS. O controle é fácil de usar e pode ser instanciado usando marcação ou código [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)]. O exemplo a seguir demonstra como instanciar e abrir o controle no código e como imprimir por meio dele. Também mostra como assegurar que o diálogo dê ao usuário a opção de definir um intervalo específico de páginas. O código do exemplo pressupõe que existe um arquivo FixedDocumentSequence.xps na raiz da unidade C:.  
  
 [!code-csharp[printdialog#1](~/samples/snippets/csharp/VS_Snippets_Wpf/PrintDialog/CSharp/Window1.xaml.cs#1)]
 [!code-vb[printdialog#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PrintDialog/visualbasic/window1.xaml.vb#1)]  
  
 Uma vez que a caixa de diálogo estiver aberta, os usuários poderão selecionar as impressoras instaladas em seu computador. Eles também terão a opção de selecionar o [Microsoft XPS Document Writer](/windows/win32/printdocs/microsoft-xps-document-writer) para criar um arquivo XPS (XML Paper Specification) em vez de imprimir.  
  
> [!NOTE]
> O <xref:System.Windows.Controls.PrintDialog?displayProperty=nameWithType> controle do WPF, que é discutido neste tópico, não deve ser confundido com o <xref:System.Windows.Forms.PrintDialog?displayProperty=nameWithType> componente do Windows Forms.  
  
 Estritamente falando, você pode usar o <xref:System.Windows.Controls.PrintDialog.PrintDocument%2A> método sem nunca abrir a caixa de diálogo. Assim, o controle pode ser utilizado como um componente de impressão não visto. Mas, por motivos de desempenho, seria melhor usar o <xref:System.Printing.PrintQueue.AddJob%2A> método ou um dos muitos <xref:System.Windows.Xps.XpsDocumentWriter.Write%2A> e <xref:System.Windows.Xps.XpsDocumentWriter.WriteAsync%2A> métodos do <xref:System.Windows.Xps.XpsDocumentWriter> . Para saber mais sobre isso, veja [arquivos XPS de impressão programaticamente](how-to-programmatically-print-xps-files.md).  
  
## <a name="see-also"></a>Veja também

- <xref:System.Windows.Controls.PrintDialog>
- [Documentos no WPF](documents-in-wpf.md)
- [Visão geral da impressão](printing-overview.md)
- [Microsoft XPS Document Writer](/windows/win32/printdocs/microsoft-xps-document-writer)
