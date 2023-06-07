---
title: Estendendo os DOM
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: b5489c96-4afd-439a-a25d-fc82eb4a148d
ms.openlocfilehash: 4a1a7af0e841601542a30c7bd3f71395faa6cb57
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84287786"
---
# <a name="extending-the-dom"></a>Estendendo os DOM

O Microsoft .NET Framework inclui um conjunto básico de classes que fornece uma implementação de DOM (XML Document Object Model). <xref:System.Xml.XmlNode>, e suas classes derivadas, fornecem métodos e propriedades que permitem que você navega, consulte, e o conteúdo e a estrutura de um documento XML.

Quando o conteúdo XML é carregado na memória usando os DOM, os nós criados contêm informações como o nome de nó, tipo de nó, e assim por diante. Pode haver ocasiões as onde você requer informações específicas do nó que classes base não fornecem. Por exemplo, você pode desejar consultar o número da linha e posição do nó. Nesse caso, você pode derivar novas classes de classes DOM existentes e adicionar funcionalidade adicional.

Há duas diretrizes gerais para derivar novas classes:

- É recomendável que você nunca deriva de classe de <xref:System.Xml.XmlNode> . Em vez disso, é recomendável que você derivado classes de classe correspondente ao tipo de nó que você está interessado. Por exemplo, se você quiser retornar informações adicionais em nós de atributo, você pode derivar da classe de <xref:System.Xml.XmlAttribute> .

- A exceção dos métodos de criação de nó, é recomendável que substituir uma função, você sempre deve chamar a versão de base de função e adicionar qualquer processamento adicional.

## <a name="creating-your-own-node-instances"></a>Criando suas próprias instâncias do nó

A classe de <xref:System.Xml.XmlDocument> contém métodos de criação de nó. Quando um arquivo XML é carregado, esses métodos são chamados para criar os nós. Você pode substituir esses métodos de modo que as instâncias do nó são criados quando um documento é carregado. Por exemplo, se você estendeu a classe de <xref:System.Xml.XmlElement> , você herdaria a classe de <xref:System.Xml.XmlDocument> e poderia substituir o método de <xref:System.Xml.XmlDocument.CreateElement%2A> .

O exemplo a seguir mostra como substituir o método de <xref:System.Xml.XmlDocument.CreateElement%2A> retornar sua implementação da classe de <xref:System.Xml.XmlElement> .

```vb
Class LineInfoDocument
    Inherits XmlDocument
        Public Overrides Function CreateElement(prefix As String, localname As String, nsURI As String) As XmlElement
        Dim elem As New LineInfoElement(prefix, localname, nsURI, Me)
        Return elem
    End Function 'CreateElement
End Class 'LineInfoDocument
```

```csharp
class LineInfoDocument : XmlDocument
{
    public override XmlElement CreateElement(string prefix, string localname, string nsURI)
    {
        LineInfoElement elem = new LineInfoElement(prefix, localname, nsURI, this);
        return elem;
    }
}
```

## <a name="extending-a-class"></a>Estendendo uma classe

Para estender uma classe derivada, a classe de uma das classes DOM existentes. Você pode então substitua alguns dos métodos ou propriedades virtuais na classe base, ou adicione seus próprios.

No exemplo a seguir, uma nova classe é criada, que implementa a classe de <xref:System.Xml.XmlElement> e a interface de <xref:System.Xml.IXmlLineInfo> . Os métodos e propriedades adicionais são definidos que permite que os usuários coleta a linha informações.

```vb
Class LineInfoElement
   Inherits XmlElement
   Implements IXmlLineInfo
   Private lineNumber As Integer = 0
   Private linePosition As Integer = 0

   Friend Sub New(prefix As String, localname As String, nsURI As String, doc As XmlDocument)
      MyBase.New(prefix, localname, nsURI, doc)
      CType(doc, LineInfoDocument).IncrementElementCount()
   End Sub

   Public Sub SetLineInfo(linenum As Integer, linepos As Integer)
      lineNumber = linenum
      linePosition = linepos
   End Sub

   Public ReadOnly Property LineNumber() As Integer
      Get
         Return lineNumber
      End Get
   End Property

   Public ReadOnly Property LinePosition() As Integer
      Get
         Return linePosition
      End Get
   End Property

   Public Function HasLineInfo() As Boolean
      Return True
   End Function
End Class ' End LineInfoElement class.
```

```csharp
class LineInfoElement : XmlElement, IXmlLineInfo {
   int lineNumber = 0;
   int linePosition = 0;
   internal LineInfoElement( string prefix, string localname, string nsURI, XmlDocument doc ) : base( prefix, localname, nsURI, doc ) {
       ( (LineInfoDocument)doc ).IncrementElementCount();
  }
  public void SetLineInfo( int linenum, int linepos ) {
      lineNumber = linenum;
      linePosition = linepos;
  }
  public int LineNumber {
     get {
       return lineNumber;
     }
  }
  public int LinePosition {
      get {
        return linePosition;
      }
  }
  public bool HasLineInfo() {
    return true;
  }
} // End LineInfoElement class.
```

