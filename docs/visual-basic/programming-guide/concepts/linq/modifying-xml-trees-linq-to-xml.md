---
title: Modificar árvores XML (LINQ to XML)
ms.date: 07/20/2015
ms.assetid: 4ae511a5-4fc9-4178-9c8e-761357deae3f
ms.openlocfilehash: 3402188c4e34ef81bad41ed49f9236b4fb7c47ef
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84406879"
---
# <a name="modifying-xml-trees-linq-to-xml-visual-basic"></a>Modificando árvores XML (LINQ to XML) (Visual Basic)
O [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] é um repositório na memória para uma árvore XML. Depois que você carrega ou analisa uma árvore XML de uma fonte, o [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] permite modificar essa árvore no lugar e, em seguida, serializar a árvore, talvez salvando-a em um arquivo ou enviando-a a um servidor remoto.  
  
 Quando você modifica uma árvore no lugar, você usa determinados métodos, como o <xref:System.Xml.Linq.XContainer.Add%2A>.  
  
 No entanto, há outra abordagem, que é usar a construção funcional para gerar uma nova árvore com uma outra forma. Dependendo dos tipos de alterações que você precisa fazer em sua árvore XML e do tamanho da árvore, essa abordagem pode ser mais robusta e mais fácil de desenvolver. O primeiro tópico desta seção compara essas duas abordagens.  
  
## <a name="in-this-section"></a>Nesta seção  
  
|Tópico|Descrição|  
|-----------|-----------------|  
|[Modificação da árvore XML na memória versus construção funcional (LINQ to XML) (Visual Basic)](in-memory-xml-tree-modification-vs-functional-construction.md)|Compara a modificação de uma árvore XML na memória com a construção funcional.|  
|[Adicionando elementos, atributos e nós a uma árvore XML (Visual Basic)](adding-elements-attributes-and-nodes-to-an-xml-tree.md)|Fornece informações sobre como adicionar elementos, atributos ou nós a uma árvore XML.|  
|[Modificando elementos, atributos e nós em uma árvore XML](modifying-elements-attributes-and-nodes-in-an-xml-tree.md)|Fornece informações sobre como modificar elementos, atributos ou nós existentes.|  
|[Removendo elementos, atributos e nós de uma árvore XML (Visual Basic)](removing-elements-attributes-and-nodes-from-an-xml-tree.md)|Fornece informações sobre como remover elementos, atributos ou nós da árvore XML.|  
|[Mantendo pares de nome/valor (Visual Basic)](maintaining-name-value-pairs.md)|Descreve como manter informações do aplicativo que são melhor mantidas como pares nome/valor, como informações de configuração ou configurações globais.|  
|[Como: alterar o namespace para uma árvore XML inteira (Visual Basic)](how-to-change-the-namespace-for-an-entire-xml-tree.md)|Mostra como mover uma árvore XML de um namespace para outro.|  
  
## <a name="see-also"></a>Confira também

- [Guia de programação (LINQ to XML) (Visual Basic)](programming-guide-linq-to-xml.md)
