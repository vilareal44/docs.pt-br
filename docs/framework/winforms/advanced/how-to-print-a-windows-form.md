---
title: Como imprimir um formulário do Windows Forms
description: Saiba como imprimir programaticamente uma cópia do Windows Form atual usando o método CopyFromScreen.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Windows Forms, printing
- printing [Windows Forms]
- printing a form
- printing [Windows Forms], printing a form
ms.assetid: c8dff5f8-f56a-4c07-ae31-64643b31f8fc
ms.openlocfilehash: b59ea4b5347903b36a166c4f8ac0d8d7db18635e
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174686"
---
# <a name="how-to-print-a-windows-form"></a>Como imprimir um formulário do Windows Forms
Como parte do processo de desenvolvimento, você normalmente desejará imprimir uma cópia do seu Windows Form. O exemplo de código a seguir mostra como imprimir uma cópia do formulário atual usando o <xref:System.Drawing.Graphics.CopyFromScreen%2A> método.  
  
## <a name="example"></a>Exemplo  
 [!code-csharp[System.Drawing.Graphics.CopyFromScreen#1](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.Graphics.CopyFromScreen/CS/Form1.cs#1)]
 [!code-vb[System.Drawing.Graphics.CopyFromScreen#1](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.Graphics.CopyFromScreen/VB/Form1.vb#1)]  
  
## <a name="robust-programming"></a>Programação robusta  
 As seguintes condições podem causar uma exceção:  
  
- Você não tem permissão para acessar a impressora.  
  
- Não há impressora instalada.  
  
## <a name="net-framework-security"></a>Segurança do .NET Framework  
 Para executar este exemplo de código, você deve ter permissão para acessar a impressora que você usa com seu computador.  
  
## <a name="see-also"></a>Veja também

- <xref:System.Drawing.Printing.PrintDocument>
- [Como renderizar imagens com o GDI+](how-to-render-images-with-gdi.md)
- [Como imprimir elementos gráficos no Windows Forms](how-to-print-graphics-in-windows-forms.md)
