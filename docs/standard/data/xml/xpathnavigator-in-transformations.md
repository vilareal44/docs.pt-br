---
title: XPathNavigator nas transformações
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 118f97d1-7110-4d1b-b0bd-4143252c0bb0
ms.openlocfilehash: 59fb399d80e1d4d33d1a3c659d2ff74a37fd367d
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84282799"
---
# <a name="xpathnavigator-in-transformations"></a>XPathNavigator nas transformações
A classe de <xref:System.Xml.XPath.XPathNavigator> fornece de acesso aleatório somente leitura a dados e é criada para uso como uma entrada ao idioma extensível de folha de estilos para transformações (XSLT). É implementada em <xref:System.Xml.XPath.XPathDocument>, em <xref:System.Xml.XmlDataDocument>, e em <xref:System.Xml.XmlDocument>. <xref:System.Xml.XPath.XPathNavigator> é baseado no modelo de dados do World Wide Web Consortium (W3C) como descrito na seção 5 de recomendação de idioma do caminho de XML (XPath).  
  
 <xref:System.Xml.XPath.XPathNavigator> define um modelo de cursor sobre qualquer armazenamento e fornece consultas XPath rápidas, somente leitura sobre qualquer armazenamento de dados. <xref:System.Xml.XPath.XPathNavigator> também é a classe usar para iterar sobre partes da árvore de resultado.  
  
 API permite que você obtenha informações do nó atual no armazenamento e para mover a nós conectados. O <xref:System.Xml.XPath.XPathNavigator> é um modelo de estilo do cursor que executa a passagem sobre um armazenamento usando um conjunto de métodos **Mover**. <xref:System.Xml.XPath.XPathNavigator> sempre é posicionado em um nó. Qualquer método **Mover** que falhar as folhas <xref:System.Xml.XPath.XPathNavigator> inalterado.  
  
 <xref:System.Xml.XPath.XPathNavigator> é a classe usar para iterar sobre partes da árvore de resultado. O exemplo de código a seguir cria um fragmento da árvore de resultado em uma folha de estilos chamar a função com o parâmetro, `fragment`, que contém XML.  
  
## <a name="testxsl"></a>test.xsl  
  
```xml  
<xsl:stylesheet xmlns:xsl="http://www.w3.org/1999/XSL/Transform"  
                xmlns:msxsl ="urn:schemas-microsoft-com:xslt"  
                xmlns:user="http://www.adventure-works.com"  
                version="1.0">  
  
<xsl:variable name="fragment">  
    <authorlist>  
       <author>Joe</author>  
    </authorlist>  
</xsl:variable>  
  
<msxsl:script language="C#" implements-prefix="user">  
<![CDATA[  
   string NodeFragment(XPathNavigator nav)  
   {  
      if (nav.HasChildren)  
        return nav.Value;  
      else  
        return "";  
   }  
]]>  
</msxsl:script>  
  
<xsl:template match="/">  
     <xsl:value-of select="user:NodeFragment($fragment)"/>  
</xsl:template>  
  
</xsl:stylesheet>  
```  
  
## <a name="testxml"></a>test.xml  
  
```xml  
<root>Some text</root>  
```  
  
 O código a seguir usa os dados de folha de estilos de **test.xsl** e de entrada de **test.xml**.  
  
```vb  
Imports System  
Imports System.IO  
Imports System.Xml  
Imports System.Xml.Xsl  
Imports System.Xml.XPath  
Imports System.Text  
Public Class sample  
  
    Public Shared Sub Main()  
        Dim xslt As New XslTransform()  
        xslt.Load("test.xsl")  
  
        Dim xd As New XPathDocument("test.xml")  
  
        Dim strmTemp = New FileStream("out.xml", FileMode.Create, FileAccess.ReadWrite)  
        xslt.Transform(xd, Nothing, strmTemp, Nothing)  
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
        XslTransform xslt = new XslTransform();  
        xslt.Load("test.xsl");  
  
        XPathDocument xd = new XPathDocument("test.xml");  
  
        Stream strmTemp = new FileStream("out.xml", FileMode.Create, FileAccess.ReadWrite);  
        xslt.Transform(xd, null, strmTemp, null);  
    }  
}  
```  
  
## <a name="output"></a>Saída  
 O efeito da transformação for encontrado no arquivo **out.xml**:  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>Joe  
```  
  
## <a name="see-also"></a>Veja também

- [A classe XslTransform implementa do processador XSLT](xsltransform-class-implements-the-xslt-processor.md)
