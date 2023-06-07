---
title: Recuperar o texto dos parágrafos
ms.date: 07/20/2015
ms.assetid: 095fa0d9-7b1b-4cbb-9c13-e2c9d8923d31
ms.openlocfilehash: 24167ade2d0326ef9382536f79f9e45eb22c89c5
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84413382"
---
# <a name="retrieving-the-text-of-the-paragraphs-visual-basic"></a>Recuperando o texto dos parágrafos (Visual Basic)
Este exemplo baseia-se no exemplo anterior, [recuperando os parágrafos e seus estilos (Visual Basic)](retrieving-the-paragraphs-and-their-styles.md). Esse novo exemplo recupera o texto de cada parágrafo como uma cadeia de caracteres.  
  
 Para recuperar o texto, este exemplo adiciona uma consulta adicional que executa iterações através da coleção de tipos anônimos e projeta uma nova coleção de um tipo anônimo com a adição de um novo membro, `Text`. Usa o operador padrão de consulta de <xref:System.Linq.Enumerable.Aggregate%2A> para concatenar várias cadeias de caracteres em uma cadeia de caracteres.  
  
 Essa técnica (isto é, principalmente se projetando a uma coleção de um tipo anônimo, usando nessa coleção para se a projetar uma nova coleção de um tipo anônimo) é uma linguagem comum e útil. Esta consulta pode ter sido gravados sem projeto para o primeiro tipo anônimo. Entretanto, por causa da avaliação lazy, fazer isso não usa de poder de processamento adicional. O idioma cria um breves mais objetos na heap, mas isso não prejudica significativamente o desempenho.  
  
 Naturalmente, seria possível escrever uma única consulta que contém a funcionalidade para recuperar os parágrafos, o estilo de cada parágrafo, e o texto de cada parágrafo. No entanto, geralmente é útil dividir uma consulta mais complexa em consultas múltiplas porque o código resultante é mais modular e fácil de manter. Além disso, se você precisar de reutilizar parte de consulta, é mais fácil refatorar se as consultas são gravadas dessa maneira.  
  
 Essas consultas, que são encadeadas, usam o modelo de processamento que é examinado em detalhes no tópico [tutorial: execução adiada (Visual Basic)](tutorial-deferred-execution.md).  
  
## <a name="example"></a>Exemplo  
 Este exemplo processa um documento de WordprocessingML, determinando o nó do elemento, o nome do estilo, e o texto de cada parágrafo. Este exemplo cria nos exemplos anteriores neste tutorial. A nova consulta é chamada nos comentários no código a seguir.  
  
 Para obter instruções sobre como criar o documento de origem para este exemplo, consulte [criando o documento Office Open XML de origem (Visual Basic)](creating-the-source-office-open-xml-document.md).  
  
 Este exemplo usa classes do assembly WindowsBase. Ele usa tipos no namespace <xref:System.IO.Packaging?displayProperty=nameWithType>.  
  
