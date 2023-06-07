---
title: Localizando o estilo de parágrafo padrão (C#)
description: Saiba como processar um documento do WordprocessingML com LINQ em C#. Este exemplo localiza o estilo padrão de parágrafos no documento.
ms.date: 07/20/2015
ms.assetid: be102177-8ab0-444a-b671-7023e555ffdb
ms.openlocfilehash: e18bbbdbd5b2627c9ff4c3c3eedd84d7cb166a62
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87103826"
---
# <a name="finding-the-default-paragraph-style-c"></a>Localizando o estilo de parágrafo padrão (C#)
A primeira tarefa no tutorial Manipulando informações em um documento de WordprocessingML é localizar o estilo padrão dos parágrafos no documento.  
  
## <a name="example"></a>Exemplo  
  
### <a name="description"></a>Descrição  
 O exemplo a seguir abre um documento do Office Open XML WordprocessingML, encontrar as partes do documento e estilo de pacote em seguida, executa uma consulta que localize o nome padrão de estilo. Para obter informações sobre pacotes de documento do Office Open XML, bem como as partes que os compõem, consulte em [Detalhes de documentos do Office Open XML WordprocessingML (C#)](./wordprocessingml-document-with-styles.md).  
  
 A consulta encontrar um nó chamado `w:style` que tem um atributo chamado `w:type` com um valor de “parágrafo”, e também tem um atributo chamado `w:default` com um valor de “1 ". Haverá porque apenas um nó XML com esses atributos, a consulta usa o operador de <xref:System.Linq.Enumerable.First%2A?displayProperty=nameWithType> para converter uma coleção para um único. Então obtém o valor do atributo com o nome `w:styleId`.  
  
 Este exemplo usa classes do assembly WindowsBase. Ele usa tipos no namespace <xref:System.IO.Packaging?displayProperty=nameWithType>.  
  
### <a name="code"></a>Código  
  
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
  
// The following query finds all the paragraphs that have the default style.  
string defaultStyle =
    (string)(  
        from style in styleDoc.Root.Elements(w + "style")  
        where (string)style.Attribute(w + "type") == "paragraph"&&  
              (string)style.Attribute(w + "default") == "1"  
              select style  
    ).First().Attribute(w + "styleId");  
  
Console.WriteLine("The default style is: {0}", defaultStyle);  
```  
  
### <a name="comments"></a>Comentários  
 Esse exemplo gera a saída a seguir:  
  
```output  
The default style is: Normal  
```  
  
## <a name="next-steps"></a>Próximas etapas  
 No exemplo a seguir, você criará uma consulta semelhante que localiza todos os parágrafos em um documento e seus estilos:  
  
- [Recuperando os parágrafos e seus estilos (C#)](./retrieving-the-paragraphs-and-their-styles.md)  
