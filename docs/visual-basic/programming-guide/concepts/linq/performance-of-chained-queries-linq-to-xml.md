---
title: Desempenho de consultas encadeadas (LINQ to XML)
ms.date: 07/20/2015
ms.assetid: 589f2adc-69f9-404d-b9d6-4c28dabea7f7
ms.openlocfilehash: 6b87f2744f663ebd45dceb036dcaac71b80765fc
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396383"
---
# <a name="performance-of-chained-queries-linq-to-xml-visual-basic"></a>Desempenho de consultas encadeadas (LINQ to XML) (Visual Basic)

Um dos benefícios mais importantes LINQ (e LINQ to XML) é que as consultas encadeadas podem executar bem como uma única consulta maior, mais complicada.

Uma consulta encadeada é uma consulta que usa outra consulta como sua fonte. Por exemplo, o seguinte código simples, `query2` tem `query1` como sua fonte:

```vb
Dim root As New XElement("Root", New XElement("Child", 1), New XElement("Child", 2), New XElement("Child", 3), New XElement("Child", 4))

Dim query1 = From x In root.Elements("Child") Where CInt(x) >= 3x

Dim query2 = From e In query1 Where CInt(e) Mod 2 = 0e

For Each i As var In query2
    Console.WriteLine("{0}", CInt(i))
Next
```

Esse exemplo gera a saída a seguir:

```console
4
```

Esta consulta encadeada fornece o mesmo perfil de desempenho que iterando através de uma lista vinculada.

- O eixo de <xref:System.Xml.Linq.XContainer.Elements%2A> tem essencialmente o mesmo desempenho que iterando através de uma lista vinculada. <xref:System.Xml.Linq.XContainer.Elements%2A> é implementado como um iterador com execução adiada. Isso significa que faz qualquer trabalho adicional a iterar através da lista vinculada, como alocar do objeto de iterador e manter controle de estado de execução. Este trabalho pode ser dividida em duas categorias: o trabalho que é feito no momento no iterador é configurado, e o trabalho que é feito em cada iteração. O trabalho de configuração é uma pequena quantidade, fixa de trabalho e o trabalho feito em cada iteração é proporcionalmente para o número de itens na coleção de origem.

- Em `query1`, a cláusula de `Where` faz com que a consulta chama o método de <xref:System.Linq.Enumerable.Where%2A> . Esse método também é implementado como um iterador. O trabalho de configuração consiste em criar uma instância do representante que fará referência a expressão lambda, mais a configuração normal para um iterador. A cada iteração, o representante é chamado para executar o predicado. O trabalho de configuração e trabalho feitos em cada iteração são semelhantes ao trabalho feito para iterar através do eixo.

- Em `query1`, a cláusula SELECT faz com que a consulta chama o método de <xref:System.Linq.Enumerable.Select%2A> . Este método tem o mesmo perfil de desempenho que o método de <xref:System.Linq.Enumerable.Where%2A> .

- Em `query2`, a cláusula de `Where` e a cláusula `Select` têm o mesmo perfil de desempenho que em `query1`.

 A interação com `query2` é portanto diretamente proporcionalmente para o número de itens na fonte da primeira consulta, ou ser uma das vezes.

## <a name="see-also"></a>Confira também

- [Desempenho (LINQ to XML) (Visual Basic)](performance-linq-to-xml.md)
