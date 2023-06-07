---
title: Visão geral dos eixos do LINQ to XML (C#)
description: Saiba mais sobre os métodos de eixo em C#, também chamados de eixos. Você pode consultar uma árvore XML no LINQ para localizar elementos e atributos e recuperar seus valores por meio de eixos.
ms.date: 07/20/2015
ms.assetid: 516792fb-461d-40a8-8a50-9993a51258fc
ms.openlocfilehash: bbda844e8cb2e3b1ff116fd834c6ab1fdd20c1f8
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87165449"
---
# <a name="linq-to-xml-axes-overview-c"></a>Visão geral dos eixos do LINQ to XML (C#)
Após criar uma árvore XML ou carregar um documento XML em uma árvore XML, você poderá consultá-la para localizar elementos e atributos, e recuperar seus valores. Você recupera coleções por meio dos *métodos de eixo*, também denominados *eixos*. Alguns eixos são métodos nas classes <xref:System.Xml.Linq.XElement> e <xref:System.Xml.Linq.XDocument> que retornam coleções <xref:System.Collections.Generic.IEnumerable%601>. Alguns eixos são métodos de extensão na classe <xref:System.Xml.Linq.Extensions>. Os eixos implementados como métodos de extensão operam em coleções e retornam coleções.  
  
 Conforme descrito em [Visão geral da classe XElement](./xelement-class-overview.md), um objeto <xref:System.Xml.Linq.XElement> representa um nó de um único elemento. O conteúdo de um elemento pode ser complexo (às vezes denominado conteúdo estruturado) ou pode ser um elemento simples. Um elemento simples pode ser vazio ou conter um valor. Se o nó contiver o conteúdo estruturado, você poderá usar os vários métodos de eixo para recuperar enumerações de elementos descendentes. Os métodos de eixo mais usados do eixo são <xref:System.Xml.Linq.XContainer.Elements%2A> e <xref:System.Xml.Linq.XContainer.Descendants%2A>.  
  
 Além dos métodos de eixo, que retornam coleções, há mais dois métodos que você geralmente usa nas consultas do [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)]. O método <xref:System.Xml.Linq.XContainer.Element%2A> retorna um <xref:System.Xml.Linq.XElement> único. O método <xref:System.Xml.Linq.XElement.Attribute%2A> retorna um <xref:System.Xml.Linq.XAttribute> único.  
  
 Para muitas finalidades, as consultas LINQ fornecem a maneira mais poderosa de examinar uma árvore, extrair dados dela e transformá-la. Consultas LINQ operam em objetos que implementam <xref:System.Collections.Generic.IEnumerable%601> e os [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] eixos retornam <xref:System.Collections.Generic.IEnumerable%601> de <xref:System.Xml.Linq.XElement> coleções e <xref:System.Collections.Generic.IEnumerable%601> de <xref:System.Xml.Linq.XAttribute> coleções. Você precisa dessas coleções para executar suas consultas.  
  
 Além dos métodos de eixo que recuperam coleções de elementos e atributos, há métodos de eixo que permitem a você iterar na árvore detalhadamente. Por exemplo, em vez de tratar elementos e atributos, você pode trabalhar com os nós da árvore. Os nós são um nível mais refinado de granularidade do que os elementos e os atributos. Ao trabalhar com os nós, você pode examinar comentários XML, nós de texto, instruções de processamento e muito mais. Essa funcionalidade é importante, por exemplo, para alguém que estiver escrevendo em um processador de texto e deseja salvar documentos como XML. No entanto, a maioria dos programadores XML se preocupam basicamente com os elementos, os atributos e seus valores.  
  
## <a name="methods-for-retrieving-a-collection-of-elements"></a>Métodos para recuperar uma coleção de elementos  
 Este é um resumo dos métodos da classe <xref:System.Xml.Linq.XElement> (ou de suas classes base) que você chama em um <xref:System.Xml.Linq.XElement> para retornar uma coleção de elementos.  
  
