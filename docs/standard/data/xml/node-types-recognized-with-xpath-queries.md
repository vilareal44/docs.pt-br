---
title: Tipos de nós reconhecidos com consultas XPath
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 1d33e22d-18e5-43f8-a466-2e3d0a8dd094
ms.openlocfilehash: b9fc55b11455491406970af2a9232b277160875f
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84288726"
---
# <a name="node-types-recognized-with-xpath-queries"></a>Tipos de nós reconhecidos com consultas XPath
Os tipos de nós reconhecidas em uma consulta XPath não são os mesmos tipos de nós localizados em Document Object Model (DOM).  
  
## <a name="w3c-xpath-node-types"></a>Tipos de nós XPath W3C  
 Os tipos de nós reconhecidas em uma consulta XPath não são tipos de nós localizados em Document Object Model (DOM). A seguir estão os tipos de nós XPath representados pela enumeração de <xref:System.Xml.XPath.XPathNodeType> .  
  
- <xref:System.Xml.XPath.XPathNodeType.All>  
  
- <xref:System.Xml.XPath.XPathNodeType.Attribute>  
  
- <xref:System.Xml.XPath.XPathNodeType.Comment>  
  
- <xref:System.Xml.XPath.XPathNodeType.Element>  
  
- <xref:System.Xml.XPath.XPathNodeType.Namespace>  
  
- <xref:System.Xml.XPath.XPathNodeType.ProcessingInstruction>  
  
- <xref:System.Xml.XPath.XPathNodeType.Root>  
  
- <xref:System.Xml.XPath.XPathNodeType.SignificantWhitespace>  
  
- <xref:System.Xml.XPath.XPathNodeType.Text>  
  
- <xref:System.Xml.XPath.XPathNodeType.Whitespace>  
  
 Esses tipos de nós são baseados no modelo de dados XPath, onde os nós são derivados do conjunto de informações XML. Os tipos de nós de <xref:System.Xml.XPath.XPathNodeType.SignificantWhitespace> e de <xref:System.Xml.XPath.XPathNodeType.Whitespace> são extensões do Microsoft .NET Framework para tipos de nós de base descritos no modelo de dados XPath.  
  
 O tipo de nó de atributo é usado de forma diferente no modelo de dados XPath do que está em DOM. No modelo de dados XPath, o nó do elemento tem um conjunto de nós de atributo relacionados a ele e o nó do elemento é o pai de cada nó de atributo. No entanto, em DOM, o nó do elemento é o proprietário e não o pai. Em ambos os modelos, o atributo e os nós de namespace não são considerados nós filho do nó do elemento.  
  
 O tipo de nó de namespace é uma adição ao modelo de dados XPath e não é um tipo de nó reconhecido DOM.  
  
 Para saber mais sobre o elemento de navegação, o atributo e nós de namespace, confira os tópicos [Navegação do nó usando XPathNavigator](node-set-navigation-using-xpathnavigator.md) e [Navegação do nó de atributo e do namespace usando XPathNavigator](attribute-and-namespace-node-navigation-using-xpathnavigator.md).  
  
## <a name="see-also"></a>Veja também

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [Processar dados XML usando o modelo de dados XPath](process-xml-data-using-the-xpath-data-model.md)
- [Selecionar dados XML usando XPathNavigator](select-xml-data-using-xpathnavigator.md)
- [Avalia as expressões XPath usando XPathNavigator](evaluate-xpath-expressions-using-xpathnavigator.md)
- [Nós compatíveis usando XPathNavigator](matching-nodes-using-xpathnavigator.md)
- [Consultas XPath e namespaces](xpath-queries-and-namespaces.md)
- [Expressões XPath compilados](compiled-xpath-expressions.md)
