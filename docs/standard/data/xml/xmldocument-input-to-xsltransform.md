---
title: XmlDocument inseriu a XslTransform
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 97115892-410a-4657-ab47-1e14dfba73f8
ms.openlocfilehash: e990c8ca3bb2a7145157fcedd06da4ea769c6ad3
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84290247"
---
# <a name="xmldocument-input-to-xsltransform"></a>XmlDocument inseriu a XslTransform
A classe de <xref:System.Xml.XmlDocument> fornece recursos de edição de um documento XML. Se o XML precisa ser editado ou alterado antes de ser enviado para o método de <xref:System.Xml.Xsl.XslTransform.Transform%2A> , carregar XML em <xref:System.Xml.XmlDocument>, editá-lo, e enviá-lo na <xref:System.Xml.Xsl.XslTransform>.  
  
> [!NOTE]
> A classe <xref:System.Xml.Xsl.XslTransform> está obsoleta no .NET Framework 2.0. Você pode executar a linguagem XSL Transformations (XSLT) usando a classe <xref:System.Xml.Xsl.XslCompiledTransform>. Confira [Usar a classe XslCompiledTransform](using-the-xslcompiledtransform-class.md) e [Migrar da classe XslTransform](migrating-from-the-xsltransform-class.md) para saber mais.  
  
 <xref:System.Xml.XmlDocument> implementa a interface de <xref:System.Xml.XPath.IXPathNavigable> , o documento pode ser passado para o método de <xref:System.Xml.Xsl.XslTransform.Transform%2A> após editar.  
  
 Devido ao recurso de <xref:System.Xml.XmlDocument>, use a classe de <xref:System.Xml.XmlDocument> como entrada uma transformação é mais lento do que usar <xref:System.Xml.XPath.XPathDocument> para o idioma extensível de folha de estilos para transformações de transformações (XSLT), porque <xref:System.Xml.XPath.XPathDocument> é otimizado para consultas de idioma do caminho de XML (XPath) devido ao armazenamento interno.  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir mostra como <xref:System.Xml.XmlDocument> pode ser fornecido para <xref:System.Xml.Xsl.XslTransform>, com a saída enviadas a <xref:System.Xml.XmlReader>.  
  
```vb  
Dim doc as XmlDocument = new XmlDocument()  
doc.Load("books.xml")  
Dim trans As XslTransform = new XslTransform()  
trans.Load("book.xsl")  
Dim rdr As XmlReader = trans.Transform(doc, Nothing, Nothing)  
while (rdr.Read())  
end while  
```  
  
```csharp  
XmlDocument doc = new XmlDocument();  
doc.Load("books.xml");  
XslTransform trans = new XslTransform();  
trans.Load("book.xsl");  
XmlReader rdr = trans.Transform(doc, null, null);  
while (rdr.Read()) {}  
```  
  
## <a name="see-also"></a>Veja também

- <xref:System.Xml.XmlDocument>
- [Transformações XSLT com a classe XslTransform](xslt-transformations-with-the-xsltransform-class.md)
- [A classe XslTransform implementa do processador XSLT](xsltransform-class-implements-the-xslt-processor.md)
- [XPathNavigator nas transformações](xpathnavigator-in-transformations.md)
- [XPathNodeIterator nas transformações](xpathnodeiterator-in-transformations.md)
- [XPathDocument inseriu a XslTransform](xpathdocument-input-to-xsltransform.md)
- [XmlDataDocument inseriu a XslTransform](xmldatadocument-input-to-xsltransform.md)
