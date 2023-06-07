---
title: Remova os dados XML usando XPathNavigator
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 9f436bca-1b96-494b-a6d2-e102c7551752
ms.openlocfilehash: fa331757fac3f30ee86a24bbd0ee12b5f1031a4b
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84288661"
---
# <a name="remove-xml-data-using-xpathnavigator"></a>Remova os dados XML usando XPathNavigator
A classe de <xref:System.Xml.XPath.XPathNavigator> fornece um conjunto de métodos usados para remover os nós e valores de um documento XML. Para usar esses métodos, o objeto <xref:System.Xml.XPath.XPathNavigator> deve ser editável, ou seja, sua propriedade <xref:System.Xml.XPath.XPathNavigator.CanEdit%2A> deve ser `true`.  
  
 Os objetos <xref:System.Xml.XPath.XPathNavigator> que podem editar um documento XML são criados pelo método <xref:System.Xml.XmlDocument.CreateNavigator%2A> da classe <xref:System.Xml.XmlDocument>. os objetos de<xref:System.Xml.XPath.XPathNavigator> criados pela classe de <xref:System.Xml.XPath.XPathDocument> são somente leitura e qualquer tentativa de usar os métodos de um objeto de <xref:System.Xml.XPath.XPathNavigator> criado por <xref:System.Xml.XPath.XPathDocument> objetos resultados em <xref:System.NotSupportedException>.  
  
 Para saber mais sobre como criar objetos <xref:System.Xml.XPath.XPathNavigator> editáveis, confira [Leitura de dados XML usando XPathDocument e XmlDocument](reading-xml-data-using-xpathdocument-and-xmldocument.md).  
  
## <a name="removing-nodes"></a>Removendo os nós  
 A classe de <xref:System.Xml.XPath.XPathNavigator> fornece o método de <xref:System.Xml.XPath.XPathNavigator.DeleteSelf%2A> para remover os nós de um documento XML.  
  
### <a name="removing-a-node"></a>Removendo um nó  
 A classe de <xref:System.Xml.XPath.XPathNavigator> fornece o método de <xref:System.Xml.XPath.XPathNavigator.DeleteSelf%2A> para excluir o nó atual que um objeto de <xref:System.Xml.XPath.XPathNavigator> é posicionado atualmente de um documento XML.  
  
 Depois que um nó foi excluído usando o método <xref:System.Xml.XPath.XPathNavigator.DeleteSelf%2A> , não é mais alcançável raiz do objeto de <xref:System.Xml.XmlDocument> . Depois que um nó foi excluído, <xref:System.Xml.XPath.XPathNavigator> está localizado no nó pai do nó excluído.  
  
 Uma operação de exclusão não afeta a posição de nenhum objeto de <xref:System.Xml.XPath.XPathNavigator> posicionado sobre o nó excluído. Esses objetos de <xref:System.Xml.XPath.XPathNavigator> são válidos no sentido que podem mover dentro da sub-árvore excluída, mas não podem ser movidos à árvore do nó usando os métodos definidos de navegação do nó normal da classe de <xref:System.Xml.XPath.XPathNavigator> .  
  
> [!NOTE]
> O método de <xref:System.Xml.XPath.XPathNavigator.MoveTo%2A> da classe de <xref:System.Xml.XPath.XPathNavigator> pode ser usado para mover esses objetos de <xref:System.Xml.XPath.XPathNavigator> de novo na árvore do nó principal, ou de árvore nó principal da subárvore excluída.  
  
 No exemplo a seguir, o elemento de `price` do primeiro elemento de `book` do arquivo de `contosoBooks.xml` é excluído usando o método <xref:System.Xml.XPath.XPathNavigator.DeleteSelf%2A> . A posição do objeto de <xref:System.Xml.XPath.XPathNavigator> após o elemento de `price` é excluída está no elemento pai de `book` .  
  
```vb  
Dim document As XmlDocument = New XmlDocument()  
document.Load("contosoBooks.xml")  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
navigator.MoveToChild("bookstore", "http://www.contoso.com/books")  
navigator.MoveToChild("book", "http://www.contoso.com/books")  
navigator.MoveToChild("price", "http://www.contoso.com/books")  
  
navigator.DeleteSelf()  
  
Console.WriteLine("Position after delete: {0}", navigator.Name)  
Console.WriteLine(navigator.OuterXml)  
```  
  
