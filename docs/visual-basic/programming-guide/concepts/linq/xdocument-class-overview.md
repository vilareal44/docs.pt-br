---
title: Visão geral da classe XDocument
ms.date: 07/20/2015
ms.assetid: 45cb7e71-196a-47da-bfe9-7a5589db1eed
ms.openlocfilehash: 90c97349006407b2eca861be31080ec17901fa5b
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84413226"
---
# <a name="xdocument-class-overview-visual-basic"></a>Visão geral da classe XDocument (Visual Basic)
Este tópico apresenta a classe <xref:System.Xml.Linq.XDocument>.  
  
## <a name="overview-of-the-xdocument-class"></a>Visão geral da classe XDocument  
 A classe <xref:System.Xml.Linq.XDocument> contém as informações necessárias para um documento XML válido. Isso inclui uma declaração XML, instruções de processamento e comentários.  
  
 Observe que você só precisará criar objetos <xref:System.Xml.Linq.XDocument> se precisar da funcionalidade específica fornecida pela classe <xref:System.Xml.Linq.XDocument>. Em muitas circunstâncias, você pode trabalhar diretamente com <xref:System.Xml.Linq.XElement>. Trabalhar diretamente com <xref:System.Xml.Linq.XElement> é um modelo de programação mais simples.  
  
 <xref:System.Xml.Linq.XDocument> deriva de <xref:System.Xml.Linq.XContainer>. Portanto, pode conter nós filho. Entretanto, os objetos <xref:System.Xml.Linq.XDocument> podem ter apenas um nó <xref:System.Xml.Linq.XElement> filho. Isso reflete o padrão XML de que pode haver apenas um elemento raiz em um documento XML.  
  
## <a name="components-of-xdocument"></a>Componentes de XDocument  
 Um <xref:System.Xml.Linq.XDocument> pode conter os seguintes elementos:  
  
- Um objeto <xref:System.Xml.Linq.XDeclaration>. <xref:System.Xml.Linq.XDeclaration> permite que você especifique as partes pertinentes de uma declaração XML: a versão XML, a codificação do documento e se o documento XML é autônomo.  
  
- Um objeto <xref:System.Xml.Linq.XElement>. Esse é o nó raiz do documento XML.  
  
- Qualquer número de objetos <xref:System.Xml.Linq.XProcessingInstruction>. Uma instrução de processamento comunica informações a um aplicativo que processa o XML.  
  
- Qualquer número de objetos <xref:System.Xml.Linq.XComment>. Os comentários serão irmãos do elemento raiz. O objeto <xref:System.Xml.Linq.XComment> não pode ser o primeiro argumento da lista, pois não é válido um documento XML iniciar com um comentário.  
  
- Um <xref:System.Xml.Linq.XDocumentType> para o DTD.  
  
 Quando você serializa um <xref:System.Xml.Linq.XDocument>, mesmo que `XDocument.Declaration` seja `null`, a saída terá uma declaração XML se o gravador tiver `Writer.Settings.OmitXmlDeclaration` definido como `false` (o padrão).  
  
 Por padrão, o [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] define a versão como “1.0 " e a codificação como “utf-8”.  
  
## <a name="using-xelement-without-xdocument"></a>Usando XElement sem XDocument  
 Conforme mencionado anteriormente, a classe <xref:System.Xml.Linq.XElement> é a classe principal da interface de programação [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)]. Em muitos casos, seu aplicativo não exigirá que você crie um documento. Usando a classe <xref:System.Xml.Linq.XElement>, você poderá criar uma árvore XML, adicionar outras árvores XML a ela, modificar a árvore XML e salvá-la.  
  
## <a name="using-xdocument"></a>Usando XDocument  
 Para construir um <xref:System.Xml.Linq.XDocument>, use a construção funcional exatamente como você faz para criar objetos <xref:System.Xml.Linq.XElement>.  
  
 O código a seguir cria um objeto <xref:System.Xml.Linq.XDocument> e seus objetos independentes associados.  
  
```vb  
Dim doc As XDocument = <?xml version="1.0" encoding="utf-8"?>  
                       <!--This is a comment.-->  
                       <?xml-stylesheet href='mystyle.css' title='Compact' type='text/css'?>  
                       <Pubs>  
                           <Book>  
                               <Title>Artifacts of Roman Civilization</Title>  
                               <Author>Moreno, Jordao</Author>  
                           </Book>  
                           <Book>  
                               <Title>Midieval Tools and Implements</Title>  
                               <Author>Gazit, Inbar</Author>  
                           </Book>  
                       </Pubs>  
                       <!--This is another comment.-->  
doc.Save("test.xml")  
```  
  
 Quando você examina o arquivo test.xml, obtém a seguinte saída:  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<!--This is a comment.-->  
<?xml-stylesheet href='mystyle.css' title='Compact' type='text/css'?>  
<Pubs>  
  <Book>  
    <Title>Artifacts of Roman Civilization</Title>  
    <Author>Moreno, Jordao</Author>  
  </Book>  
  <Book>  
    <Title>Midieval Tools and Implements</Title>  
    <Author>Gazit, Inbar</Author>  
  </Book>  
</Pubs>  
<!--This is another comment.-->  
```  
  
## <a name="see-also"></a>Confira também

- [Visão geral da programação de LINQ to XML (Visual Basic)](linq-to-xml-programming-overview.md)
