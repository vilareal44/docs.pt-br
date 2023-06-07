---
title: Salvando e escrevendo um documento
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 097b0cb1-5743-4c3a-86ef-caf5cbe6750d
ms.openlocfilehash: 40d031c06f0b76668a634fac46b8defccce62f01
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84289038"
---
# <a name="saving-and-writing-a-document"></a>Salvando e escrevendo um documento
Quando você carrega e salvar <xref:System.Xml.XmlDocument>, o documento salvo pode diferir do original das seguintes maneiras:  
  
- Se a propriedade <xref:System.Xml.XmlDocument.PreserveWhitespace%2A> for definida como `true` antes que o método <xref:System.Xml.XmlDocument.Save%2A> seja chamado, o espaço em branco no documento será preservado na saída; se essa propriedade for `false`, o <xref:System.Xml.XmlDocument> recua automaticamente a saída.  
  
- Todos os espaços em branco entre atributos são reduzidos a um único caractere de espaço.  
  
- O espaço em branco entre elementos é alterado. O espaço em branco significativo é preservado e o espaço em branco insignificante não é. Mas quando o documento é salvo, ele usará o modo <xref:System.Xml.XmlTextWriter> **Recuo** por padrão para imprimir ordenadamente a saída e torná-la mais legível.  
  
- O caractere de aspas usado ao redor de valores de atributo é alterado para aspas duplas por padrão. Você pode usar a propriedade <xref:System.Xml.XmlTextReader.QuoteChar%2A> em <xref:System.Xml.XmlTextWriter> para definir o caractere de aspas para aspas duplas ou aspas simples.  
  
- Por padrão, as entidades de caracteres numéricos como `{` são expandidas.  
  
- A marca de ordem de byte encontrada no documento de entrada não é preservada. O UCS-2 é salvo como UTF-8 a menos que você crie explicitamente uma declaração XML que especifica uma codificação diferente.  
  
- Se você quiser escrever o <xref:System.Xml.XmlDocument> em um arquivo ou um fluxo, a saída escreve o mesmo que o conteúdo do documento. Isto é, o <xref:System.Xml.XmlDeclaration> é escrito somente se houver um contido no documento e a codificação usada ao escrever o documento é a mesma fornecida no nó de declaração.  
  
## <a name="writing-an-xmldeclaration"></a>Escrevendo um XmlDeclaration  
 Os membros <xref:System.Xml.XmlDocument> e <xref:System.Xml.XmlDeclaration> de <xref:System.Xml.XmlNode.OuterXml%2A>, <xref:System.Xml.XmlNode.InnerXml%2A> e <xref:System.Xml.XmlNode.WriteTo%2A>, além dos métodos <xref:System.Xml.XmlDocument> de <xref:System.Xml.XmlDocument.Save%2A> e de <xref:System.Xml.XmlDocument.WriteContentTo%2A>, criam uma declaração XML.  
  
 Para as propriedades <xref:System.Xml.XmlDocument> de <xref:System.Xml.XmlNode.OuterXml%2A>, <xref:System.Xml.XmlDocument.InnerXml%2A> e <xref:System.Xml.XmlDocument.Save%2A>, <xref:System.Xml.XmlDocument.WriteTo%2A>, e os métodos <xref:System.Xml.XmlDocument.WriteContentTo%2A>, a codificação escrita na declaração XML é obtida do nó <xref:System.Xml.XmlDeclaration>. Se não houver nenhum <xref:System.Xml.XmlDeclaration> nó, o <xref:System.Xml.XmlDeclaration> não será gravado. Se não houver nenhuma codificação no <xref:System.Xml.XmlDeclaration> nó, a codificação não será gravada na declaração XML.  
  
 Os métodos <xref:System.Xml.XmlDocument.Save%2A?displayProperty=nameWithType> e <xref:System.Xml.XmlDocument.Save%2A?displayProperty=nameWithType> sempre escrevem <xref:System.Xml.XmlDeclaration>. Esses métodos utilizam a codificação do escritor para o qual eles estão escrevendo. Isto é, o valor de codificação no escritor substitui a codificação no documento e no <xref:System.Xml.XmlDeclaration>. Por exemplo, o código a seguir não escreve uma codificação na declaração XML encontrada no arquivo de saída `out.xml`.  
  
```vb  
Dim doc As New XmlDocument()  
Dim tw As XmlTextWriter = New XmlTextWriter("out.xml", Nothing)  
doc.Load("text.xml")  
doc.Save(tw)  
```  
  
```csharp  
XmlDocument doc = new XmlDocument();  
XmlTextWriter tw = new XmlTextWriter("out.xml", null);  
doc.Load("text.xml");  
doc.Save(tw);  
```  
  
 Para o método <xref:System.Xml.XmlDocument.Save%2A>, a declaração XML é escrita usando o método <xref:System.Xml.XmlWriter.WriteStartDocument%2A> na classe <xref:System.Xml.XmlWriter>. Portanto, substituir o método <xref:System.Xml.XmlWriter.WriteStartDocument%2A> altera o modo como o início do documento é escrito.  
  
 Para os <xref:System.Xml.XmlDeclaration> membros de <xref:System.Xml.XmlNode.OuterXml%2A> , <xref:System.Xml.XmlDeclaration.WriteTo%2A> e <xref:System.Xml.XmlNode.InnerXml%2A> , se a <xref:System.Xml.XmlDeclaration.Encoding%2A> propriedade não estiver definida, nenhuma codificação será gravada. Caso contrário, a codificação escrita na declaração XML é igual à codificação encontrada na <xref:System.Xml.XmlDeclaration.Encoding%2A> propriedade.  
  
## <a name="writing-document-content-using-the-outerxml-property"></a>Escrevendo o conteúdo do documento usando a propriedade OuterXml  
 A propriedade <xref:System.Xml.XmlNode.OuterXml%2A> é uma extensão da Microsoft para os padrões DOM (Document Object Model) de XML do W3C (World Wide Web Consortium). A propriedade <xref:System.Xml.XmlNode.OuterXml%2A> é usada para obter a marcação do documento XML inteiro ou apenas a marcação de um único nó e seus nós filho. <xref:System.Xml.XmlNode.OuterXml%2A> retorna a marcação que representa o nó determinado e todos os seus nós filho.  
  
 O exemplo de código a seguir mostra como salvar um documento em sua totalidade como uma cadeia de caracteres.  
  
```vb  
Dim mydoc As New XmlDocument()  
' Perform application needs here, like mydoc.Load("myfile");  
' Now save the entire document to a string variable called "xml".  
Dim xml As String = mydoc.OuterXml  
```  
  
```csharp  
XmlDocument mydoc = new XmlDocument();  
// Perform application needs here, like mydoc.Load("myfile");  
// Now save the entire document to a string variable called "xml".  
string xml = mydoc.OuterXml;  
```  
  
 O exemplo de código a seguir mostra como salvar apenas o elemento do documento.  
  
```vb  
' For the content of the Document Element only.  
Dim xml As String = mydoc.DocumentElement.OuterXml  
```  
  
```csharp  
// For the content of the Document Element only.  
string xml = mydoc.DocumentElement.OuterXml;  
```  
  
 Ao contrário, você pode usar a propriedade <xref:System.Xml.XmlNode.InnerText%2A> se quiser o conteúdo dos nós filho.  
  
## <a name="see-also"></a>Veja também

- [XML Document Object Model (DOM)](xml-document-object-model-dom.md)
