---
title: Conjuntos de nó nas transformações
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: ad034f0e-ff8b-4a71-9a4c-528c754263c4
ms.openlocfilehash: 33cbae05cf35904903189ce767090d3d3cca8e4d
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84288739"
---
# <a name="node-sets-in-transformations"></a>Conjuntos de nó nas transformações
Conjuntos de nó é um dos quatro tipos de dados básicos que são retornados das expressões de idioma do caminho de XML (XPath). Um conjunto de nó, que é uma coleção não ordenada de nós sem duplicatas, criada na ordem de documento, pode ser atribuído a um variável em uma folha de estilos.  
  
> [!NOTE]
> A classe <xref:System.Xml.Xsl.XslTransform> está obsoleta no .NET Framework 2.0. Você pode executar a linguagem XSL Transformations (XSLT) usando a classe <xref:System.Xml.Xsl.XslCompiledTransform>. Confira [Usar a classe XslCompiledTransform](using-the-xslcompiledtransform-class.md) e [Migrar da classe XslTransform](migrating-from-the-xsltransform-class.md) para saber mais.  
  
 Conjuntos de nó é um dos quatro tipos de dados básicos que são retornados de expressões XPath. Um conjunto de nó, que é uma coleção não ordenada de nós sem duplicatas, criada na ordem de documento, pode ser atribuído a um variável em uma folha de estilos. Este conjunto de nó, que é um resultado de uma expressão XPath usado em um atributo de `select` em uma transformação, tem o mesmo comportamento que um nó definir o modelo de objeto (DOM) de documento XML. Você pode navegar em um conjunto de nós usando um conjunto de métodos mostrados em [Navegação do nó usando XPathNavigator](node-set-navigation-using-xpathnavigator.md), diferentemente de um fragmento de árvore de resultado, que usa <xref:System.Xml.XPath.XPathNodeIterator> para navegação.  
  
 O exemplo de código a seguir mostra como iterar sobre um nó definida quando um elemento de `variable` ou de `parameter` em uma folha de estilos é avaliada como um conjunto de nó.  
  
## <a name="style-sheet"></a>Folha de Estilos  
  
```xml  
<xsl:stylesheet xmlns:xsl="http://www.w3.org/1999/XSL/Transform" version="1.0">  
  
    <xsl:variable name="x" select="bookstore/book/title"></xsl:variable>  
  
    <xsl:template match="/">  
        <xsl:for-each select="$x">  
            ******  
            <xsl:value-of select="."></xsl:value-of>  
            ******  
        </xsl:for-each>  
    </xsl:template>  
  
</xsl:stylesheet>  
```  
  
## <a name="input"></a>Entrada  
  
```xml  
<bookstore>  
   <book style="autobiography">  
      <title>Seven Years in Trenton</title>  
   </book>  
  
   <book style="autobiography">  
      <title>Seven Years in Trenton2</title>  
   </book>  
  
   <book style="textbook">  
      <title>History of Trenton Vol 3</title>  
   </book>  
</bookstore>  
```  
  
## <a name="output"></a>Saída  
  
```output  
******  
Seven Years in Trenton  
******  
  
******  
Seven Years in Trenton2  
******  
  
******  
History of Trenton Vol 3  
******  
```  
  
## <a name="see-also"></a>Veja também

- <xref:System.Xml.XPath.XPathNodeIterator>
- [Transformações XSLT com a classe XslTransform](xslt-transformations-with-the-xsltransform-class.md)
- [A classe XslTransform implementa do processador XSLT](xsltransform-class-implements-the-xslt-processor.md)
