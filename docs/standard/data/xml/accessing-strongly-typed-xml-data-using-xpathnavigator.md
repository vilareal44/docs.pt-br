---
title: Acessando dados fortemente tipados XML usando XPathNavigator
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 898e0f52-8a7c-4d1f-afcd-6ffb28b050b4
ms.openlocfilehash: 61957ff88ef57703aff1861238ee10b23c2f16ff
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84291598"
---
# <a name="accessing-strongly-typed-xml-data-using-xpathnavigator"></a>Acessando dados fortemente tipados XML usando XPathNavigator
Como uma instância do modelo de dados XPath 2,0, a <xref:System.Xml.XPath.XPathNavigator> classe pode conter dados fortemente tipados que mapeiam para tipos Common Language Runtime (CLR). De acordo com o modelo de dados XPath 2,0, somente elementos e atributos podem conter dados fortemente tipados. A <xref:System.Xml.XPath.XPathNavigator> classe fornece mecanismos para acessar dados dentro de <xref:System.Xml.XPath.XPathDocument> um <xref:System.Xml.XmlDocument> objeto ou como dados fortemente tipados, bem como mecanismos para conversão de um tipo de dados para outro.  
  
## <a name="type-information-exposed-by-xpathnavigator"></a>Informações de tipo expostos por XPathNavigator  
 XML 1,0 dados é tecnicamente sem tipo, a menos que processado com um DTD, o esquema de linguagem de definição de esquema XML (XSD), ou outro mecanismo. Há um número de categorias das informações que podem ser associadas com um elemento XML ou um atributo.  
  
- Tipos simples de CLR: Nenhum de suporte a Common Language Runtime (CLR) linguagens de esquema tipos XML diretamente. Porque é útil poder exibir conteúdo simples de elementos e atributos porque os a maioria exibe o tipo de CLR, todo o conteúdo simples pode ser digitado como <xref:System.String> na ausência de informações de esquema com qualquer adicionadas informações de esquema que limita potencialmente esse conteúdo a um tipo apropriado. Você pode localizar o melhor tipo correspondente de CLR de conteúdo simples de elementos e atributos usando a propriedade de <xref:System.Xml.XPath.XPathNavigator.ValueType%2A> . Para saber mais sobre o mapeamento de tipos internos de esquema para tipos de CLR, confira [Suporte a tipo nas classes System.Xml](type-support-in-the-system-xml-classes.md).  
  
- Listas de tipos simples (de CLR): Um elemento ou atributo com conteúdo simples podem conter uma lista de valores separados por um espaço em branco. Os valores são especificados por um esquema XML para ser de um tipo “lista.” Na ausência de um esquema XML, tal conteúdo simples seria tratado como um único nó de texto. Quando um esquema XML está disponível, esse conteúdo simples pode ser exposta como uma série de valores atômicos cada um que tenha um tipo simples que mapeia a uma coleção de objetos CLR. Para saber mais sobre o mapeamento de tipos internos de esquema para tipos de CLR, confira [Suporte a tipo nas classes System.Xml](type-support-in-the-system-xml-classes.md).  
  
- Valor tipado: Um atributo ou um elemento esquema- validado com um tipo simples têm um valor tipado. Esse valor é um tipo primitivo como um numérico, uma cadeia de caracteres, ou um tipo de dados. Todos os tipos simples internos em XSD podem ser mapeados para os tipos de CLR de que fornece acesso ao valor de um nó como um tipo mais apropriado em vez de apenas como <xref:System.String>. Um elemento com atributos ou filhos do elemento é considerado ser um tipo complexo. O valor tipado de um tipo complexo com conteúdo simples (somente os nós de texto como filhos) é o mesmo que o do tipo simples de seu conteúdo. O valor tipado de um tipo complexo com conteúdo complexo (um ou mais elementos filho) é o valor da cadeia de caracteres de concatenação de todos os nós filhos de texto retornados como <xref:System.String>. Para saber mais sobre o mapeamento de tipos internos de esquema para tipos de CLR, confira [Suporte a tipo nas classes System.Xml](type-support-in-the-system-xml-classes.md).  
  
