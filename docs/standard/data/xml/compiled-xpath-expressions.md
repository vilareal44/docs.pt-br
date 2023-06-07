---
title: Expressões XPath compilados
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: e25dd95f-b64c-4d8b-a3a4-379e1aa0ad55
ms.openlocfilehash: e74b52e471699fc663504fa42d6c7e502859adda
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84291520"
---
# <a name="compiled-xpath-expressions"></a>Expressões XPath compilados
Um objeto de <xref:System.Xml.XPath.XPathExpression> representa uma consulta XPath compilado retornada do método estático de <xref:System.Xml.XPath.XPathExpression.Compile%2A> da classe de <xref:System.Xml.XPath.XPathExpression> ou método de <xref:System.Xml.XPath.XPathNavigator.Compile%2A> da classe de <xref:System.Xml.XPath.XPathNavigator> .  
  
## <a name="the-xpathexpression-class"></a>A classe de XPathExpression  
 Uma consulta XPath compilado é representada por um objeto de <xref:System.Xml.XPath.XPathExpression> é útil se a mesma consulta XPath está sendo usado mais de uma vez.  
  
 Por exemplo, quando chamar o método de <xref:System.Xml.XPath.XPathNavigator.Select%2A> várias vezes, em vez de usar uma cadeia de caracteres que representa a consulta XPath cada vez, usa o método de <xref:System.Xml.XPath.XPathExpression.Compile%2A> da classe de <xref:System.Xml.XPath.XPathExpression> ou o método de <xref:System.Xml.XPath.XPathNavigator.Compile%2A> da classe de <xref:System.Xml.XPath.XPathNavigator> para criar e armazenar em cachê a consulta XPath em um objeto de <xref:System.Xml.XPath.XPathExpression> para reutilização e melhor desempenho.  
  
 Uma vez que compilado, o objeto de <xref:System.Xml.XPath.XPathExpression> pode ser usado como entrada para os seguintes métodos da classe <xref:System.Xml.XPath.XPathNavigator> dependendo do tipo retornado de consulta XPath.  
  
- <xref:System.Xml.XPath.XPathNavigator.Evaluate%2A?displayProperty=nameWithType>  
  
- <xref:System.Xml.XPath.XPathNavigator.Evaluate%2A?displayProperty=nameWithType>  
  
- <xref:System.Xml.XPath.XPathNavigator.Matches%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.Select%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.SelectSingleNode%2A>  
  
 A tabela a seguir descreve cada um dos tipos de retorno XPath W3C, suas equivalências do Microsoft.NET Framework, e métodos que o objeto de <xref:System.Xml.XPath.XPathExpression> pode ser usado com base no seu tipo de retorno.  
  
|Tipo de retorno XPath W3C|tipo equivalente do .NET Framework|Description|Métodos|  
|---------------------------|------------------------------------|-----------------|-------------|  
|`Node set`|<xref:System.Xml.XPath.XPathNodeIterator>|Uma coleção não ordenada de nós sem duplicatas criadas na ordem de documento.|<xref:System.Xml.XPath.XPathNavigator.Select%2A> ou <xref:System.Xml.XPath.XPathNavigator.Evaluate%2A>|  
|`Boolean`|<xref:System.Boolean>|Um valor de `true` ou de `false` .|<xref:System.Xml.XPath.XPathNavigator.Evaluate%2A> ou<br /><br /> <xref:System.Xml.XPath.XPathNavigator.Matches%2A>|  
|`Number`|<xref:System.Double>|Um número de ponto flutuante.|<xref:System.Xml.XPath.XPathNavigator.Evaluate%2A>|  
|`String`|<xref:System.String>|Uma sequência de caracteres de UCS.|<xref:System.Xml.XPath.XPathNavigator.Evaluate%2A>|  
  
> [!NOTE]
> O método de <xref:System.Xml.XPath.XPathNavigator.Matches%2A> aceita uma expressão XPath como seu parâmetro. O método de <xref:System.Xml.XPath.XPathNavigator.SelectSingleNode%2A> retorna um objeto de <xref:System.Xml.XPath.XPathNavigator> , não um dos tipos de retorno XPath W3C.  
  
### <a name="the-returntype-property"></a>A propriedade ReturnType  
 Depois que uma consulta XPath foi compilada em um objeto de <xref:System.Xml.XPath.XPathExpression> , você pode usar a propriedade de <xref:System.Xml.XPath.XPathExpression.ReturnType%2A> do objeto de <xref:System.Xml.XPath.XPathExpression> para determinar o que a consulta XPath retorna.  
  
 A propriedade de <xref:System.Xml.XPath.XPathExpression.ReturnType%2A> retorna um dos seguintes valores de enumeração <xref:System.Xml.XPath.XPathResultType> que representam os tipos de retorno XPath W3C.  
  
