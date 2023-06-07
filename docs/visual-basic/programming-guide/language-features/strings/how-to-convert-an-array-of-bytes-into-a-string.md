---
title: Como converter uma matriz de bytes em uma cadeia de caracteres
ms.date: 07/20/2015
helpviewer_keywords:
- string conversion [Visual Basic], arrays
- byte arrays [Visual Basic], converting to strings
- examples [Visual Basic], strings
- arrays [Visual Basic], converting to strings
ms.assetid: d0dc8317-9ab3-4324-99f7-3f5788c0e72a
ms.openlocfilehash: 6dbbaafedeca4d2cea625a300d764f61bb575750
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410613"
---
# <a name="how-to-convert-an-array-of-bytes-into-a-string-in-visual-basic"></a>Como converter uma matriz de bytes em uma cadeia de caracteres no Visual Basic
Este tópico mostra como converter os bytes de uma matriz de bytes em uma cadeia de caracteres.  
  
## <a name="example"></a>Exemplo  
 Este exemplo usa o <xref:System.Text.Encoding.GetString%2A> método da <xref:System.Text.Encoding.Unicode%2A?displayProperty=nameWithType> classe Encoding para converter todos os bytes de uma matriz de bytes em uma cadeia de caracteres.  
  
 [!code-vb[VbVbalrStrings#72](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#72)]  
  
 Você pode escolher entre várias opções de codificação para converter uma matriz de bytes em uma cadeia de caracteres:  
  
- <xref:System.Text.Encoding.ASCII%2A?displayProperty=nameWithType>: Obtém uma codificação para o conjunto de caracteres ASCII (7 bits).  
  
- <xref:System.Text.Encoding.BigEndianUnicode%2A?displayProperty=nameWithType>: Obtém uma codificação para o formato UTF-16 usando a ordem de bytes big-endian.  
  
- <xref:System.Text.Encoding.Default%2A?displayProperty=nameWithType>: Obtém uma codificação para a página de código ANSI atual do sistema.  
  
- <xref:System.Text.Encoding.Unicode%2A?displayProperty=nameWithType>: Obtém uma codificação para o formato UTF-16 usando a ordem de byte little-endian.  
  
- <xref:System.Text.Encoding.UTF32%2A?displayProperty=nameWithType>: Obtém uma codificação para o formato UTF-32 usando a ordem de byte little-endian.  
  
- <xref:System.Text.Encoding.UTF7%2A?displayProperty=nameWithType>: Obtém uma codificação para o formato UTF-7.  
  
- <xref:System.Text.Encoding.UTF8%2A?displayProperty=nameWithType>: Obtém uma codificação para o formato UTF-8.  
  
## <a name="see-also"></a>Confira também

- <xref:System.Text.Encoding?displayProperty=nameWithType>
- <xref:System.Text.Encoding.GetString%2A>
- [Como converter cadeias de caracteres em uma matriz de bytes no Visual Basic](how-to-convert-strings-into-an-array-of-bytes.md)
