---
title: 'Como: usar anotações para transformar árvores LINQ to XML em um estilo XSLT'
ms.date: 07/20/2015
ms.assetid: 08e91fa2-dac2-4463-9ef1-87b1ac3fa890
ms.openlocfilehash: 099457eaab8c80605138d7e67d7bc2823e316234
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84364442"
---
# <a name="how-to-use-annotations-to-transform-linq-to-xml-trees-in-an-xslt-style-visual-basic"></a>Como: usar anotações para transformar LINQ to XML árvores em um estilo XSLT (Visual Basic)

As anotações podem ser usadas para facilitar tornam-se de uma árvore XML.

Alguns documentos XML são “centralizado no documento misturado com conteúdo.” Como com documentos, você não souber necessariamente a forma de nós filho de um elemento. Por exemplo, um nó que contém o texto pode ter esta aparência:

```xml
<text>A phrase with <b>bold</b> and <i>italic</i> text.</text>
```

Para qualquer nó de texto, pode haver qualquer número de `<b>` filho e elementos de `<i>` . Essa abordagem se estende a várias outras situações: por exemplo, páginas que podem conter uma variedade de elementos filho, como parágrafos comuns, parágrafos com marcadores e bitmaps. As células em uma tabela podem conter texto, soltar para baixo, listas ou bitmaps. Uma das principais características de documento XML é centralizado em que você não sabe qual elemento filho qualquer elemento específico terá.

Se você deseja transformar elementos em uma árvore onde você não sabe necessariamente muito sobre os filhos dos elementos que você deseja transformar, então essa abordagem que usa anotações é uma abordagem eficiente.

O resumo de abordagem é:

- Primeiro, anotações os elementos na árvore com um elemento de substituição.

- Segundo, iterar através da árvore inteira, criando uma nova árvore onde você substitui cada elemento com a anotação. Este exemplo implementa a iteração e a criação de novo em árvore em uma função chamada `XForm`.

Em detalhes, a abordagem consiste de:

- Execute uma ou mais consultas LINQ to XML que retornam conjunto de elementos que você deseja transformar de uma forma para outra. Para cada elemento na consulta, adicione um novo objeto de <xref:System.Xml.Linq.XElement> como uma anotação ao elemento. Esse novo elemento substituirá o elemento anotado em novo, transformada árvore. Esse código é simples para escrever, como demonstrado por exemplo.

- O novo elemento que é adicionado como uma anotação pode conter novos nós filho; pode formar uma subárvore com qualquer forma desejada.

- Há uma regra especial: Se um nó filho do novo elemento é em um namespace diferente, um namespace que é compensada essa finalidade (nesse exemplo, o namespace é `http://www.microsoft.com/LinqToXmlTransform/2007`), então esse elemento filho não são copiados para a nova árvore. Em vez disso, se o namespace é o namespace especial mencionado acima, e o nome local do elemento é `ApplyTransforms`, então os nós filho do elemento na árvore de origem são iterados, e copiados para a nova árvore (exceto para elementos filhos detalhados ele é transformadas de acordo com essas regras).

- Isso é um pouco análogo à especificação de transformações em XSL. A consulta selecionar um conjunto de nós é análoga a expressão XPath para um modelo. O código para criar um novo <xref:System.Xml.Linq.XElement> que é salvo como uma anotação é análogo ao construtor de sequência em XSL, e o elemento de `ApplyTransforms` são análogos a função para o elemento de `xsl:apply-templates` em XSL.

- Uma vantagem para tomar essa abordagem - porque você formula consultas, você está sempre escrevendo consultas na árvore de origem inalterados. Você não precisará se preocupar com sobre como alterações na árvore de consultas que você está escrevendo.

## <a name="transforming-a-tree"></a>Transformando uma árvore

Este primeiro exemplo renomeia todos os `Paragraph` nós para `para` :

```vb
Imports <xmlns:xf="http://www.microsoft.com/LinqToXmlTransform/2007">

Module Module1
    Dim at As XName = GetXmlNamespace(xf) + "ApplyTransforms"

    Sub Main()
        Dim root As XElement = _
            <Root>
                <Paragraph>This is a sentence with <b>bold</b> and <i>italic</i> text.</Paragraph>
                <Paragraph>More text.</Paragraph>
            </Root>

        ' Replace Paragraph with p.
        For Each el In root...<Paragraph>
            ' same idea as xsl:apply-templates
            el.AddAnnotation( _
                <para>
                    <<%= at %>></>
                </para>)
        Next

        ' The XForm function, shown later in this topic, accomplishes the transform
        Dim newRoot As XElement = XForm(root)
        Console.WriteLine(newRoot)
    End Sub
End Module
```

 Esse exemplo gera a saída a seguir:

```xml
<Root>
  <para>This is a sentence with <b>bold</b> and <i>italic</i> text.</para>
  <para>More text.</para>
</Root>
```