- <xref:System.Xml.XPath.XPathResultType.Any>  
  
- <xref:System.Xml.XPath.XPathResultType.Boolean>  
  
- <xref:System.Xml.XPath.XPathResultType.Error>  
  
- <xref:System.Xml.XPath.XPathResultType.Navigator>  
  
- <xref:System.Xml.XPath.XPathResultType.NodeSet>  
  
- <xref:System.Xml.XPath.XPathResultType.Number>  
  
- <xref:System.Xml.XPath.XPathResultType.String>  
  
 O exemplo a seguir usa o objeto de <xref:System.Xml.XPath.XPathExpression> para retornar um número e um nó definidas do arquivo de `books.xml` . A propriedade de <xref:System.Xml.XPath.XPathExpression.ReturnType%2A> de cada objeto de <xref:System.Xml.XPath.XPathExpression> bem como os resultados dos métodos de <xref:System.Xml.XPath.XPathNavigator.Evaluate%2A> e de <xref:System.Xml.XPath.XPathNavigator.Select%2A> são gravados no console.  
  
```vb  
Dim document As XPathDocument = New XPathDocument("books.xml")  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
' Returns a number.  
Dim query1 As XPathExpression = navigator.Compile("bookstore/book/price/text()*10")  
Console.WriteLine(query1.ReturnType)  
  
Dim number As Double = CType(navigator.Evaluate(query1), Double)  
Console.WriteLine(number)  
  
' Returns a node set.  
Dim query2 As XPathExpression = navigator.Compile("bookstore/book/price")  
Console.WriteLine(query2.ReturnType)  
  
Dim nodes As XPathNodeIterator = navigator.Select(query2)  
nodes.MoveNext()  
Console.WriteLine(nodes.Current.Value)  
```  
  
```csharp  
XPathDocument document = new XPathDocument("books.xml");  
XPathNavigator navigator = document.CreateNavigator();  
  
// Returns a number.  
XPathExpression query1 = navigator.Compile("bookstore/book/price/text()*10");  
Console.WriteLine(query1.ReturnType);  
  
Double number = (Double)navigator.Evaluate(query1);  
Console.WriteLine(number);  
  
// Returns a node set.  
XPathExpression query2 = navigator.Compile("bookstore/book/price");  
Console.WriteLine(query2.ReturnType);  
  
XPathNodeIterator nodes = navigator.Select(query2);  
nodes.MoveNext();  
Console.WriteLine(nodes.Current.Value);  
```  
  
 O exemplo usa o arquivo `books.xml` como entrada.  
  
 [!code-xml[XPathXMLExamples#1](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/books.xml#1)]  
  
### <a name="higher-performance-xpath-expressions"></a>Expressões XPath de desempenho mais alto  
 Para melhor desempenho, use a expressão XPath a mais específica possível em suas consultas. Por exemplo, se o nó de `book` é um nó filho do nó de `bookstore` e o nó de `bookstore` é o elemento top-most em um documento XML, usar a expressão XPath `/bookstore/book` é mais rápido do que usar `//book`. A expressão XPath de `//book` digitalizará cada nó na árvore XML para identificar nós compatíveis.  
  
 Além disso, usar os métodos definidos de navegação do nó fornecidos pela classe de <xref:System.Xml.XPath.XPathNavigator> pode resultar em melhor desempenho sobre os métodos de seleção fornecidos pela classe de <xref:System.Xml.XPath.XPathNavigator> em casos onde seus critérios de seleção são simples. Por exemplo, se você precisar selecione o primeiro filho do nó atual, é mais rápido usar o método de <xref:System.Xml.XPath.XPathNavigator.MoveToFirst%2A> de usas a expressão XPath de `child::*[1]` e o método de <xref:System.Xml.XPath.XPathNavigator.Select%2A> .  
  
 Para saber mais sobre os métodos de navegação de conjunto de nós da classe <xref:System.Xml.XPath.XPathNavigator>, confira [Navegação de conjunto de nós usando XPathNavigator](node-set-navigation-using-xpathnavigator.md).  
  
## <a name="see-also"></a>Veja também

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [Processar dados XML usando o modelo de dados XPath](process-xml-data-using-the-xpath-data-model.md)
- [Selecionar dados XML usando XPathNavigator](select-xml-data-using-xpathnavigator.md)
- [Avalia as expressões XPath usando XPathNavigator](evaluate-xpath-expressions-using-xpathnavigator.md)
- [Nós compatíveis usando XPathNavigator](matching-nodes-using-xpathnavigator.md)
- [Tipos de nós reconhecidos com consultas XPath](node-types-recognized-with-xpath-queries.md)
- [Consultas XPath e namespaces](xpath-queries-and-namespaces.md)
