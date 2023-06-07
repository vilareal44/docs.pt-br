---
title: Refatoração em funções puras
ms.date: 07/20/2015
ms.assetid: 99e7d27b-a3ff-4577-bdb2-5a8278d6d7af
ms.openlocfilehash: 415b088661eca347330f4776901d68ee514d8dad
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84413473"
---
# <a name="refactoring-into-pure-functions-visual-basic"></a>Refatoração em funções puras (Visual Basic)

Um aspecto importante de transformações e puras é aprender como o código do refatorar usando funções puras.

Conforme observado anteriormente nesta seção, uma função pura tem duas características úteis:

- Não tem efeito colateral. A função não altera quaisquer variáveis ou os dados de qualquer tipo fora da função.

- É consistente. Dado o mesmo conjunto de dados de entrada, sempre retornará o mesmo valor de saída.

 Uma maneira de fazer a transição para programação funcional é o código existente do refatorar para eliminar efeitos colaterais desnecessários e dependências externas. Dessa maneira, você pode criar versões puras de função do código existente.

Este tópico descreve o que é uma função pura e o que não é. O tutorial [: manipulando conteúdo em um documento do WordprocessingML (Visual Basic)](tutorial-manipulating-content-in-a-wordprocessingml-document.md) mostra como manipular um documento do WordprocessingML e inclui dois exemplos de como refatorar usando uma função pura.

## <a name="eliminating-side-effects-and-external-dependencies"></a>Eliminando efeitos colaterais e dependências externas

Os seguintes exemplos contrastam duas funções não puras e uma função pura.

### <a name="non-pure-function-that-changes-a-class-member"></a>Função não pura que altera um membro de classe

No código a seguir, a função de `HyphenatedConcat` não é uma função pura, porque altera o membro de dados `aMember` na classe:

```vb
Module Module1
    Dim aMember As String = "StringOne"

    Public Sub HyphenatedConcat(ByVal appendStr As String)
        aMember = aMember & "-" & appendStr
    End Sub

    Sub Main()
        HyphenatedConcat("StringTwo")
        Console.WriteLine(aMember)
    End Sub
End Module
```

Esse código gera a seguinte saída:

```console
StringOne-StringTwo
```

Observe que é irrelevante se os dados que estão sendo modificados têm `public` ou têm `private` acesso, ou é um membro `shared` ou um membro de instância. Uma função pura não altera quaisquer dados fora da função.

### <a name="non-pure-function-that-changes-an-argument"></a>Função não pura que altera um argumento

Além disso, a seguinte versão dessa mesma função não é pura porque modifica o conteúdo do seu parâmetro, `sb`.

```vb
Module Module1
    Public Sub HyphenatedConcat(ByVal sb As StringBuilder, ByVal appendStr As String)
        sb.Append("-" & appendStr)
    End Sub

    Sub Main()
        Dim sb1 As StringBuilder = New StringBuilder("StringOne")
        HyphenatedConcat(sb1, "StringTwo")
        Console.WriteLine(sb1)
    End Sub
End Module
```

Esta versão do programa gerenciar as mesmas saída que a primeira versão, porque a função de `HyphenatedConcat` alterou o valor (estado) do primeiro parâmetro chamar a função de membro de <xref:System.Text.StringBuilder.Append%2A> . Observe que essa mudança ocorre independentemente do fato de que `HyphenatedConcat` usa passar o parâmetro de atendimento-por- valor.

> [!IMPORTANT]
> Para tipos de referência, se você passa um parâmetro por valor, ele resulta em uma cópia de referência a um objeto que está sendo passado. Esta cópia ainda está associada com os mesmos dados de instância que a referência original (até que a variável de referência é atribuído a um novo objeto). a Atendimento-por- referência não necessariamente é necessária para uma função modifique um parâmetro.

### <a name="pure-function"></a>Função pura

Seguir essa versão de hows de programa como implementar a função de `HyphenatedConcat` como uma função pura.

```vb
Module Module1
    Public Function HyphenatedConcat(ByVal s As String, ByVal appendStr As String) As String
        Return (s & "-" & appendStr)
    End Function

    Sub Main()
        Dim s1 As String = "StringOne"
        Dim s2 As String = HyphenatedConcat(s1, "StringTwo")
        Console.WriteLine(s2)
    End Sub
End Module
```

Além disso, esta versão gerencia a mesma linha de saída: `StringOne-StringTwo`. Observe que para manter o valor concatenado, ele é armazenado em `s2`variável intermediária.

Uma abordagem que pode ser muito útil é escrever as funções que são localmente impuros (isto é, declare e alteram variáveis locais) mas é global pura. Tais funções têm muitas das características desejáveis de composability, mas impedem alguns de linguagem e mais complicados de programação, como ter que usar a recursão quando um loop simples realizaria a mesma coisa.

## <a name="standard-query-operators"></a>Operadores de consulta padrão

Uma característica importante dos operadores de consulta padrão é que são implementados como funções puras.

Para obter mais informações, consulte [visão geral dos operadores de consulta padrão (Visual Basic)](standard-query-operators-overview.md).

## <a name="see-also"></a>Confira também

- [Introdução às transformações funcionais puras (Visual Basic)](introduction-to-pure-functional-transformations.md)
- [Programação funcional versus programação imperativa (Visual Basic)](functional-programming-vs-imperative-programming.md)
