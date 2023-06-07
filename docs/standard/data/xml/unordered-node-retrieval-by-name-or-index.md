---
title: Recuperação não ordenada do nó por nome ou índice
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 2038a90b-92af-4a0a-baaa-08e688d95194
ms.openlocfilehash: 6847f3c5d233b720f8f4c41cfc52ac663e5e810f
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84288622"
---
# <a name="unordered-node-retrieval-by-name-or-index"></a>Recuperação não ordenada do nó por nome ou índice
**XmlNamedNodeMap** é descrito na especificação do W3C (World Wide Web Consortium) como NamedNodeMap e é necessário para tratar um conjunto não ordenado de nós com a capacidade de fazer referência a nós pelos respectivos nome ou índice. A única maneira de ter acesso a um **XmlNamedNodeMap** é quando um **XmlNamedNodeMap** é retornado através de um método ou de uma propriedade. Há três métodos ou propriedades que retornam um **XmlNamedNodeMap**:  
  
- XmlElement.Attributes  
  
- XmlDocumentType.Entities  
  
- XmlDocumentType.Notations  
  
 Por exemplo, a propriedade **XmlDocumentType.Entities** obtém a coleção de nós de **XmlEntity** declarados na declaração de tipo de documento. Essa coleção é retornada como **XmlNamedNodeMap**, e você pode fazer iterações através da coleção, usando a propriedade **Count** e as informações da entidade de exibição. Para ver um exemplo de iteração através de um **XmlNamedNodeMap**, confira <xref:System.Xml.XmlDocumentType.Entities%2A>.  
  
 **XmlAttributeCollection** é derivado de **XmlNamedNodeMap** e apenas os atributos são modificáveis, ao passo que as anotações e entidades são destinadas a somente leitura. Quando usa **XmlNamedNodeMap** nos atributos, você pode obter nós para esses atributos com base nos respectivos nomes XML. Isso fornece um método fácil para manipular a coleção de atributos em um nó de elemento. Isso pode ser comparado diretamente com **XmlNodeList**, que também implementa a interface de **IEnumerable**, mas com um acessador de índice em vez de uma cadeia de caracteres. Os métodos **RemoveNamedItem** e **SetNamedItem** são usados somente em **XmlAttributeCollection**. Ao adicionar ou remover de uma coleção de atributos usando a implementação de **AttributeCollection** ou **XmlNamedNodeMap** modificará a coleção de atributos no elemento. O exemplo de código a seguir mostra como mover um atributo e criar um novo atributo.  
  
```vb  
Imports System  
Imports System.Xml  
  
Class test  
  
    Public Shared Sub Main()  
        Dim doc As New XmlDocument()  
        doc.LoadXml("<root> <child1 attr1='val1' attr2='val2'> text1 </child1> <child2 attr3='val3'> text2 </child2> </root> ")  
  
        ' Get the attributes of node "child2 "  
        Dim ac As XmlAttributeCollection = doc.DocumentElement.ChildNodes(1).Attributes  
  
        ' Print out the number of attributes and their names.  
        Console.WriteLine(("Number of Attributes: " + ac.Count))  
        Dim i As Integer  
        For i = 0 To ac.Count - 1  
            Console.WriteLine((i + 1 + ".  Attribute Name: '" + ac(i).Name + "'  Attribute Value:  '" + ac(i).Value + "'"))  
        Next i  
        ' Get the 'attr1' from child1.  
        Dim attr As XmlAttribute = doc.DocumentElement.ChildNodes(0).Attributes(0)  
  
        ' Add this attribute to the attributecollection "ac".  
        ac.SetNamedItem(attr)  
  
        ''attr1' will be removed from 'child1' and added to 'child2'.  
        ' Print out the number of attributes and their names.  
        Console.WriteLine(("Number of Attributes: " + ac.Count))  
  
        For i = 0 To ac.Count - 1  
            Console.WriteLine((i + 1 + ".  Attribute Name: '" + ac(i).Name + "'  Attribute Value:  '" + ac(i).Value + "'"))  
        Next i  
        ' Create a new attribute and add to the collection.  
        Dim attr2 As XmlAttribute = doc.CreateAttribute("attr4")  
        attr2.Value = "val4"  
        ac.SetNamedItem(attr2)  
  
        ' Print out the number of attributes and their names.  
        Console.WriteLine(("Number of Attributes: " + ac.Count))  
  
        For i = 0 To ac.Count - 1  
            Console.WriteLine((i + 1 + ".  Attribute Name: '" + ac(i).Name + "'  Attribute Value:  '" + ac(i).Value + "'"))  
        Next i  
    End Sub 'Main  
End Class 'test  
```  
  
```csharp  
using System;  
using System.Xml;  
class test {  
    public static void Main() {  
        XmlDocument doc = new XmlDocument();  
        doc.LoadXml( "<root> <child1 attr1='val1' attr2='val2'> text1 </child1> <child2 attr3='val3'> text2 </child2> </root> " );  
  
        // Get the attributes of node "child2"  
        XmlAttributeCollection ac = doc.DocumentElement.ChildNodes[1].Attributes;  
  
        // Print out the number of attributes and their names.  
        Console.WriteLine( "Number of Attributes: "+ac.Count );  
        for( int i = 0; i < ac.Count; i++ )  
            Console.WriteLine( (i+1) + ".  Attribute Name: '" +ac[i].Name+ "'  Attribute Value:  '"+ ac[i].Value +"'" );
  
        // Get the 'attr1' from child1.  
        XmlAttribute attr = doc.DocumentElement.ChildNodes[0].Attributes[0];  
  
        // Add this attribute to the attributecollection "ac".  
        ac.SetNamedItem( attr );  
  
        // 'attr1' will be removed from 'child1' and added to 'child2'.  
        // Print out the number of attributes and their names.  
        Console.WriteLine( "Number of Attributes: "+ac.Count );
        for( int i = 0; i < ac.Count; i++ )  
            Console.WriteLine( (i+1) + ".  Attribute Name: '" +ac[i].Name+ "'  Attribute Value:  '"+ ac[i].Value +"'" );
  
        // Create a new attribute and add to the collection.  
        XmlAttribute attr2 = doc.CreateAttribute( "attr4" );  
        attr2.Value = "val4";  
        ac.SetNamedItem( attr2 );  
  
        // Print out the number of attributes and their names.  
        Console.WriteLine( "Number of Attributes: "+ac.Count );
        for( int i = 0; i < ac.Count; i++ )  
            Console.WriteLine( (i+1) + ".  Attribute Name: '" +ac[i].Name+ "'  Attribute Value:  '"+ ac[i].Value +"'" );
  
    }  
}  
```  
  
 Para ver um exemplo de código adicional que mostra um atributo que está sendo removido de **AttributeCollection**, confira o [Método XmlNamedNodeMap.RemoveNamedItem](xref:System.Xml.XmlNamedNodeMap.RemoveNamedItem%2A). Para saber mais sobre métodos e propriedades, confira [Membros XmlNamedNodeMap](xref:System.Xml.XmlNamedNodeMap).  
  
## <a name="see-also"></a>Veja também

- [XML Document Object Model (DOM)](xml-document-object-model-dom.md)
