---
title: Saída de um XslTransform
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 8e149d32-4b2f-493f-9e4b-d0d93475acde
ms.openlocfilehash: 5fa8c8228382f7f326a8d2243ed0450fca31f6fb
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84288687"
---
# <a name="outputs-from-an-xsltransform"></a>Saída de um XslTransform
Como as folhas de estilos podem determinar o formato de saída usando uma instrução de `<xsl:output>` com o atributo de `method` , a tabela a seguir descreve quais o formato de saída é quando o método de <xref:System.Xml.Xsl.XslTransform.Transform%2A> é usado para gravar a saída, e o formato de saída é declarado como <xref:System.IO.Stream> ou <xref:System.IO.TextWriter>.  
  
> [!NOTE]
> A classe <xref:System.Xml.Xsl.XslTransform> está obsoleta no .NET Framework 2.0. Você pode executar a linguagem XSL Transformations (XSLT) usando a classe <xref:System.Xml.Xsl.XslCompiledTransform>. Confira [Usar a classe XslCompiledTransform](using-the-xslcompiledtransform-class.md) e [Migrar da classe XslTransform](migrating-from-the-xsltransform-class.md) para saber mais.  
  
 Como as folhas de estilos podem determinar o formato de saída usando uma instrução de `<xsl:output>` com o atributo de `method` , a tabela a seguir descreve quais o formato de saída é quando o método de <xref:System.Xml.Xsl.XslTransform.Transform%2A> é usado para gravar a saída, e o formato de saída é declarado como <xref:System.IO.Stream> ou <xref:System.IO.TextWriter>. A tabela a seguir descreve o que acontece quando um tipo de saída é declarado pelo método de <xref:System.Xml.Xsl.XslTransform.Transform%2A> em conjunto com o uso de uma instrução de `<xsl:output>` :  
  
|Atributo \<xsl:output method = >|Formato de resultado|  
|-----------------------------------------|-------------------|  
|method= " XML”|XML|  
|method= " HTML”|HTML|  
|method= " texto”|Texto|  
  
> [!NOTE]
> Observação: A declaração de `<xsl:output>` é ignorado quando a saída de método <xref:System.Xml.Xsl.XslTransform.Transform%2A> são <xref:System.Xml.XmlReader> ou <xref:System.Xml.XmlWriter>.  
  
 Os seguintes atributos são suportados quando a saída de método <xref:System.Xml.Xsl.XslTransform.Transform%2A> são <xref:System.IO.Stream> ou <xref:System.IO.TextWriter>:  
  
- encoding*  
  
- omit-xml-declaration  
  
- autônomos  
  
- doctype-public  
  
- doctype-system  
  
- cdata-section-elements  
  
- indent  
  
    > [!NOTE]
    > \*O atributo de codificação é ignorado quando o método <xref:System.Xml.Xsl.XslTransform.Transform%2A> está enviando sua saída para um <xref:System.IO.TextWriter>. A propriedade de codificação em <xref:System.IO.TextWriter> é usada em vez.
  
 O seguinte atributo é ignorado quando a saída de método <xref:System.Xml.Xsl.XslTransform.Transform%2A> são <xref:System.IO.Stream>:  
  
- versão: a versão é sempre 1,0  
  
- médio tipo: o tipo médio  
  
## <a name="escaping-special-characters"></a>Caracteres de escape especiais  
 A marca de `<xsl:text disable-output-escaping>` é usada para indicar se os caracteres especiais precisam ser escapados em um formulário XML (por exemplo, usando `<&lt>` no lugar do símbolo de `"<"` ) ou esquerdo em condição atual. O atributo de `disable-output-escaping` é ignorado quando uma transformação a <xref:System.Xml.XmlReader> ou <xref:System.Xml.XmlWriter> objetos e não tem efeito em caracteres especiais.  
  
## <a name="see-also"></a>Veja também

- [A classe XslTransform implementa do processador XSLT](xsltransform-class-implements-the-xslt-processor.md)