## <a name="a-more-complicated-transform"></a>Uma transformação mais complicada

O exemplo a seguir consulta a árvore e calcula a média e a soma dos elementos de `Data` , e adicioná-los como os novos elementos na árvore.

```vb
Imports <xmlns:xf="http://www.microsoft.com/LinqToXmlTransform/2007">

Module Module1
    Dim at As XName = GetXmlNamespace(xf) + "ApplyTransforms"

    Sub Main()
        Dim data As XElement = _
            <Root>
                <Data>20</Data>
                <Data>10</Data>
                <Data>3</Data>
            </Root>

        ' While adding annotations, you can query the source tree all you want,
        ' as the tree is not mutated while annotating.
        data.AddAnnotation( _
            <Root>
                <<%= at %>/>
                <Average>
                    <%= _
                        String.Format("{0:F4}", _
                        data.Elements("Data") _
                        .Select(Function(z) CDec(z)).Average()) _
                    %>
                </Average>
                <Sum>
                    <%= _
                        data.Elements("Data").Select(Function(z) CInt(z)).Sum() _
                    %>
                </Sum>
            </Root> _
        )

        Console.WriteLine("Before Transform")
        Console.WriteLine("----------------")
        Console.WriteLine(data)
        Console.WriteLine(vbNewLine)

        ' The XForm function, shown later in this topic, accomplishes the transform
        Dim newData As XElement = XForm(data)

        Console.WriteLine("After Transform")
        Console.WriteLine("----------------")
        Console.WriteLine(newData)
    End Sub
End Module
```

Esse exemplo gera a saída a seguir:

```console
Before Transform
----------------
<Root>
  <Data>20</Data>
  <Data>10</Data>
  <Data>3</Data>
</Root>

After Transform
----------------
<Root>
  <Data>20</Data>
  <Data>10</Data>
  <Data>3</Data>
  <Average>11.0000</Average>
  <Sum>33</Sum>
</Root>
```

## <a name="effecting-the-transform"></a>Afetando a transformação

Uma função pequena, `XForm`, cria uma nova árvore transformada de original, a árvore anotada.

O código pseudo- para a função é bastante simples:

> A função usa um XElement como um argumento e retorna um XElement.
>
> Se um elemento tiver uma anotação XElement, retorne um novo XElement:
>
> - O nome do novo XElement é o nome do elemento de anotação.
> - Todos os atributos são copiados da anotação para o novo nó.
> - Todos os nós filho são copiados da anotação, com a exceção de que o nó especial XF: ApplyTransforms é reconhecido e os nós filho do elemento de origem são iterados. Se o nó filho de origem não for um XElement, ele será copiado para a nova árvore. Se o filho de origem for um XElement, ele será transformado chamando essa função recursivamente.
>
> Se um elemento não estiver anotado:
>
> - Retornar um novo XElement
>   - O nome do novo XElement é o nome do elemento de origem.
>   - Todos os atributos são copiados do elemento de origem para o elemento de destino.
>   - Todos os nós filho são copiados do elemento de origem.
>   - Se o nó filho de origem não for um XElement, ele será copiado para a nova árvore. Se o filho de origem for um XElement, ele será transformado chamando essa função recursivamente.

O código a seguir é a implementação dessa função:

```vb
' Build a transformed XML tree per the annotations.
Function XForm(ByVal source As XElement) As XElement
    If source.Annotation(Of XElement)() IsNot Nothing Then
        Dim anno As XElement = source.Annotation(Of XElement)()
        Return _
            <<%= anno.Name.ToString() %>>
                <%= anno.Attributes() %>
                <%= anno.Nodes().Select(Function(n As XNode) _
                    GetSubNodes(n, source)) %>
            </>
    Else
        Return _
            <<%= source.Name %>>
                <%= source.Attributes() %>
                <%= source.Nodes().Select(Function(n) GetExpandedNodes(n)) %>
            </>
    End If
End Function

Private Function GetSubNodes(ByVal n As XNode, ByVal s As XElement) As Object
    Dim annoEl As XElement = TryCast(n, XElement)
    If annoEl IsNot Nothing Then
        If annoEl.Name = at Then
            Return s.Nodes().Select(Function(n2 As XNode) GetExpandedNodes(n2))
        End If
    End If
    Return n
End Function

Private Function GetExpandedNodes(ByVal n2 As XNode) As XNode
    Dim e2 As XElement = TryCast(n2, XElement)
    If e2 Is Nothing Then
        Return n2
    Else
        Return XForm(e2)
    End If
End Function
```

## <a name="complete-example"></a>Exemplo completo

O código a seguir é um exemplo completo que inclui a função de `XForm` . Inclui alguns usos típicos desse tipo de transformações:

