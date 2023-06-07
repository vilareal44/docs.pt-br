---
title: Validação de esquema usando XPathNavigator
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 81fa0e41-d9c9-46f0-b22b-50da839c77f5
ms.openlocfilehash: f6e56616543bf7d2ad2e6be4d7bf7cbc50ba3a23
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84292001"
---
# <a name="schema-validation-using-xpathnavigator"></a>Validação de esquema usando XPathNavigator
Se você usar a classe <xref:System.Xml.XmlDocument>, poderá validar o conteúdo XML contido em um objeto <xref:System.Xml.XmlDocument> de duas maneiras. A primeira maneira é validar o conteúdo XML usando um objeto <xref:System.Xml.XmlReader> de validação, e a segunda maneira é usar o método <xref:System.Xml.XmlDocument.Validate%2A> da classe <xref:System.Xml.XmlDocument>. Você também pode executar a validação somente leitura do conteúdo XML usando a classe <xref:System.Xml.XPath.XPathDocument>.  
  
## <a name="validating-xml-data"></a>Validando dados XML  
 A classe <xref:System.Xml.XmlDocument> não valida um documento XML usando validação de esquema DTD nem XSD (linguagem de definição de esquema XML) por padrão. Ela só verifica se o documento XML está bem-formado.  
  
 A primeira maneira de validar um documento XML é validar o documento enquanto ele é carregado para um objeto <xref:System.Xml.XmlDocument> usando um objeto <xref:System.Xml.XmlReader> de validação. A segunda maneira é validar um documento XML anteriormente não tipado usando o método <xref:System.Xml.XmlDocument.Validate%2A> da classe <xref:System.Xml.XmlDocument>. Nos dois casos, as alterações no documento XML validado podem ser revalidadas usando o método <xref:System.Xml.XmlDocument.Validate%2A> da classe <xref:System.Xml.XmlDocument>.  
  
### <a name="validating-a-document-as-it-is-loaded"></a>Validando um documento enquanto ele é carregado  
 Um objeto <xref:System.Xml.XmlReader> de validação é criado passando um objeto <xref:System.Xml.XmlReaderSettings> para o método <xref:System.Xml.XmlReader.Create%2A> da classe <xref:System.Xml.XmlReader> que usa um objeto <xref:System.Xml.XmlReaderSettings> como parâmetro. O objeto <xref:System.Xml.XmlReaderSettings> passado como parâmetro tem uma propriedade <xref:System.Xml.XmlReaderSettings.ValidationType%2A> definida como `Schema` e um esquema XML para o documento XML contido no objeto <xref:System.Xml.XmlDocument> adicionado à sua propriedade <xref:System.Xml.XmlReaderSettings.Schemas%2A>. O objeto <xref:System.Xml.XmlReader> de validação é usado para criar o objeto <xref:System.Xml.XmlDocument>.  
  
 O exemplo a seguir valida o arquivo `contosoBooks.xml` enquanto ele é carregado para o objeto <xref:System.Xml.XmlDocument> criando o objeto <xref:System.Xml.XmlDocument> que usa um objeto <xref:System.Xml.XmlReader> de validação. Como o documento XML é válido de acordo com o esquema, nenhum erro ou aviso de validação de esquema é gerado.  
  
```vb  
Imports System  
Imports System.Xml  
Imports System.Xml.Schema  
Imports System.Xml.XPath  
  
Class ValidatingReaderExample  
  
    Shared Sub Main(ByVal args() As String)  
  
        Try  
            Dim settings As XmlReaderSettings = New XmlReaderSettings()  
            settings.Schemas.Add("http://www.contoso.com/books", "contosoBooks.xsd")  
            settings.ValidationType = ValidationType.Schema  
  
            Dim reader As XmlReader = XmlReader.Create("contosoBooks.xml", settings)  
  
            Dim document As XmlDocument = New XmlDocument()  
            document.Load(reader)  
            Dim navigator As XPathNavigator = document.CreateNavigator()  
  
        Catch e As Exception  
            Console.WriteLine("ValidatingReaderExample.Exception: {0}", e.Message)  
        End Try  
  
    End Sub  
  
    Shared Sub SchemaValidationHandler(ByVal sender As Object, ByVal e As ValidationEventArgs)  
  
        Select Case e.Severity  
            Case XmlSeverityType.Error  
                Console.WriteLine("Schema Validation Error: {0}", e.Message)  
                Exit Sub  
            Case XmlSeverityType.Warning  
                Console.WriteLine("Schema Validation Warning: {0}", e.Message)  
                Exit Sub  
        End Select  
  
    End Sub  
  
End Class  
```  
  
