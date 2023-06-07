---
title: Literal de Documento XML
ms.date: 07/20/2015
f1_keywords:
- vb.XmlLiteralDocument
helpviewer_keywords:
- XML document literal [Visual Basic]
- XML literals [Visual Basic], document
- XML documents [Visual Basic], creating
- document literal [Visual Basic]
ms.assetid: f7bbee56-0911-41de-b907-96f20450137b
ms.openlocfilehash: 3a2182d2937827bc8dc45e22307a3668420261a2
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400196"
---
# <a name="xml-document-literal-visual-basic"></a>Literal de documento XML (Visual Basic)
Um literal que representa um <xref:System.Xml.Linq.XDocument> objeto.  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<?xml version="1.0" [encoding="encoding"] [standalone="standalone"] ?>  
[ piCommentList ]  
rootElement  
[ piCommentList ]  
```  
  
## <a name="parts"></a>Partes  
  
|Termo|Definição|  
|---|---|  
|`encoding`|Opcional. Texto literal que declara a codificação usada pelo documento.|  
|`standalone`|Opcional. Texto literal. Deve ser "Yes" ou "no".|  
|`piCommentList`|Opcional. Lista de instruções de processamento XML e comentários XML. Usa o seguinte formato:<br /><br /> `piComment [` `piComment` `... ]`<br /><br /> Cada `piComment` um pode ser um dos seguintes:<br /><br /> -   [Literal de instrução de processamento XML](xml-processing-instruction-literal.md).<br />-   [Literal de comentário XML](xml-comment-literal.md).|  
|`rootElement`|Obrigatórios. Elemento raiz do documento. O formato é um dos seguintes:<br /><br /> <ul><li>[Literal de elemento XML](xml-element-literal.md).</li><li>Expressão inserida do formulário `<%=` `elementExp` `%>` . O `elementExp` retorna um dos seguintes:<br /><br /> <ul><li>Um objeto <xref:System.Xml.Linq.XElement>.</li><li>Uma coleção que contém um <xref:System.Xml.Linq.XElement> objeto e qualquer número de <xref:System.Xml.Linq.XProcessingInstruction> <xref:System.Xml.Linq.XComment> objetos e.</li></ul></li></ul><br /> Para obter mais informações, consulte [expressões inseridas em XML](../../programming-guide/language-features/xml/embedded-expressions-in-xml.md).|  
  
## <a name="return-value"></a>Valor Retornado  
 Um objeto <xref:System.Xml.Linq.XDocument>.  
  
## <a name="remarks"></a>Comentários  
 Um literal de documento XML é identificado pela declaração XML no início do literal. Embora cada literal de documento XML deva ter exatamente um elemento XML raiz, ele pode ter qualquer número de instruções de processamento XML e comentários XML.  
  
 Um literal de documento XML não pode aparecer em um elemento XML.  
  
> [!NOTE]
> Um literal XML pode abranger várias linhas sem usar caracteres de continuação de linha. Isso permite que você copie o conteúdo de um documento XML e cole-o diretamente em um programa Visual Basic.  
  
 O compilador de Visual Basic converte o literal de documento XML em chamadas para os <xref:System.Xml.Linq.XDocument.%23ctor%2A> <xref:System.Xml.Linq.XDeclaration.%23ctor%2A> construtores e.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir cria um documento XML que tem uma declaração XML, uma instrução de processamento, um comentário e um elemento que contém outro elemento.  
  
 [!code-vb[VbXMLSamples#30](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples13.vb#30)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.Xml.Linq.XElement>
- <xref:System.Xml.Linq.XProcessingInstruction>
- <xref:System.Xml.Linq.XComment>
- <xref:System.Xml.Linq.XDocument>
- [Literal de Instrução de Processamento XML](xml-processing-instruction-literal.md)
- [Literal de Comentário XML](xml-comment-literal.md)
- [Literal do Elemento XML](xml-element-literal.md)
- [Literais XML](index.md)
- [Criando XML no Visual Basic](../../programming-guide/language-features/xml/creating-xml.md)
- [Expressões inseridas no XML](../../programming-guide/language-features/xml/embedded-expressions-in-xml.md)
