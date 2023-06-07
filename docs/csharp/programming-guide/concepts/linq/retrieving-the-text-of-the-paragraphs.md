---
title: Recuperando o texto dos parágrafos (C#)
description: Saiba como usar consultas LINQ para obter o texto de cada parágrafo em um documento do WordprocessingML como uma cadeia de caracteres em C#. Este exemplo usa consultas encadeadas.
ms.date: 07/20/2015
ms.assetid: 127d635e-e559-408f-90c8-2bb621ca50ac
ms.openlocfilehash: 58a07ab848307c886927815e4e49e90806f61346
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87302588"
---
# <a name="retrieving-the-text-of-the-paragraphs-c"></a>Recuperando o texto dos parágrafos (C#)
Este exemplo é criado com base no exemplo anterior, [Recuperando os parágrafos e seus estilos (C#)](./retrieving-the-paragraphs-and-their-styles.md). Esse novo exemplo recupera o texto de cada parágrafo como uma cadeia de caracteres.  
  
 Para recuperar o texto, este exemplo adiciona uma consulta adicional que executa iterações através da coleção de tipos anônimos e projeta uma nova coleção de um tipo anônimo com a adição de um novo membro, `Text`. Usa o operador padrão de consulta de <xref:System.Linq.Enumerable.Aggregate%2A> para concatenar várias cadeias de caracteres em uma cadeia de caracteres.  
  
 Essa técnica (isto é, principalmente se projetando a uma coleção de um tipo anônimo, usando nessa coleção para se a projetar uma nova coleção de um tipo anônimo) é uma linguagem comum e útil. Esta consulta pode ter sido gravados sem projeto para o primeiro tipo anônimo. Entretanto, por causa da avaliação lazy, fazer isso não usa de poder de processamento adicional. O idioma cria um breves mais objetos na heap, mas isso não prejudica significativamente o desempenho.  
  
 Naturalmente, seria possível escrever uma única consulta que contém a funcionalidade para recuperar os parágrafos, o estilo de cada parágrafo, e o texto de cada parágrafo. No entanto, geralmente é útil dividir uma consulta mais complexa em consultas múltiplas porque o código resultante é mais modular e fácil de manter. Além disso, se você precisar de reutilizar parte de consulta, é mais fácil refatorar se as consultas são gravadas dessa maneira.  
  
 Essas consultas, que são encadeadas juntas, usam o modelo de processamento que é examinado em detalhes no tópico [Tutorial: encadear consultas juntas (C#)](deferred-execution-and-lazy-evaluation-in-linq-to-xml.md).  
  
## <a name="example"></a>Exemplo  
 Este exemplo processa um documento de WordprocessingML, determinando o nó do elemento, o nome do estilo, e o texto de cada parágrafo. Este exemplo cria nos exemplos anteriores neste tutorial. A nova consulta é chamada nos comentários no código a seguir.  
  
 Para obter instruções para criar o documento de origem deste exemplo, consulte [Criando o documento do Office Open XML de origem (C#)](./creating-the-source-office-open-xml-document.md).  
  
 Este exemplo usa classes do assembly WindowsBase. Ele usa tipos no namespace <xref:System.IO.Packaging?displayProperty=nameWithType>.  
  
```csharp  
const string fileName = "SampleDoc.docx";  
  
const string documentRelationshipType =  
  "http://schemas.openxmlformats.org/officeDocument/2006/relationships/officeDocument";  
const string stylesRelationshipType =  
  "http://schemas.openxmlformats.org/officeDocument/2006/relationships/styles";  
const string wordmlNamespace =  
  "http://schemas.openxmlformats.org/wordprocessingml/2006/main";  
XNamespace w = wordmlNamespace;  
  
XDocument xDoc = null;  
XDocument styleDoc = null;  
  
using (Package wdPackage = Package.Open(fileName, FileMode.Open, FileAccess.Read))  
{  
    PackageRelationship docPackageRelationship =  
      wdPackage.GetRelationshipsByType(documentRelationshipType).FirstOrDefault();  
    if (docPackageRelationship != null)  
    {  
        Uri documentUri = PackUriHelper.ResolvePartUri(new Uri("/", UriKind.Relative),  
          docPackageRelationship.TargetUri);  
        PackagePart documentPart = wdPackage.GetPart(documentUri);  
  
        //  Load the document XML in the part into an XDocument instance.  
        xDoc = XDocument.Load(XmlReader.Create(documentPart.GetStream()));  
  
        //  Find the styles part. There will only be one.  
        PackageRelationship styleRelation =  
          documentPart.GetRelationshipsByType(stylesRelationshipType).FirstOrDefault();  
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
  
// Find all paragraphs in the document.  
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
  
// Following is the new query that retrieves the text of  
// each paragraph.  
var paraWithText =  
    from para in paragraphs  
    select new  
    {  
        ParagraphNode = para.ParagraphNode,  
        StyleName = para.StyleName,  
        Text = para  
               .ParagraphNode  
               .Elements(w + "r")  
               .Elements(w + "t")  
               .Aggregate(  
                   new StringBuilder(),  
                   (s, i) => s.Append((string)i),  
                   s => s.ToString()  
               )  
    };  
  
foreach (var p in paraWithText)  
    Console.WriteLine("StyleName:{0} >{1}<", p.StyleName, p.Text);  
```  
  
 Este exemplo produz a seguinte saída quando aplicado ao documento descrito em [Criando o documento do Office Open XML de origem (C#)](./creating-the-source-office-open-xml-document.md).  
  
```output  
StyleName:Heading1 >Parsing WordprocessingML with LINQ to XML<  
StyleName:Normal ><  
StyleName:Normal >The following example prints to the console.<  
StyleName:Normal ><  
StyleName:Code >using System;<  
StyleName:Code ><  
StyleName:Code >class Program {<  
StyleName:Code >    public static void (string[] args) {<  
StyleName:Code >        Console.WriteLine("Hello World");<  
StyleName:Code >    }<  
StyleName:Code >}<  
StyleName:Normal ><  
StyleName:Normal >This example produces the following output:<  
StyleName:Normal ><  
StyleName:Code >Hello World<  
```  
  
## <a name="next-steps"></a>Próximas etapas  
 O exemplo a seguir mostra como usar um método de extensão, em vez de <xref:System.Linq.Enumerable.Aggregate%2A>, para concatenar várias cadeias de caracteres em uma única cadeia de caracteres.  
  
- [Refatoração usando um método de extensão (C#)](./refactoring-using-an-extension-method.md)  
  
## <a name="see-also"></a>Veja também

- [Tutorial: manipulando conteúdo em um documento WordprocessingML (C#)](shape-of-wordprocessingml-documents.md)
- [Execução adiada e avaliação lenta em LINQ to XML (C#)](./deferred-execution-and-lazy-evaluation-in-linq-to-xml.md)
