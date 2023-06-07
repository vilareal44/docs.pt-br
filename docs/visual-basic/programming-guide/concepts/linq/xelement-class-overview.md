---
title: Visão geral da classe XElement
ms.date: 07/20/2015
ms.assetid: 52331fcd-6023-4d19-b423-7b24f2d86ded
ms.openlocfilehash: a0e50c8a5a14150ee09a328f4dcdd5bc88363621
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84413213"
---
# <a name="xelement-class-overview-visual-basic"></a>Visão geral da classe XElement (Visual Basic)
A classe <xref:System.Xml.Linq.XElement> é uma das classes fundamentais no [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)]. Representa um elemento XML. Você pode usar essa classe para criar elementos; alterar o conteúdo do elemento; adicionar, alterar ou excluir elementos filho; adicionar atributos a um elemento; ou serializar o conteúdo de um elemento no formulário de texto. Você também pode interoperar com outras classes no <xref:System.Xml?displayProperty=nameWithType>, como <xref:System.Xml.XmlReader>, <xref:System.Xml.XmlWriter> e <xref:System.Xml.Xsl.XslCompiledTransform>.  
  
## <a name="xelement-functionality"></a>Funcionalidade de XElement  
 Este tópico descreve a funcionalidade fornecida pela classe <xref:System.Xml.Linq.XElement>.  
  
### <a name="constructing-xml-trees"></a>Construindo árvores XML  
 Você pode construir árvores XML em uma variedade de maneiras, incluindo:  
  
- Você pode construir uma árvore XML em código. Para obter mais informações, consulte [CREATING XML Trees (Visual Basic)](creating-xml-trees.md).  
  
- Você pode analisar XML de várias fontes, incluindo <xref:System.IO.TextReader>, arquivos de texto ou um endereço Web (URL). Para obter mais informações, consulte [analisando XML (Visual Basic)](parsing-xml.md).  
  
- Você pode usar um <xref:System.Xml.XmlReader> para popular a árvore. Para obter mais informações, consulte <xref:System.Xml.Linq.XNode.ReadFrom%2A>.  
  
- Se você tiver um módulo que possa gravar conteúdo em um <xref:System.Xml.XmlWriter>, poderá usar o método <xref:System.Xml.Linq.XContainer.CreateWriter%2A> para criar um gravador, passar o gravador para o módulo e usar o conteúdo que está gravado no <xref:System.Xml.XmlWriter> para popular a árvore XML.  
  
 No entanto, a maneira mais comum de criar uma árvore XML é a seguinte:  
  
```vb  
Dim contacts As XElement = _  
    <Contacts>  
        <Contact>  
            <Name>Patrick Hines</Name>  
            <Phone>206-555-0144</Phone>  
            <Address>  
                <Street1>123 Main St</Street1>  
                <City>Mercer Island</City>  
                <State>WA</State>  
                <Postal>68042</Postal>  
            </Address>  
        </Contact>  
    </Contacts>  
```  
  
 Outra técnica muito comum para criar uma árvore XML envolve o uso dos resultados de uma consulta LINQ para popular uma árvore XML, conforme mostrado no exemplo a seguir:  
  
```vb  
Dim srcTree As XElement = _  
    <Root>  
        <Element>1</Element>  
        <Element>2</Element>  
        <Element>3</Element>  
        <Element>4</Element>  
        <Element>5</Element>  
    </Root>  
Dim xmlTree As XElement = _  
    <Root>  
        <Child>1</Child>  
        <Child>2</Child>  
        <%= From el In srcTree.Elements() _  
            Where el.Value > 2 _  
            Select el %>  
    </Root>  
Console.WriteLine(xmlTree)  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<Root>  
  <Child>1</Child>  
  <Child>2</Child>  
  <Element>3</Element>  
  <Element>4</Element>  
  <Element>5</Element>  
</Root>  
```  
  
### <a name="serializing-xml-trees"></a>Serializando árvores XML  
 Você pode serializar a árvore XML para um <xref:System.IO.File>, um <xref:System.IO.TextWriter> ou um <xref:System.Xml.XmlWriter>.  
  
 Para obter mais informações, consulte [serializando árvores XML (Visual Basic)](serializing-xml-trees.md).  
  
### <a name="retrieving-xml-data-via-axis-methods"></a>Recuperando dados XML por meio de métodos de eixo  
 Você pode usar métodos de eixo para recuperar atributos, elementos filho, elementos descendentes e elementos ancestrais. As consultas LINQ operam em métodos de eixo e fornecem várias maneiras flexíveis e poderosas de navegar e processar uma árvore XML.  
  
 Para obter mais informações, consulte [eixos de LINQ to XML (Visual Basic)](linq-to-xml-axes.md).  
  
### <a name="querying-xml-trees"></a>Consultando árvores XML  
 Você pode escrever consultas LINQ que extraem dados de uma árvore XML.  
  
 Para obter mais informações, consulte [consultando árvores XML (Visual Basic)](querying-xml-trees.md).  
  
### <a name="modifying-xml-trees"></a>Modificando árvores XML  
 Você pode modificar um elemento de várias maneiras, incluindo alterar seu conteúdo ou atributos. Você também pode remover um elemento de seu pai.  
  
 Para obter mais informações, consulte [modificando árvores XML (LINQ to XML) (Visual Basic)](modifying-xml-trees-linq-to-xml.md).  
  
## <a name="see-also"></a>Confira também

- [Visão geral da programação de LINQ to XML (Visual Basic)](linq-to-xml-programming-overview.md)
