---
title: Como modificar literais XML
ms.date: 07/20/2015
helpviewer_keywords:
- XML axis [Visual Basic], Value
- XML literals [Visual Basic]
- XML literals [Visual Basic], modifying
ms.assetid: 4e864522-a37a-43a2-8236-af80277c5482
ms.openlocfilehash: a2ac2e9802d4c8ab522bb430d15cce5616430437
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84374869"
---
# <a name="how-to-modify-xml-literals-visual-basic"></a>Como modificar literais XML (Visual Basic)

Visual Basic fornece maneiras convenientes de modificar literais XML. Você pode adicionar ou excluir elementos e atributos, e também pode substituir um elemento existente por um novo elemento XML. Este tópico fornece vários exemplos de como modificar um literal XML existente.

### <a name="to-modify-the-value-of-an-xml-literal"></a>Para modificar o valor de um literal XML

1. Para modificar o valor de um literal XML, obtenha uma referência para o literal XML e defina a `Value` propriedade para o valor desejado.

    O exemplo de código a seguir atualiza o valor de todos os \<Price> elementos em um documento XML.

    [!code-vb[VbXmlSamples2#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXmlSamples2/VB/Module2.vb#4)]

    O exemplo a seguir mostra o XML de origem de exemplo e o XML modificado deste código.

    XML de origem:

    ```xml
    <?xml version="1.0"?>
    <Catalog>
      <Book id="bk101">
        <Author>Garghentini, Davide</Author>
        <Title>XML Developer's Guide</Title>
        <Price>44.95</Price>
      </Book>
      <Book id="bk331">
        <Author>Spencer, Phil</Author>
        <Title>Developing Applications with Visual Basic .NET</Title>
        <Price>45.95</Price>
      </Book>
    </Catalog>
    ```

    XML modificado:

    ```xml
    <?xml version="1.0"?>
    <Catalog>
      <Book id="bk101">
        <Author>Garghentini, Davide</Author>
        <Title>XML Developer's Guide</Title>
        <Price>47.20</Price>
      </Book>
      <Book id="bk331">
        <Author>Spencer, Phil</Author>
        <Title>Developing Applications with Visual Basic .NET</Title>
        <Price>48.25</Price>
      </Book>
    </Catalog>
    ```

    > [!NOTE]
    > A `Value` Propriedade refere-se ao primeiro elemento XML em uma coleção. Se houver mais de um elemento que tenha o mesmo nome em uma coleção, a definição da `Value` propriedade afetará apenas o primeiro elemento da coleção.

### <a name="to-add-an-attribute-to-an-xml-literal"></a>Para adicionar um atributo a um literal XML

1. Para adicionar um atributo a um literal XML, primeiro obtenha uma referência para o literal XML. Em seguida, você pode adicionar um atributo adicionando uma nova propriedade de eixo de atributo XML. Você também pode adicionar um novo <xref:System.Xml.Linq.XAttribute> objeto ao literal XML usando o <xref:System.Xml.Linq.XContainer.Add%2A> método. O exemplo a seguir mostra as duas opções.

    [!code-vb[VbXmlSamples2#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXmlSamples2/VB/Module2.vb#5)]

    O exemplo a seguir mostra o XML de origem de exemplo e o XML modificado deste código.

    XML de origem:

    ```xml
    <?xml version="1.0"?>
    <Catalog>
      <Book id="bk101" >
        <Author>Garghentini, Davide</Author>
        <Title>XML Developer's Guide</Title>
        <Price>44.95</Price>
      </Book>
      <Book id="bk331">
        <Author>Spencer, Phil</Author>
        <Title>Developing Applications with Visual Basic .NET</Title>
        <Price>45.95</Price>
      </Book>
    </Catalog>
    ```

    XML modificado:

    ```xml
    <?xml version="1.0"?>
    <Catalog>
      <Book id="bk101" genre="Computer" editorEmail="someone@example.com">
        <Author>Garghentini, Davide</Author>
        <Title>XML Developer's Guide</Title>
        <Price>44.95</Price>
      </Book>
      <Book id="bk331" genre="Computer" editorEmail="someone@example.com">
        <Author>Spencer, Phil</Author>
        <Title>Developing Applications with Visual Basic .NET</Title>
        <Price>45.95</Price>
      </Book>
    </Catalog>
    ```

    Para obter mais informações sobre propriedades do eixo de atributo XML, consulte [Propriedade do eixo de atributo XML](../../../language-reference/xml-axis/xml-attribute-axis-property.md).

### <a name="to-add-an-element-to-an-xml-literal"></a>Para adicionar um elemento a um literal XML

1. Para adicionar um elemento a um literal XML, primeiro obtenha uma referência para o literal XML. Em seguida, você pode adicionar um novo <xref:System.Xml.Linq.XElement> objeto como o último subelemento do elemento usando o <xref:System.Xml.Linq.XContainer.Add%2A> método. Você pode adicionar um novo <xref:System.Xml.Linq.XElement> objeto como o primeiro subelemento usando o <xref:System.Xml.Linq.XContainer.AddFirst%2A> método.

    Para adicionar um novo elemento em um local específico em relação a outros subelementos, primeiro obtenha uma referência a um sub-elemento adjacente. Em seguida, você pode adicionar o novo <xref:System.Xml.Linq.XElement> objeto antes do subelemento adjacente usando o <xref:System.Xml.Linq.XNode.AddBeforeSelf%2A> método. Você também pode adicionar o novo <xref:System.Xml.Linq.XElement> objeto após o subelemento adjacente usando o <xref:System.Xml.Linq.XNode.AddAfterSelf%2A> método.

    O exemplo a seguir mostra exemplos de cada uma dessas técnicas.

    [!code-vb[VbXmlSamples2#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXmlSamples2/VB/Module2.vb#6)]

    O exemplo a seguir mostra o XML de origem de exemplo e o XML modificado deste código.

    XML de origem:

    ```xml
    <?xml version="1.0"?>
    <Catalog>
      <Book id="bk101" >
        <Author>Garghentini, Davide</Author>
        <Title>XML Developer's Guide</Title>
        <Price>44.95</Price>
      </Book>
      <Book id="bk331">
        <Author>Spencer, Phil</Author>
        <Title>Developing Applications with Visual Basic .NET</Title>
        <Price>45.95</Price>
      </Book>
    </Catalog>
    ```

    XML modificado:

    ```xml
    <?xml version="1.0"?>
    <Catalog>
      <Book id="bk101" >
        <Author>Garghentini, Davide</Author>
        <Title>XML Developer's Guide</Title>
        <Price>44.95</Price>
      </Book>
      <Book id="bk000"></Book>
      <Book id="bk331">
        <Publisher>Microsoft Press</Publisher>
        <Author>Spencer, Phil</Author>
        <Title>Developing Applications with Visual Basic .NET</Title>
        <Price>45.95</Price>
        <PublishDate>2005-2-14</PublishDate>
      </Book>
      <Book id="bk999"></Book>
    </Catalog>
    ```

### <a name="to-remove-an-element-or-attribute-from-an-xml-literal"></a>Para remover um elemento ou atributo de um literal XML

1. Para remover um elemento ou um atributo de um literal XML, obtenha uma referência para o elemento ou atributo e chame o `Remove` método, conforme mostrado no exemplo a seguir.

    [!code-vb[VbXmlSamples2#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXmlSamples2/VB/Module2.vb#7)]

    O exemplo a seguir mostra o XML de origem de exemplo e o XML modificado deste código.

    XML de origem:

    ```xml
    <?xml version="1.0"?>
    <Catalog>
      <Book id="bk101" genre="Computer" editorEmail="someone@example.com">
        <Author>Garghentini, Davide</Author>
        <Title>XML Developer's Guide</Title>
        <Price>44.95</Price>
      </Book>
      <Book id="bk000"></Book>
      <Book id="bk331" genre="Computer" editorEmail="someone@example.com">
        <Author>Spencer, Phil</Author>
        <Title>Developing Applications with Visual Basic .NET</Title>
        <Price>45.95</Price>
      </Book>
      <Book id="bk999"></Book>
    </Catalog>
    ```

    XML modificado:

    ```xml
    <?xml version="1.0"?>
    <Catalog>
      <Book id="bk101" editorEmail="someone@example.com">
        <Author>Garghentini, Davide</Author>
        <Title>XML Developer's Guide</Title>
        <Price>44.95</Price>
      </Book>
      <Book id="bk000"></Book>
      <Book id="bk331" editorEmail="someone@example.com">
        <Author>Spencer, Phil</Author>
        <Title>Developing Applications with Visual Basic .NET</Title>
        <Price>45.95</Price>
      </Book></Catalog>
    ```

    Para remover todos os elementos ou atributos de um literal XML, obtenha uma referência para o literal XML e chame o <xref:System.Xml.Linq.XElement.RemoveAll%2A> método.

### <a name="to-modify-an-xml-literal"></a>Para modificar um literal XML

1. Para alterar o nome de um elemento XML, primeiro obtenha uma referência para o elemento. Você pode criar um novo <xref:System.Xml.Linq.XElement> objeto que tem um novo nome e passar o novo <xref:System.Xml.Linq.XElement> objeto para o <xref:System.Xml.Linq.XNode.ReplaceWith%2A> método do objeto existente <xref:System.Xml.Linq.XElement> .

    Se o elemento que você está substituindo tiver subelementos que devam ser preservados, defina o valor do novo <xref:System.Xml.Linq.XElement> objeto como a <xref:System.Xml.Linq.XContainer.Nodes%2A> Propriedade do elemento existente. Isso definirá o valor do novo elemento como o XML interno do elemento existente. Caso contrário, você pode definir o valor do novo elemento como a `Value` Propriedade do elemento existente.

    O exemplo de código a seguir substitui todos os \<Description> elementos por um \<Abstract> elemento. O conteúdo do \<Description> elemento é preservado no novo \<Abstract> elemento usando a <xref:System.Xml.Linq.XContainer.Nodes%2A> Propriedade do \<Description> <xref:System.Xml.Linq.XElement> objeto.

    [!code-vb[VbXmlSamples2#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXmlSamples2/VB/Module2.vb#8)]

    O exemplo a seguir mostra o XML de origem de exemplo e o XML modificado deste código.

    XML de origem:

    ```xml
    <?xml version="1.0"?>
    <Catalog>
      <Book id="bk101">
        <Author>Garghentini, Davide</Author>
        <Title>XML Developer's Guide</Title>
        <Price>44.95</Price>
        <Description>
          An in-depth look at creating applications
          with <technology>XML</technology>. For
          <audience>beginners</audience> or
          <audience>advanced</audience> developers.
        </Description>
      </Book>
      <Book id="bk331">
        <Author>Spencer, Phil</Author>
        <Title>Developing Applications with Visual Basic .NET</Title>
        <Price>45.95</Price>
        <Description>
          Get the expert insights, practical code samples, and best
          practices you need to advance your expertise with
          <technology>Visual Basic .NET</technology>.
          Learn how to create faster, more reliable applications
          based on professional, pragmatic guidance by today's top
          <audience>developers</audience>.
        </Description>
      </Book>
    </Catalog>
    ```

    XML modificado:

    ```xml
    <?xml version="1.0"?>
    <Catalog>
      <Book id="bk101">
        <Author>Garghentini, Davide</Author>
        <Title>XML Developer's Guide</Title>
        <MSRP>44.95</MSRP>    <Abstract>
          An in-depth look at creating applications
          with <technology>XML</technology>. For
          <audience>beginners</audience> or
          <audience>advanced</audience> developers.
        </Abstract>
      </Book>
      <Book id="bk331">
        <Author>Spencer, Phil</Author>
        <Title>Developing Applications with Visual Basic .NET</Title>
        <MSRP>45.95</MSRP>    <Abstract>
          Get the expert insights, practical code samples, and best
          practices you need to advance your expertise with
          <technology>Visual Basic .NET</technology>.
          Learn how to create faster, more reliable applications
          based on professional, pragmatic guidance by today's top
          <audience>developers</audience>.
        </Abstract>
      </Book>
    </Catalog>
    ```

## <a name="see-also"></a>Confira também

- [Manipulando XML no Visual Basic](manipulating-xml.md)
- [XML](index.md)
- [Como carregar XML a partir de um arquivo, cadeia de caracteres ou fluxo](how-to-load-xml-from-a-file-string-or-stream.md)
- [LINQ](../linq/index.md)
- [Introdução a LINQ no Visual Basic](../linq/introduction-to-linq.md)
