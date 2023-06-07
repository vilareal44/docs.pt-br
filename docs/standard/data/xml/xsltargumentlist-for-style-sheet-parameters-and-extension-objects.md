---
title: XsltArgumentList para parâmetros de folha de estilos e objetos de extensão
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: de2f0dce-6b98-4908-bba7-ed150cc50355
ms.openlocfilehash: 1c69a6e78207e146c8dbd6cdc252f27f36ab37a2
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84281694"
---
# <a name="xsltargumentlist-for-style-sheet-parameters-and-extension-objects"></a>XsltArgumentList para parâmetros de folha de estilos e objetos de extensão
A classe de <xref:System.Xml.Xsl.XsltArgumentList> contém o idioma extensível de folha de estilos para objetos de parâmetros de transformações (XSLT) e a extensão XSLT. Quando passados para o método de <xref:System.Xml.Xsl.XslTransform.Transform%2A> , esses parâmetros e objetos de extensão podem ser chamados de folhas de estilos.  
  
> [!NOTE]
> As classes <xref:System.Xml.Xsl.XslTransform> e <xref:System.Xml.Xsl.XsltArgumentList> estão obsoletas no .NET Framework 2.0. Você pode realizar transformações XSLT usando a classe de <xref:System.Xml.Xsl.XslCompiledTransform> . Confira [Usar a classe XslCompiledTransform](using-the-xslcompiledtransform-class.md) e [Migrar da classe XslTransform](migrating-from-the-xsltransform-class.md) para saber mais.  
  
 A classe de <xref:System.Xml.Xsl.XsltArgumentList> contém objetos de parâmetros XSLT e de extensão XSLT. Quando passados para o método de <xref:System.Xml.Xsl.XslTransform.Transform%2A> , esses parâmetros e objetos de extensão podem ser chamados de folhas de estilos.  
  
 Os seguintes são vantagens de passar um objeto em vez de usar um script inserido:  
  
- Fornece a melhor encapsulamento e reutilização de classes.  
  
- Permite que as folhas de estilos são menores e mais sustentável.  
  
- Suporte que chamam métodos nas classes que pertencem aos espaços para nomes diferentes de aquelas definidas dentro do conjunto de namespaces suporte de <xref:System> .  
  
- Suporte que passam partes da árvore de resultado à folha de estilos com o uso de <xref:System.Xml.XPath.XPathNodeIterator>.  
  
## <a name="xslt-style-sheet-parameters"></a>Parâmetros de folha de estilos XSLT  
 Os parâmetros XSLT são adicionados a <xref:System.Xml.Xsl.XsltArgumentList> usando o método <xref:System.Xml.Xsl.XsltArgumentList.AddParam%2A> . Um nome qualificado e um namespace Uniform Resource Identifier (URI) são associados com o objeto de parâmetro no momento.  
  
 O objeto de parâmetro deve corresponder a um tipo de World Wide Web Consortium (W3C). A tabela seguinte mostra tipos correspondentes W3C, as classes equivalentes do .NET Framework (tipo), e se o tipo W3C é um tipo de XPath (linguagem de caminho XML) ou tipo de XSLT.  
  
|Tipo W3C|Classe equivalente do .NET Framework (tipo)|Tipo XPath ou XSLT|  
|--------------|----------------------------------------------|-----------------------------|  
|String|System.String|XPath|  
|Boolean|System.Boolean|XPath|  
|Número|System.Double|XPath|  
|Fragmento da árvore de resultado|System.Xml.XPath.XPathNavigator|XSLT|  
|Node Set|System.Xml.XPath.XPathNodeIterator|XPath|  
  
 Se o objeto de parâmetro não é uma das classes anterior, é forçado a um double ou para a cadeia de caracteres, como apropriado. Int16, UInt16, Int32, UInt32, UInt64, Int64, e escolha os tipos decimais são forçados para um double. Todos os outros tipos são forçados a uma cadeia de caracteres usando o método `ToString` .  
  
#### <a name="to-use-the-xslt-parameter-the-user-needs-to-do-the-following"></a>Para usar o parâmetro XSLT, o usuário precisará fazer o seguinte:  
  
1. Crie <xref:System.Xml.Xsl.XsltArgumentList> e adicione os objetos usando <xref:System.Xml.Xsl.XsltArgumentList.AddParam%2A>.  
  
2. Chame os parâmetros de folha de estilos.  
  
3. Passar <xref:System.Xml.Xsl.XsltArgumentList> para o método de <xref:System.Xml.Xsl.XslTransform.Transform%2A> .  
  
### <a name="example"></a>Exemplo  
 O exemplo a seguir usa o método de <xref:System.Xml.Xsl.XsltArgumentList.AddParam%2A> para criar um parâmetro para conter uma data calculada de desconto. A data de desconto é calculada para ser a 20 dias de data pedido.  
  