- Nome do tipo específico de linguagem Esquema-: Na maioria dos casos, os tipos de CLR, que são definidos como um efeito colateral de aplicar um esquema externo, são usados para fornecer acesso ao valor de um nó. No entanto, pode haver situações onde você pode querer examinar o tipo associado com um esquema específico aplicado a um documento XML. Por exemplo, você pode desejar pesquisar por um documento XML, extraindo todos os elementos que são determinados ter o conteúdo do tipo “PurchaseOrder” de acordo com o esquema anexado. Essas informações de tipo pode ser definida somente como resultado de validação de esquema e essa informação é acessada com <xref:System.Xml.XPath.XPathNavigator.XmlType%2A> e as propriedades de <xref:System.Xml.XPath.XPathNavigator.SchemaInfo%2A> de <xref:System.Xml.XPath.XPathNavigator> classe. Para obter mais informações, consulte a seção de Infoset (PSVI) de validação do esquema de postagem abaixo.  
  
- Reflexão do tipo específico de linguagem Esquema-: Em outros casos, você pode desejar obter mais detalhes de tipo específico esquema- aplicado a um documento XML. Por exemplo, ao ler um arquivo XML, você pode querer extrair o atributo de `maxOccurs` para cada nó válido no documento XML para executar um cálculo personalizado. Porque essa informação é definida apenas com a validação do esquema, é acessada através da propriedade de <xref:System.Xml.XPath.XPathNavigator.SchemaInfo%2A> da classe de <xref:System.Xml.XPath.XPathNavigator> . Para obter mais informações, consulte a seção de Infoset (PSVI) de validação do esquema de postagem abaixo.  
  
## <a name="xpathnavigator-typed-accessors"></a>XPathNavigator digitou acessadores  
 A tabela a seguir mostra as várias propriedades e métodos de <xref:System.Xml.XPath.XPathNavigator> classe que pode ser usado para acessar informações de tipo sobre um nó.  
  
|Propriedade|Descrição|  
|--------------|-----------------|  
|<xref:System.Xml.XPath.XPathNavigator.XmlType%2A>|Isso contém informações de tipo o esquema XML para o nó se é válido.|  
|<xref:System.Xml.XPath.XPathNavigator.SchemaInfo%2A>|Isso contém a validação Infoset do esquema de postagem de nó que é adicionado após a validação. Isso inclui informações de tipo do esquema XML, bem como informações de validade.|  
|<xref:System.Xml.XPath.XPathNavigator.ValueType%2A>|O tipo de CLR de valor tipado de nó.|  
|<xref:System.Xml.XPath.XPathNavigator.TypedValue%2A>|O conteúdo do nó como um ou mais valores de CLR cujo tipo é mais próximo correspondente ao tipo de esquema XML de nó.|  
|<xref:System.Xml.XPath.XPathNavigator.ValueAsBoolean%2A>|O valor de <xref:System.String> de conversão atual do nó com um valor de <xref:System.Boolean> , de acordo com o XPath 2,0 regras para converter `xs:boolean`.|  
|<xref:System.Xml.XPath.XPathNavigator.ValueAsDateTime%2A>|O valor de <xref:System.String> de conversão atual do nó com um valor de <xref:System.DateTime> , de acordo com o XPath 2,0 regras para converter `xs:datetime`.|  
|<xref:System.Xml.XPath.XPathNavigator.ValueAsDouble%2A>|O valor de <xref:System.String> de conversão atual do nó com um valor de <xref:System.Double> , de acordo com o XPath 2,0 regras para converter `xsd:double`.|  
|<xref:System.Xml.XPath.XPathNavigator.ValueAsInt%2A>|O valor de <xref:System.String> de conversão atual do nó com um valor de <xref:System.Int32> , de acordo com o XPath 2,0 regras para converter `xs:integer`.|  
|<xref:System.Xml.XPath.XPathNavigator.ValueAsLong%2A>|O valor de <xref:System.String> de conversão atual do nó com um valor de <xref:System.Int64> , de acordo com o XPath 2,0 regras para converter `xs:integer`.|  
|<xref:System.Xml.XPath.XPathNavigator.ValueAs%2A>|O conteúdo do nó convertem ao tipo de destino de acordo com o XPath 2,0 regras converter.|  
  
 Para saber mais sobre o mapeamento de tipos internos de esquema para tipos de CLR, confira [Suporte a tipo nas classes System.Xml](type-support-in-the-system-xml-classes.md).  
  
## <a name="the-post-schema-validation-infoset-psvi"></a>A validação Infoset (PSVI) do esquema de postagem  
 Um processador XML de esquema XML Infoset aceita como entrada e o converte em uma validação Infoset (PSVI) do esquema de postagem. Um PSVI é o infoset original XML de entrada com novos elementos de informações adicionados e as novas propriedades adicionadas a elementos existentes de informações. Há três classes de informações de adicionadas a Infoset XML em PSVI que são expostos por <xref:System.Xml.XPath.XPathNavigator>.  
  
