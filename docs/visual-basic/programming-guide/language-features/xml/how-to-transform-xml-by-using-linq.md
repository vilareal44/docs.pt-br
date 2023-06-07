---
title: Como transformar XML usando LINQ
ms.date: 07/20/2015
helpviewer_keywords:
- XML [Visual Basic], transforming
- LINQ to XML [Visual Basic], transforming XML
ms.assetid: 815687f4-0bc2-4c0b-adc6-d78744aa356f
ms.openlocfilehash: dab394ec45567589e002b5d2ac76ec19fb0f76c6
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84374875"
---
# <a name="how-to-transform-xml-by-using-linq-visual-basic"></a>Como transformar XML usando LINQ (Visual Basic)

Os [literais XML](../../../language-reference/xml-literals/index.md) facilitam a leitura de XML de uma fonte e a transformação para um novo formato XML. Você pode aproveitar as consultas LINQ para recuperar o conteúdo para transformar ou alterar o conteúdo de um documento existente para um novo formato XML.

O exemplo neste tópico transforma o conteúdo de um documento de origem XML em HTML a ser exibido em um navegador.

[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]

### <a name="to-transform-an-xml-document"></a>Para transformar um documento XML

1. No Visual Studio, crie um novo projeto de Visual Basic no modelo de projeto de **aplicativo de console** .

2. Clique duas vezes no arquivo Module1. vb criado no projeto para modificar o código de Visual Basic. Adicione o código a seguir ao `Sub Main` do `Module1` módulo. Esse código cria o documento XML de origem como um <xref:System.Xml.Linq.XDocument> objeto.

    ```vb
    Dim catalog =
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
              Get the expert insights, practical code samples,
              and best practices you need
              to advance your expertise with <technology>Visual
              Basic .NET</technology>.
              Learn how to create faster, more reliable applications
              based on professional,
              pragmatic guidance by today's top <audience>developers</audience>.
            </Description>
          </Book>
        </Catalog>
    ```

     [Como carregar XML de um arquivo, uma cadeia de caracteres ou um fluxo](how-to-load-xml-from-a-file-string-or-stream.md).

3. Depois do código para criar o documento XML de origem, adicione o código a seguir para recuperar todos os \<Book> elementos do objeto e transformá-los em um documento HTML. A lista de \<Book> elementos é criada usando uma consulta LINQ que retorna uma coleção de <xref:System.Xml.Linq.XElement> objetos que contêm o HTML transformado. Você pode usar expressões inseridas para colocar os valores do documento de origem no novo formato XML.

     O documento HTML resultante é gravado em um arquivo usando o <xref:System.Xml.Linq.XElement.Save%2A> método.

    ```vb
    Dim htmlOutput =
      <html>
        <body>
          <%= From book In catalog.<Catalog>.<Book>
              Select <div>
                       <h1><%= book.<Title>.Value %></h1>
                       <h3><%= "By " & book.<Author>.Value %></h3>
                        <h3><%= "Price = " & book.<Price>.Value %></h3>
                        <h2>Description</h2>
                        <%= TransformDescription(book.<Description>(0)) %>
                        <hr/>
                      </div> %>
        </body>
      </html>

    htmlOutput.Save("BookDescription.html")
    ```

4. Depois `Sub Main` de `Module1` , adicione um novo método ( `Sub` ) para transformar um \<Description> nó no formato HTML especificado. Esse método é chamado pelo código na etapa anterior e é usado para preservar o formato dos \<Description> elementos.

     Esse método substitui os subelementos do \<Description> elemento por HTML. O `ReplaceWith` método é usado para preservar o local dos subelementos. O conteúdo transformado do \<Description> elemento é incluído em um elemento HTML Paragraph ( \<p> ). A <xref:System.Xml.Linq.XContainer.Nodes%2A> propriedade é usada para recuperar o conteúdo transformado do \<Description> elemento. Isso garante que os subelementos sejam incluídos no conteúdo transformado.

     Adicione o código a seguir `Sub Main` após `Module1` .

    ```vb
    Public Function TransformDescription(ByVal desc As XElement) As XElement

      ' Replace <technology> elements with <b>.
      Dim content = (From element In desc...<technology>).ToList()

      If content.Count > 0 Then
        For i = 0 To content.Count - 1
          content(i).ReplaceWith(<b><%= content(i).Value %></b>)
        Next
      End If

      ' Replace <audience> elements with <i>.
      content = (From element In desc...<audience>).ToList()

      If content.Count > 0 Then
        For i = 0 To content.Count - 1
          content(i).ReplaceWith(<i><%= content(i).Value %></i>)
        Next
      End If

      ' Return the updated contents of the <Description> element.
      Return <p><%= desc.Nodes %></p>
    End Function
    ```

5. Salve suas alterações.

6. Pressione F5 para executar o código. O documento salvo resultante será semelhante ao seguinte:

    ```html
    <?xml version="1.0"?>
    <html>
      <body>
        <div>
          <h1>XML Developer's Guide</h1>
          <h3>By Garghentini, Davide</h3>
          <h3>Price = 44.95</h3>
          <h2>Description</h2>
          <p>
            An in-depth look at creating applications
            with <b>XML</b>. For
            <i>beginners</i> or
            <i>advanced</i> developers.
          </p>
          <hr />
        </div>
        <div>
          <h1>Developing Applications with Visual Basic .NET</h1>
          <h3>By Spencer, Phil</h3>
          <h3>Price = 45.95</h3>
          <h2>Description</h2>
          <p>
            Get the expert insights, practical code
            samples, and best practices you need
            to advance your expertise with <b>Visual
            Basic .NET</b>. Learn how to create faster,
            more reliable applications based on
            professional, pragmatic guidance by today's
            top <i>developers</i>.
          </p>
          <hr />
        </div>
      </body>
    </html>
    ```

## <a name="see-also"></a>Confira também

- [Literais XML](../../../language-reference/xml-literals/index.md)
- [Manipulando XML no Visual Basic](manipulating-xml.md)
- [XML](index.md)
- [Como carregar XML a partir de um arquivo, cadeia de caracteres ou fluxo](how-to-load-xml-from-a-file-string-or-stream.md)
- [LINQ](../linq/index.md)
- [Introdução a LINQ no Visual Basic](../linq/introduction-to-linq.md)