```vb  
Imports System  
Imports System.IO  
Imports System.Xml  
Imports System.Xml.XPath  
Imports System.Xml.Xsl  
  
Public class Sample  
  
   Private Const filename As String = "order.xml"  
   Private Const stylesheet As String = "discount.xsl"  
  
   Public Shared Sub Main()  
  
    'Create the XslTransform and load the style sheet.  
    Dim xslt As XslTransform = New XslTransform  
    xslt.Load(stylesheet)  
  
    'Load the XML data file.  
    Dim doc As XPathDocument = New XPathDocument(filename)  
  
    'Create an XsltArgumentList.  
    Dim xslArg As XsltArgumentList = New XsltArgumentList  
  
    'Calculate the discount date.  
    Dim today As DateTime = DateTime.Now  
    Dim d As DateTime = today.AddDays(20)  
    xslArg.AddParam("discount", "", d.ToString())  
  
    'Create an XmlTextWriter to handle the output.  
    Dim writer As XmlTextWriter = New XmlTextWriter("orderout.xml", Nothing)  
  
    'Transform the file.  
    xslt.Transform(doc, xslArg, writer, Nothing)  
  
    writer.Close()  
  
  End Sub  
End Class  
```  
  
```csharp  
using System;  
using System.IO;  
using System.Xml;  
using System.Xml.XPath;  
using System.Xml.Xsl;  
  
public class Sample  
{  
   private const String filename = "order.xml";  
   private const String stylesheet = "discount.xsl";  
  
   public static void Main() {  
  
    //Create the XslTransform and load the style sheet.  
    XslTransform xslt = new XslTransform();  
    xslt.Load(stylesheet);  
  
    //Load the XML data file.  
    XPathDocument doc = new XPathDocument(filename);  
  
    //Create an XsltArgumentList.  
    XsltArgumentList xslArg = new XsltArgumentList();  
  
    //Calculate the discount date.  
    DateTime today = DateTime.Now;  
    DateTime d = today.AddDays(20);  
    xslArg.AddParam("discount", "", d.ToString());  
  
    //Create an XmlTextWriter to handle the output.  
    XmlTextWriter writer = new XmlTextWriter("orderout.xml", null);  
  
    //Transform the file.  
    xslt.Transform(doc, xslArg, writer, null);  
    writer.Close();  
  }  
}  
```  
  
### <a name="input"></a>Entrada  
 order.xml  
  
```xml  
<!--Represents a customer order-->  
<order>  
  <book ISBN='10-861003-324'>  
    <title>The Handmaid's Tale</title>  
    <price>19.95</price>  
  </book>  
  <cd ISBN='2-3631-4'>  
    <title>Americana</title>  
    <price>16.95</price>  
  </cd>  
</order>  
```  
  
 discount.xsl  
  
```xml  
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">  
  <xsl:param name="discount"/>  
  <xsl:template match="/">  
    <order>  
      <xsl:variable name="sub-total" select="sum(//price)"/>  
      <total><xsl:value-of select="$sub-total"/></total>  
      15% discount if paid by: <xsl:value-of select="$discount"/>  
    </order>  
  </xsl:template>  
</xsl:stylesheet>  
```  
  
### <a name="output"></a>Saída  
  
```xml  
<order>  
   <total>36.9</total>
   15% discount if paid by: 5/6/2001 5:01:15 PM
</order>  
```  
  
## <a name="xslt-extension-objects"></a>Objetos de extensão XSLT  
 Os objetos de extensão XSLT são adicionados a <xref:System.Xml.Xsl.XsltArgumentList> usando o método <xref:System.Xml.Xsl.XsltArgumentList.AddExtensionObject%2A> . Um nome qualificado e URI de namespace são associados com o objeto de extensão no momento.  
  
 Quando um objeto é adicionado, o chamador de <xref:System.Xml.Xsl.XsltArgumentList.AddExtensionObject%2A> deve ser totalmente confiável na política de segurança. Se o chamador é de confiança parcial, a adição falhará.  
  
 Um objeto é adicionado embora com êxito, ele não garante que a execução será com êxito. Quando o método de <xref:System.Xml.Xsl.XslTransform.Transform%2A> é chamado, as permissões são calculadas com a evidência fornecida em tempo de <xref:System.Xml.Xsl.XslTransform.Load%2A> , e esse conjunto de permissões é atribuído ao processo inteiro de transformação. Se um objeto de extensão tentar iniciar uma ação que requer permissões não encontradas no dataset, uma exceção é lançada.  
  
 Os tipos de dados retornados de objetos de extensão é um dos quatro tipos de dados básicos XPath número, cadeia de caracteres, conjunto booleano, e o nó.  
  
#### <a name="to-use-the-xslt-extension-object-the-user-needs-to-do-the-following"></a>Para usar o objeto de extensão XSLT, o usuário precisará fazer o seguinte:  
  
