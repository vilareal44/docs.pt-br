---
title: Comparação de objeto usando XmlNameTable
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 8d94e041-d340-4ddf-9a2c-d7319e3f4f86
ms.openlocfilehash: 0dd68e8c9beadf26f858a4a5100e2824bbbd4a19
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84292027"
---
# <a name="object-comparison-using-xmlnametable"></a>Comparação de objeto usando XmlNameTable
**XmlDocuments**, quando criado, possui uma tabela de nome especificamente projetada para esse documento. Quando XML é carregado no documento, ou novos elementos ou atributos são criados, os nomes de atributo e de elemento são colocados em **XmlNameTable**. Você também pode criar **XmlDocument** usando **NameTable** existente de outro documento. Quando **XmlDocuments** é criado com o constructo que aceita um parâmetro **XmlNameTable**, o documento tem acesso aos nomes de nó, namespaces e prefixos já armazenados em **XmlNameTable**. Independentemente de como a tabela de nome é carregada com nomes, uma vez que os nomes são armazenados na tabela, os nomes podem ser comparados rapidamente usando a comparação de objeto em vez de comparação de cadeia de caracteres. As cadeias de caracteres também podem ser adicionadas à tabela de nome usando o <xref:System.Xml.NameTable.Add%2A>. O exemplo de código a seguir mostra uma tabela de nome que está sendo criada e a cadeia de caracteres **MyString** sendo adicionada à tabela. Em seguida, um **XmlDocument** é criado usando a tabela e os nomes de elementos e atributos de **Myfile.xml** serão adicionados à tabela de nome existente.  
  
```vb  
Dim nt As New NameTable()  
nt.Add("MyString")  
Dim doc As New XmlDocument(nt)  
doc.Load("Myfile.xml")  
```  
  
```csharp  
NameTable nt = new NameTable();  
nt.Add("MyString");  
XmlDocument doc = new XmlDocument(nt);  
doc.Load("Myfile.xml");  
```  
  
 O exemplo de código a seguir mostra a criação de um documento, dois novos elementos que estão sendo adicionados ao documento, que também os adiciona à tabela do nome do documento, e a comparação de objeto em nomes.  
  
```vb  
Dim doc1 As XmlDocument = imp.CreateDocument()  
Dim node1 As XmlElement = doc.CreateElement("node1")  
Dim doc2 As XmlDocument = imp.CreateDocument()  
Dim node2 As XmlElement = doc.CreateElement("node2")  
if (CType(node1.Name, object) = CType(node2.Name, object))  
```  
  
```csharp  
XmlDocument doc1 = imp.CreateDocument();  
node1 = doc1.CreateElement ("node1");  
XmlDocument doc2 = imp.CreateDocument();  
node2 = doc2.CreateElement ("node1");  
if (((object)node1.Name) == ((object)node2.Name))  
{ ...  
```  
  
 A situação acima de uma tabela de nome transmitida entre dois documentos é típico quando o mesmo tipo de documento está sendo processado repetidamente, como documentos de ordem em um site de comércio eletrônico, que atendem a um esquema de linguagem de definição de esquema XML (XSD) ou Document type definition (DTD) e as mesmas cadeias de caracteres são repetidas. Usar a mesma tabela de nome fornece uma melhoria de desempenho, porque o mesmo nome de elemento ocorre em vários documentos.  
  
## <a name="see-also"></a>Veja também

- [XML Document Object Model (DOM)](xml-document-object-model-dom.md)