```csharp  
XmlDocument document = new XmlDocument();  
document.Load("contosoBooks.xml");  
XPathNavigator navigator = document.CreateNavigator();  
  
navigator.MoveToChild("bookstore", "http://www.contoso.com/books");  
navigator.MoveToChild("book", "http://www.contoso.com/books");  
navigator.MoveToChild("price", "http://www.contoso.com/books");  
  
navigator.DeleteSelf();  
  
Console.WriteLine("Position after delete: {0}", navigator.Name);  
Console.WriteLine(navigator.OuterXml);  
```  
  
 O exemplo usa o arquivo `contosoBooks.xml` como entrada.  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
### <a name="removing-an-attribute-node"></a>Removendo um nó de atributo  
 Os nós de atributo são removidos de um documento XML usando o método <xref:System.Xml.XPath.XPathNavigator.DeleteSelf%2A> .  
  
 Depois que um nó de atributo foi excluído, não é mais alcançável do nó raiz de um objeto de <xref:System.Xml.XmlDocument> e o objeto de <xref:System.Xml.XPath.XPathNavigator> é posicionado no elemento pai.  
  
#### <a name="default-attributes"></a>Atributos padrão  
 Independentemente do método usado para remover os atributos, há limitações especiais em remover os atributos que são definidos como atributos padrão no DTD ou no esquema XML para o documento XML. Os atributos padrão não podem ser removidos a menos que o elemento que pertencem a também é removido. Os atributos padrão são sempre atual para elementos que têm atributos padrões declarados, e como resultado, excluindo resultados de um atributo padrão em uma substituição atributo ser inserido no elemento e ser inicializado para o valor padrão que foi declarado.  
  
## <a name="removing-values"></a>Removendo os valores  
 A classe de <xref:System.Xml.XPath.XPathNavigator> fornece os métodos de <xref:System.Xml.XPath.XPathNavigator.SetValue%2A> e de <xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A> para remover sem tipo e os valores tipados de um documento XML.  
  
### <a name="removing-untyped-values"></a>Removendo os valores sem tipo  
 O método <xref:System.Xml.XPath.XPathNavigator.SetValue%2A> simplesmente insere o valor sem tipo de `string` passado como um parâmetro como o valor do nó no qual o objeto <xref:System.Xml.XPath.XPathNavigator> está posicionado no momento. Passando uma cadeia de caracteres vazia para o método de <xref:System.Xml.XPath.XPathNavigator.SetValue%2A> remove o valor do nó atual.  
  
 O exemplo a seguir remove o valor do elemento de `price` do primeiro elemento de `book` no arquivo de `contosoBooks.xml` usando o método <xref:System.Xml.XPath.XPathNavigator.SetValue%2A> .  
  
```vb  
Dim document As XmlDocument = New XmlDocument()  
document.Load("contosoBooks.xml")  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
navigator.MoveToChild("bookstore", "http://www.contoso.com/books")  
navigator.MoveToChild("book", "http://www.contoso.com/books")  
navigator.MoveToChild("price", "http://www.contoso.com/books")  
  
navigator.SetValue("")  
  
navigator.MoveToRoot()  
Console.WriteLine(navigator.OuterXml)  
```  
  
```csharp  
XmlDocument document = new XmlDocument();  
document.Load("contosoBooks.xml");  
XPathNavigator navigator = document.CreateNavigator();  
  
navigator.MoveToChild("bookstore", "http://www.contoso.com/books");  
navigator.MoveToChild("book", "http://www.contoso.com/books");  
navigator.MoveToChild("price", "http://www.contoso.com/books");  
  
navigator.SetValue("");  
  
navigator.MoveToRoot();  
Console.WriteLine(navigator.OuterXml);  
```  
  
 O exemplo usa o arquivo `contosoBooks.xml` como entrada.  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
### <a name="removing-typed-values"></a>Removendo os valores tipados  
 Quando o tipo de um nó é um tipo simples de Esquema XML do W3C, o novo valor inserido pelo método <xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A> é verificado em relação às facetas do tipo simples antes que o valor seja definido. Se o novo valor não for válido de acordo com o tipo de nó (por exemplo, definir um valor de `-1` em um elemento cujo tipo seja `xs:positiveInteger`), isso resultará em uma exceção. O método de <xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A> também não pode ser `null` passado como um parâmetro. Como resultado remova o valor de um nó tipado deve seguir com o tipo do nó.  
  
 O exemplo a seguir remove o valor do elemento de `price` do primeiro elemento de `book` no arquivo de `contosoBooks.xml` usando o método <xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A> definindo o valor a `0`. O valor do nó não é removido, mas o custo de livro foi removido de acordo com seu tipo de dados de `xs:decimal`.  
  
