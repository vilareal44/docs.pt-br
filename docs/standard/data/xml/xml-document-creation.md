---
title: Criação de documentos XML
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 877e9c62-b082-4bfb-bc5b-f47297eb30ef
ms.openlocfilehash: 577d353a30c986d198140b4596ae1a7e199ddd6e
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84291975"
---
# <a name="xml-document-creation"></a>Criação de documentos XML
Existem duas maneiras de criar um documento XML. Uma maneira é criar um **XmlDocument** sem parâmetros. Outra maneira é criar um **XmlDocument** e passar para ele uma XmlNameTable como parâmetro. O exemplo a seguir mostra como criar um novo **XmlDocument** vazio sem parâmetros.  
  
```vb  
Dim doc As New XmlDocument()  
```  
  
```csharp  
XmlDocument doc = new XmlDocument();  
```  
  
 Uma vez criado o documento, você pode carregá-lo com dados de uma cadeia de caracteres, de um fluxo, de uma URL, de um leitor de texto ou de uma classe **XmlReader** derivada usando o método **Load**. Existe também outro método de carregamento, o método **LoadXML**, que lê XML de uma cadeia de caracteres. Para saber mais sobre os vários métodos **Load**, consulte [Ler um documento XML no DOM](reading-an-xml-document-into-the-dom.md).  
  
 Há uma classe chamada **XmlNameTable**. Essa classe é uma tabela de objetos atomizados de cadeias de caracteres. Essa tabela fornece uma forma eficiente para que o analisador XML use o mesmo objeto de cadeia de caracteres para todos os nomes repetidos de elementos e de atributos em um documento XML. Uma **XmlNameTable** é criada automaticamente quando um documento é criado, como mostrado acima, e carregada com nomes de atributo e de elemento quando o documento é carregado. Se você já tiver um documento com uma tabela de nomes, que poderiam ser úteis em outro documento, poderá criar um novo documento usando o método **Load** que tem **XmlNameTable** como parâmetro. Quando o documento é criado com esse método, ele usa a classe **XmlNameTable** existente com todos os atributos e elementos já carregados para ela de outro documento. Ela pode ser usada para comparar nomes de elementos e de atributos de forma mais eficaz. Para saber mais sobre a classe **XmlNameTable**, consulte [Comparação de objetos usando XmlNameTable](object-comparison-using-xmlnametable.md). Para referência, veja <xref:System.Xml.XmlNameTable>.  
  
## <a name="see-also"></a>Veja também

- [XML Document Object Model (DOM)](xml-document-object-model-dom.md)
