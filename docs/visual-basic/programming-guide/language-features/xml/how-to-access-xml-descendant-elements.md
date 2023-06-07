---
title: Como acessar elementos descendentes XML
ms.date: 07/20/2015
helpviewer_keywords:
- XML descendent axis property [Visual Basic]
- XML axis [Visual Basic], descendent
- descendent axis property [Visual Basic]
- XML [Visual Basic], accessing
ms.assetid: aabfa258-4112-4e7e-bab9-403f96072ef7
ms.openlocfilehash: 03c403aa3c187b0b9d2006104eccaa1f9cd8aec5
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84392630"
---
# <a name="how-to-access-xml-descendant-elements-visual-basic"></a>Como acessar elementos descendentes XML (Visual Basic)
Este exemplo mostra como usar uma propriedade de eixo descendente para acessar todos os elementos XML que têm um nome especificado e que estão contidos em um elemento XML. Em particular, ele usa a `Value` propriedade para obter o valor do primeiro elemento na coleção que a propriedade do `name` eixo descendente retorna. A `name` Propriedade do eixo descendente Obtém todos os elementos chamados `name` que estão contidos no `contacts` objeto. Este exemplo também usa a `phone` Propriedade Axis descendente para acessar todos os descendentes nomeados `phone` contidos no `contacts` objeto.  
  
## <a name="example"></a>Exemplo  
 [!code-vb[VbXMLSamples#31](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples13.vb#31)]  
  
## <a name="compile-the-code"></a>Compilar o código  
 Este exemplo requer:  
  
- Uma referência ao <xref:System.Xml.Linq> namespace.  
  
## <a name="see-also"></a>Confira também

- <xref:System.Xml.Linq.XContainer.Descendants%2A?displayProperty=nameWithType>
- [Propriedade de Eixo Descendente XML](../../../language-reference/xml-axis/xml-descendant-axis-property.md)
- [Propriedade do Valor XML](../../../language-reference/xml-axis/xml-value-property.md)
- [Acessando XML no Visual Basic](accessing-xml.md)
- [XML](index.md)
