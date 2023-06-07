---
title: Visão geral da classe XElement (C#)
description: A classe XElement representa um elemento XML em C#. É uma das classes fundamentais em LINQ to XML. Saiba mais sobre a funcionalidade fornecida pelo XElement.
ms.date: 07/20/2015
ms.assetid: 2b9f0cd8-a1d1-4037-accf-0f38a410fa11
ms.openlocfilehash: f76f51703de054443f47531294777b43a9c0b004
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87302172"
---
# <a name="xelement-class-overview-c"></a>Visão geral da classe XElement (C#)
A classe <xref:System.Xml.Linq.XElement> é uma das classes fundamentais no [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)]. Representa um elemento XML. Você pode usar essa classe para criar elementos; alterar o conteúdo do elemento; adicionar, alterar ou excluir elementos filho; adicionar atributos a um elemento; ou serializar o conteúdo de um elemento no formulário de texto. Você também pode interoperar com outras classes no <xref:System.Xml?displayProperty=nameWithType>, como <xref:System.Xml.XmlReader>, <xref:System.Xml.XmlWriter> e <xref:System.Xml.Xsl.XslCompiledTransform>.  
  
Este tópico descreve a funcionalidade fornecida pela classe <xref:System.Xml.Linq.XElement>.  
  
## <a name="constructing-xml-trees"></a>Construindo árvores XML  
 Você pode construir árvores XML em uma variedade de maneiras, incluindo:  
  
- Você pode construir uma árvore XML em código. Para obter mais informações, consulte [Criando árvores XML (C#)](./linq-to-xml-overview.md).  
  
- Você pode analisar XML de várias fontes, incluindo <xref:System.IO.TextReader>, arquivos de texto ou um endereço Web (URL). Para obter mais informações, consulte [Analisando XML (C#)](./how-to-parse-a-string.md).  
  
- Você pode usar um <xref:System.Xml.XmlReader> para popular a árvore. Para obter mais informações, consulte <xref:System.Xml.Linq.XNode.ReadFrom%2A>.  
  
- Se você tiver um módulo que possa gravar conteúdo em um <xref:System.Xml.XmlWriter>, poderá usar o método <xref:System.Xml.Linq.XContainer.CreateWriter%2A> para criar um gravador, passar o gravador para o módulo e usar o conteúdo que está gravado no <xref:System.Xml.XmlWriter> para popular a árvore XML.  
  
 No entanto, a maneira mais comum de criar uma árvore XML é a seguinte:  
  
```csharp  
XElement contacts =  
    new XElement("Contacts",  
        new XElement("Contact",  
            new XElement("Name", "Patrick Hines"),
            new XElement("Phone", "206-555-0144"),  
            new XElement("Address",  
                new XElement("Street1", "123 Main St"),  
                new XElement("City", "Mercer Island"),  
                new XElement("State", "WA"),  
                new XElement("Postal", "68042")  
            )  
        )  
    );  
```  
  
 Outra técnica muito comum para criar uma árvore XML envolve o uso dos resultados de uma consulta LINQ para popular uma árvore XML, conforme mostrado no exemplo a seguir:  
  
```csharp  
XElement srcTree = new XElement("Root",  
    new XElement("Element", 1),  
    new XElement("Element", 2),  
    new XElement("Element", 3),  
    new XElement("Element", 4),  
    new XElement("Element", 5)  
);  
XElement xmlTree = new XElement("Root",  
    new XElement("Child", 1),  
    new XElement("Child", 2),  
    from el in srcTree.Elements()  
    where (int)el > 2  
    select el  
);  
Console.WriteLine(xmlTree);  
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
  
## <a name="serializing-xml-trees"></a>Serializando árvores XML  
 Você pode serializar a árvore XML para um <xref:System.IO.File>, um <xref:System.IO.TextWriter> ou um <xref:System.Xml.XmlWriter>.  
  
 Para obter mais informações, consulte [Serializando árvores XML (C#)](./preserving-white-space-while-serializing.md).  
  
## <a name="retrieving-xml-data-via-axis-methods"></a>Recuperando dados XML por meio de métodos de eixo  
 Você pode usar métodos de eixo para recuperar atributos, elementos filho, elementos descendentes e elementos ancestrais. As consultas LINQ operam em métodos de eixo e fornecem várias maneiras flexíveis e poderosas de navegar e processar uma árvore XML.  
  
 Para obter mais informações, consulte [Eixos LINQ to XML (C#)](./linq-to-xml-axes-overview.md).  
  
## <a name="querying-xml-trees"></a>Consultando árvores XML  
 Você pode escrever consultas LINQ que extraem dados de uma árvore XML.  
  
 Para obter mais informações, consulte [Consultando árvores XML (C#)](./how-to-find-an-element-with-a-specific-attribute.md).  
  
## <a name="modifying-xml-trees"></a>Modificando árvores XML  
 Você pode modificar um elemento de várias maneiras, incluindo alterar seu conteúdo ou atributos. Você também pode remover um elemento de seu pai.  
  
 Para obter mais informações, consulte [Modificando árvores XML (LINQ to XML) (C#)](./in-memory-xml-tree-modification-vs-functional-construction-linq-to-xml.md).  
  
## <a name="see-also"></a>Veja também

- [Visão geral da programação LINQ to XML (C#)](serializing-to-files-textwriters-and-xmlwriters.md)