```vb
Imports System.Collections.Generic
Imports System.Linq
Imports System.Text
Imports System.Xml
Imports System.Xml.Linq

Imports <xmlns:xf="http://www.microsoft.com/LinqToXmlTransform/2007">

Module Module1
    Dim at As XName = GetXmlNamespace(xf) + "ApplyTransforms"

    ' Build a transformed XML tree per the annotations.
    Function XForm(ByVal source As XElement) As XElement
        If source.Annotation(Of XElement)() IsNot Nothing Then
            Dim anno As XElement = source.Annotation(Of XElement)()
            Return _
                <<%= anno.Name.ToString() %>>
                    <%= anno.Attributes() %>
                    <%= anno.Nodes().Select(Function(n As XNode) _
                        GetSubNodes(n, source)) %>
                </>
        Else
            Return _
                <<%= source.Name %>>
                    <%= source.Attributes() %>
                    <%= source.Nodes().Select(Function(n) GetExpandedNodes(n)) %>
                </>
        End If
    End Function

    Private Function GetSubNodes(ByVal n As XNode, ByVal s As XElement) As Object
        Dim annoEl As XElement = TryCast(n, XElement)
        If annoEl IsNot Nothing Then
            If annoEl.Name = at Then
                Return s.Nodes().Select(Function(n2 As XNode) GetExpandedNodes(n2))
            End If
        End If
        Return n
    End Function

    Private Function GetExpandedNodes(ByVal n2 As XNode) As XNode
        Dim e2 As XElement = TryCast(n2, XElement)
        If e2 Is Nothing Then
            Return n2
        Else
            Return XForm(e2)
        End If
    End Function

    Sub Main()
        Dim root As XElement = _
<Root Att1='123'>
    <!--A comment-->
    <Child>1</Child>
    <Child>2</Child>
    <Other>
        <GC>3</GC>
        <GC>4</GC>
    </Other>
    <SomeMixedContent>This is <i>an</i> element that <b>has</b> some mixed content</SomeMixedContent>
    <AnUnchangedElement>42</AnUnchangedElement>
</Root>

        ' Each of the following serves the same semantic purpose as
        ' XSLT templates and sequence constructors.

        ' Replace Child with NewChild.
        For Each el In root.<Child>
            el.AddAnnotation(<NewChild><%= CStr(el) %></NewChild>)
        Next

        ' Replace first GC with GrandChild, add an attribute.
        For Each el In root...<GC>.Take(1)
            el.AddAnnotation(<GrandChild ANewAttribute='999'><%= CStr(el) %></GrandChild>)
        Next

        ' Replace Other with NewOther, add new child elements around original content.
        For Each el In root.<Other>
            el.AddAnnotation( _
                <NewOther>
                    <MyNewChild>1</MyNewChild>
                    <<%= at %>></>
                    <ChildThatComesAfter/>
                </NewOther>)
        Next

        ' Change name of element that has mixed content.
        root...<SomeMixedContent>(0).AddAnnotation( _
                <MixedContent><<%= at %>></></MixedContent>)

        ' Replace <b> with <Bold>.
        For Each el In root...<b>
            el.AddAnnotation(<Bold><<%= at %>></></Bold>)
        Next

        ' Replace <i> with <Italic>.
        For Each el In root...<i>
            el.AddAnnotation(<Italic><<%= at %>></></Italic>)
        Next

        Console.WriteLine("Before Transform")
        Console.WriteLine("----------------")
        Console.WriteLine(root)
        Console.WriteLine(vbNewLine)
        Dim newRoot As XElement = XForm(root)

        Console.WriteLine("After Transform")
        Console.WriteLine("----------------")
        Console.WriteLine(newRoot)
    End Sub
End Module
```

Esse exemplo gera a saída a seguir:

```console
Before Transform
----------------
<Root Att1="123">
  <!--A comment-->
  <Child>1</Child>
  <Child>2</Child>
  <Other>
    <GC>3</GC>
    <GC>4</GC>
  </Other>
  <SomeMixedContent>This is <i>an</i> element that <b>has</b> some mixed content</SomeMixedContent>
  <AnUnchangedElement>42</AnUnchangedElement>
</Root>

After Transform
----------------
<Root Att1="123">
  <!--A comment-->
  <NewChild>1</NewChild>
  <NewChild>2</NewChild>
  <NewOther>
    <MyNewChild>1</MyNewChild>
    <GrandChild ANewAttribute="999">3</GrandChild>
    <GC>4</GC>
    <ChildThatComesAfter />
  </NewOther>
  <MixedContent>This is <Italic>an</Italic> element that <Bold>has</Bold> some mixed content</MixedContent>
  <AnUnchangedElement>42</AnUnchangedElement>
</Root>
```

## <a name="see-also"></a>Confira também

- [Programação de LINQ to XML avançada (Visual Basic)](advanced-linq-to-xml-programming.md)
