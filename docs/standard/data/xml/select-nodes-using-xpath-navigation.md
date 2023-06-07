---
title: Selecionar nós usando a navegação XPath
description: Saiba como selecionar nós XML no .NET. Use os métodos Modelo de Objeto do Documento (DOM), permitindo que você use a navegação XPath para consultar informações do DOM.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 8e4450dc-56b3-472b-b467-32f5694f83ad
ms.openlocfilehash: aa8b6d93e25d974a0e1b53ae8be9868f6bf64be6
ms.sourcegitcommit: 7137e12f54c4e83a94ae43ec320f8cf59c1772ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84662505"
---
# <a name="select-nodes-using-xpath-navigation"></a>Selecionar nós usando a navegação XPath
O DOM (Document Object Model) XML contém métodos que permitem que você use a navegação da linguagem XPath para consultar informações no DOM. Você pode usar a linguagem XPath para localizar um único nó específico ou todos os nós que correspondam a alguns critérios.  
  
## <a name="xpath-select-methods"></a>Métodos de seleção XPath  
 As classes DOM fornecem dois métodos de seleção XPath: o método <xref:System.Xml.XmlNode.SelectSingleNode%2A> e o método <xref:System.Xml.XmlNode.SelectNodes%2A>. O método <xref:System.Xml.XmlNode.SelectSingleNode%2A> retorna o primeiro nó que corresponde aos critérios de seleção. O método <xref:System.Xml.XmlNode.SelectNodes%2A> retorna um <xref:System.Xml.XmlNodeList> que contém os nós correspondentes.  
  
 O exemplo a seguir usa o método <xref:System.Xml.XmlNode.SelectSingleNode%2A> para selecionar o primeiro nó `book`, no qual o sobrenome do autor atende a critérios específicos. O arquivo bookstore.xml (que é fornecido no final deste tópico) é usado como o arquivo de entrada.  
  
```vb  
Dim doc As New XmlDocument()  
doc.Load("bookstore.xml")  
Dim root As XmlNode = doc.DocumentElement  
  
' Add the namespace.  
Dim nsmgr As New XmlNamespaceManager(doc.NameTable)  
nsmgr.AddNamespace("bk", "urn:newbooks-schema")  
  
' Select and display the first node in which the author's
' last name is Kingsolver.  
Dim node As XmlNode = root.SelectSingleNode( _  
     "descendant::bk:book[bk:author/bk:last-name='Kingsolver']", nsmgr)  
Console.WriteLine(node.InnerXml)  
```  
  
```csharp  
// Load the document and set the root element.  
XmlDocument doc = new XmlDocument();  
doc.Load("bookstore.xml");  
XmlNode root = doc.DocumentElement;  
  
// Add the namespace.  
XmlNamespaceManager nsmgr = new XmlNamespaceManager(doc.NameTable);  
nsmgr.AddNamespace("bk", "urn:newbooks-schema");  
  
// Select and display the first node in which the author's
// last name is Kingsolver.  
XmlNode node = root.SelectSingleNode(  
    "descendant::bk:book[bk:author/bk:last-name='Kingsolver']", nsmgr);  
Console.WriteLine(node.InnerXml);  
```  
  
 O exemplo a seguir usa o método <xref:System.Xml.XmlNode.SelectNodes%2A> para selecionar todos os nós de livros em que o preço é maior do que um valor especificado. O preço de cada livro na lista selecionada é reduzido programaticamente em dez por cento. Finalmente, o arquivo atualizado é gravado no console. O arquivo bookstore.xml (que é fornecido no final deste tópico) é usado como o arquivo de entrada.  
  
```vb  
' Load the document and set the root element.  
Dim doc As New XmlDocument()  
doc.Load("bookstore.xml")  
Dim root As XmlNode = doc.DocumentElement  
  
' Add the namespace.  
Dim nsmgr As New XmlNamespaceManager(doc.NameTable)  
nsmgr.AddNamespace("bk", "urn:newbooks-schema")  
  
' Select all nodes where the book price is greater than 10.00.  
Dim nodeList As XmlNodeList = root.SelectNodes( _  
     "descendant::bk:book[bk:price>10.00]", nsmgr)  
For Each book As XmlNode In nodeList  
     Dim price As Double  
     price = Math.Round(Convert.ToSingle( _  
          book.LastChild.InnerText) * 0.9, 2)  
     book.LastChild.InnerText = price.ToString()  
Next  
  
' Display the updated document.  
doc.Save(Console.Out)  
```  
  
