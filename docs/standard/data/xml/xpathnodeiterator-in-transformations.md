---
title: XPathNodeIterator nas transformações
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 2bc6ddc6-674a-4f75-b264-abc35e4e5857
ms.openlocfilehash: 88b8f4acbb9fa92d71659ee006ee544275353954
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84282747"
---
# <a name="xpathnodeiterator-in-transformations"></a>XPathNodeIterator nas transformações
<xref:System.Xml.XPath.XPathNodeIterator> fornece métodos para iterar sobre um conjunto de nós criados como resultado de uma consulta de idioma do caminho de XML (XPath) ou de um fragmento da árvore de resultado convertida em um nó definido por meio do método nó- definido. <xref:System.Xml.XPath.XPathNodeIterator> permite que você para iterar sobre os nós dentro desse conjunto de nó. Uma vez que um conjunto de nó é recuperado, a classe de <xref:System.Xml.XPath.XPathNodeIterator> fornece um cursor somente leitura, e somente para frente ao dataset selecionado de nós. O nó é criado na ordem de documento, o que move em chamar esse método para o nó seguir na ordem de documento. <xref:System.Xml.XPath.XPathNodeIterator> não cria uma árvore de nós de todos os nós no dataset. Em vez disso, fornece uma janela de único nó nos dados, expõe o nó subjacente que aponta para a medida que você se move ao redor de árvore. Os métodos e propriedades disponíveis de classe de <xref:System.Xml.XPath.XPathNodeIterator> permite que você obtenha informações do nó atual. Para obter uma lista de métodos e propriedades disponíveis, consulte <xref:System.Windows.Forms.ToolBar>.  
  
 Desde que <xref:System.Xml.XPath.XPathNodeIterator> se move sobre um conjunto de nós criados de uma consulta XPath e se avança apenas, a maneira para mover é usando o método <xref:System.Xml.XPath.XPathNodeIterator.MoveNext%2A> . O tipo de retorno desse método é `Boolean`, retornando `true` se move para o nó selecionado seguinte, e `false` se não há mais nó selecionado. Se retorna `true`, a lista a seguir mostra as propriedades disponíveis:  
  
- <xref:System.Xml.XPath.XPathNodeIterator.Current%2A>  
  
- <xref:System.Xml.XPath.XPathNodeIterator.CurrentPosition%2A>  
  
- <xref:System.Xml.XPath.XPathNodeIterator.Count%2A>  
  
 Quando você está examinando um nó definida pela primeira vez, uma chamada a <xref:System.Xml.XPath.XPathNodeIterator.MoveNext%2A> deve ser feito para posicionar <xref:System.Xml.XPath.XPathNodeIterator> no primeiro nó do dataset selecionado. Isso permite que um loop de quando é escrito.  
  
 O exemplo de código a seguir mostra como passar <xref:System.Xml.XPath.XPathNodeIterator> a <xref:System.Xml.Xsl.XslTransform> como um parâmetro em <xref:System.Xml.Xsl.XsltArgumentList>. A entrada ao código é **books.xml**, e a folha de estilos é **text.xsl**. O arquivo **test.xml** é o <xref:System.Xml.XPath.XPathDocument>.  
  
```vb  
Imports System  
Imports System.IO  
Imports System.Xml  
Imports System.Xml.Xsl  
Imports System.Xml.XPath  
Imports System.Text  
  
Public Class sample  
  
   Public Shared Sub Main()  
      Dim Doc As New XPathDocument("books.xml")  
      Dim nav As XPathNavigator = Doc.CreateNavigator()  
      Dim Iterator As XPathNodeIterator = nav.Select("/bookstore/book")  
  
      Dim arg As New XsltArgumentList()  
      arg.AddParam("param1", "", Iterator)  
  
      Dim xslt As New XslTransform()  
      xslt.Load("test.xsl")  
  
      Dim xd As New XPathDocument("test.xml")  
  
      Dim strmTemp = New FileStream("out.xml", FileMode.Create, FileAccess.ReadWrite)  
      xslt.Transform(xd, arg, strmTemp, Nothing)  
   End Sub 'Main  
End Class 'sample  
```  
  
```csharp  
using System;  
using System.IO;  
using System.Xml;  
using System.Xml.Xsl;  
using System.Xml.XPath;  
using System.Text;  
  
public class sample  
{  
    public static void Main()  
    {  
        XPathDocument Doc = new XPathDocument("books.xml");  
        XPathNavigator nav = Doc.CreateNavigator();  
        XPathNodeIterator Iterator = nav.Select("/bookstore/book");  
  
        XsltArgumentList arg = new XsltArgumentList();  
        arg.AddParam("param1", "", Iterator);  
  
        XslTransform xslt = new XslTransform();  
        xslt.Load("test.xsl");  
  
        XPathDocument xd = new XPathDocument("test.xml");  
  
        Stream strmTemp = new FileStream("out.xml", FileMode.Create, FileAccess.ReadWrite);  
        xslt.Transform(xd, arg, strmTemp, null);  
    }  
}  
```  
  
## <a name="booksxml"></a>books.xml  
  
```xml  
<?xml version='1.0'?>  
<!-- This file represents a fragment of a book store inventory database. -->  
<bookstore specialty="novel">  
    <book style="autobiography">  
    <title>Seven Years in Trenton</title>  
        <author>  
            <first-name>Jay</first-name>  
            <last-name>Adams</last-name>  
            <award>Trenton Literary Review Honorable Mention</award>  
            <country>USA</country>  
        </author>  
        <price>12</price>  
    </book>  
    <book style="textbook">  
        <title>History of Trenton</title>  
        <author>  
            <first-name>Kim</first-name>  
            <last-name>Akers</last-name>  
            <publication>  
                Selected Short Stories of  
                <first-name>Scott</first-name>  
                <last-name>Bishop</last-name>  
                <country>US</country>  
            </publication>  
        </author>  
        <price>55</price>  
    </book>  
</bookstore>  
```  
  
## <a name="testxsl"></a>test.xsl  
  
```xml  
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform"  
xmlns:msxsl="urn:schemas-microsoft-com:xslt" exclude-result-prefixes="msxsl">  
  
<xsl:output method="xml" indent="yes"/>  
<xsl:param name="param1"/>  
  
<xsl:template match="/">  
    <out>  
        <xsl:for-each select="$param1/title">  
            <title><xsl:value-of select="."/></title>  
        </xsl:for-each>  
    </out>  
</xsl:template>  
  
</xsl:stylesheet>  
```  
  
## <a name="testxml"></a>test.xml  
  
```xml  
<Title attr="Test">this is a test</Title>  
```  
  
## <a name="output-outxml"></a>Saída (out.xml)  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<out>  
  <title>Seven Years in Trenton</title>  
  <title>History of Trenton</title>  
</out>  
```  
  
## <a name="see-also"></a>Veja também

- [A classe XslTransform implementa do processador XSLT](xsltransform-class-implements-the-xslt-processor.md)
