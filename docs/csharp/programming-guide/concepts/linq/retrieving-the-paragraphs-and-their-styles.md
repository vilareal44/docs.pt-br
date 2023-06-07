---
title: Recuperando os parágrafos e seus estilos (C#)
description: Saiba como escrever uma consulta que recupera parágrafos e, em seguida, identifica o estilo de cada parágrafo.
ms.date: 07/20/2015
ms.assetid: c2f767f8-57b1-4b4b-af04-89ffb1f7067d
ms.openlocfilehash: 94d01a13485f70bc9d9cef55b5f390c3b30d7f14
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87303394"
---
# <a name="retrieving-the-paragraphs-and-their-styles-c"></a>Recuperando os parágrafos e seus estilos (C#)
Nesse exemplo, nós escrevemos uma consulta que recupera os nós de parágrafo de um documento de WordprocessingML. Também identifica o estilo de cada parágrafo.  
  
 Esta consulta é baseada na consulta no exemplo anterior, [Localizando o estilo de parágrafo padrão (C#)](./finding-the-default-paragraph-style.md), que recupera o estilo padrão da lista de estilos. Essa informação é necessária de modo que a consulta pode identificar o estilo dos parágrafos que não têm um estilo definir explicitamente. Os estilos de parágrafo são definidos por meio do elemento de `w:pPr` ; se um parágrafo não contém esse elemento, é formatado com o estilo padrão.  
  
 Este tópico explica o significado de algumas partes de consulta, então mostra a consulta como parte de um exemplo completo, que funciona.  
  
## <a name="example"></a>Exemplo  
 A fonte da consulta para recuperar todos os parágrafos em um documento e seus estilos é a seguinte:  
  
```csharp  
xDoc.Root.Element(w + "body").Descendants(w + "p")  
```  
  
 Esta expressão é semelhante à fonte da consulta no exemplo anterior, [Localizando o estilo de parágrafo padrão (C#)](./finding-the-default-paragraph-style.md). A principal diferença é que usa o eixo de <xref:System.Xml.Linq.XContainer.Descendants%2A> em vez do eixo de <xref:System.Xml.Linq.XContainer.Elements%2A> . A consulta usa o eixo de <xref:System.Xml.Linq.XContainer.Descendants%2A> porque em documentos que têm seções, os parágrafos não serão os filhos diretos do elemento do corpo; em vez, os parágrafos serão dois níveis para baixo na hierarquia. Usando o eixo de <xref:System.Xml.Linq.XContainer.Descendants%2A> , o código funcionará de mesmo se o documento usa seções.  
  
## <a name="example"></a>Exemplo  
 A consulta usa uma cláusula de `let` para determinar o elemento que contém o nó de estilo. Se não houver nenhum elemento, então `styleNode` é definido como `null`:  
  
```csharp  
let styleNode = para.Elements(w + "pPr").Elements(w + "pStyle").FirstOrDefault()  
```  
  
 A cláusula de `let` primeiro usa o eixo de <xref:System.Xml.Linq.XContainer.Elements%2A> para localizar todos os elementos chamados `pPr`, então usa o método de extensão de <xref:System.Xml.Linq.Extensions.Elements%2A> para localizar todos os elementos filho chamados `pStyle`, e finalmente usa o operador padrão de consulta de <xref:System.Linq.Enumerable.FirstOrDefault%2A> para converter a um único. Se a coleção estiver vazia, `styleNode` é definido como `null`. Este é um idioma útil para procurar o nó descendente de `pStyle` . Observe que se o nó filho de `pPr` não existir, o código faz ou falhas lançando uma exceção; em vez disso, `styleNode` é definido como `null`, que é o comportamento desejado desta cláusula de `let` .  
  
 A consulta em uma coleção de um tipo anônimo com dois membros, `StyleName` e `ParagraphNode`.  
  
## <a name="example"></a>Exemplo  
 Este exemplo processa um documento de WordprocessingML, recuperando os nós de parágrafo de um documento de WordprocessingML. Também identifica o estilo de cada parágrafo. Este exemplo cria nos exemplos anteriores neste tutorial. A nova consulta é chamada nos comentários no código a seguir.  
  
 É possível obter instruções para criar o documento de origem deste exemplo em [Criando o documento do Office Open XML de origem (C#)](./creating-the-source-office-open-xml-document.md).  
  
 Este exemplo usa as classes encontradas no assembly WindowsBase. Ele usa tipos no namespace <xref:System.IO.Packaging?displayProperty=nameWithType>.  
  
```csharp  
const string fileName = "SampleDoc.docx";  
  
const string documentRelationshipType = "http://schemas.openxmlformats.org/officeDocument/2006/relationships/officeDocument";  
const string stylesRelationshipType = "http://schemas.openxmlformats.org/officeDocument/2006/relationships/styles";  
const string wordmlNamespace = "http://schemas.openxmlformats.org/wordprocessingml/2006/main";  
XNamespace w = wordmlNamespace;  
  
XDocument xDoc = null;  
XDocument styleDoc = null;  
  
using (Package wdPackage = Package.Open(fileName, FileMode.Open, FileAccess.Read))  
{  
    PackageRelationship docPackageRelationship = wdPackage.GetRelationshipsByType(documentRelationshipType).FirstOrDefault();  
    if (docPackageRelationship != null)  
    {  
        Uri documentUri = PackUriHelper.ResolvePartUri(new Uri("/", UriKind.Relative), docPackageRelationship.TargetUri);  
        PackagePart documentPart = wdPackage.GetPart(documentUri);  
  
        //  Load the document XML in the part into an XDocument instance.  
        xDoc = XDocument.Load(XmlReader.Create(documentPart.GetStream()));  
  
        //  Find the styles part. There will only be one.  
        PackageRelationship styleRelation = documentPart.GetRelationshipsByType(stylesRelationshipType).FirstOrDefault();  
        if (styleRelation != null)  
        {  
            Uri styleUri = PackUriHelper.ResolvePartUri(documentUri, styleRelation.TargetUri);  
            PackagePart stylePart = wdPackage.GetPart(styleUri);  
  
            //  Load the style XML in the part into an XDocument instance.  
            styleDoc = XDocument.Load(XmlReader.Create(stylePart.GetStream()));  
        }  
    }  
}  
  
string defaultStyle =
    (string)(  
        from style in styleDoc.Root.Elements(w + "style")  
        where (string)style.Attribute(w + "type") == "paragraph"&&  
              (string)style.Attribute(w + "default") == "1"  
              select style  
    ).First().Attribute(w + "styleId");  
  
// Following is the new query that finds all paragraphs in the  
// document and their styles.  
var paragraphs =  
    from para in xDoc  
                 .Root  
                 .Element(w + "body")  
                 .Descendants(w + "p")  
    let styleNode = para  
                    .Elements(w + "pPr")  
                    .Elements(w + "pStyle")  
                    .FirstOrDefault()  
    select new  
    {  
        ParagraphNode = para,  
        StyleName = styleNode != null ?  
            (string)styleNode.Attribute(w + "val") :  
            defaultStyle  
    };  
  
foreach (var p in paragraphs)  
    Console.WriteLine("StyleName:{0}", p.StyleName);  
```  
  
 Este exemplo produz a seguinte saída quando aplicado ao documento descrito em [Criando o documento do Office Open XML de origem (C#)](./creating-the-source-office-open-xml-document.md).  
  
```output  
StyleName:Heading1  
StyleName:Normal  
StyleName:Normal  
StyleName:Normal  
StyleName:Code  
StyleName:Code  
StyleName:Code  
StyleName:Code  
StyleName:Code  
StyleName:Code  
StyleName:Code  
StyleName:Normal  
StyleName:Normal  
StyleName:Normal  
StyleName:Code  
```  
  
## <a name="next-steps"></a>Próximas etapas  
 No próximo tópico, [Recuperando o texto dos parágrafos (C#)](./retrieving-the-text-of-the-paragraphs.md), você criará uma consulta para recuperar o texto dos parágrafos.  
  
## <a name="see-also"></a>Confira também

- [Tutorial: manipulando conteúdo em um documento WordprocessingML (C#)](./shape-of-wordprocessingml-documents.md)