|Método|Descrição|  
|------------|-----------------|  
|<xref:System.Xml.Linq.XNode.Ancestors%2A?displayProperty=nameWithType>|Retorna um <xref:System.Collections.Generic.IEnumerable%601> do <xref:System.Xml.Linq.XElement> dos ancestrais desse elemento. Uma sobrecarga retorna um <xref:System.Collections.Generic.IEnumerable%601> do <xref:System.Xml.Linq.XElement> dos ancestrais que têm o <xref:System.Xml.Linq.XName> especificado.|  
|<xref:System.Xml.Linq.XContainer.Descendants%2A?displayProperty=nameWithType>|Retorna um <xref:System.Collections.Generic.IEnumerable%601> do <xref:System.Xml.Linq.XElement> dos descendentes desse elemento. Uma sobrecarga retorna um <xref:System.Collections.Generic.IEnumerable%601> do <xref:System.Xml.Linq.XElement> dos descendentes que têm o <xref:System.Xml.Linq.XName> especificado.|  
|<xref:System.Xml.Linq.XContainer.Elements%2A?displayProperty=nameWithType>|Retorna um <xref:System.Collections.Generic.IEnumerable%601> do <xref:System.Xml.Linq.XElement> dos elementos filho desse elemento. Uma sobrecarga retorna um <xref:System.Collections.Generic.IEnumerable%601> do <xref:System.Xml.Linq.XElement> dos elementos filho que têm o <xref:System.Xml.Linq.XName> especificado.|  
|<xref:System.Xml.Linq.XNode.ElementsAfterSelf%2A?displayProperty=nameWithType>|Retorna um <xref:System.Collections.Generic.IEnumerable%601> do <xref:System.Xml.Linq.XElement> dos elementos que vêm após esse elemento. Uma sobrecarga retorna um <xref:System.Collections.Generic.IEnumerable%601> do <xref:System.Xml.Linq.XElement> dos elementos após esse elemento que têm o <xref:System.Xml.Linq.XName> especificado.|  
|<xref:System.Xml.Linq.XNode.ElementsBeforeSelf%2A?displayProperty=nameWithType>|Retorna um <xref:System.Collections.Generic.IEnumerable%601> do <xref:System.Xml.Linq.XElement> dos elementos que vêm antes desse elemento. Uma sobrecarga retorna um <xref:System.Collections.Generic.IEnumerable%601> do <xref:System.Xml.Linq.XElement> dos elementos antes desse elemento que têm o <xref:System.Xml.Linq.XName> especificado.|  
|<xref:System.Xml.Linq.XElement.AncestorsAndSelf%2A?displayProperty=nameWithType>|Retorna um <xref:System.Collections.Generic.IEnumerable%601> do <xref:System.Xml.Linq.XElement> dos elementos e seus ancestrais. Uma sobrecarga retorna um <xref:System.Collections.Generic.IEnumerable%601> do <xref:System.Xml.Linq.XElement> dos elementos que têm o <xref:System.Xml.Linq.XName> especificado.|  
|<xref:System.Xml.Linq.XElement.DescendantsAndSelf%2A?displayProperty=nameWithType>|Retorna um <xref:System.Collections.Generic.IEnumerable%601> do <xref:System.Xml.Linq.XElement> desse elemento e seus descendentes. Uma sobrecarga retorna um <xref:System.Collections.Generic.IEnumerable%601> do <xref:System.Xml.Linq.XElement> dos elementos que têm o <xref:System.Xml.Linq.XName> especificado.|  
  
## <a name="method-for-retrieving-a-single-element"></a>Método para recuperar um único elemento  
 O método a seguir recupera um único filho de um objeto <xref:System.Xml.Linq.XElement>.  
  
|Método|Descrição|  
|------------|-----------------|  
|<xref:System.Xml.Linq.XContainer.Element%2A?displayProperty=nameWithType>|Retorna o primeiro objeto filho <xref:System.Xml.Linq.XElement> que tem o <xref:System.Xml.Linq.XName> especificado.|  
  
## <a name="method-for-retrieving-a-collection-of-attributes"></a>Método para recuperar uma coleção de atributos  
 O método a seguir recupera atributos de um objeto <xref:System.Xml.Linq.XElement>.  
  
|Método|Descrição|  
|------------|-----------------|  
|<xref:System.Xml.Linq.XElement.Attributes%2A?displayProperty=nameWithType>|Retorna um <xref:System.Collections.Generic.IEnumerable%601> do <xref:System.Xml.Linq.XAttribute> de todos os atributos.|  
  
## <a name="method-for-retrieving-a-single-attribute"></a>Método para recuperar um único atributo  
 O método a seguir recupera um único atributo de um objeto <xref:System.Xml.Linq.XElement>.  
  
|Método|Descrição|  
|------------|-----------------|  
|<xref:System.Xml.Linq.XElement.Attribute%2A?displayProperty=nameWithType>|Retorna o <xref:System.Xml.Linq.XAttribute> que tem o <xref:System.Xml.Linq.XName> especificado.|  
  
## <a name="see-also"></a>Confira também

- [Eixos do LINQ to XML (C#)](linq-to-xml-axes-overview.md)