1. Crie <xref:System.Xml.Xsl.XsltArgumentList> e adicione o objeto de extensão usando <xref:System.Xml.Xsl.XsltArgumentList.AddExtensionObject%2A>.  
  
2. Chamar o objeto de extensão folha de estilos.  
  
3. Passar <xref:System.Xml.Xsl.XsltArgumentList> para o método de <xref:System.Xml.Xsl.XslTransform.Transform%2A> .  
  
### <a name="example"></a>Exemplo  
 O exemplo a seguir calcula a circunferência de um círculo determinado o raio.  
  
```vb  
Imports System  
Imports System.IO  
Imports System.Xml  
Imports System.Xml.XPath  
Imports System.Xml.Xsl  
  
Public Class Sample  
   Private Const filename As String = "number.xml"  
   Private Const stylesheet As String = "circle.xsl"  
  
   Public Shared Sub Main()  
        Dim test As Sample = New Sample  
   End Sub  
  
  Public Sub New()  
    'Create the XslTransform and load the style sheet.  
    Dim xslt As XslTransform = New XslTransform  
    xslt.Load(stylesheet)  
  
    'Load the XML data file.  
    Dim doc As XPathDocument = New XPathDocument(filename)  
  
    'Create an XsltArgumentList.  
    Dim xslArg As XsltArgumentList = New XsltArgumentList  
  
    'Add an object to calculate the circumference of the circle.  
    Dim obj As Calculate = New Calculate  
    xslArg.AddExtensionObject("urn:myObj", obj)  
  
    'Create an XmlTextWriter to output to the console.  
    Dim writer As XmlTextWriter = New XmlTextWriter(Console.Out)  
  
    'Transform the file.  
    xslt.Transform(doc, xslArg, writer, Nothing)  
    writer.Close()  
  
  End Sub  
  
  'Calculates the circumference of a circle given the radius.  
  Public Class Calculate  
  
    Private circ As double = 0  
  
    Public Function Circumference(radius As Double) As Double  
       circ = Math.PI*2*radius  
       Return circ  
    End Function  
  End Class  
End Class  
```  
  
```csharp  
using System;  
using System.IO;  
using System.Xml;  
using System.Xml.XPath;  
using System.Xml.Xsl;  
  
public class Sample  
{  
   private const String filename = "number.xml";  
   private const String stylesheet = "circle.xsl";  
  
   public static void Main() {  
  
        Sample test = new Sample();  
    }  
  
  public Sample() {  
  
    //Create the XslTransform and load the style sheet.  
    XslTransform xslt = new XslTransform();  
    xslt.Load(stylesheet);  
  
    //Load the XML data file.  
    XPathDocument doc = new XPathDocument(filename);  
  
    //Create an XsltArgumentList.  
    XsltArgumentList xslArg = new XsltArgumentList();  
  
    //Add an object to calculate the circumference of the circle.  
    Calculate obj = new Calculate();  
    xslArg.AddExtensionObject("urn:myObj", obj);  
  
    //Create an XmlTextWriter to output to the console.  
    XmlTextWriter writer = new XmlTextWriter(Console.Out);  
  
    //Transform the file.  
    xslt.Transform(doc, xslArg, writer, null);  
    writer.Close();  
  
  }  
  
  //Calculates the circumference of a circle given the radius.  
  public class Calculate {  
  
    private double circ = 0;  
  
    public double Circumference(double radius){  
       circ = Math.PI*2*radius;  
       return circ;  
    }  
  }  
}  
```  
  
### <a name="input"></a>Entrada  
 number.xml  
  
```xml  
<?xml version='1.0'?>  
<data>  
  <circle>  
    <radius>12</radius>  
  </circle>  
  <circle>  
    <radius>37.5</radius>  
  </circle>  
</data>
```  
  
 circle.xsl  
  
```xml  
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform"  
    xmlns:myObj="urn:myObj">  
  
  <xsl:template match="data">  
  <circles>  
  <xsl:for-each select="circle">  
    <circle>  
    <xsl:copy-of select="node()"/>  
       <circumference>  
          <xsl:value-of select="myObj:Circumference(radius)"/>
       </circumference>  
    </circle>  
  </xsl:for-each>  
  </circles>  
  </xsl:template>  
</xsl:stylesheet>  
```  
  
### <a name="output"></a>Saída  
 `<circles xmlns:myObj="urn:myObj">`  
  
 `<circle>`  
  
 `<radius>12</radius>`  
  
 `<circumference>75.398223686155</circumference>`  
  
 `</circle>`  
  
 `<circle>`  
  
 `<radius>37.5</radius>`  
  
 `<circumference>235.61944901923448</circumference>`  
  
 `</circle>`  
  
 `</circles>`  
  
## <a name="see-also"></a>Veja também

- [A classe XslTransform implementa do processador XSLT](xsltransform-class-implements-the-xslt-processor.md)
