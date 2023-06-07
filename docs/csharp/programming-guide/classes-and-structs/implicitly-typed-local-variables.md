---
title: Variáveis locais de tipo implícito – Guia de Programação em C#
description: A palavra-chave var no C# instrui o compilador a inferir o tipo da variável a partir da expressão no lado direito da instrução de inicialização.
ms.date: 07/20/2015
helpviewer_keywords:
- implicitly-typed local variables [C#]
- var [C#]
ms.assetid: b9218fb2-ef5d-4814-8a8e-2bc29b0bbc9b
ms.openlocfilehash: 6badb8588dedda80227ab38bee027cf2890c8672
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86864209"
---
# <a name="implicitly-typed-local-variables-c-programming-guide"></a>Variáveis locais de tipo implícito (Guia de Programação em C#)

Variáveis locais podem ser declaradas sem fornecer um tipo explícito. A palavra-chave `var` instrui o compilador a inferir o tipo da variável da expressão no lado direito da instrução de inicialização. O tipo inferido pode ser um tipo interno, um tipo anônimo, um tipo definido pelo usuário ou um tipo definido na biblioteca de classes do .NET. Para obter mais informações sobre como inicializar matrizes com `var`, consulte [Matrizes de tipo implícito](../arrays/implicitly-typed-arrays.md).

Os exemplos a seguir mostram várias maneiras em que as variáveis locais podem ser declaradas com `var`:

[!code-csharp[csProgGuideLINQ#43](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideLINQ/CS/csRef30LangFeatures_2.cs#43)]

É importante entender que a palavra-chave `var` não significa "variante" e não indica que a variável é vagamente tipada ou de associação tardia. Isso apenas significa que o compilador determina e atribui o tipo mais apropriado.

A palavra-chave `var` pode ser usada nos seguintes contextos:

- Em variáveis locais (variáveis declaradas no escopo do método) conforme mostrado no exemplo anterior.

- Em uma instrução de inicialização [for](../../language-reference/keywords/for.md).

    ```csharp
    for (var x = 1; x < 10; x++)
    ```

- Em uma instrução de inicialização [foreach](../../language-reference/keywords/foreach-in.md).

    ```csharp
    foreach (var item in list) {...}
    ```

- Em uma instrução [using](../../language-reference/keywords/using-statement.md).

    ```csharp
    using (var file = new StreamReader("C:\\myfile.txt")) {...}
    ```

Para obter mais informações, consulte [como usar variáveis locais e matrizes de tipo implícito em uma expressão de consulta](how-to-use-implicitly-typed-local-variables-and-arrays-in-a-query-expression.md).

## <a name="var-and-anonymous-types"></a>Tipos var e anônimos

Em muitos casos, o uso de `var` é opcional e é apenas uma conveniência sintática. No entanto, quando uma variável é inicializada com um tipo anônimo você deve declarar a variável como `var` se precisar acessar as propriedades do objeto em um momento posterior. Esse é um cenário comum em expressões de consulta LINQ. Para obter mais informações, consulte [Tipos Anônimos](anonymous-types.md).

Da perspectiva do código-fonte, um tipo anônimo não tem nome. Portanto, se uma variável de consulta tiver sido inicializada com `var`, a única maneira de acessar as propriedades na sequência retornada será usar `var` como o tipo da variável de iteração na instrução `foreach`.

[!code-csharp[csProgGuideLINQ#44](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideLINQ/CS/csRef30LangFeatures_2.cs#44)]

## <a name="remarks"></a>Comentários

As seguintes restrições se aplicam às declarações de variável de tipo implícito:

- `var` pode ser usado apenas quando uma variável local é declarada e inicializada na mesma instrução, a variável não pode ser inicializada como nula, um grupo de métodos ou uma função anônima.

- `var` não pode ser usado em campos no escopo da classe.

- Variáveis declaradas usando `var` não podem ser usadas na expressão de inicialização. Em outras palavras, essa expressão é válida: `int i = (i = 20);` mas essa expressão produz um erro de tempo de compilação:`var i = (i = 20);`

- Diversas variáveis de tipo implícito não podem ser inicializadas na mesma instrução.

- Se um tipo nomeado `var` estiver no escopo, a palavra-chave `var` será resolvida para esse nome de tipo e não será tratada como parte de uma declaração de variável local de tipo implícito.

Tipagem implícita com a palavra-chave `var` só pode ser aplicada às variáveis no escopo do método local. Digitação implícita não está disponível para os campos de classe, uma vez que o compilador C# encontraria um paradoxo lógico ao processar o código: o compilador precisa saber o tipo do campo, mas não é possível determinar o tipo até que a expressão de atribuição seja analisada. A expressão não pode ser avaliada sem saber o tipo. Considere o seguinte código:

```csharp
private var bookTitles;
```

`bookTitles` é um campo de classe dado o tipo `var`. Como o campo não tem nenhuma expressão para avaliar, é impossível para o compilador inferir que tipo `bookTitles` deveria ser. Além disso, também é insuficiente adicionar uma expressão ao campo (como você faria para uma variável local):

```csharp
private var bookTitles = new List<string>();
```

Quando o compilador encontra campos durante a compilação de código, ele registra cada tipo de campo antes de processar quaisquer expressões associadas. O compilador encontra o mesmo paradoxo ao tentar analisar `bookTitles`: ele precisa saber o tipo do campo, mas o compilador normalmente determinaria o tipo de `var` analisando a expressão, o que não possível sem saber o tipo com antecedência.

Você pode descobrir que `var` também pode ser útil com expressões de consulta em que o tipo construído exato da variável de consulta é difícil de ser determinado. Isso pode ocorrer com operações de agrupamento e classificação.

A palavra-chave `var` também pode ser útil quando o tipo específico da variável é enfadonho de digitar no teclado, é óbvio ou não acrescenta à legibilidade do código. Um exemplo em que `var` é útil dessa maneira é com os tipos genéricos aninhados, como os usados com operações de grupo. Na consulta a seguir, o tipo da variável de consulta é `IEnumerable<IGrouping<string, Student>>`. Contanto que você e as outras pessoas que devem manter o código entendam isso, não há problema em usar a tipagem implícita por questões de conveniência e brevidade.

[!code-csharp[cscsrefQueryKeywords#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/Group.cs#13)]

O uso de `var` ajuda a simplificar seu código, mas seu uso deve ser restrito a casos em que é necessário, ou quando torna seu código mais fácil de ler. Para obter mais informações sobre quando usar `var` corretamente, consulte a seção [variáveis locais digitadas implicitamente](../inside-a-program/coding-conventions.md#implicitly-typed-local-variables) no artigo diretrizes de codificação em C#.

## <a name="see-also"></a>Veja também

- [Referência do C#](../../language-reference/index.md)
- [Matrizes de tipo implícito](../arrays/implicitly-typed-arrays.md)
- [Como usar matrizes e variáveis locais de tipo implícito em uma expressão de consulta](how-to-use-implicitly-typed-local-variables-and-arrays-in-a-query-expression.md)
- [Tipos anônimos](anonymous-types.md)
- [Inicializadores de objeto e coleção](object-and-collection-initializers.md)
- [var](../../language-reference/keywords/var.md)
- [LINQ em C#](../../linq/index.md)
- [LINQ (Consulta Integrada à Linguagem)](../../linq/index.md)
- [for](../../language-reference/keywords/for.md)
- [foreach, in](../../language-reference/keywords/foreach-in.md)
- [Instrução using](../../language-reference/keywords/using-statement.md)
