---
title: 'Como: carregar o XML de um arquivo'
ms.date: 07/20/2015
ms.assetid: e2d337ad-8ac8-4671-b694-30e5ca1413b7
ms.openlocfilehash: faea93b8eea2b713a8beb7fe199be7d644a07706
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84397993"
---
# <a name="how-to-load-xml-from-a-file-visual-basic"></a>Como carregar XML de um arquivo (Visual Basic)

Este tópico mostra como carregar XML de um URI usando o método <xref:System.Xml.Linq.XElement.Load%2A?displayProperty=nameWithType>.

## <a name="example"></a>Exemplo

O seguinte exemplo mostra como carregar um documento XML de um arquivo. O seguinte exemplo carrega books.xml e gera a árvore XML no console.

Este exemplo usa o seguinte documento XML: [Arquivo XML de exemplo: livros (LINQ to XML)](sample-xml-file-books-linq-to-xml.md).

```vb
Dim booksFromFile As XElement = XElement.Load("books.xml")
Console.WriteLine(booksFromFile)
```

Esse código gera a seguinte saída:

```xml
<Catalog>
  <Book id="bk101">
    <Author>Garghentini, Davide</Author>
    <Title>XML Developer's Guide</Title>
    <Genre>Computer</Genre>
    <Price>44.95</Price>
    <PublishDate>2000-10-01</PublishDate>
    <Description>An in-depth look at creating applications
      with XML.</Description>
  </Book>
  <Book id="bk102">
    <Author>Garcia, Debra</Author>
    <Title>Midnight Rain</Title>
    <Genre>Fantasy</Genre>
    <Price>5.95</Price>
    <PublishDate>2000-12-16</PublishDate>
    <Description>A former architect battles corporate zombies,
      an evil sorceress, and her own childhood to become queen
      of the world.</Description>
  </Book>
</Catalog>
```

## <a name="see-also"></a>Confira também

- [Analisando XML (Visual Basic)](parsing-xml.md)
