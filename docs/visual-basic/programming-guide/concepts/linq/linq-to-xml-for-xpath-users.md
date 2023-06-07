---
title: Usuários de LINQ to XML para XPath
ms.date: 07/20/2015
ms.assetid: 0e64911c-a7cc-4c20-b927-ca99078b5656
ms.openlocfilehash: 7942d33831644875f390e9128965fe1c36433808
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84368784"
---
# <a name="linq-to-xml-for-xpath-users-visual-basic"></a>LINQ to XML para usuários do XPath (Visual Basic)

Este conjunto de tópicos mostra algumas expressões XPath e seus equivalentes [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)].  
  
 Todos os exemplos usam a funcionalidade XPath no [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] disponibilizada pelos métodos de extensão em <xref:System.Xml.XPath.Extensions?displayProperty=nameWithType>. Os exemplos executam a expressão XPath e a expressão [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)]. Cada exemplo compara os resultados das duas consultas, validando se a expressão XPath é funcionalmente equivalente à consulta [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)]. Como os dois tipos de consultas retornam nós da mesma árvore XML, a comparação do resultado da consulta é feita usando identidade referencial.  
  
## <a name="in-this-section"></a>Nesta seção  
  
|Tópico|Descrição|  
|-----------|-----------------|  
|[Comparação XPath e de LINQ to XML](../../../../csharp/programming-guide/concepts/linq/comparison-of-xpath-and-linq-to-xml.md)|Apresenta uma visão geral das semelhanças e das diferenças entre XPath e [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)].|  
|[Como localizar um elemento filho (XPath-LINQ to XML) (Visual Basic)](how-to-find-a-child-element-xpath-linq-to-xml.md)|Compara o eixo do elemento filho XPath com o [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] <xref:System.Xml.Linq.XContainer.Element%2A> método.<br /><br /> A expressão XPath associada é:`"DeliveryNotes"`.|  
|[Como localizar uma lista de elementos filho (XPath-LINQ to XML) (Visual Basic)](how-to-find-a-list-of-child-elements-xpath-linq-to-xml.md)|Compara o eixo dos elementos filho XPath com o [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] <xref:System.Xml.Linq.XContainer.Elements%2A> eixo.<br /><br /> A expressão XPath associada é:`"./*"`|  
|[Como localizar o elemento raiz (XPath-LINQ to XML) (Visual Basic)](how-to-find-the-root-element-xpath-linq-to-xml.md)|Compara como obter o elemento raiz com XPath e [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)].<br /><br /> A expressão XPath associada é:`"/PurchaseOrders"`|  
|[Como localizar elementos descendentes (XPath-LINQ to XML) (Visual Basic)](how-to-find-descendant-elements-xpath-linq-to-xml.md)|Compara como obter os elementos descendentes com um nome específico com XPath e [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)].<br /><br /> A expressão XPath associada é:`"//Name"`|  
|[Como filtrar em um atributo (XPath-LINQ to XML) (Visual Basic)](how-to-filter-on-an-attribute-xpath-linq-to-xml.md)|Compara como obter os elementos descendentes com um nome especificado e com um atributo com um valor especificado com XPath e [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)].<br /><br /> A expressão XPath associada é:`"./Address[@Type='Shipping']"`|  
|[Como localizar elementos relacionados (XPath-LINQ to XML) (Visual Basic)](how-to-find-related-elements-xpath-linq-to-xml.md)|Compara como obter um elemento selecionando em um atributo que é referenciado pelo valor de outro elemento com XPath e [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)].<br /><br /> A expressão XPath associada é:`"./Customer[@CustomerID=/Root/Orders/Order[12]/CustomerID]"`|  
|[Como localizar elementos em um namespace (XPath-LINQ to XML) (Visual Basic)](how-to-find-elements-in-a-namespace.md)|Compara o uso da classe XPath <xref:System.Xml.XmlNamespaceManager> com a [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] <xref:System.Xml.Linq.XName.Namespace%2A> propriedade da <xref:System.Xml.Linq.XName> classe para trabalhar com namespaces XML.<br /><br /> A expressão XPath associada é:`"./aw:*"`|  
|[Como: localizar irmãos precedentes (XPath-LINQ to XML) (Visual Basic)](how-to-find-preceding-siblings-xpath-linq-to-xml.md)|Compara o eixo XPath `preceding-sibling` com o eixo [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] filho <xref:System.Xml.Linq.XNode.ElementsBeforeSelf%2A?displayProperty=nameWithType>.<br /><br /> A expressão XPath associada é:`"preceding-sibling::*"`|  
|[Como localizar descendentes de um elemento filho (XPath-LINQ to XML) (Visual Basic)](how-to-find-descendants-of-a-child-element-xpath-linq-to-xml.md)|Compara como obter os elementos descendentes de um elemento filho com um nome específico com XPath e [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)].<br /><br /> A expressão XPath associada é:`"./Paragraph//Text/text()"`|  
|[Como localizar uma União de dois caminhos de local (XPath-LINQ to XML) (Visual Basic)](how-to-find-a-union-of-two-location-paths-xpath.md)|Compara o uso do operador de união, <code>&#124;</code>, no XPath com o operador de consulta padrão <xref:System.Linq.Enumerable.Concat%2A> em [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)].<br /><br /> A expressão XPath associada é:<code>"//Category&#124;//Price"</code>|  
|[Como localizar nós irmãos (XPath-LINQ to XML) (Visual Basic)](how-to-find-sibling-nodes-xpath-linq-to-xml.md)|Compara como localizar todos os irmãos de um nó que têm um nome específico com XPath e [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)].<br /><br /> A expressão XPath associada é:`"../Book"`|  
|[Como localizar um atributo do pai (XPath-LINQ to XML) (Visual Basic)](how-to-find-an-attribute-of-the-parent-xpath-linq-to-xml.md)|Compara como navegar para o elemento pai e localizar um atributo associado usando XPath e [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)].<br /><br /> A expressão XPath associada é:`"../@id"`|  
|[Como localizar atributos de irmãos com um nome específico (XPath-LINQ to XML) (Visual Basic)](how-to-find-attributes-of-siblings-with-a-specific-name.md)|Compara como localizar atributos específicos dos irmãos do nó de contexto com XPath e [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)].<br /><br /> A expressão XPath associada é:`"../Book/@id"`|  
|[Como localizar elementos com um atributo específico (XPath-LINQ to XML) (Visual Basic)](how-to-find-elements-with-a-specific-attribute.md)|Compara como localizar todos os elementos que contêm um atributo específico usando XPath e [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)].<br /><br /> A expressão XPath associada é:`"./*[@Select]"`|  
|[Como localizar elementos filho com base na posição (XPath-LINQ to XML) (Visual Basic)](how-to-find-child-elements-based-on-position.md)|Compara como localizar um elemento com base em sua posição relativa usando XPath e [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)].<br /><br /> A expressão XPath associada é:`"Test[position() >= 2 and position() <= 4]"`|  
|[Como localizar o irmão precedentes imediatos (XPath-LINQ to XML) (Visual Basic)](how-to-find-the-immediate-preceding-sibling-xpath-linq-to-xml.md)|Compara como localizar o irmão imediatamente precedente de um nó usando XPath e [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)].<br /><br /> A expressão XPath associada é:`"preceding-sibling::*[1]"`|  
  
## <a name="see-also"></a>Confira também

- <xref:System.Xml.XPath?displayProperty=nameWithType>
- [Consultando árvores XML (Visual Basic)](querying-xml-trees.md)
- [Processar dados XML usando o modelo de dados XPath](../../../../standard/data/xml/process-xml-data-using-the-xpath-data-model.md)