```csharp  
using System;  
using System.Xml;  
using System.Xml.Schema;  
using System.Xml.XPath;  
  
class ValidatingReaderExample  
{  
    static void Main(string[] args)  
    {  
        try  
        {  
            XmlReaderSettings settings = new XmlReaderSettings();  
            settings.Schemas.Add("http://www.contoso.com/books", "contosoBooks.xsd");  
            settings.ValidationType = ValidationType.Schema;  
  
            XmlReader reader = XmlReader.Create("contosoBooks.xml", settings);  
  
            XmlDocument document = new XmlDocument();  
            document.Load(reader);  
            XPathNavigator navigator = document.CreateNavigator();  
        }  
        catch (Exception e)  
        {  
            Console.WriteLine("ValidatingReaderExample.Exception: {0}", e.Message);  
        }  
    }  
  
    static void SchemaValidationHandler(object sender, ValidationEventArgs e)  
    {  
        switch (e.Severity)  
        {  
            case XmlSeverityType.Error:  
                Console.WriteLine("Schema Validation Error: {0}", e.Message);  
                break;  
            case XmlSeverityType.Warning:  
                Console.WriteLine("Schema Validation Warning: {0}", e.Message);  
                break;  
        }  
    }  
}  
```  
  
 O exemplo usa o arquivo `contosoBooks.xml` como entrada.  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
 O exemplo também usa `contosoBooks.xsd` como entrada.  
  
 [!code-xml[XPathXMLExamples#3](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xsd#3)]  
  
 No exemplo acima, uma <xref:System.Xml.Schema.XmlSchemaValidationException> será gerado quando <xref:System.Xml.XmlDocument.Load%2A> for chamado se nenhum tipo de atributo ou de elemento corresponder ao tipo correspondente especificado no esquema de validação. Se um <xref:System.Xml.XmlReaderSettings.ValidationEventHandler> não for definido no objeto <xref:System.Xml.XmlReader> de validação, <xref:System.Xml.XmlReaderSettings.ValidationEventHandler> será chamado sempre que um tipo inválido for localizado.  
  
 Uma <xref:System.Xml.Schema.XmlSchemaException> será gerada quando um atributo ou um elemento com <xref:System.Xml.XPath.XPathNavigator.TypedValue%2A> definido como `invalid` for acessado pela classe <xref:System.Xml.XPath.XPathNavigator>.  
  
 A propriedade <xref:System.Xml.Schema.XmlSchemaInfo.Validity%2A> pode ser usada para determinar se um atributo ou um elemento individual é válido para acessar atributos ou elementos com a classe <xref:System.Xml.XPath.XPathNavigator>.  
  
> [!NOTE]
> Quando um documento XML é carregado para um objeto <xref:System.Xml.XmlDocument> com um esquema associado que define os valores padrão, o objeto <xref:System.Xml.XmlDocument> trata esses padrões como se eles tivessem aparecido no documento XML. Isso significa que a propriedade <xref:System.Xml.XPath.XPathNavigator.IsEmptyElement%2A> sempre retorna `false` para um elemento do esquema que é usado como padrão, mesmo se o documento XML tiver sido criado como um elemento vazio.  
  
### <a name="validating-a-document-using-the-validate-method"></a>Validando um documento com o método Validate  
 O método <xref:System.Xml.XmlDocument.Validate%2A> da classe <xref:System.Xml.XmlDocument> valida o documento XML contido em um objeto <xref:System.Xml.XmlDocument> em relação aos esquemas especificados na propriedade <xref:System.Xml.XmlDocument> do objeto <xref:System.Xml.XmlDocument.Schemas%2A> e executa o aumento do conjunto de informações. O resultado é um documento XML anteriormente não tipado no objeto <xref:System.Xml.XmlDocument> substituído por um documento tipado.  
  
 O objeto <xref:System.Xml.XmlDocument> relata erros e avisos de validação do esquema de relatórios que usa o delegado <xref:System.Xml.Schema.ValidationEventHandler> passado como parâmetro para o método <xref:System.Xml.XmlDocument.Validate%2A>.  
  
 O exemplo a seguir valida o arquivo `contosoBooks.xml` contido no objeto <xref:System.Xml.XmlDocument> em relação ao esquema `contosoBooks.xsd` contido na propriedade <xref:System.Xml.XmlDocument> do objeto <xref:System.Xml.XmlDocument.Schemas%2A>.  
  
```vb  
Imports System  
Imports System.Xml  
Imports System.Xml.Schema  
Imports System.Xml.XPath  
  
Class ValidateExample  
  
    Shared Sub Main(ByVal args() As String)  
  
        Dim document As XmlDocument = New XmlDocument()  
        document.Load("contosoBooks.xml")  
        Dim navigator As XPathNavigator = document.CreateNavigator()  
  
        document.Schemas.Add("http://www.contoso.com/books", "contosoBooks.xsd")  
        Dim validation As ValidationEventHandler = New ValidationEventHandler(AddressOf SchemaValidationHandler)  
  
        document.Validate(validation)  
  
    End Sub  
  
    Shared Sub SchemaValidationHandler(ByVal sender As Object, ByVal e As ValidationEventArgs)  
  
        Select Case e.Severity  
            Case XmlSeverityType.Error  
                Console.WriteLine("Schema Validation Error: {0}", e.Message)  
                Exit Sub  
            Case XmlSeverityType.Warning  
                Console.WriteLine("Schema Validation Warning: {0}", e.Message)  
                Exit Sub  
        End Select  
  
    End Sub  
  
End Class  
```  
  
```csharp  
using System;  
using System.Xml;  
using System.Xml.Schema;  
using System.Xml.XPath;  
  
class ValidateExample  
{  
    static void Main(string[] args)  
    {  
        XmlDocument document = new XmlDocument();  
        document.Load("contosoBooks.xml");  
        XPathNavigator navigator = document.CreateNavigator();  
  
        document.Schemas.Add("http://www.contoso.com/books", "contosoBooks.xsd");  
        ValidationEventHandler validation = new ValidationEventHandler(SchemaValidationHandler);  
  
        document.Validate(validation);  
    }  
  
    static void SchemaValidationHandler(object sender, ValidationEventArgs e)  
    {  
        switch (e.Severity)  
        {  
            case XmlSeverityType.Error:  
                Console.WriteLine("Schema Validation Error: {0}", e.Message);  
                break;  
            case XmlSeverityType.Warning:  
                Console.WriteLine("Schema Validation Warning: {0}", e.Message);  
                break;  
        }  
    }  
}  
```  
  
 O exemplo usa o arquivo `contosoBooks.xml` como entrada.  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
 O exemplo também usa `contosoBooks.xsd` como entrada.  
  
 [!code-xml[XPathXMLExamples#3](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xsd#3)]  
  
### <a name="validating-modifications"></a>Validando modificações  
 Depois que as alterações são feitas em um documento XML, você pode validar as modificações em relação ao esquema do documento XML usando o método <xref:System.Xml.XmlDocument.Validate%2A> da classe <xref:System.Xml.XmlDocument>.  
  
 O exemplo a seguir valida o arquivo `contosoBooks.xml` enquanto ele é carregado para o objeto <xref:System.Xml.XmlDocument> criando o objeto <xref:System.Xml.XmlDocument> que usa um objeto <xref:System.Xml.XmlReader> de validação. O documento XML é validado com êxito enquanto é carregado sem gerar erros nem avisos de validação de esquema. Em seguida, o exemplo faz duas modificações no documento XML que são inválidas de acordo com o esquema `contosoBooks.xsd`. A primeira modificação insere um elemento filho inválido que resulta em um erro de validação de esquema, e a segunda modificação define o valor de um nó tipado como um valor inválido de acordo com o tipo de nó que resulta em uma exceção.  
  
```vb  
Imports System  
Imports System.Xml  
Imports System.Xml.Schema  
Imports System.Xml.XPath  
  
Class ValidatingReaderExample  
  
    Shared Sub Main(ByVal args() As String)  
  
        Try  
            Dim settings As XmlReaderSettings = New XmlReaderSettings()  
            settings.Schemas.Add("http://www.contoso.com/books", "contosoBooks.xsd")  
            settings.ValidationType = ValidationType.Schema  
  
            Dim reader As XmlReader = XmlReader.Create("contosoBooks.xml", settings)  
  
            Dim document As XmlDocument = New XmlDocument()  
            document.Load(reader)  
            Dim navigator As XPathNavigator = document.CreateNavigator()  
  
            Dim validation As ValidationEventHandler = New ValidationEventHandler(AddressOf SchemaValidationHandler)  
  
            navigator.MoveToChild("bookstore", "http://www.contoso.com/books")  
            navigator.MoveToChild("book", "http://www.contoso.com/books")  
            navigator.MoveToChild("author", "http://www.contoso.com/books")  
  
            navigator.AppendChild("<title>Book Title</title>")  
  
            document.Validate(validation)  
  
            navigator.MoveToParent()  
            navigator.MoveToChild("price", "http://www.contoso.com/books")  
            navigator.SetTypedValue(DateTime.Now)  
        Catch e As Exception  
            Console.WriteLine("ValidatingReaderExample.Exception: {0}", e.Message)  
        End Try  
  
    End Sub  
  
    Shared Sub SchemaValidationHandler(ByVal sender As Object, ByVal e As ValidationEventArgs)  
  
        Select Case e.Severity  
            Case XmlSeverityType.Error  
                Console.WriteLine("Schema Validation Error: {0}", e.Message)  
                Exit Sub  
            Case XmlSeverityType.Warning  
                Console.WriteLine("Schema Validation Warning: {0}", e.Message)  
                Exit Sub  
        End Select  
  
    End Sub  
  
End Class  
```  
  
```csharp  
using System;  
using System.Xml;  
using System.Xml.Schema;  
using System.Xml.XPath;  
  
class ValidatingReaderExample  
{  
    static void Main(string[] args)  
    {  
        try  
        {  
            XmlReaderSettings settings = new XmlReaderSettings();  
            settings.Schemas.Add("http://www.contoso.com/books", "contosoBooks.xsd");  
            settings.ValidationType = ValidationType.Schema;  
  
            XmlReader reader = XmlReader.Create("contosoBooks.xml", settings);  
  
            XmlDocument document = new XmlDocument();  
            document.Load(reader);  
            XPathNavigator navigator = document.CreateNavigator();  
  
            ValidationEventHandler validation = new ValidationEventHandler(SchemaValidationHandler);  
  
            navigator.MoveToChild("bookstore", "http://www.contoso.com/books");  
            navigator.MoveToChild("book", "http://www.contoso.com/books");  
            navigator.MoveToChild("author", "http://www.contoso.com/books");  
  
            navigator.AppendChild("<title>Book Title</title>");  
  
            document.Validate(validation);  
  
            navigator.MoveToParent();  
            navigator.MoveToChild("price", "http://www.contoso.com/books");  
            navigator.SetTypedValue(DateTime.Now);  
        }  
        catch (Exception e)  
        {  
            Console.WriteLine("ValidatingReaderExample.Exception: {0}", e.Message);  
        }  
    }  
  
    static void SchemaValidationHandler(object sender, ValidationEventArgs e)  
    {  
        switch (e.Severity)  
        {  
            case XmlSeverityType.Error:  
                Console.WriteLine("Schema Validation Error: {0}", e.Message);  
                break;  
            case XmlSeverityType.Warning:  
                Console.WriteLine("Schema Validation Warning: {0}", e.Message);  
                break;  
        }  
    }  
}  
```  
  
 O exemplo usa o arquivo `contosoBooks.xml` como entrada.  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
 O exemplo também usa `contosoBooks.xsd` como entrada.  
  
 [!code-xml[XPathXMLExamples#3](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xsd#3)]  
  
 No exemplo anterior, duas modificações são feitas no documento XML contido no objeto <xref:System.Xml.XmlDocument>. Enquanto o documento XML era carregado, todos os erros de validação de esquema encontrados teriam sido tratados pelo método de manipulador de eventos de validação e gravados no console.  
  
 Neste exemplo, os erros de validação foram introduzidos após o documento XML ser carregado e foram encontrados usando o método <xref:System.Xml.XmlDocument.Validate%2A> da classe <xref:System.Xml.XmlDocument>.  
  
 As modificações feitas usando o método <xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A> da classe <xref:System.Xml.XPath.XPathNavigator> resultaram em uma <xref:System.InvalidCastException> porque o novo valor era inválido de acordo com o tipo do nó de esquema.  
  
 Para saber mais sobre como modificar os valores usando o método <xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A>, confira o tópico [Modificar dados XML usando XPathNavigator](modify-xml-data-using-xpathnavigator.md).  
  
### <a name="read-only-validation"></a>Validação somente leitura  
 A classe <xref:System.Xml.XPath.XPathDocument> é uma representação somente leitura na memória de um documento XML. As classes <xref:System.Xml.XPath.XPathDocument> e <xref:System.Xml.XmlDocument> criam objetos <xref:System.Xml.XPath.XPathNavigator> para navegar em documentos XML e editá-los. Como a classe <xref:System.Xml.XPath.XPathDocument> é uma classe somente leitura, o objeto <xref:System.Xml.XPath.XPathNavigator> retornado de objetos <xref:System.Xml.XPath.XPathDocument> não pode editar o documento XML contido no objeto <xref:System.Xml.XPath.XPathDocument>.  
  
 No caso de validação, você pode criar um objeto <xref:System.Xml.XPath.XPathDocument> da mesma forma como cria um objeto <xref:System.Xml.XmlDocument> usando um objeto <xref:System.Xml.XmlReader> de validação, como descrito anteriormente neste tópico. O objeto <xref:System.Xml.XPath.XPathDocument> valida o documento XML enquanto ele é carregado, mas, como não é possível editar os dados XML no objeto <xref:System.Xml.XPath.XPathDocument>, você não pode revalidar o documento XML.  
  
 Para saber mais sobre objetos <xref:System.Xml.XPath.XPathNavigator> editáveis e somente leitura, confira o tópico [Leitura de dados XML usando XPathDocument e XmlDocument](reading-xml-data-using-xpathdocument-and-xmldocument.md).  
  
## <a name="see-also"></a>Veja também

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [Processar dados XML usando o modelo de dados XPath](process-xml-data-using-the-xpath-data-model.md)
- [Lendo dados XML usando XPathDocument e XmlDocument](reading-xml-data-using-xpathdocument-and-xmldocument.md)
- [Selecionando, avaliando e correspondente de dados XML usando XPathNavigator](selecting-evaluating-and-matching-xml-data-using-xpathnavigator.md)
- [Acessando dados XML usando XPathNavigator](accessing-xml-data-using-xpathnavigator.md)
- [Editando dados XML usando XPathNavigator](editing-xml-data-using-xpathnavigator.md)