```csharp  
// Load the document and set the root element.  
XmlDocument doc = new XmlDocument();  
doc.Load("bookstore.xml");  
XmlNode root = doc.DocumentElement;  
  
// Add the namespace.  
XmlNamespaceManager nsmgr = new XmlNamespaceManager(doc.NameTable);  
nsmgr.AddNamespace("bk", "urn:newbooks-schema");  
  
// Select all nodes where the book price is greater than 10.00.  
XmlNodeList nodeList = root.SelectNodes(  
     "descendant::bk:book[bk:price>10.00]", nsmgr);  
foreach (XmlNode book in nodeList)  
{  
     // Discount prices by 10%.  
     double price;  
     price = Math.Round(Convert.ToSingle(  
          book.LastChild.InnerText) * 0.9, 2);  
     book.LastChild.InnerText = price.ToString();  
}  
  
// Display the updated document.  
doc.Save(Console.Out);  
```  
  
 Os exemplos acima iniciam a consulta XPath no elemento de documento. A definição do ponto de partida para a consulta XPath define o nó de contexto, que é o ponto de partida para a consulta XPath. Se você não deseja começar no elemento de documento, mas a partir do primeiro elemento filho do documento, você pode codificar a instrução de seleção da seguinte maneira:  
  
```vb  
doc.DocumentElement.FirstChild.SelectNodes(. . . )  
```  
  
```csharp  
this doc.DocumentElement.FirstChild.SelectNodes(. . .);  
```  
  
 Todos os objetos <xref:System.Xml.XmlNodeList> são sincronizados com o documento subjacente. Portanto, se você iterar pela lista de nós e modificar o valor de um nó, esse nó também será atualizado no documento de origem. Observe no exemplo anterior que, quando um nó é modificado no <xref:System.Xml.XmlNodeList> selecionado, o documento subjacente também é modificado.  
  
> [!NOTE]
> Quando o documento subjacente é modificado, é aconselhável executar novamente a seleção. Se o nó modificado for um que possa fazer com que o nó seja adicionado à lista de nós quando não tiver sido anteriormente, ou que agora faça com que ele seja removido da lista, não haverá nenhuma garantia de que a lista de nós agora esteja exata.  
  
## <a name="namespaces-in-xpath-expressions"></a>Namespaces em expressões XPath  
 As expressões XPath podem incluir namespaces A resolução de namespace tem suporte com o uso do <xref:System.Xml.XmlNamespaceManager>. Se a expressão XPath incluir um prefixo, o par de prefixo e URI de namespace deverá ser adicionado a <xref:System.Xml.XmlNamespaceManager>, e <xref:System.Xml.XmlNamespaceManager> será passado ao método <xref:System.Xml.XmlNode.SelectNodes%28System.String%2CSystem.Xml.XmlNamespaceManager%29> ou <xref:System.Xml.XmlNode.SelectSingleNode%28System.String%2CSystem.Xml.XmlNamespaceManager%29>. Observe que os exemplos de código acima usam <xref:System.Xml.XmlNamespaceManager> para resolver o namespace do documento bookstore.xml.  
  
> [!NOTE]
> Se a expressão XPath não incluir um prefixo, presume-se que o URI do namespace seja o namespace vazio. Se o XML incluir um namespace padrão, você ainda deverá adicionar um prefixo e um URI de namespace a <xref:System.Xml.XmlNamespaceManager>; caso contrário, nenhum nó será selecionado.  
  
#### <a name="input-file"></a>Arquivo de entrada  
 A seguir está o arquivo bookstore.xml que é usado como o arquivo de entrada nos exemplos deste tópico:  
  
```xml  
<?xml version='1.0'?>  
<bookstore xmlns="urn:newbooks-schema">  
  <book genre="novel" style="hardcover">  
    <title>The Handmaid's Tale</title>  
    <author>  
      <first-name>Margaret</first-name>  
      <last-name>Atwood</last-name>  
    </author>  
    <price>19.95</price>  
  </book>  
  <book genre="novel" style="other">  
    <title>The Poisonwood Bible</title>  
    <author>  
      <first-name>Barbara</first-name>  
      <last-name>Kingsolver</last-name>  
    </author>  
    <price>11.99</price>  
  </book>  
  <book genre="novel" style="paperback">  
    <title>The Bean Trees</title>  
    <author>  
      <first-name>Barbara</first-name>  
      <last-name>Kingsolver</last-name>  
    </author>  
    <price>5.99</price>  
  </book>  
</bookstore>  
```  
  
## <a name="see-also"></a>Confira também

- [XML Document Object Model (DOM)](xml-document-object-model-dom.md)
