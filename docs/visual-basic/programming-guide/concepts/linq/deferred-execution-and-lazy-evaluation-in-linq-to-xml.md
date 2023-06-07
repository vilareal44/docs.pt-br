---
title: Execução adiada e avaliação lenta em LINQ to XML
ms.date: 07/20/2015
ms.assetid: 31998eed-b95e-47fb-a865-9de1f337d1fb
ms.openlocfilehash: 98a5010de6ea7ef46d845c6a921c54d4e7692370
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410806"
---
# <a name="deferred-execution-and-lazy-evaluation-in-linq-to-xml-visual-basic"></a>Execução retardada e avaliação lenta no LINQ to XML (Visual Basic)
As operações de consulta e eixo são geralmente implementadas para usar a execução adiada. Este tópico explica os requisitos e as vantagens da execução adiada, além de algumas considerações sobre a implementação.  
  
## <a name="deferred-execution"></a>Execução Adiada  
 A execução adiada significa que a avaliação de uma expressão será atrasada até que seu valor *realizado* seja realmente necessário. A execução adiada pode aumentar muito o desempenho quando você precisa manipular grandes coleções de dados, especialmente em programas que contêm uma série de consultas ou manipulações encadeadas. Na melhor das hipóteses, a execução adiada permite apenas uma única iteração pela coleção de origem.  
  
 As tecnologias LINQ usam de modo intenso a execução adiada em ambos os membros das classes <xref:System.Linq?displayProperty=nameWithType> centrais e nos métodos de extensão nos diversos namespaces LINQ, como <xref:System.Xml.Linq.Extensions?displayProperty=nameWithType>.  
  
## <a name="eager-vs-lazy-evaluation"></a>Avaliação adiantada versus lenta  
 Quando você escreve um método que implementa a execução adiada, também precisa decidir se deve implementar o método usando a avaliação lenta ou a avaliação ansiosa.  
  
- Na *avaliação lenta*, um único elemento da coleção de origem é processado durante cada chamada ao iterador. Essa é a forma comum de implementação de iteradores.  
  
- Na *avaliação adiantada*, a primeira chamada ao iterador resultará no processamento da coleção inteira. Uma cópia temporária da coleção de origem também pode ser necessária. Por exemplo, o método <xref:System.Linq.Enumerable.OrderBy%2A> precisa classificar toda a coleção antes de retornar o primeiro elemento.  
  
 A avaliação lenta normalmente gera um melhor desempenho porque distribui a sobrecarga de processamento uniformemente por toda a avaliação da coleção e minimiza o uso de dados temporários. Naturalmente, para algumas operações, não há nenhuma outra opção que não seja materializar resultados intermediários.  
  
## <a name="next-steps"></a>Próximas etapas  
 O próximo tópico deste tutorial ilustra a execução adiada:  
  
- [Exemplo de execução adiada (Visual Basic)](deferred-execution-example.md)  
  
## <a name="see-also"></a>Confira também

- [Tutorial: execução adiada (Visual Basic)](tutorial-deferred-execution.md)
- [Conceitos e terminologia (transformação funcional) (Visual Basic)](concepts-and-terminology-functional-transformation.md)
- [Operações de agregação (Visual Basic)](aggregation-operations.md)
