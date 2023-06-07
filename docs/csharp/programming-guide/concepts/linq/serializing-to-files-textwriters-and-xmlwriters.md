---
title: Serializando arquivos, o TextWriters, e o XmlWriters
description: Saiba mais sobre as opções para serializar árvores XML para um arquivo, um TextWriter ou um XmlWriter em C# usando os métodos ToString ou Save.
ms.date: 07/20/2015
ms.assetid: bd3ea6f7-895b-4ff4-a625-fe2bb55b1886
ms.openlocfilehash: 43c51ae7e9bf1a7848d45fd900424d6186671e53
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87302380"
---
# <a name="serializing-to-files-textwriters-and-xmlwriters"></a>Serializando arquivos, o TextWriters, e o XmlWriters

Você pode serializar árvores XML a <xref:System.IO.File>, a <xref:System.IO.TextWriter>, ou a <xref:System.Xml.XmlWriter>.

Você pode serializar qualquer componente XML, incluindo <xref:System.Xml.Linq.XDocument> e <xref:System.Xml.Linq.XElement>, uma cadeia de caracteres usando o método `ToString` .

Se você desejar suprimir formatação ao serializar a uma cadeia de caracteres, você pode usar o método de <xref:System.Xml.Linq.XNode.ToString%2A?displayProperty=nameWithType> .

O comportamento padrão ao serializar para um arquivo é formatar (recuar) o documento XML resultante. Quando você identar, o espaço em branco irrisória na árvore XML não é preservada. Para serializar com formatação, use uma das sobrecargas dos seguintes métodos que não têm <xref:System.Xml.Linq.SaveOptions> como um argumento:

- <xref:System.Xml.Linq.XDocument.Save%2A?displayProperty=nameWithType>

- <xref:System.Xml.Linq.XElement.Save%2A?displayProperty=nameWithType>

Se você desejar que a opção não recuar e não preservar espaço em branco irrisória na árvore XML, use uma das sobrecargas dos seguintes métodos que leva <xref:System.Xml.Linq.SaveOptions> como um argumento:

- <xref:System.Xml.Linq.XDocument.Save%2A?displayProperty=nameWithType>

- <xref:System.Xml.Linq.XElement.Save%2A?displayProperty=nameWithType>

Para exemplos, consulte o tópico de referência apropriado.

## <a name="see-also"></a>Veja também

- [Serializando árvores XML (C#)](serializing-to-files-textwriters-and-xmlwriters.md)
