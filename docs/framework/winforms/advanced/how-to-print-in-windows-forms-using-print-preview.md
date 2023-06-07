---
title: Imprimir usando a visualização de impressão
description: Saiba como adicionar serviços de visualização de impressão ao seu aplicativo usando o Windows Forms controle PrintPreviewDialog.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- printing [Windows Forms], using print preview
- printing [Windows Forms], with print preview
- print preview
ms.assetid: 4a16f7e2-ae10-4485-b0ae-3d558334d0fe
ms.openlocfilehash: abcf77db40f648df1a0cd49922bb49e5c9407811
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621607"
---
# <a name="how-to-print-in-windows-forms-using-print-preview"></a>Como imprimir no Windows Forms usando Visualizar Impressão
É muito comum na programação do Windows Forms oferecer a visualização de impressão além dos serviços de impressão. Uma maneira fácil de adicionar serviços de visualização de impressão ao seu aplicativo é usar um <xref:System.Windows.Forms.PrintPreviewDialog> controle em combinação com a <xref:System.Drawing.Printing.PrintDocument.PrintPage> lógica de manipulação de eventos para imprimir um arquivo.  
  
### <a name="to-preview-a-text-document-with-a-printpreviewdialog-control"></a>Visualizar um documento de texto com um controle PrintPreviewDialog  
  
1. Adicione uma <xref:System.Windows.Forms.PrintPreviewDialog> , <xref:System.Drawing.Printing.PrintDocument> e duas cadeias de caracteres ao seu formulário.  
  
     [!code-csharp[System.Drawing.Printing.PrintPreviewExample#1](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.Printing.PrintPreviewExample/CS/Form1.cs#1)]
     [!code-vb[System.Drawing.Printing.PrintPreviewExample#1](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.Printing.PrintPreviewExample/VB/Form1.vb#1)]  
  
2. Defina a <xref:System.Drawing.Printing.PrintDocument.DocumentName%2A> propriedade para o documento que você deseja imprimir e abra e leia o conteúdo do documento para a cadeia de caracteres que você adicionou anteriormente.  
  
     [!code-csharp[System.Drawing.Printing.PrintPreviewExample#2](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.Printing.PrintPreviewExample/CS/Form1.cs#2)]
     [!code-vb[System.Drawing.Printing.PrintPreviewExample#2](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.Printing.PrintPreviewExample/VB/Form1.vb#2)]  
  
3. Como você faria para imprimir o documento, no <xref:System.Drawing.Printing.PrintDocument.PrintPage> manipulador de eventos, use a <xref:System.Drawing.Printing.PrintPageEventArgs.Graphics%2A> propriedade da <xref:System.Drawing.Printing.PrintPageEventArgs> classe e o conteúdo do arquivo para calcular as linhas por página e renderizar o conteúdo do documento. Depois que cada página for desenhada, verifique se ela é a última página e defina a <xref:System.Drawing.Printing.PrintPageEventArgs.HasMorePages%2A> propriedade de <xref:System.Drawing.Printing.PrintPageEventArgs> adequadamente. O <xref:System.Drawing.Printing.PrintDocument.PrintPage> evento é gerado até que <xref:System.Drawing.Printing.PrintPageEventArgs.HasMorePages%2A> seja `false` . Após a renderização do documento ser concluída, redefina a cadeia de caracteres a ser renderizada. Além disso, verifique se o <xref:System.Drawing.Printing.PrintDocument.PrintPage> evento está associado ao seu método de manipulação de eventos.  
  
    > [!NOTE]
    > Você poderá já ter concluído as etapas 2 e 3 se tiver implementado a impressão em seu aplicativo.  
  
     No exemplo de código a seguir, o manipulador de eventos é usado para imprimir o arquivo “testPage.txt” na mesma fonte usada no formulário.  
  
     [!code-csharp[System.Drawing.Printing.PrintPreviewExample#3](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.Printing.PrintPreviewExample/CS/Form1.cs#3)]
     [!code-vb[System.Drawing.Printing.PrintPreviewExample#3](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.Printing.PrintPreviewExample/VB/Form1.vb#3)]  
  
4. Defina a <xref:System.Windows.Forms.PrintPreviewDialog.Document%2A> Propriedade do <xref:System.Windows.Forms.PrintPreviewDialog> controle para o <xref:System.Drawing.Printing.PrintDocument> componente no formulário.  
  
     [!code-csharp[System.Drawing.Printing.PrintPreviewExample#5](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.Printing.PrintPreviewExample/CS/Form1.cs#5)]
     [!code-vb[System.Drawing.Printing.PrintPreviewExample#5](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.Printing.PrintPreviewExample/VB/Form1.vb#5)]  
  
5. Chame o <xref:System.Windows.Forms.CommonDialog.ShowDialog%2A> método no <xref:System.Windows.Forms.PrintPreviewDialog> controle. Normalmente, você chamaria <xref:System.Windows.Forms.CommonDialog.ShowDialog%2A> do <xref:System.Windows.Forms.Control.Click> método de manipulação de eventos de um botão. Chamar <xref:System.Windows.Forms.CommonDialog.ShowDialog%2A> gera o <xref:System.Drawing.Printing.PrintDocument.PrintPage> evento e renderiza a saída para o <xref:System.Windows.Forms.PrintPreviewDialog> controle. Quando o usuário clica no ícone de impressão na caixa de diálogo, o <xref:System.Drawing.Printing.PrintDocument.PrintPage> evento é gerado novamente, enviando a saída para a impressora em vez da caixa de diálogo de visualização. É por isso a cadeia de caracteres é redefinida no final do processo de renderização na etapa 3.  
  
     O exemplo de código a seguir mostra o <xref:System.Windows.Forms.Control.Click> método de manipulação de eventos para um botão no formulário. Esse método de manipulação de eventos chama os métodos para ler o documento e mostrar a caixa de diálogo de visualização de impressão.  
  
     [!code-csharp[System.Drawing.Printing.PrintPreviewExample#4](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.Printing.PrintPreviewExample/CS/Form1.cs#4)]
     [!code-vb[System.Drawing.Printing.PrintPreviewExample#4](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.Printing.PrintPreviewExample/VB/Form1.vb#4)]  
  
## <a name="example"></a>Exemplo  
 [!code-csharp[System.Drawing.Printing.PrintPreviewExample#0](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.Printing.PrintPreviewExample/CS/Form1.cs#0)]
 [!code-vb[System.Drawing.Printing.PrintPreviewExample#0](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.Printing.PrintPreviewExample/VB/Form1.vb#0)]  
  
## <a name="compiling-the-code"></a>Compilando o código  
 Este exemplo requer:  
  
- Referências aos assemblies System, System.Windows.Forms e System.Drawing.  
  
## <a name="see-also"></a>Consulte também

- [Como imprimir um arquivo de texto de várias páginas no Windows Forms](how-to-print-a-multi-page-text-file-in-windows-forms.md)
- [Suporte à impressão no Windows Forms](windows-forms-print-support.md)
- [Impressão mais segura no Windows Forms](../more-secure-printing-in-windows-forms.md)