### <a name="example"></a>Exemplo

O exemplo a seguir conta o número de elementos em um documento XML:

```vb
Imports System.Xml
Imports System.IO

Class LineInfoDocument
   Inherits XmlDocument

   Private elementCount As Integer

   Friend Sub New()
      elementCount = 0
   End Sub

   Public Overrides Function CreateElement(prefix As String, localname As String, nsURI As String) As XmlElement
      Dim elem As New LineInfoElement(prefix, localname, nsURI, Me)
      Return elem
   End Function

   Public Sub IncrementElementCount()
      elementCount += 1
   End Sub

   Public Function GetCount() As Integer
      Return elementCount
   End Function
End Class 'End LineInfoDocument class.

Class LineInfoElement
   Inherits XmlElement

   Friend Sub New(prefix As String, localname As String, nsURI As String, doc As XmlDocument)
      MyBase.New(prefix, localname, nsURI, doc)
      CType(doc, LineInfoDocument).IncrementElementCount()
   End Sub 'New
End Class 'LineInfoElement
 _ 'End LineInfoElement class.

Public Class Test

   Private filename As [String] = "book.xml"

   Public Shared Sub Main()

      Dim doc As New LineInfoDocument()
      doc.Load(filename)
      Console.WriteLine("Number of elements in {0}: {1}", filename, doc.GetCount())
   End Sub
End Class
```

```csharp
using System;
using System.Xml;
using System.IO;

class LineInfoDocument : XmlDocument {

  int elementCount;
  internal LineInfoDocument():base() {
    elementCount = 0;
  }

  public override XmlElement CreateElement( string prefix, string localname, string nsURI) {
    LineInfoElement elem = new LineInfoElement(prefix, localname, nsURI, this );
    return elem;
  }

  public void IncrementElementCount() {
     elementCount++;
  }

  public int GetCount() {
     return elementCount;
  }
} // End LineInfoDocument class.

class LineInfoElement:XmlElement {

    internal LineInfoElement( string prefix, string localname, string nsURI, XmlDocument doc ):base( prefix,localname,nsURI, doc ){
      ((LineInfoDocument)doc).IncrementElementCount();
    }
} // End LineInfoElement class.

public class Test {

  const String filename = "book.xml";
  public static void Main() {

     LineInfoDocument doc =new LineInfoDocument();
     doc.Load(filename);
     Console.WriteLine("Number of elements in {0}: {1}", filename, doc.GetCount());

  }
}
```

#### <a name="input"></a>Entrada

book.xml

```xml
<!--sample XML fragment-->
<book genre='novel' ISBN='1-861001-57-5' misc='sale-item'>
  <title>The Handmaid's Tale</title>
  <price>14.95</price>
</book>
```

#### <a name="output"></a>Saída

```console
Number of elements in book.xml: 3
```

## <a name="node-event-handler"></a>Manipulador de eventos do nó

A implementação do .NET Framework DOM também inclui um sistema de eventos que permite que você receber e manipular eventos quando os nós em um documento XML são alterados. Usando as classes de <xref:System.Xml.XmlNodeChangedEventHandler> e de <xref:System.Xml.XmlNodeChangedEventArgs> , você pode capturar `NodeChanged`, `NodeChanging`, `NodeInserted`, `NodeInserting`, `NodeRemoved`, e eventos de `NodeRemoving` .

Os trabalhos de processo de manipulação de eventos exatamente em classes derivadas como em DOM original de classe.

Para saber mais sobre a manipulação de eventos do nó, confira [Eventos](../../events/index.md) e <xref:System.Xml.XmlNodeChangedEventHandler>.

## <a name="default-attributes-and-the-createelement-method"></a>Atributos padrões e o método de CreateElement

Se você está substituindo o método de <xref:System.Xml.XmlDocument.CreateElement%2A> em uma classe derivada, os atributos padrão não são adicionados quando você está criando novos elementos ao editar o documento. Este é apenas um problema ao editar. Porque o método de <xref:System.Xml.XmlDocument.CreateElement%2A> é responsável para adicionar atributos padrão para <xref:System.Xml.XmlDocument>, você deve codificar essa funcionalidade no método de <xref:System.Xml.XmlDocument.CreateElement%2A> . Se você está carregando <xref:System.Xml.XmlDocument> que inclui atributos padrão, serão tratados corretamente. Para saber mais sobre os atributos padrão, confira [Criando novos atributos para os elementos em DOM](creating-new-attributes-for-elements-in-the-dom.md).

## <a name="see-also"></a>Veja também

- [XML Document Object Model (DOM)](xml-document-object-model-dom.md)
