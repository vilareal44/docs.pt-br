---
title: 'Como: depurar conjuntos de resultados de consultas vazios'
ms.date: 07/20/2015
ms.assetid: b242c90a-d2b8-4309-8a1e-e4e70736c727
ms.openlocfilehash: b2059ccd4638d2fb77c524773cb4bd50f721b5b9
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84398032"
---
# <a name="how-to-debug-empty-query-results-sets-visual-basic"></a>Como: Depurar conjuntos de resultados de consulta vazios (Visual Basic)

Um dos problemas mais comuns para o consulte árvores XML é que se a árvore tem um namespace XML padrão, o desenvolvedor escreve às vezes a consulta como se o XML não estar em um namespace.

Definir primeiro exemplos neste tópico mostra uma maneira comum que XML em um namespace padrão é carregado, e deduzido de modo inadequado.

O segundo conjunto de exemplos a seguir mostra as correções necessárias para que você possa ver XML em um namespace.

Para obter mais informações, consulte [visão geral de namespaces (LINQ to XML) (Visual Basic)](namespaces-overview-linq-to-xml.md).

## <a name="example"></a>Exemplo

Este exemplo mostra como criar XML em um namespace, e uma consulta que retorna um conjunto de resultados vazia.

```vb
Dim root As XElement = _
    <Root xmlns='http://www.adventure-works.com'>
        <Child>1</Child>
        <Child>2</Child>
        <Child>3</Child>
        <AnotherChild>4</AnotherChild>
        <AnotherChild>5</AnotherChild>
        <AnotherChild>6</AnotherChild>
    </Root>
Dim c1 As IEnumerable(Of XElement) = _
        From el In root.<Child> _
        Select el
Console.WriteLine("Result set follows:")
For Each el As XElement In c1
    Console.WriteLine(el.Value)
Next
Console.WriteLine("End of result set")
```

Este exemplo gerencia o resultado seguinte:

```console
Result set follows:
End of result set
```

## <a name="example"></a>Exemplo

Este exemplo mostra como criar XML em um namespace, e uma consulta que é codificado corretamente.

A solução é declarar e inicializar um namespace padrão global. Isso coloca todas as propriedades XML no namespace padrão. Outras alterações necessárias ao exemplo para fazê-lo funcionar corretamente.

```vb
Imports <xmlns="http://www.adventure-works.com">

Module Module1
    Sub Main()
        Dim root As XElement = _
            <Root xmlns='http://www.adventure-works.com'>
                <Child>1</Child>
                <Child>2</Child>
                <Child>3</Child>
                <AnotherChild>4</AnotherChild>
                <AnotherChild>5</AnotherChild>
                <AnotherChild>6</AnotherChild>
            </Root>
        Dim c1 As IEnumerable(Of XElement) = _
                From el In root.<Child> _
                Select el
        Console.WriteLine("Result set follows:")
        For Each el As XElement In c1
            Console.WriteLine(CInt(el))
        Next
        Console.WriteLine("End of result set")
    End Sub
End Module
```

Este exemplo gerencia o resultado seguinte:

```console
Result set follows:
1
2
3
End of result set
```

## <a name="see-also"></a>Confira também

- [Consultas básicas (LINQ to XML) (Visual Basic)](basic-queries-linq-to-xml.md)