```vb  
Dim settings As XmlReaderSettings = New XmlReaderSettings()  
settings.Schemas.Add("http://www.contoso.com/books", "contosoBooks.xsd")  
settings.ValidationType = ValidationType.Schema  
  
Dim reader As XmlReader = XmlReader.Create("contosoBooks.xml", settings)  
  
Dim document As XmlDocument = New XmlDocument()  
document.Load(reader)  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
navigator.MoveToChild("bookstore", "http://www.contoso.com/books")  
navigator.MoveToChild("book", "http://www.contoso.com/books")  
navigator.MoveToChild("price", "http://www.contoso.com/books")  
  
navigator.SetTypedValue(0)  
  
navigator.MoveToRoot()  
Console.WriteLine(navigator.OuterXml)  
```  
  
```csharp  
XmlReaderSettings settings = new XmlReaderSettings();  
settings.Schemas.Add("http://www.contoso.com/books", "contosoBooks.xsd");  
settings.ValidationType = ValidationType.Schema;  
  
XmlReader reader = XmlReader.Create("contosoBooks.xml", settings);  
  
XmlDocument document = new XmlDocument();  
document.Load(reader);  
XPathNavigator navigator = document.CreateNavigator();  
  
navigator.MoveToChild("bookstore", "http://www.contoso.com/books");  
navigator.MoveToChild("book", "http://www.contoso.com/books");  
navigator.MoveToChild("price", "http://www.contoso.com/books");  
  
navigator.SetTypedValue(0);  
  
navigator.MoveToRoot();  
Console.WriteLine(navigator.OuterXml);  
```  
  
## <a name="namespace-nodes"></a>Nós de namespace  
 Os nós de namespace não podem ser excluídos de um objeto de <xref:System.Xml.XmlDocument> . Tentativas de excluir os nós de namespace usando o método de <xref:System.Xml.XPath.XPathNavigator.DeleteSelf%2A> resultam em uma exceção.  
  
## <a name="the-innerxml-and-outerxml-properties"></a>As propriedades de InnerXml e de OuterXml  
 As propriedades <xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> e <xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> da classe <xref:System.Xml.XPath.XPathNavigator> alteram a marcação XML dos nós nos quais um objeto <xref:System.Xml.XPath.XPathNavigator> está posicionado no momento.  
  
 A propriedade <xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> altera a marcação XML dos nós filho no qual um objeto <xref:System.Xml.XPath.XPathNavigator> está posicionado no momento com o conteúdo analisado da `string` do XML determinada. Da mesma maneira, a propriedade <xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> altera a marcação XML dos nós filho no qual um objeto <xref:System.Xml.XPath.XPathNavigator> está posicionado no momento além do próprio nó atual.  
  
 Além dos métodos descritos neste tópico, <xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> e as propriedades de <xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> podem ser usados para remover os nós e valores de um documento XML. Para saber mais sobre como usar as propriedades <xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> e <xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> para modificar nós, confira o tópico [Modificar dados XML usando XPathNavigator](modify-xml-data-using-xpathnavigator.md).  
  
## <a name="saving-an-xml-document"></a>Salvando um documento XML  
 Salvando as alterações feitas a um objeto de <xref:System.Xml.XmlDocument> como resultado dos métodos descritos neste tópico é executado usando os métodos da classe <xref:System.Xml.XmlDocument> . Para saber mais sobre como salvar as alterações feitas em um objeto <xref:System.Xml.XmlDocument>, confira [Salvar e gravar um documento](saving-and-writing-a-document.md).  
  
## <a name="see-also"></a>Veja também

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [Processar dados XML usando o modelo de dados XPath](process-xml-data-using-the-xpath-data-model.md)
- [Dados XML de inserção usando XPathNavigator](insert-xml-data-using-xpathnavigator.md)
- [Modificar dados XML usando XPathNavigator](modify-xml-data-using-xpathnavigator.md)
