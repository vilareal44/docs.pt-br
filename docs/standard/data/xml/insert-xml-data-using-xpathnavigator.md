---
title: Dados XML de inserção usando XPathNavigator
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
ms.assetid: 2ed8c28b-b88d-4be7-9c87-92df01f0821f
ms.openlocfilehash: 1dbe1a709f7c1b527a1754ab943a0a10ff52c6e8
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84289181"
---
# <a name="insert-xml-data-using-xpathnavigator"></a>Dados XML de inserção usando XPathNavigator
A classe de <xref:System.Xml.XPath.XPathNavigator> fornece um conjunto de métodos usados para irmão, o filho, e nós de atributo de inserção em um documento XML. Para usar esses métodos, o objeto <xref:System.Xml.XPath.XPathNavigator> deve ser editável, ou seja, sua propriedade <xref:System.Xml.XPath.XPathNavigator.CanEdit%2A> deve ser `true`.  
  
 Os objetos <xref:System.Xml.XPath.XPathNavigator> que podem editar um documento XML são criados pelo método <xref:System.Xml.XmlDocument.CreateNavigator%2A> da classe <xref:System.Xml.XmlDocument>. os objetos de<xref:System.Xml.XPath.XPathNavigator> criados pela classe de <xref:System.Xml.XPath.XPathDocument> são somente leitura e qualquer tentativa de usar os métodos de um objeto de <xref:System.Xml.XPath.XPathNavigator> criado por <xref:System.Xml.XPath.XPathDocument> objetos resultados em <xref:System.NotSupportedException>.  
  
 Para saber mais sobre como criar objetos <xref:System.Xml.XPath.XPathNavigator> editáveis, confira [Leitura de dados XML usando XPathDocument e XmlDocument](reading-xml-data-using-xpathdocument-and-xmldocument.md).  
  
## <a name="inserting-nodes"></a>Inserindo nós  
 A classe de <xref:System.Xml.XPath.XPathNavigator> fornece métodos para irmão, o filho, e nós de atributo de inserção em um documento XML. Esses métodos permitem que você insira nós e atributos em locais diferentes com relação a posição atual de <xref:System.Xml.XPath.XPathNavigator> objeto e são descritas nas seções.  
  
### <a name="inserting-sibling-nodes"></a>Inserindo nós irmãos  
 A classe de <xref:System.Xml.XPath.XPathNavigator> fornece os seguintes métodos para nós irmãos de inserção.  
  
