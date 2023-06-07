---
title: Modificar dados XML usando XPathNavigator
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
ms.assetid: 03a7c5a1-b296-4af4-b209-043c958dc0a5
ms.openlocfilehash: 3b64bc8666274798ebaefc87ef3883fcec1ef6b1
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84288830"
---
# <a name="modify-xml-data-using-xpathnavigator"></a>Modificar dados XML usando XPathNavigator
A classe <xref:System.Xml.XPath.XPathNavigator> fornece um conjunto de métodos usados para modificar nós e valores em um documento XML. Para usar esses métodos, o objeto <xref:System.Xml.XPath.XPathNavigator> deve ser editável, ou seja, sua propriedade <xref:System.Xml.XPath.XPathNavigator.CanEdit%2A> deve ser `true`.  
  
 Os objetos <xref:System.Xml.XPath.XPathNavigator> que podem editar um documento XML são criados pelo método <xref:System.Xml.XmlDocument.CreateNavigator%2A> da classe <xref:System.Xml.XmlDocument>. Os objetos <xref:System.Xml.XPath.XPathNavigator> criados pela classe <xref:System.Xml.XPath.XPathDocument> são somente leitura e qualquer tentativa de usar os métodos de um objeto <xref:System.Xml.XPath.XPathNavigator> criado por um objeto <xref:System.Xml.XPath.XPathDocument> resultará em <xref:System.NotSupportedException>.  
  
 Para saber mais sobre como criar objetos <xref:System.Xml.XPath.XPathNavigator> editáveis, confira [Leitura de dados XML usando XPathDocument e XmlDocument](reading-xml-data-using-xpathdocument-and-xmldocument.md).  
  
## <a name="modifying-nodes"></a>Modificando nós  
 Uma técnica simples para alterar o valor de um nó é usar os métodos <xref:System.Xml.XPath.XPathNavigator.SetValue%2A> e <xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A> da classe <xref:System.Xml.XPath.XPathNavigator>.  
  
 A tabela a seguir lista os efeitos desses métodos em tipos de nós diferentes.  
  
|<xref:System.Xml.XPath.XPathNodeType>|Dados alterados|  
|---------------------------------------------------------------------------------------------------------------------------------------------|------------------|  
|<xref:System.Xml.XPath.XPathNodeType.Root>|Sem suporte.|  
|<xref:System.Xml.XPath.XPathNodeType.Element>|O conteúdo do elemento.|  
|<xref:System.Xml.XPath.XPathNodeType.Attribute>|O valor do atributo.|  
|<xref:System.Xml.XPath.XPathNodeType.Text>|O conteúdo do texto.|  
|<xref:System.Xml.XPath.XPathNodeType.ProcessingInstruction>|O conteúdo, excluindo o destino.|  
|<xref:System.Xml.XPath.XPathNodeType.Comment>|O conteúdo do comentário.|  
|<xref:System.Xml.XPath.XPathNodeType.Namespace>|Sem suporte.|  
  
> [!NOTE]
> Não há suporte para a edição de nós <xref:System.Xml.XPath.XPathNodeType.Namespace> ou o nó <xref:System.Xml.XPath.XPathNodeType.Root>.  
  
 A classe <xref:System.Xml.XPath.XPathNavigator> também fornece um conjunto de métodos usados para inserir e remover nós. Para saber mais sobre como inserir e remover nós de um documento XML, consulte os tópicos [Inserir dados XML usando XPathNavigator](insert-xml-data-using-xpathnavigator.md) e [Remover dados XML usando XPathNavigator](remove-xml-data-using-xpathnavigator.md).  
  
