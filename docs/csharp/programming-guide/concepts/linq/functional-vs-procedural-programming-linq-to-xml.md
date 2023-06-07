---
title: Programação funcional vs. de procedimentos (LINQ to XML) (C#)
description: Para o processamento XML, o LINQ to XML dá suporte à modificação de árvore XML na memória e a uma construção funcional que usa uma abordagem declarativa.
ms.date: 07/20/2015
ms.assetid: fc64e39c-a487-4882-9169-da4de97917d9
ms.openlocfilehash: e19f9311c56f4fe2c5e7e5f228aca6c03c6fe04d
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87103654"
---
# <a name="functional-vs-procedural-programming-linq-to-xml-c"></a>Programação funcional vs. de procedimentos (LINQ to XML) (C#)
Há vários tipos de aplicativos XML:  
  
- Alguns aplicativos utilizam documentos XML de origem e geram novos documentos XML que estão em um formato diferente do que os documentos de origem.  
  
- Alguns aplicativos utilizam documentos XML de origem e produzem documentos de resultado em um formato totalmente diferente, como arquivos de texto HTML ou CSV.  
  
- Alguns aplicativos utilizam documentos XML de origem e inserem registros em um banco de dados.  
  
- Alguns aplicativos utilizam dados de outra origem, como um banco de dados e criam documentos XML a partir deles.  
  
 Esses não são todos os tipos de aplicativos XML, mas são um conjunto representativo de tipos de funcionalidade que um programador XML tem que implementar.  
  
 Com todos esses tipos de aplicativos, há duas abordagens contrastantes que um desenvolvedor pode utilizar:  
  
- Construção funcional usando uma abordagem declarativa.  
  
- Modificação da árvore XML na memória usando código procedural.  
  
 O LINQ to XML oferece suporte às duas abordagens.  
  
 Ao usar a abordagem funcional, você escreve as transformações que utilizam os documentos de origem e geram documentos de resultado completamente novos com o formato desejado.  
  
 Ao modificar uma árvore XML no lugar, você escreve o código que percorre e navega por meio de nós em uma árvore XML na memória, inserindo, excluindo e modificando os nós conforme o necessário.  
  
 Você pode usar LINQ to XML com qualquer abordagem. Você usa as mesmas classes e, em alguns casos, os mesmos métodos. No entanto, a estrutura e as metas das duas abordagens são muito diferentes. Por exemplo, em situações diferentes, uma ou outra abordagem terá geralmente melhor desempenho, e usará mais ou menos memória. Além disso, uma ou outra abordagem será mais fácil de escrever e produzirá um código mais sustentável.  
  
 Para ver as duas abordagens contrastadas, consulte [modificação da árvore XML na memória versus construção funcional (LINQ to XML) (C#)](./in-memory-xml-tree-modification-vs-functional-construction-linq-to-xml.md).  
  
 Para obter um tutorial sobre como escrever transformações funcionais, consulte [Transformações funcionais puras de XML (C#)](./introduction-to-pure-functional-transformations.md).  
  
## <a name="see-also"></a>Confira também

- [Visão geral da programação LINQ to XML (C#)](./linq-to-xml-overview.md)