- <xref:System.Xml.XPath.XPathNavigator.InsertAfter%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.InsertBefore%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.InsertElementAfter%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.InsertElementBefore%2A>  
  
 Esses nós irmãos de inserção dos métodos antes e após o nó um objeto de <xref:System.Xml.XPath.XPathNavigator> é posicionado atualmente.  
  
 Os métodos de <xref:System.Xml.XPath.XPathNavigator.InsertAfter%2A> e de <xref:System.Xml.XPath.XPathNavigator.InsertBefore%2A> são sobrecarregados e aceitam `string`, um objeto de <xref:System.Xml.XmlReader> , ou um objeto de <xref:System.Xml.XPath.XPathNavigator> que contém o nó irmãos para adicionar como parâmetros. Ambos os métodos também retornam um objeto de <xref:System.Xml.XmlWriter> usado para nós irmãos de inserção.  
  
 Os métodos de <xref:System.Xml.XPath.XPathNavigator.InsertElementAfter%2A> e de <xref:System.Xml.XPath.XPathNavigator.InsertElementBefore%2A> inserirem um único nó irmãos antes e após o nó que um objeto de <xref:System.Xml.XPath.XPathNavigator> é posicionado atualmente em usar o prefixo do namespace, o nome local, URI de namespace, e o valor especificado como parâmetros.  
  
 No exemplo a seguir um novo elemento de `pages` é inserido antes que o elemento filho de `price` do primeiro elemento de `book` no arquivo de `contosoBooks.xml` .  
  
 [!code-cpp[XPathNavigatorMethods#19](../../../../samples/snippets/cpp/VS_Snippets_Data/XPathNavigatorMethods/CPP/xpathnavigatormethods.cpp#19)]
 [!code-csharp[XPathNavigatorMethods#19](../../../../samples/snippets/csharp/VS_Snippets_Data/XPathNavigatorMethods/CS/xpathnavigatormethods.cs#19)]
 [!code-vb[XPathNavigatorMethods#19](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XPathNavigatorMethods/VB/xpathnavigatormethods.vb#19)]  
  
 O exemplo usa o arquivo `contosoBooks.xml` como entrada.  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
 Para obter mais informações sobre a <xref:System.Xml.XPath.XPathNavigator.InsertAfter%2A>, <xref:System.Xml.XPath.XPathNavigator.InsertBefore%2A>, <xref:System.Xml.XPath.XPathNavigator.InsertElementAfter%2A> e métodos de <xref:System.Xml.XPath.XPathNavigator.InsertElementBefore%2A> , consulte a documentação de referência da classe <xref:System.Xml.XPath.XPathNavigator> .  
  
### <a name="inserting-child-nodes"></a>Inserindo nós filho  
 A classe de <xref:System.Xml.XPath.XPathNavigator> fornece os seguintes métodos para nós filho de inserção.  
  
- <xref:System.Xml.XPath.XPathNavigator.AppendChild%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.PrependChild%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.AppendChildElement%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.PrependChildElement%2A>  
  
 Esses métodos append preceda e nós filho ao final de e a lista de nós filho do nó um objeto de <xref:System.Xml.XPath.XPathNavigator> é posicionado atualmente.  
  
 Como os métodos “que inserção na seção de nós irmãos”, <xref:System.Xml.XPath.XPathNavigator.AppendChild%2A> e métodos de <xref:System.Xml.XPath.XPathNavigator.PrependChild%2A> aceitam `string`, um objeto de <xref:System.Xml.XmlReader> , ou um objeto de <xref:System.Xml.XPath.XPathNavigator> que contém o nó filho para adicionar como parâmetros. Ambos os métodos também retornam um objeto de <xref:System.Xml.XmlWriter> usado para nós filho de inserção.  
  
 Também como os métodos “que inserção na seção de nós irmãos”, <xref:System.Xml.XPath.XPathNavigator.AppendChildElement%2A> e métodos de <xref:System.Xml.XPath.XPathNavigator.PrependChildElement%2A> inserirem um único nó filho e fim da lista de nós filho do nó um objeto de <xref:System.Xml.XPath.XPathNavigator> é posicionado atualmente em usar o prefixo de namespace, o nome local, o URI de namespace, e o valor especificado como parâmetros.  
  
 No exemplo a seguir, um novo elemento filho de `pages` é acrescentado à lista de elementos filho do primeiro elemento de `book` no arquivo de `contosoBooks.xml` .  
  
 [!code-cpp[XPathNavigatorMethods#2](../../../../samples/snippets/cpp/VS_Snippets_Data/XPathNavigatorMethods/CPP/xpathnavigatormethods.cpp#2)]
 [!code-csharp[XPathNavigatorMethods#2](../../../../samples/snippets/csharp/VS_Snippets_Data/XPathNavigatorMethods/CS/xpathnavigatormethods.cs#2)]
 [!code-vb[XPathNavigatorMethods#2](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XPathNavigatorMethods/VB/xpathnavigatormethods.vb#2)]  
  
 O exemplo usa o arquivo `contosoBooks.xml` como entrada.  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
 Para obter mais informações sobre a <xref:System.Xml.XPath.XPathNavigator.AppendChild%2A>, <xref:System.Xml.XPath.XPathNavigator.PrependChild%2A>, <xref:System.Xml.XPath.XPathNavigator.AppendChildElement%2A> e métodos de <xref:System.Xml.XPath.XPathNavigator.PrependChildElement%2A> , consulte a documentação de referência da classe <xref:System.Xml.XPath.XPathNavigator> .  
  
### <a name="inserting-attribute-nodes"></a>Inserindo nós de atributo  
 A classe de <xref:System.Xml.XPath.XPathNavigator> fornece os seguintes métodos para nós de atributo de inserção.  
  
- <xref:System.Xml.XPath.XPathNavigator.CreateAttribute%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.CreateAttributes%2A>  
  
 Esses nós de atributo de inserção dos métodos no nó do elemento um objeto de <xref:System.Xml.XPath.XPathNavigator> são posicionados atualmente. O método de <xref:System.Xml.XPath.XPathNavigator.CreateAttribute%2A> cria um nó de atributo no nó do elemento de <xref:System.Xml.XPath.XPathNavigator> um objeto é posicionado atualmente em usar o prefixo do namespace, o nome local, o URI de namespace, e o valor especificado como parâmetros. O método de <xref:System.Xml.XPath.XPathNavigator.CreateAttributes%2A> retorna um objeto de <xref:System.Xml.XmlWriter> usado para nós de atributo de inserção.  
  
 No exemplo a seguir, um novo `discount` e atributos de `currency` são criados no elemento filho de `price` do primeiro elemento de `book` no arquivo de `contosoBooks.xml` usando o objeto de <xref:System.Xml.XmlWriter> retornado pelo método de <xref:System.Xml.XPath.XPathNavigator.CreateAttributes%2A> .  
  
 [!code-cpp[XPathNavigatorMethods#8](../../../../samples/snippets/cpp/VS_Snippets_Data/XPathNavigatorMethods/CPP/xpathnavigatormethods.cpp#8)]
 [!code-csharp[XPathNavigatorMethods#8](../../../../samples/snippets/csharp/VS_Snippets_Data/XPathNavigatorMethods/CS/xpathnavigatormethods.cs#8)]
 [!code-vb[XPathNavigatorMethods#8](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XPathNavigatorMethods/VB/xpathnavigatormethods.vb#8)]  
  
 O exemplo usa o arquivo `contosoBooks.xml` como entrada.  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
 Para obter mais informações sobre métodos de <xref:System.Xml.XPath.XPathNavigator.CreateAttribute%2A> e de <xref:System.Xml.XPath.XPathNavigator.CreateAttributes%2A> , consulte a documentação de referência da classe <xref:System.Xml.XPath.XPathNavigator> .  
  
## <a name="copying-nodes"></a>Copiando nós  
 Em alguns casos você pode desejar preencher um documento XML com o conteúdo de um outro documento XML. Ambas a classe de <xref:System.Xml.XPath.XPathNavigator> e a classe de <xref:System.Xml.XmlWriter> podem copiar nós em um objeto de <xref:System.Xml.XmlDocument> de um objeto existente de <xref:System.Xml.XmlReader> ou do objeto de <xref:System.Xml.XPath.XPathNavigator> .  
  
 Todos <xref:System.Xml.XPath.XPathNavigator.AppendChild%2A>, <xref:System.Xml.XPath.XPathNavigator.PrependChild%2A>, <xref:System.Xml.XPath.XPathNavigator.InsertBefore%2A> e métodos de <xref:System.Xml.XPath.XPathNavigator.InsertAfter%2A> de <xref:System.Xml.XPath.XPathNavigator> classe tem sobrecargas que podem aceitar um objeto de <xref:System.Xml.XPath.XPathNavigator> ou um objeto de <xref:System.Xml.XmlReader> como um parâmetro.  
  
 O método de <xref:System.Xml.XmlWriter.WriteNode%2A> da classe de <xref:System.Xml.XmlWriter> possui sobrecargas que podem aceitar <xref:System.Xml.XmlNode>, <xref:System.Xml.XmlReader>, ou o objeto de <xref:System.Xml.XPath.XPathNavigator> .  
  
 O exemplo a seguir copia todos os elementos de `book` de um documento para outro.  
  
```vb  
Dim document As XmlDocument = New XmlDocument()  
document.Load("books.xml")  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
navigator.MoveToChild("bookstore", String.Empty)  
  
Dim newBooks As XPathDocument = New XPathDocument("newBooks.xml")  
Dim newBooksNavigator As XPathNavigator = newBooks.CreateNavigator()  
  
Dim nav As XPathNavigator  
For Each nav in newBooksNavigator.SelectDescendants("book", "", false)  
    navigator.AppendChild(nav)  
Next  
  
document.Save("newBooks.xml");  
```  
  
```csharp  
XmlDocument document = new XmlDocument();  
document.Load("books.xml");  
XPathNavigator navigator = document.CreateNavigator();  
  
navigator.MoveToChild("bookstore", String.Empty);  
  
XPathDocument newBooks = new XPathDocument("newBooks.xml");  
XPathNavigator newBooksNavigator = newBooks.CreateNavigator();  
  
foreach (XPathNavigator nav in newBooksNavigator.SelectDescendants("book", "", false))  
{  
    navigator.AppendChild(nav);  
}  
  
document.Save("newBooks.xml");  
```  
  
## <a name="inserting-values"></a>Inserindo valores  
 A classe de <xref:System.Xml.XPath.XPathNavigator> fornece <xref:System.Xml.XPath.XPathNavigator.SetValue%2A> e métodos de <xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A> a valores de inserção para um nó em <xref:System.Xml.XmlDocument> objeto.  
  
### <a name="inserting-untyped-values"></a>Inserindo valores sem tipo  
 O método <xref:System.Xml.XPath.XPathNavigator.SetValue%2A> simplesmente insere o valor sem tipo de `string` passado como um parâmetro como o valor do nó no qual o objeto <xref:System.Xml.XPath.XPathNavigator> está posicionado no momento. O valor é inserido sem nenhum tipo ou sem verificar se o novo valor é válido de acordo com o tipo de nó se as informações do esquema estiverem disponíveis.  
  
 No exemplo a seguir, o método <xref:System.Xml.XPath.XPathNavigator.SetValue%2A> é usado para atualizar todos os elementos `price` no arquivo `contosoBooks.xml`.  
  
 [!code-cpp[XPathNavigatorMethods#47](../../../../samples/snippets/cpp/VS_Snippets_Data/XPathNavigatorMethods/CPP/xpathnavigatormethods.cpp#47)]
 [!code-csharp[XPathNavigatorMethods#47](../../../../samples/snippets/csharp/VS_Snippets_Data/XPathNavigatorMethods/CS/xpathnavigatormethods.cs#47)]
 [!code-vb[XPathNavigatorMethods#47](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XPathNavigatorMethods/VB/xpathnavigatormethods.vb#47)]  
  
 O exemplo usa o arquivo `contosoBooks.xml` como entrada.  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
### <a name="inserting-typed-values"></a>Inserindo valores tipados  
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
  
## <a name="the-innerxml-and-outerxml-properties"></a>As propriedades de InnerXml e de OuterXml  
 As propriedades <xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> e <xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> da classe <xref:System.Xml.XPath.XPathNavigator> alteram a marcação XML dos nós nos quais um objeto <xref:System.Xml.XPath.XPathNavigator> está posicionado no momento.  
  
 A propriedade <xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> altera a marcação XML dos nós filho no qual um objeto <xref:System.Xml.XPath.XPathNavigator> está posicionado no momento com o conteúdo analisado da `string` do XML determinada. Da mesma maneira, a propriedade <xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> altera a marcação XML dos nós filho no qual um objeto <xref:System.Xml.XPath.XPathNavigator> está posicionado no momento além do próprio nó atual.  
  
 Além dos métodos descritos neste tópico, <xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> e as propriedades de <xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> podem ser usados para inserir nós e valores em um documento XML. Para saber mais sobre como usar as propriedades <xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> e <xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> para inserir nós e valores, confira o tópico [Modificar dados XML usando XPathNavigator](modify-xml-data-using-xpathnavigator.md).  
  
## <a name="namespace-and-xmllang-conflicts"></a>Namespace e XML: conflitos de idioma  
 Alguns conflitos relacionados ao escopo de namespace e declarações de `xml:lang` podem ocorrer ao inserir dados XML usando <xref:System.Xml.XPath.XPathNavigator.InsertBefore%2A>, <xref:System.Xml.XPath.XPathNavigator.InsertAfter%2A>, <xref:System.Xml.XPath.XPathNavigator.AppendChild%2A> e métodos de <xref:System.Xml.XPath.XPathNavigator.PrependChild%2A> de <xref:System.Xml.XPath.XPathNavigator> classe que utiliza objetos de <xref:System.Xml.XmlReader> como parâmetros.  
  
 Estes são os conflitos possíveis de namespace.  
  
- Se houver um em- escopo de namespace no contexto de objeto de <xref:System.Xml.XmlReader> , onde o prefixo para mapear URI de namespace não está no contexto de objeto de <xref:System.Xml.XPath.XPathNavigator> , uma nova declaração de namespace é adicionada ao nó recentemente inserido.  
  
- Se o mesmo URI de namespace é em- escopo dentro do contexto de objeto de <xref:System.Xml.XmlReader> e o contexto de objeto de <xref:System.Xml.XPath.XPathNavigator> , mas tem um prefixo diferente mapeado a ele em ambos os contextos, uma nova declaração de namespace é adicionada ao nó recentemente inserido, com o prefixo e o URI de namespace tirado de <xref:System.Xml.XmlReader> objetos.  
  
- Se o mesmo prefixo de namespace é em- escopo dentro do contexto de objeto de <xref:System.Xml.XmlReader> e o contexto de objeto de <xref:System.Xml.XPath.XPathNavigator> , mas tem URI diferente de um namespace mapeado a ele em ambos os contextos, uma nova declaração de namespace é adicionada ao nó recentemente inserido que a declara que um URI com o prefixo do namespace extraído do objeto de <xref:System.Xml.XmlReader> .  
  
- Se o prefixo bem como o URI de namespace no contexto de objeto de <xref:System.Xml.XmlReader> e no contexto de objeto de <xref:System.Xml.XPath.XPathNavigator> são o mesmo, nenhuma nova declaração de namespace é adicionada ao nó recentemente inserido.  
  
> [!NOTE]
> A descrição anterior também se aplica às declarações namespace com `string` vazia como um prefixo (por exemplo, a declaração de namespace padrão).  
  
 Estes são os conflitos possíveis de `xml:lang` .  
  
- Se houver um em- escopo de atributo de `xml:lang` dentro do contexto de objeto de <xref:System.Xml.XmlReader> mas não no contexto de objeto de <xref:System.Xml.XPath.XPathNavigator> , um atributo de `xml:lang` cujo valor é extraído do objeto de <xref:System.Xml.XmlReader> é adicionado ao nó recentemente inserido.  
  
- Se houver um em- escopo de atributo de `xml:lang` dentro do contexto de objeto de <xref:System.Xml.XmlReader> e o contexto de objeto de <xref:System.Xml.XPath.XPathNavigator> , mas cada um possui um valor diferente, um atributo de `xml:lang` cujo valor é extraído do objeto de <xref:System.Xml.XmlReader> é adicionado ao nó recentemente inserido.  
  
- Se houver um em- escopo de atributo de `xml:lang` dentro do contexto de objeto de <xref:System.Xml.XmlReader> e o contexto de objeto de <xref:System.Xml.XPath.XPathNavigator> , mas cada um com o mesmo valor, nenhum novo atributo de `xml:lang` é adicionado no nó recentemente inserido.  
  
- Se houver um em- escopo de atributo de `xml:lang` dentro do contexto de objeto de <xref:System.Xml.XPath.XPathNavigator> , mas nenhum que existem no contexto de objeto de <xref:System.Xml.XmlReader> , nenhum atributo de `xml:lang` é adicionado ao nó recentemente inserido.  
  
## <a name="inserting-nodes-with-xmlwriter"></a>Inserindo nós com XmlWriter  
 Os métodos usados para inserir o irmão, o filho e os nós de atributo “inserindo descritos na seção de nós e valores são sobrecarregados.” <xref:System.Xml.XPath.XPathNavigator.InsertAfter%2A>, <xref:System.Xml.XPath.XPathNavigator.InsertBefore%2A>, <xref:System.Xml.XPath.XPathNavigator.AppendChild%2A>, <xref:System.Xml.XPath.XPathNavigator.PrependChild%2A> e métodos de <xref:System.Xml.XPath.XPathNavigator.CreateAttributes%2A> da classe de <xref:System.Xml.XPath.XPathNavigator> retornam um objeto de <xref:System.Xml.XmlWriter> usado para nós de inserção.  
  
### <a name="unsupported-xmlwriter-methods"></a>Métodos sem suporte de XmlWriter  
 Nem todos os métodos usados para gravar informações em um documento XML que usa a classe de <xref:System.Xml.XmlWriter> são suportados pela classe de <xref:System.Xml.XPath.XPathNavigator> devido a diferença entre o modelo de dados XPath e o Document Object Model (DOM).  
  
 A tabela a seguir descreve os métodos da classe <xref:System.Xml.XmlWriter> não suportados pela classe de <xref:System.Xml.XPath.XPathNavigator> .  
  
|Método|Descrição|  
|------------|-----------------|  
|<xref:System.Xml.XmlWriter.WriteEntityRef%2A>|Gerencie uma exceção de <xref:System.NotSupportedException> .|  
|<xref:System.Xml.XmlWriter.WriteDocType%2A>|Ignorado no nível raiz e em gera uma exceção de <xref:System.NotSupportedException> se chamado em qualquer outro o nível no documento XML.|  
|<xref:System.Xml.XmlWriter.WriteCData%2A>|Tratado como uma chamada para o método de <xref:System.Xml.XmlWriter.WriteString%2A> para o caractere ou os caracteres equivalentes.|  
|<xref:System.Xml.XmlWriter.WriteCharEntity%2A>|Tratado como uma chamada para o método de <xref:System.Xml.XmlWriter.WriteString%2A> para o caractere ou os caracteres equivalentes.|  
|<xref:System.Xml.XmlWriter.WriteSurrogateCharEntity%2A>|Tratado como uma chamada para o método de <xref:System.Xml.XmlWriter.WriteString%2A> para o caractere ou os caracteres equivalentes.|  
  
 Para obter mais informações sobre a classe de <xref:System.Xml.XmlWriter> , consulte a documentação de referência da classe <xref:System.Xml.XmlWriter> .  
  
### <a name="multiple-xmlwriter-objects"></a>Vários objetos de XmlWriter  
 É possível ter vários objetos de <xref:System.Xml.XPath.XPathNavigator> que apontam para partes diferentes de um documento XML com um ou mais objetos abertos de <xref:System.Xml.XmlWriter> . Vários objetos de <xref:System.Xml.XmlWriter> são permitidos e suportados em cenários de único thread.  
  
 Os seguintes são notas importantes a considerar ao usar vários <xref:System.Xml.XmlWriter> objeto.  
  
- Os fragmentos XML escritos por objetos de <xref:System.Xml.XmlWriter> são adicionados ao documento XML quando o método de <xref:System.Xml.XmlWriter.Close%2A> de cada objeto de <xref:System.Xml.XmlWriter> é chamado. Até esse ponto, o objeto de <xref:System.Xml.XmlWriter> estiver escrevendo um fragmento desconectados. Se uma operação é executada no documento XML, quaisquer informações que estão sendo gravados por um objeto de <xref:System.Xml.XmlWriter> , antes que <xref:System.Xml.XmlWriter.Close%2A> é chamado, não são afetados.  
  
- Se houver um objeto de abertura de <xref:System.Xml.XmlWriter> em uma subárvore específico XML e a subárvore é excluída, o objeto de <xref:System.Xml.XmlWriter> ainda pode adicionar à subárvore. A subárvore transformações somente um fragmento excluído.  
  
- Se vários objetos de <xref:System.Xml.XmlWriter> são abertos no mesmo ponto no documento XML, são adicionados ao documento XML na ordem em que os objetos de <xref:System.Xml.XmlWriter> são fechados, não na ordem em que foram abertos.  
  
 O seguinte exemplo cria um objeto de <xref:System.Xml.XmlDocument> , cria um objeto de <xref:System.Xml.XPath.XPathNavigator> em seguida, usa o objeto de <xref:System.Xml.XmlWriter> retornado pelo método de <xref:System.Xml.XPath.XPathNavigator.PrependChild%2A> para criar a estrutura do primeiro livro no arquivo de `books.xml` . O exemplo salvá-los em como o arquivo de `book.xml` .  
  
```vb  
Dim document As XmlDocument = New XmlDocument()  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
Using writer As XmlWriter = navigator.PrependChild()  
  
    writer.WriteStartElement("bookstore")  
    writer.WriteStartElement("book")  
    writer.WriteAttributeString("genre", "autobiography")  
    writer.WriteAttributeString("publicationdate", "1981-03-22")  
    writer.WriteAttributeString("ISBN", "1-861003-11-0")  
    writer.WriteElementString("title", "The Autobiography of Benjamin Franklin")  
    writer.WriteStartElement("author")  
    writer.WriteElementString("first-name", "Benjamin")  
    writer.WriteElementString("last-name", "Franklin")  
    writer.WriteElementString("price", "8.99")  
    writer.WriteEndElement()  
    writer.WriteEndElement()  
    writer.WriteEndElement()  
  
End Using  
  
document.Save("book.xml")  
```  
  
```csharp  
XmlDocument document = new XmlDocument();  
XPathNavigator navigator = document.CreateNavigator();  
  
using (XmlWriter writer = navigator.PrependChild())  
{  
    writer.WriteStartElement("bookstore");  
    writer.WriteStartElement("book");  
    writer.WriteAttributeString("genre", "autobiography");  
    writer.WriteAttributeString("publicationdate", "1981-03-22");  
    writer.WriteAttributeString("ISBN", "1-861003-11-0");  
    writer.WriteElementString("title", "The Autobiography of Benjamin Franklin");  
    writer.WriteStartElement("author");  
    writer.WriteElementString("first-name", "Benjamin");  
    writer.WriteElementString("last-name", "Franklin");  
    writer.WriteElementString("price", "8.99");  
    writer.WriteEndElement();  
    writer.WriteEndElement();  
    writer.WriteEndElement();  
}  
document.Save("book.xml");  
```  
  
## <a name="saving-an-xml-document"></a>Salvando um documento XML  
 Salvando as alterações feitas a um objeto de <xref:System.Xml.XmlDocument> como resultado dos métodos descritos neste tópico é executado usando os métodos da classe <xref:System.Xml.XmlDocument> . Para saber mais sobre como salvar as alterações feitas em um objeto <xref:System.Xml.XmlDocument>, confira [Salvar e gravar um documento](saving-and-writing-a-document.md).  
  
## <a name="see-also"></a>Veja também

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [Processar dados XML usando o modelo de dados XPath](process-xml-data-using-the-xpath-data-model.md)
- [Modificar dados XML usando XPathNavigator](modify-xml-data-using-xpathnavigator.md)
- [Remova os dados XML usando XPathNavigator](remove-xml-data-using-xpathnavigator.md)
