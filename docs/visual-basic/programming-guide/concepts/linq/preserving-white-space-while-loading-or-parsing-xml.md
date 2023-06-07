---
title: Preservando o espaço em branco ao carregar ou analisar XML2
ms.date: 07/20/2015
ms.assetid: ef6518e0-2c8d-462c-8b92-a16e9dc737ad
ms.openlocfilehash: 9c60c707730ed0b07e82040a4ce3aab5d83eef1c
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396435"
---
# <a name="preserving-white-space-while-loading-or-parsing-xml"></a>Preservar espaço em branco para carregar ou ao analisar XML
Este tópico descreve como controlar o comportamento de espaço em branco de [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)].  
  
 Um cenário comum é ler o XML recuado, criar uma árvore XML na memória sem nenhum nó de texto de espaço em branco (isto é, não preservar espaço em branco), executar algumas operações no XML e, em seguida, salvar o XML com recuo. Quando você serializa o XML com formatação, somente os espaços em branco significativos na árvore XML são preservados. Este é o comportamento padrão para [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)].  
  
 Outro cenário comum é ler e modificar XML que já foi recuado intencionalmente. Você pode não querer modificar este recuo de nenhuma forma. Para fazer isso em [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)], você preserva o espaço em branco quando você carregar ou analisar XML e desativa o formatação quando você serializa XML.  
  
 Este tópico descreve o comportamento de espaço em branco dos métodos que preenchem árvores XML. Para obter informações sobre o controle de espaço em branco ao serializar árvores XML, consulte [Preservar espaço em branco ao serializar](preserving-white-space-while-serializing.md).  
  
## <a name="behavior-of-methods-that-populate-xml-trees"></a>Comportamento dos métodos que preenchem árvores XML  
 Os seguintes métodos nas classes de <xref:System.Xml.Linq.XElement> e de <xref:System.Xml.Linq.XDocument> preenchem uma árvore XML. Você pode preencher uma árvore XML de um arquivo, um <xref:System.IO.TextReader>, um <xref:System.Xml.XmlReader>, ou de uma cadeia de caracteres:  
  
- <xref:System.Xml.Linq.XElement.Load%2A?displayProperty=nameWithType>  
  
- <xref:System.Xml.Linq.XElement.Parse%2A?displayProperty=nameWithType>  
  
- <xref:System.Xml.Linq.XDocument.Load%2A?displayProperty=nameWithType>  
  
- <xref:System.Xml.Linq.XDocument.Parse%2A?displayProperty=nameWithType>  
  
 Se o método não utiliza <xref:System.Xml.Linq.LoadOptions> como um argumento, o método não irá preservar espaço em branco irrisória.  
  
 Na maioria dos casos, se o método utiliza <xref:System.Xml.Linq.LoadOptions> como um argumento, você pode opcionalmente preservar espaço em branco irrisória como nós de texto na árvore XML. Entretanto, se o método é carregar XML de <xref:System.Xml.XmlReader>, então <xref:System.Xml.XmlReader> determina se o espaço em branco será preservada ou não. A configuração <xref:System.Xml.Linq.LoadOptions.PreserveWhitespace> não terá efeito.  
  
 Com esses métodos, se o espaço em branco é preservada, o espaço em branco irrisória é inserido na árvore XML como nós de <xref:System.Xml.Linq.XText> . Se o espaço em branco não é preservada, os nós de texto não são inseridos.  
  
 Você pode criar uma árvore XML usando <xref:System.Xml.XmlWriter>. Os nós que são gravados em <xref:System.Xml.XmlWriter> são preenchidos na árvore. No entanto, quando você cria uma árvore XML usando esse método, todos os nós são mantidos, independentemente se o nó é o espaço em branco ou não, ou se o espaço em branco é significativo ou não.  
  
## <a name="see-also"></a>Confira também

- [Analisando XML (Visual Basic)](parsing-xml.md)
