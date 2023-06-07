---
title: 'Como: ler e escrever um documento codificado'
ms.date: 07/20/2015
ms.assetid: 159d868f-5ac8-40f2-95ca-07dd925f35c6
ms.openlocfilehash: f66737429950e04f447dfbd58cf47b6434a22976
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84397902"
---
# <a name="how-to-read-and-write-an-encoded-document-visual-basic"></a>Como: ler e gravar um documento codificado (Visual Basic)

Para criar um documento XML codificado, adicione um <xref:System.Xml.Linq.XDeclaration> à árvore XML, definindo a codificação para o nome da página de código desejada.

Qualquer valor retornado por <xref:System.Text.Encoding.WebName%2A> é um valor válido.

Se você ler um documento codificado, a propriedade <xref:System.Xml.Linq.XDeclaration.Encoding%2A> será definida para o nome da página de código.

Se você definir <xref:System.Xml.Linq.XDeclaration.Encoding%2A> como um nome válido de página de código, o [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] serializará com a codificação especificada.

## <a name="example"></a>Exemplo

O exemplo a seguir cria dois documentos, um com codificação utf-8 e um com codificação utf-16. Ele em seguida carrega os documentos e imprime a codificação para o console.

```vb
Console.WriteLine("Creating a document with utf-8 encoding")
Dim encodedDoc8 As XDocument = _
        <?xml version='1.0' encoding='utf-8' standalone='yes'?>
        <Root>Content</Root>

encodedDoc8.Save("EncodedUtf8.xml")
Console.WriteLine("Encoding is:{0}", encodedDoc8.Declaration.Encoding)
Console.WriteLine()

Console.WriteLine("Creating a document with utf-16 encoding")
Dim encodedDoc16 As XDocument = _
        <?xml version='1.0' encoding='utf-16' standalone='yes'?>
        <Root>Content</Root>

encodedDoc16.Save("EncodedUtf16.xml")
Console.WriteLine("Encoding is:{0}", encodedDoc16.Declaration.Encoding)
Console.WriteLine()

Dim newDoc8 As XDocument = XDocument.Load("EncodedUtf8.xml")
Console.WriteLine("Encoded document:")
Console.WriteLine(File.ReadAllText("EncodedUtf8.xml"))
Console.WriteLine()
Console.WriteLine("Encoding of loaded document is:{0}", newDoc8.Declaration.Encoding)
Console.WriteLine()

Dim newDoc16 As XDocument = XDocument.Load("EncodedUtf16.xml")
Console.WriteLine("Encoded document:")
Console.WriteLine(File.ReadAllText("EncodedUtf16.xml"))
Console.WriteLine()
Console.WriteLine("Encoding of loaded document is:{0}", newDoc16.Declaration.Encoding)
```

Esse exemplo gera a saída a seguir:

```console
Creating a document with utf-8 encoding
Encoding is:utf-8

Creating a document with utf-16 encoding
Encoding is:utf-16

Encoded document:
<?xml version="1.0" encoding="utf-8" standalone="yes"?>
<Root>Content</Root>

Encoding of loaded document is:utf-8

Encoded document:
<?xml version="1.0" encoding="utf-16" standalone="yes"?>
<Root>Content</Root>

Encoding of loaded document is:utf-16
```

## <a name="see-also"></a>Confira também

- <xref:System.Xml.Linq.XDeclaration.Encoding%2A?displayProperty=nameWithType>
- [Programação de LINQ to XML avançada (Visual Basic)](advanced-linq-to-xml-programming.md)