### <a name="modifying-untyped-values"></a>Modificando valores sem tipo  
 O método <xref:System.Xml.XPath.XPathNavigator.SetValue%2A> simplesmente insere o valor sem tipo de `string` passado como um parâmetro como o valor do nó no qual o objeto <xref:System.Xml.XPath.XPathNavigator> está posicionado no momento. O valor é inserido sem nenhum tipo ou sem verificar se o novo valor é válido de acordo com o tipo de nó se as informações do esquema estiverem disponíveis.  
  
 No exemplo a seguir, o método <xref:System.Xml.XPath.XPathNavigator.SetValue%2A> é usado para atualizar todos os elementos `price` no arquivo `contosoBooks.xml`.  
  
 [!code-cpp[XPathNavigatorMethods#47](../../../../samples/snippets/cpp/VS_Snippets_Data/XPathNavigatorMethods/CPP/xpathnavigatormethods.cpp#47)]
 [!code-csharp[XPathNavigatorMethods#47](../../../../samples/snippets/csharp/VS_Snippets_Data/XPathNavigatorMethods/CS/xpathnavigatormethods.cs#47)]
 [!code-vb[XPathNavigatorMethods#47](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XPathNavigatorMethods/VB/xpathnavigatormethods.vb#47)]  
  
 O exemplo usa o arquivo `contosoBooks.xml` como entrada.  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
### <a name="modifying-typed-values"></a>Modificando valores com tipo  
 Quando o tipo de um nó é um tipo simples de Esquema XML do W3C, o novo valor inserido pelo método <xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A> é verificado em relação às facetas do tipo simples antes que o valor seja definido. Se o novo valor não for válido de acordo com o tipo de nó (por exemplo, definir um valor de `-1` em um elemento cujo tipo seja `xs:positiveInteger`), isso resultará em uma exceção.  
  
 O exemplo a seguir tenta alterar o valor do elemento `price` do primeiro elemento `book` no arquivo `contosoBooks.xml` para um valor <xref:System.DateTime>. Como o tipo de esquema XML do elemento `price` está definido como `xs:decimal` nos arquivos `contosoBooks.xsd`, isso resulta em uma exceção.  
  
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
  
navigator.SetTypedValue(DateTime.Now)  
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
  
navigator.SetTypedValue(DateTime.Now);  
```  
  
 O exemplo usa o arquivo `contosoBooks.xml` como entrada.  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
 O exemplo também usa `contosoBooks.xsd` como entrada.  
  
 [!code-xml[XPathXMLExamples#3](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xsd#3)]  
  
#### <a name="the-effects-of-editing-strongly-typed-xml-data"></a>Os efeitos de editar dados XML fortemente tipados  
 A <xref:System.Xml.XPath.XPathNavigator> classe usa o esquema XML W3C como base para descrever o XML com rigidez de tipos. Elementos e atributos podem ser anotados com informações de tipo com base na validação em relação a um documento de Esquema XML do W3C. Os elementos que podem conter outros elementos ou atributos são chamados de tipos complexos, enquanto os que podem conter apenas texto são chamados de tipos simples.  
  
> [!NOTE]
> Os atributos podem ter apenas tipos simples.  
  
 Um elemento ou atributo poderá ser considerado como válido para esquema se estiver de acordo com as regras específicas para sua definição de tipo. Um elemento que tem o tipo simples `xs:int` precisa conter um valor numérico entre -2147483648 e 2147483647 para ser válido para esquema. Para tipos complexos, a validade do esquema do elemento dependente da validade do esquema de seus elementos filho e atributos. Sendo assim, se um elemento é válido em relação a sua definição de tipo complexo, todos os seus elementos filho e atributos serão válidos com suas definições de tipo. Da mesma forma, mesmo que um dos elementos filho ou atributos de um elemento não seja válido em relação a sua definição de tipo, ou se tiver uma validade desconhecida, o elemento também será inválido ou de validade desconhecida.  
  
 Considerando que a validade de um elemento depende da validade de seus elementos filho e atributos, as alterações em algum deles resultará em alteração da validade do elemento se tiver sido válido anteriormente. Especificamente, se os elementos filho ou atributos de um elemento forem inseridos, atualizados ou excluídos, a validade do elemento se tornará desconhecida. Isso é representado pela propriedade <xref:System.Xml.Schema.IXmlSchemaInfo.Validity%2A> da propriedade <xref:System.Xml.XPath.XPathNavigator.SchemaInfo%2A> do elemento que está sendo definida como <xref:System.Xml.Schema.XmlSchemaValidity.NotKnown>. Além disso, este efeito propaga-se para cima recursivamente no documento XML, porque a validade do elemento pai do elemento (e seu elemento pai, e assim por diante) também se tornará desconhecida.  
  
 Para saber mais sobre validação do esquema e sobre a classe <xref:System.Xml.XPath.XPathNavigator>, consulte [Validação de esquema usando XPathNavigator](schema-validation-using-xpathnavigator.md).  
  
### <a name="modifying-attributes"></a>Modificando atributos  
 Os métodos <xref:System.Xml.XPath.XPathNavigator.SetValue%2A> e <xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A> podem ser usados para modificar nós de atributo sem tipo e tipados bem como outros tipos de nós listados na seção “Modificando nós”.  
  
 O exemplo a seguir altera o valor do atributo `genre` do primeiro elemento `book` no arquivo `books.xml`.  
  
```vb  
Dim document As XmlDocument = New XmlDocument()  
document.Load("books.xml")  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
navigator.MoveToChild("bookstore", String.Empty)  
navigator.MoveToChild("book", String.Empty)  
navigator.MoveToAttribute("genre", String.Empty)  
  
navigator.SetValue("non-fiction")  
  
navigator.MoveToRoot()  
Console.WriteLine(navigator.OuterXml)  
```  
  
```csharp  
XmlDocument document = new XmlDocument();  
document.Load("books.xml");  
XPathNavigator navigator = document.CreateNavigator();  
  
navigator.MoveToChild("bookstore", String.Empty);  
navigator.MoveToChild("book", String.Empty);  
navigator.MoveToAttribute("genre", String.Empty);  
  
navigator.SetValue("non-fiction");  
  
navigator.MoveToRoot();  
Console.WriteLine(navigator.OuterXml);  
```  
  
 Para obter mais informações sobre os métodos <xref:System.Xml.XPath.XPathNavigator.SetValue%2A> e <xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A>, consulte as seções "Modificando valores sem tipo" e "Modificando valores com tipo"  
  
## <a name="innerxml-and-outerxml-properties"></a>Propriedades InnerXml e OuterXml  
 As propriedades <xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> e <xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> da classe <xref:System.Xml.XPath.XPathNavigator> alteram a marcação XML dos nós nos quais um objeto <xref:System.Xml.XPath.XPathNavigator> está posicionado no momento.  
  
 A propriedade <xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> altera a marcação XML dos nós filho no qual um objeto <xref:System.Xml.XPath.XPathNavigator> está posicionado no momento com o conteúdo analisado da `string` do XML determinada. Da mesma maneira, a propriedade <xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> altera a marcação XML dos nós filho no qual um objeto <xref:System.Xml.XPath.XPathNavigator> está posicionado no momento além do próprio nó atual.  
  
 O exemplo a seguir usa a propriedade <xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> para modificar o valor do elemento `price` e inserir um novo atributo `discount` no primeiro elemento `book` no arquivo `contosoBooks.xml`.  
  
```vb  
Dim document As XmlDocument = New XmlDocument()  
document.Load("contosoBooks.xml");  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
navigator.MoveToChild("bookstore", "http://www.contoso.com/books")  
navigator.MoveToChild("book", "http://www.contoso.com/books")  
navigator.MoveToChild("price", "http://www.contoso.com/books")  
  
navigator.OuterXml = "<price discount=\"0\">10.99</price>"  
  
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
  
navigator.OuterXml = "<price discount=\"0\">10.99</price>";  
  
navigator.MoveToRoot();  
Console.WriteLine(navigator.OuterXml);  
```  
  
 O exemplo usa o arquivo `contosoBooks.xml` como entrada.  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
## <a name="modifying-namespace-nodes"></a>Modificando nós de namespace  
 No DOM (Document Object Model), as declarações de namespace são tratadas como se fossem atributos normais que podem ser inseridos, atualizados e excluídos. A classe <xref:System.Xml.XPath.XPathNavigator> não permite essas operações em nós de namespace porque alterar o valor de um nó de namespace pode modificar a identidade dos elementos e dos atributos dentro do escopo do nó do namespace conforme ilustrado no exemplo a seguir.  
  
```xml  
<root xmlns="http://www.contoso.com">  
    <child />  
</root>  
```  
  
 Se o exemplo de XML anterior for modificado da seguinte forma, isso efetivamente renomeará cada elemento no documento porque o valor da URI do namespace de cada elemento será alterado.  
  
```xml  
<root xmlns="urn:contoso.com">  
    <child />  
</root>  
```  
  
 Inserir os nós de namespace que não entrem em conflito com as declarações de namespace no escopo em que estão inseridos dentro é permitido pela classe <xref:System.Xml.XPath.XPathNavigator>. Nesse caso, as declarações de namespace não são declaradas em escopos menores no documento XML e não resultam em renomeação conforme ilustrado no exemplo a seguir.  
  
```xml  
<root xmlns:a="http://www.contoso.com">  
    <parent>  
        <a:child />  
    </parent>  
</root>  
```  
  
 Se o exemplo de XML acima for modificado da seguinte forma, as declarações de namespace serão propagadas corretamente no documento XML abaixo do escopo de outra declaração de namespace.  
  
```xml  
<root xmlns:a="http://www.contoso.com">  
    <parent a:parent-id="1234" xmlns:a="http://www.contoso.com/parent-id">  
        <a:child xmlns:a="http://www.contoso.com/" />  
    </parent>  
</root>  
```  
  
 No exemplo de XML acima, o atributo `a:parent-id` é inserido no elemento `parent` no namespace `http://www.contoso.com/parent-id`. O método <xref:System.Xml.XPath.XPathNavigator.CreateAttribute%2A> é usado para inserir o atributo enquanto estiver posicionado no elemento `parent`. A declaração do namespace `http://www.contoso.com` é inserida automaticamente pela classe <xref:System.Xml.XPath.XPathNavigator> para manter a consistência do restante do documento XML.  
  
## <a name="modifying-entity-reference-nodes"></a>Modificando nós de referência da entidade  
 Os nós de referência de entidade em um objeto <xref:System.Xml.XmlDocument> são somente leitura e não podem ser editados usando as classes <xref:System.Xml.XPath.XPathNavigator> ou <xref:System.Xml.XmlNode>. Qualquer tentativa de modificar um nó de referência de entidade resulta em um <xref:System.InvalidOperationException>.  
  
## <a name="modifying-xsinil-nodes"></a>Modificando nós xsi:nil  
 A recomendação do Esquema XML do W3C apresenta o conceito de um elemento ser nillable. Quando um elemento é nillable, é possível que ele não tenha nenhum conteúdo e ainda ser válido. O conceito de um elemento ser nillable é semelhante ao conceito de um objeto ser `null`. A principal diferença é que um objeto `null` não pode ser acessado de nenhuma forma, enquanto que um elemento `xsi:nil` ainda tem propriedades como atributos que podem ser acessados, mas não tem conteúdo (elementos filho ou texto). A existência do atributo `xsi:nil` com um valor `true` em um elemento em um documento XML é usada para indicar que um elemento não tem conteúdo.  
  
 Se um objeto <xref:System.Xml.XPath.XPathNavigator> for usado para adicionar conteúdo a um elemento válido com um atributo `xsi:nil` com um valor `true`, o valor do seu atributo `xsi:nil` será definido como `false`.  
  
> [!NOTE]
> Se o conteúdo de um elemento com um atributo `xsi:nil` definido como `false` for excluído, o valor do atributo não será alterado para `true`.  
  
## <a name="saving-an-xml-document"></a>Salvando um documento XML  
 Salvar as alterações feitas em um objeto <xref:System.Xml.XmlDocument> como resultado dos métodos de edição descritos neste tópico é realizado usando os métodos da classe <xref:System.Xml.XmlDocument>. Para saber mais sobre como salvar as alterações feitas em um objeto <xref:System.Xml.XmlDocument>, confira [Salvar e gravar um documento](saving-and-writing-a-document.md).  
  
## <a name="see-also"></a>Veja também

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [Processar dados XML usando o modelo de dados XPath](process-xml-data-using-the-xpath-data-model.md)
- [Dados XML de inserção usando XPathNavigator](insert-xml-data-using-xpathnavigator.md)
- [Remova os dados XML usando XPathNavigator](remove-xml-data-using-xpathnavigator.md)
