---
title: Conteúdo válido de XElement e XDocument Objects2
ms.date: 07/20/2015
ms.assetid: 400bb692-478a-40b6-ac1b-4ccbb4cbbd02
ms.openlocfilehash: d222f19f6f588968a3ef1515dca522a4a80e1ffb
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84364338"
---
# <a name="valid-content-of-xelement-and-xdocument-objects"></a>Conteúdo válido de objetos XElement e XDocument
Este tópico descreve os argumentos válidos que podem ser passados para os construtores e os métodos que você usa para adicionar conteúdo a elementos e documentos.  
  
## <a name="valid-content"></a>Conteúdo válido  
 As consultas geralmente avaliam para <xref:System.Collections.Generic.IEnumerable%601> de <xref:System.Xml.Linq.XElement> ou <xref:System.Collections.Generic.IEnumerable%601> de <xref:System.Xml.Linq.XAttribute>. Você pode passar coleções de <xref:System.Xml.Linq.XElement> ou de objetos <xref:System.Xml.Linq.XAttribute> para o construtor de <xref:System.Xml.Linq.XElement>. Portanto, é conveniente passar os resultados de uma consulta como conteúdo em métodos e os construtores que você usa para popular árvores XML.  
  
 Ao adicionar conteúdo simples, vários tipos podem ser passados para esse método. Os tipos válidos incluem:  
  
- <xref:System.String>  
  
- <xref:System.Double>  
  
- <xref:System.Single>  
  
- <xref:System.Decimal>  
  
- <xref:System.Boolean>  
  
- <xref:System.DateTime>  
  
- <xref:System.TimeSpan>  
  
- <xref:System.DateTimeOffset>  
  
- Qualquer tipo que implemente `Object.ToString`.  
  
- Qualquer tipo que implemente <xref:System.Collections.Generic.IEnumerable%601>.  
  
 Ao adicionar conteúdo complexo, vários tipos podem ser passados para esse método:  
  
- <xref:System.Xml.Linq.XObject>  
  
- <xref:System.Xml.Linq.XNode>  
  
- <xref:System.Xml.Linq.XAttribute>  
  
- Qualquer tipo que implementa <xref:System.Collections.Generic.IEnumerable%601>  
  
 Se um objeto implementar <xref:System.Collections.Generic.IEnumerable%601>, a coleção no objeto será enumerada, e todos os itens da coleção serão adicionados. Se a coleção contiver objetos <xref:System.Xml.Linq.XNode> ou <xref:System.Xml.Linq.XAttribute>, cada item da coleção será adicionado separadamente. Se a coleção contiver texto (ou objetos que são convertidos em texto), o texto da coleção será concatenado e adicionado como um único nó de texto.  
  
 Se o conteúdo for `null`, nada será adicionado. Ao passar itens de um coleção, a coleção pode ser `null`. Um item `null` na coleção não tem nenhum efeito na árvore.  
  
 Um atributo adicionado deve ter um nome exclusivo dentro do elemento que o contém.  
  
 Ao adicionar objetos <xref:System.Xml.Linq.XNode> ou <xref:System.Xml.Linq.XAttribute>, se o novo conteúdo não tiver um pai, os objetos serão simplesmente anexados à árvore XML. Se o novo conteúdo já tiver um pai e fizer parte de outra árvore XML, o novo conteúdo será clonado, e o conteúdo recém-clonado será anexado à árvore XML.  
  
## <a name="valid-content-for-documents"></a>Conteúdo válido para documentos  
 Atributos e conteúdo simples não podem ser adicionados a um documento.  
  
 Não há muitos cenários que exijam a criação de um <xref:System.Xml.Linq.XDocument>. Em vez disso, você normalmente pode criar suas árvores XML com um nó raiz de <xref:System.Xml.Linq.XElement>. A menos que você tenha um requisito específico para criar um documento (por exemplo, porque você precisa criar instruções de processamento e comentários no nível superior ou precisa dar suporte a tipos de documento), geralmente é mais conveniente usar <xref:System.Xml.Linq.XElement> como o nó raiz.  
  
 O conteúdo válido de um documento inclui o seguinte:  
  
- Zero ou um objeto <xref:System.Xml.Linq.XDocumentType>. Os tipos de documento devem vir antes do elemento.  
  
- Zero ou um elemento.  
  
- Zero ou mais comentários.  
  
- Zero ou mais instruções de processamento.  
  
- Zero ou mais nós de texto que contêm somente espaço em branco.  
  
## <a name="constructors-and-functions-that-allow-adding-content"></a>Construtores e funções que permitem adicionar conteúdo  
 Os métodos a seguir permitem adicionar conteúdo filho a um <xref:System.Xml.Linq.XElement> ou a um <xref:System.Xml.Linq.XDocument>:  
  
|Método|Descrição|  
|------------|-----------------|  
|<xref:System.Xml.Linq.XElement.%23ctor%2A>|Constrói um <xref:System.Xml.Linq.XElement>.|  
|<xref:System.Xml.Linq.XDocument.%23ctor%2A>|Constrói um <xref:System.Xml.Linq.XDocument>.|  
|<xref:System.Xml.Linq.XContainer.Add%2A>|Adiciona ao final do conteúdo filho do <xref:System.Xml.Linq.XElement> ou do <xref:System.Xml.Linq.XDocument>.|  
|<xref:System.Xml.Linq.XNode.AddAfterSelf%2A>|Adiciona conteúdo depois de <xref:System.Xml.Linq.XNode>.|  
|<xref:System.Xml.Linq.XNode.AddBeforeSelf%2A>|Adiciona conteúdo antes de <xref:System.Xml.Linq.XNode>.|  
|<xref:System.Xml.Linq.XContainer.AddFirst%2A>|Adiciona conteúdo ao início do conteúdo filho de <xref:System.Xml.Linq.XContainer>.|  
|<xref:System.Xml.Linq.XElement.ReplaceAll%2A>|Substitui todo o conteúdo (nós filho e atributos) de um <xref:System.Xml.Linq.XElement>.|  
|<xref:System.Xml.Linq.XElement.ReplaceAttributes%2A>|Substitui os atributos de um <xref:System.Xml.Linq.XElement>.|  
|<xref:System.Xml.Linq.XContainer.ReplaceNodes%2A>|Substitui os nós filho pelo novo conteúdo.|  
|<xref:System.Xml.Linq.XNode.ReplaceWith%2A>|Substitui um nó pelo novo conteúdo.|  
  
## <a name="see-also"></a>Confira também

- [Criando árvores XML (Visual Basic)](creating-xml-trees.md)