```vb  
Imports <xmlns:w="http://schemas.openxmlformats.org/wordprocessingml/2006/main">  
  
Module Module1  
    ' Following function is required because Visual Basic does not support short circuit evaluation  
    Private Function GetStyleOfParagraph(ByVal styleNode As XElement, _  
                                         ByVal defaultStyle As String) As String  
        If (styleNode Is Nothing) Then  
            Return defaultStyle  
        Else  
            Return styleNode.@w:val  
        End If  
    End Function  
  
    Sub Main()  
        Dim fileName = "SampleDoc.docx"  
  
        Dim documentRelationshipType = _  
          "http://schemas.openxmlformats.org/officeDocument/2006/relationships/officeDocument"  
        Dim stylesRelationshipType = _  
          "http://schemas.openxmlformats.org/officeDocument/2006/relationships/styles"  
        Dim wordmlNamespace = _  
          "http://schemas.openxmlformats.org/wordprocessingml/2006/main"  
  
        Dim xDoc As XDocument = Nothing  
        Dim styleDoc As XDocument = Nothing  
        Using wdPackage As Package = Package.Open(fileName, FileMode.Open, FileAccess.Read)  
            Dim docPackageRelationship As PackageRelationship = _  
              wdPackage.GetRelationshipsByType(documentRelationshipType).FirstOrDefault()  
            If (docPackageRelationship IsNot Nothing) Then  
                Dim documentUri As Uri = PackUriHelper.ResolvePartUri(New Uri("/", UriKind.Relative), _  
                  docPackageRelationship.TargetUri)  
                Dim documentPart As PackagePart = wdPackage.GetPart(documentUri)  
  
                '  Load the document XML in the part into an XDocument instance.  
                xDoc = XDocument.Load(XmlReader.Create(documentPart.GetStream()))  
  
                '  Find the styles part. There will only be one.  
                Dim styleRelation As PackageRelationship = _  
                  documentPart.GetRelationshipsByType(stylesRelationshipType).FirstOrDefault()  
                If (styleRelation IsNot Nothing) Then  
                    Dim styleUri As Uri = _  
                      PackUriHelper.ResolvePartUri(documentUri, styleRelation.TargetUri)  
                    Dim stylePart As PackagePart = wdPackage.GetPart(styleUri)  
  
                    '  Load the style XML in the part into an XDocument instance.  
                    styleDoc = XDocument.Load(XmlReader.Create(stylePart.GetStream()))  
                End If  
            End If  
        End Using  
  
        Dim defaultStyle As String = _  
            ( _  
                From style In styleDoc.Root.<w:style> _  
                Where style.@w:type = "paragraph" And _  
                      style.@w:default = "1" _  
                Select style _  
            ).First().@w:styleId  
  
        ' Find all paragraphs in the document.  
        Dim paragraphs = _  
            From para In xDoc.Root.<w:body>...<w:p> _  
        Let styleNode As XElement = para.<w:pPr>.<w:pStyle>.FirstOrDefault _  
        Select New With { _  
            .ParagraphNode = para, _  
            .StyleName = GetStyleOfParagraph(styleNode, defaultStyle) _  
        }  
  
        ' Following is the new query that retrieves the text of  
        ' each paragraph.  
        Dim paraWithText = _  
            From para In paragraphs _  
            Select New With { _  
                .ParagraphNode = para.ParagraphNode, _  
                .StyleName = para.StyleName, _  
                .Text = para.ParagraphNode.<w:r>.<w:t> _  
                    .Aggregate(New StringBuilder(), _  
                               Function(s As StringBuilder, i As String) s.Append(CStr(i)), _  
                               Function(s As StringBuilder) s.ToString) _  
            }  
  
        For Each p In paraWithText  
            Console.WriteLine("StyleName:{0} >{1}<", p.StyleName, p.Text)  
        Next  
  
    End Sub  
End Module  
```  
  
 Este exemplo produz a saída a seguir quando aplicada ao documento descrito em [criando o documento Office Open XML de origem (Visual Basic)](creating-the-source-office-open-xml-document.md).  
  
```console  
StyleName:Heading1 >Parsing WordprocessingML with LINQ to XML<  
StyleName:Normal ><  
StyleName:Normal >The following example prints to the console.<  
StyleName:Normal ><  
StyleName:Code >Imports System<  
StyleName:Code ><  
StyleName:Code >Class Program<  
StyleName:Code >    Public Shared  Sub Main(ByVal args() As String)<  
StyleName:Code >        Console.WriteLine("Hello World")<  
StyleName:Code >   End Sub<  
StyleName:Code >End Class<  
StyleName:Normal ><  
StyleName:Normal >This example produces the following output:<  
StyleName:Normal ><  
StyleName:Code >Hello World<  
```  
  
## <a name="next-steps"></a>Próximas etapas  
 O exemplo a seguir mostra como usar um método de extensão, em vez de <xref:System.Linq.Enumerable.Aggregate%2A>, para concatenar várias cadeias de caracteres em uma única cadeia de caracteres.  
  
- [Refatoração usando um método de extensão (Visual Basic)](refactoring-using-an-extension-method.md)  
  
## <a name="see-also"></a>Confira também

- [Tutorial: manipulando conteúdo em um documento do WordprocessingML (Visual Basic)](tutorial-manipulating-content-in-a-wordprocessingml-document.md)
- [Execução retardada e avaliação lenta no LINQ to XML (Visual Basic)](deferred-execution-and-lazy-evaluation-in-linq-to-xml.md)
