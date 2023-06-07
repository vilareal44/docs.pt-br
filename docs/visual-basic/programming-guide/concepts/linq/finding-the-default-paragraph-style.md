---
title: Localizar o estilo de parágrafo padrão
ms.date: 07/20/2015
ms.assetid: 9d094a4a-ec8c-41b0-b7ab-a3deb2a01d45
ms.openlocfilehash: b70ae72c293d00c4f7b7a2601bfd20b85702b6d5
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84398071"
---
# <a name="finding-the-default-paragraph-style-visual-basic"></a>Localizando o estilo de parágrafo padrão (Visual Basic)
A primeira tarefa no tutorial Manipulando informações em um documento de WordprocessingML é localizar o estilo padrão dos parágrafos no documento.  
  
## <a name="example"></a>Exemplo  
  
### <a name="description"></a>Descrição  
 O exemplo a seguir abre um documento do Office Open XML WordprocessingML, encontrar as partes do documento e estilo de pacote em seguida, executa uma consulta que localize o nome padrão de estilo. Para obter informações sobre pacotes de documentos XML abertos do Office e as partes das quais eles consistem, consulte [detalhes dos documentos do Office Open XML WordprocessingML (Visual Basic)](details-of-office-open-xml-wordprocessingml-documents.md).  
  
 A consulta encontrar um nó chamado `w:style` que tem um atributo chamado `w:type` com um valor de “parágrafo”, e também tem um atributo chamado `w:default` com um valor de “1 ". Haverá porque apenas um nó XML com esses atributos, a consulta usa o operador de <xref:System.Linq.Enumerable.First%2A?displayProperty=nameWithType> para converter uma coleção para um único. Então obtém o valor do atributo com o nome `w:styleId`.  
  
 Este exemplo usa classes do assembly WindowsBase. Ele usa tipos no namespace <xref:System.IO.Packaging?displayProperty=nameWithType>.  
  
### <a name="code"></a>Código  
  
```vb  
Imports <xmlns:w="http://schemas.openxmlformats.org/wordprocessingml/2006/main">  
  
Module Module1  
  
    Sub Main()  
  
        Const fileName As String = "SampleDoc.docx"  
  
        Const documentRelationshipType As String = _  
          "http://schemas.openxmlformats.org/officeDocument/2006/relationships/officeDocument"  
        Const stylesRelationshipType As String = _  
          "http://schemas.openxmlformats.org/officeDocument/2006/relationships/styles"  
  
        Dim xDoc As XDocument = Nothing  
        Dim styleDoc As XDocument = Nothing  
  
        Using wdPackage As Package = Package.Open(fileName, FileMode.Open, FileAccess.Read)  
            Dim docPackageRelationship As PackageRelationship = _  
              wdPackage.GetRelationshipsByType(documentRelationshipType).FirstOrDefault()  
            If docPackageRelationship IsNot Nothing Then  
                Dim documentUri As Uri = PackUriHelper.ResolvePartUri(New Uri("/", UriKind.Relative), _  
                  docPackageRelationship.TargetUri)  
                Dim documentPart As PackagePart = wdPackage.GetPart(documentUri)  
  
                ' Load the document XML in the part into an XDocument instance.  
                xDoc = XDocument.Load(XmlReader.Create(documentPart.GetStream()))  
  
                ' Find the styles part. There will only be one.  
                Dim styleRelation As PackageRelationship = _  
                  documentPart.GetRelationshipsByType(stylesRelationshipType).FirstOrDefault()  
                If styleRelation IsNot Nothing Then  
                    Dim styleUri As Uri = _  
                      PackUriHelper.ResolvePartUri(documentUri, styleRelation.TargetUri)  
                    Dim stylePart As PackagePart = wdPackage.GetPart(styleUri)  
  
                    ' Load the style XML in the part into an XDocument instance.  
                    styleDoc = XDocument.Load(XmlReader.Create(stylePart.GetStream()))  
                End If  
            End If  
        End Using  
  
        ' The following query finds all the paragraphs that have the default style.  
        Dim defParas As IEnumerable(Of XElement) = _  
            From style In styleDoc.Root.<w:style> _  
            Where style.@w:type.Equals("paragraph") And _  
                   style.@w:default.Equals("1") _  
            Select style  
        ' Then find the style of the first.  
        Dim defaultStyle As String = defParas.First().@w:styleId  
  
        Console.WriteLine("The default style is: " & defaultStyle)  
    End Sub  
End Module  
```  
  
### <a name="comments"></a>Comentários  
 Esse exemplo gera a saída a seguir:  
  
```console  
The default style is: Normal  
```  
  
## <a name="next-steps"></a>Próximas etapas  
 No exemplo a seguir, você criará uma consulta semelhante que localiza todos os parágrafos em um documento e seus estilos:  
  
- [Recuperando os parágrafos e seus estilos (Visual Basic)](retrieving-the-paragraphs-and-their-styles.md)  
  
## <a name="see-also"></a>Confira também

- [Tutorial: manipulando conteúdo em um documento do WordprocessingML (Visual Basic)](tutorial-manipulating-content-in-a-wordprocessingml-document.md)