1. Resultados de validação: Informações se um elemento ou atributo estiverams bem-sucedida ou não. Isso expõe a propriedade de <xref:System.Xml.Schema.IXmlSchemaInfo.Validity%2A> de propriedade de <xref:System.Xml.XPath.XPathNavigator.SchemaInfo%2A> da classe de <xref:System.Xml.XPath.XPathNavigator> .  
  
2. Informações padrão: Em se o valor de elemento ou atributo foi obtido através dos valores padrão especificado no esquema ou não. Isso expõe a propriedade de <xref:System.Xml.Schema.IXmlSchemaInfo.IsDefault%2A> de propriedade de <xref:System.Xml.XPath.XPathNavigator.SchemaInfo%2A> da classe de <xref:System.Xml.XPath.XPathNavigator> .  
  
3. Anotações do tipo: Referências a componentes do esquema que podem ser definições de tipo ou declarações de elementos e atributos. A propriedade de <xref:System.Xml.XPath.XPathNavigator.XmlType%2A> de <xref:System.Xml.XPath.XPathNavigator> contém informações de tipo específico de nó se é válido. Se a validade de um nó é desconhecida, como quando ele foi validado em editou posteriormente. a propriedade de <xref:System.Xml.XPath.XPathNavigator.XmlType%2A> é definida na `null` mas informações de tipo está disponível ainda das várias propriedades de propriedade de <xref:System.Xml.XPath.XPathNavigator.SchemaInfo%2A> da classe de <xref:System.Xml.XPath.XPathNavigator> .  
  
 O exemplo a seguir ilustra usando as informações na validação Infoset do esquema de postagem expostos por <xref:System.Xml.XPath.XPathNavigator>.  
  
```vb  
Dim settings As XmlReaderSettings = New XmlReaderSettings()  
settings.Schemas.Add("http://www.contoso.com/books", "books.xsd")  
settings.ValidationType = ValidationType.Schema  
  
Dim reader As XmlReader = XmlReader.Create("books.xml", settings)  
  
Dim document As XmlDocument = New XmlDocument()  
document.Load(reader)  
Dim navigator As XPathNavigator = document.CreateNavigator()  
navigator.MoveToChild("books", "http://www.contoso.com/books")  
navigator.MoveToChild("book", "http://www.contoso.com/books")  
navigator.MoveToChild("published", "http://www.contoso.com/books")  
  
Console.WriteLine(navigator.SchemaInfo.SchemaType.Name)  
Console.WriteLine(navigator.SchemaInfo.Validity)  
Console.WriteLine(navigator.SchemaInfo.SchemaElement.MinOccurs)  
```  
  
```csharp  
XmlReaderSettings settings = new XmlReaderSettings();  
settings.Schemas.Add("http://www.contoso.com/books", "books.xsd");  
settings.ValidationType = ValidationType.Schema;  
  
XmlReader reader = XmlReader.Create("books.xml", settings);  
  
XmlDocument document = new XmlDocument();  
document.Load(reader);  
XPathNavigator navigator = document.CreateNavigator();  
navigator.MoveToChild("books", "http://www.contoso.com/books");  
navigator.MoveToChild("book", "http://www.contoso.com/books");  
navigator.MoveToChild("published", "http://www.contoso.com/books");  
  
Console.WriteLine(navigator.SchemaInfo.SchemaType.Name);  
Console.WriteLine(navigator.SchemaInfo.Validity);  
Console.WriteLine(navigator.SchemaInfo.SchemaElement.MinOccurs);  
```  
  
 O exemplo usa o arquivo `books.xml` como entrada.  
  
```xml  
<books xmlns="http://www.contoso.com/books">  
    <book>  
        <title>Title</title>  
        <price>10.00</price>  
        <published>2003-12-31</published>  
    </book>  
</books>  
```  
  
 O exemplo também usa o esquema de `books.xsd` como entrada.  
  
