---
title: Parte do estilo de um document2 do WordprocessingML
ms.date: 07/20/2015
ms.assetid: 292cc094-9483-4192-ac3b-a5dc51fbac12
ms.openlocfilehash: 0692a9cc3a2ce51851d1b551d49f112fc5e4e41a
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84406749"
---
# <a name="style-part-of-a-wordprocessingml-document"></a>Parte de estilo de um documento de WordprocessingML
Este tópico mostra um exemplo de parte de estilo de documento do Office Open XML WordprocessingML.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir é XML que compõem a parte de estilo de um documento do Office Open XML WordprocessingML.  
  
 O estilo padrão de parágrafo tem um elemento com a seguir marca de abertura:  
  
```xml  
<w:style w:type="paragraph" w:default="1" w:styleId="Normal">  
```  
  
 Você precisa saber essas informações quando você escreve a consulta para localizar o identificador padrão de estilo, de modo que a consulta pode identificar o estilo dos parágrafos que têm o estilo padrão.  
  
 Observe que esses documentos são muito simples quando comparados para documentos típicos que o Microsoft Word gerencia. Em muitos casos, o Word salva muitos informações adicionais, formatação extra e metadados. Além disso, Palavra não formata as linhas para ser facilmente legíveis como neste exemplo; em vez disso, XML é salvo sem recuo. No entanto, todos os documentos de WordprocessingML compartilham a mesma forma básica XML. Devido a isso, consultas apresentadas neste tutorial funcionarão com documentos mais complicados.  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<w:styles  
    xmlns:r="http://schemas.openxmlformats.org/officeDocument/2006/relationships"  
    xmlns:w="http://schemas.openxmlformats.org/wordprocessingml/2006/main">  
  <w:docDefaults>  
    <w:rPrDefault>  
      <w:rPr>  
        <w:rFonts  
            w:ascii="Times New Roman"  
            w:eastAsia="Times New Roman"  
            w:hAnsi="Times New Roman"  
            w:cs="Times New Roman" />  
        <w:sz w:val="22" />  
        <w:szCs w:val="22" />  
        <w:lang w:val="en-US" w:eastAsia="en-US" w:bidi="ar-SA" />  
      </w:rPr>  
    </w:rPrDefault>  
    <w:pPrDefault />  
  </w:docDefaults>  
  <w:style w:type="paragraph" w:default="1" w:styleId="Normal">  
    <w:name w:val="Normal" />  
    <w:qFormat />  
    <w:rPr>  
      <w:sz w:val="24" />  
      <w:szCs w:val="24" />  
    </w:rPr>  
  </w:style>  
  <w:style w:type="paragraph" w:styleId="Heading1">  
    <w:name w:val="heading 1" />  
    <w:basedOn w:val="Normal" />  
    <w:next w:val="Normal" />  
    <w:link w:val="Heading1Char" />  
    <w:uiPriority w:val="99" />  
    <w:qFormat />  
    <w:rsid w:val="006027C7" />  
    <w:pPr>  
      <w:keepNext />  
      <w:spacing w:before="240" w:after="60" />  
      <w:outlineLvl w:val="0" />  
    </w:pPr>  
    <w:rPr>  
      <w:rFonts w:ascii="Arial" w:hAnsi="Arial" w:cs="Arial" />  
      <w:b />  
      <w:bCs />  
      <w:kern w:val="32" />  
      <w:sz w:val="32" />  
      <w:szCs w:val="32" />  
    </w:rPr>  
  </w:style>  
  <w:style w:type="character" w:default="1" w:styleId="DefaultParagraphFont">  
    <w:name w:val="Default Paragraph Font" />  
    <w:uiPriority w:val="99" />  
    <w:semiHidden />  
  </w:style>  
  <w:style w:type="table" w:default="1" w:styleId="TableNormal">  
    <w:name w:val="Normal Table" />  
    <w:uiPriority w:val="99" />  
    <w:semiHidden />  
    <w:unhideWhenUsed />  
    <w:qFormat />  
    <w:tblPr>  
      <w:tblInd w:w="0" w:type="dxa" />  
      <w:tblCellMar>  
        <w:top w:w="0" w:type="dxa" />  
        <w:left w:w="108" w:type="dxa" />  
        <w:bottom w:w="0" w:type="dxa" />  
        <w:right w:w="108" w:type="dxa" />  
      </w:tblCellMar>  
    </w:tblPr>  
  </w:style>  
  <w:style w:type="numbering" w:default="1" w:styleId="NoList">  
    <w:name w:val="No List" />  
    <w:uiPriority w:val="99" />  
    <w:semiHidden />  
    <w:unhideWhenUsed />  
  </w:style>  
  <w:style w:type="character" w:customStyle="1" w:styleId="Heading1Char">  
    <w:name w:val="Heading 1 Char" />  
    <w:basedOn w:val="DefaultParagraphFont" />  
    <w:link w:val="Heading1" />  
    <w:uiPriority w:val="9" />  
    <w:rsid w:val="009765E3" />  
    <w:rPr>  
      <w:rFonts  
          w:asciiTheme="majorHAnsi"  
          w:eastAsiaTheme="majorEastAsia"  
          w:hAnsiTheme="majorHAnsi"  
          w:cstheme="majorBidi" />  
      <w:b />  
      <w:bCs />  
      <w:kern w:val="32" />  
      <w:sz w:val="32" />  
      <w:szCs w:val="32" />  
    </w:rPr>  
  </w:style>  
  <w:style w:type="paragraph" w:customStyle="1" w:styleId="Code">  
    <w:name w:val="Code" />  
    <w:aliases w:val="c" />  
    <w:uiPriority w:val="99" />  
    <w:rsid w:val="006027C7" />  
    <w:pPr>  
      <w:spacing w:after="60" w:line="300" w:lineRule="exact" />  
    </w:pPr>  
    <w:rPr>  
      <w:rFonts w:ascii="Courier New" w:hAnsi="Courier New" />  
      <w:noProof />  
      <w:color w:val="000080" />  
      <w:sz w:val="20" />  
      <w:szCs w:val="20" />  
    </w:rPr>  
  </w:style>  
</w:styles>  
```  
  
## <a name="see-also"></a>Confira também

- [Detalhes dos documentos do Office Open XML WordprocessingML (Visual Basic)](details-of-office-open-xml-wordprocessingml-documents.md)
