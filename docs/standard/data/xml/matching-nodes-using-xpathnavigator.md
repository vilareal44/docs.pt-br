---
title: Nós compatíveis usando XPathNavigator
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: e6848c47-ee5d-401a-89a5-50b5eed40f30
ms.openlocfilehash: 47b0ba7e705ad602825dcca3f24c207362174a4c
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84289116"
---
# <a name="matching-nodes-using-xpathnavigator"></a>Nós compatíveis usando XPathNavigator
A classe de <xref:System.Xml.XPath.XPathNavigator> fornece o método de <xref:System.Xml.XPath.XPathNavigator.Matches%2A> para determinar se um nó corresponde uma expressão XPath. O método de <xref:System.Xml.XPath.XPathNavigator.Matches%2A> usa uma expressão XPath como entrada e retorna <xref:System.Boolean> que indica se o nó atual corresponde a expressão XPath determinada ou o objeto compilado dado de <xref:System.Xml.XPath.XPathExpression> .  
  
## <a name="matching-nodes"></a>Nós compatíveis  
 O método de <xref:System.Xml.XPath.XPathNavigator.Matches%2A> retorna `true` se o nó atual corresponde a expressão XPath especificada. Por exemplo, no exemplo de código que segue, o método de <xref:System.Xml.XPath.XPathNavigator.Matches%2A> retornará `true` se o nó atual é o elemento `b`, e o elemento `b` tem um atributo `c`.  
  
> [!NOTE]
> O método de <xref:System.Xml.XPath.XPathNavigator.Matches%2A> não altera o estado de <xref:System.Xml.XPath.XPathNavigator>.  
  
```vb  
Dim document as XPathDocument = New XPathDocument("input.xml")  
Dim navigator as XPathNavigator = document.CreateNavigator()  
  
navigator.Matches("b[@c]")  
```  
  
```csharp  
XPathDocument document = new XPathDocument("input.xml");  
XPathNavigator navigator = document.CreateNavigator();  
  
navigator.Matches("b[@c]");  
```  
  
## <a name="see-also"></a>Veja também

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [Processar dados XML usando o modelo de dados XPath](process-xml-data-using-the-xpath-data-model.md)
- [Selecionar dados XML usando XPathNavigator](select-xml-data-using-xpathnavigator.md)
- [Avalia as expressões XPath usando XPathNavigator](evaluate-xpath-expressions-using-xpathnavigator.md)
- [Tipos de nós reconhecidos com consultas XPath](node-types-recognized-with-xpath-queries.md)
- [Consultas XPath e namespaces](xpath-queries-and-namespaces.md)
- [Expressões XPath compilados](compiled-xpath-expressions.md)