```xml  
<xs:schema xmlns="http://www.contoso.com/books"
attributeFormDefault="unqualified" elementFormDefault="qualified"
targetNamespace="http://www.contoso.com/books"
xmlns:xs="http://www.w3.org/2001/XMLSchema">  
    <xs:simpleType name="publishedType">  
        <xs:restriction base="xs:date">  
            <xs:minInclusive value="2003-01-01" />  
            <xs:maxInclusive value="2003-12-31" />  
        </xs:restriction>  
    </xs:simpleType>  
    <xs:complexType name="bookType">  
        <xs:sequence>  
            <xs:element name="title" type="xs:string"/>  
            <xs:element name="price" type="xs:decimal"/>  
            <xs:element name="published" type="publishedType"/>  
        </xs:sequence>  
    </xs:complexType>  
    <xs:complexType name="booksType">  
        <xs:sequence>  
            <xs:element name="book" type="bookType" />  
        </xs:sequence>  
    </xs:complexType>  
    <xs:element name="books" type="booksType" />  
</xs:schema>  
```  
  
## <a name="obtain-typed-values-using-valueas-properties"></a>Obtenha valores tipados usando propriedades de ValueAs  
 O valor tipado de um nó pode ser recuperado acessando a propriedade de <xref:System.Xml.XPath.XPathNavigator.TypedValue%2A> de <xref:System.Xml.XPath.XPathNavigator>. Em alguns casos você pode querer converter o valor tipado de um nó para um tipo diferente. Um exemplo comum é obter um valor numérico de um nó XML. Por exemplo, considere o seguinte documento XML unvalidated e sem tipo.  
  
```xml  
<books>  
    <book>  
        <title>Title</title>  
        <price>10.00</price>  
        <published>2003-12-31</published>  
    </book>  
</books>  
```  
  
 Se <xref:System.Xml.XPath.XPathNavigator> é posicionado no elemento de `price` a propriedade de <xref:System.Xml.XPath.XPathNavigator.XmlType%2A> seria `null`, a propriedade de <xref:System.Xml.XPath.XPathNavigator.ValueType%2A> seria <xref:System.String>, e a propriedade <xref:System.Xml.XPath.XPathNavigator.TypedValue%2A> seria a cadeia de caracteres `10.00`.  
  
 No entanto, ainda é possível extrair o valor como um valor numérico usando <xref:System.Xml.XPath.XPathItem.ValueAs%2A>, <xref:System.Xml.XPath.XPathNavigator.ValueAsDouble%2A>, <xref:System.Xml.XPath.XPathNavigator.ValueAsInt%2A>, ou métodos e propriedades de <xref:System.Xml.XPath.XPathNavigator.ValueAsLong%2A> . O exemplo a seguir ilustra executar uma conversão usando o método <xref:System.Xml.XPath.XPathItem.ValueAs%2A> .  
  
```vb  
Dim document As New XmlDocument()  
document.Load("books.xml")  
Dim navigator As XPathNavigator = document.CreateNavigator()  
navigator.MoveToChild("books", "")  
navigator.MoveToChild("book", "")  
navigator.MoveToChild("price", "")  
  
Dim price = navigator.ValueAs(GetType(Decimal))  
Dim discount As Decimal = 0.2  
  
Console.WriteLine("The price of the book has been dropped 20% from {0:C} to {1:C}", navigator.Value, (price - price * discount))  
```  
  
```csharp  
XmlDocument document = new XmlDocument();  
document.Load("books.xml");  
XPathNavigator navigator = document.CreateNavigator();  
navigator.MoveToChild("books", "");  
navigator.MoveToChild("book", "");  
navigator.MoveToChild("price", "");  
  
Decimal price = (decimal)navigator.ValueAs(typeof(decimal));  
  
Console.WriteLine("The price of the book has been dropped 20% from {0:C} to {1:C}", navigator.Value, (price - price * (decimal)0.20));  
```  
  
 Para saber mais sobre o mapeamento de tipos internos de esquema para tipos de CLR, confira [Suporte a tipo nas classes System.Xml](type-support-in-the-system-xml-classes.md).  
  
## <a name="see-also"></a>Veja também

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [Digite suporte nas classes de System.Xml](type-support-in-the-system-xml-classes.md)
- [Processar dados XML usando o modelo de dados XPath](process-xml-data-using-the-xpath-data-model.md)
- [Navegação do nó usando XPathNavigator](node-set-navigation-using-xpathnavigator.md)
- [Navegação do nó de atributo e do namespace usando XPathNavigator](attribute-and-namespace-node-navigation-using-xpathnavigator.md)
- [Extrair dados XML usando XPathNavigator](extract-xml-data-using-xpathnavigator.md)
