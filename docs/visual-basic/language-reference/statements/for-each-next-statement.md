---
title: Instrução For Each...Next
ms.date: 07/20/2015
f1_keywords:
- vb.ForEach
- vb.ForEachNext
- vb.Each
- ForEachNext
helpviewer_keywords:
- infinite loops
- Next statement [Visual Basic], For Each...Next
- endless loops
- loop structures [Visual Basic], For Each...Next
- loops, endless
- Each keyword [Visual Basic]
- instructions, repeating
- For Each statement [Visual Basic]
- collections, instruction repetition
- loops, infinite
- For Each...Next statements
- For keyword [Visual Basic], For Each...Next statements
- Exit statement [Visual Basic], For Each...Next statements
- iteration
ms.assetid: ebce3120-95c3-42b1-b70b-fa7da40c75e2
ms.openlocfilehash: 0feb938121a97b06509b472652e6a753841ab2b8
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404648"
---
# <a name="for-eachnext-statement-visual-basic"></a>Instrução For Each...Next (Visual Basic)

Repete um grupo de instruções para cada elemento em uma coleção.

## <a name="syntax"></a>Sintaxe

```vb
For Each element [ As datatype ] In group
    [ statements ]
    [ Continue For ]
    [ statements ]
    [ Exit For ]
    [ statements ]
Next [ element ]
```

## <a name="parts"></a>Partes

|Termo|Definição|
|---|---|
|`element`|Necessário na `For Each` instrução. Opcional na `Next` instrução. Variável. Usado para iterar pelos elementos da coleção.|
|`datatype`|Opcional se [`Option Infer`](option-infer-statement.md) for on (o padrão) ou `element` já estiver declarado; Required se `Option Infer` for off e `element` ainda não estiver declarado. O tipo de dados de `element`.|
|`group`|Obrigatórios. Uma variável com um tipo que é um tipo de coleção ou objeto. Refere-se à coleção na qual os `statements` devem ser repetidos.|
|`statements`|Opcional. Uma ou mais instruções entre `For Each` e `Next` que são executadas em cada item no `group` .|
|`Continue For`|Opcional. Transfere o controle para o início do `For Each` loop.|
|`Exit For`|Opcional. Transfere o controle do `For Each` loop.|
|`Next`|Obrigatórios. Encerra a definição do `For Each` loop.|

## <a name="simple-example"></a>Exemplo simples

Use um `For Each` loop... `Next` quando você quiser repetir um conjunto de instruções para cada elemento de uma coleção ou matriz.

> [!TIP]
> Um [para... A próxima instrução](for-next-statement.md) funciona bem quando você pode associar cada iteração de um loop a uma variável de controle e determinar os valores iniciais e finais da variável. No entanto, quando você está lidando com uma coleção, o conceito de valores iniciais e finais não é significativo e você não sabe necessariamente quantos elementos a coleção tem. Nesse tipo de caso, um `For Each` loop... `Next` geralmente é uma opção melhor.

No exemplo a seguir, a `For Each` ...`Next` a instrução faz a iteração por todos os elementos de uma coleção de lista.

[!code-vb[VbVbalrStatements#121](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class9.vb#121)]

Para obter mais exemplos, consulte [coleções](../../../standard/collections/index.md) e [matrizes](../../programming-guide/language-features/arrays/index.md).

## <a name="nested-loops"></a>Nested Loops

Você pode aninhar `For Each` loops colocando um loop dentro de outro.

O exemplo a seguir demonstra aninhado `For Each` ...`Next` estruturas.

[!code-vb[VbVbalrStatements#122](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class9.vb#122)]

Quando você Aninha loops, cada loop deve ter uma `element` variável exclusiva.

Você também pode aninhar diferentes tipos de estruturas de controle entre si. Para obter mais informações, consulte [estruturas de controle aninhado](../../programming-guide/language-features/control-flow/nested-control-structures.md).

## <a name="exit-for-and-continue-for"></a>Sair para e continuar para

A instrução [Exit for](exit-statement.md) faz com que a execução saia do `For` ...`Next` loop e transfere o controle para a instrução que segue a `Next` instrução.

A `Continue For` instrução transfere o controle imediatamente para a próxima iteração do loop. Para obter mais informações, consulte [instrução Continue](continue-statement.md).

O exemplo a seguir mostra como usar as `Continue For` `Exit For` instruções e.

[!code-vb[VbVbalrStatements#123](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class9.vb#123)]

Você pode colocar qualquer número de `Exit For` instruções em um `For Each` loop. Quando usado em `For Each` loops aninhados, `Exit For` faz com que a execução saia do loop mais interno e transfere o controle para o próximo nível mais alto de aninhamento.

`Exit For`geralmente é usado após uma avaliação de alguma condição, por exemplo, em um `If` ... `Then` ...`Else` estruturá. Talvez você queira usar `Exit For` o para as seguintes condições:

- Continuar a iterar é desnecessário ou impossível. Isso pode ser causado por um valor errado ou uma solicitação de encerramento.

- Uma exceção é capturada em um `Try` ... `Catch` ...`Finally`. Você pode usar `Exit For` no final do `Finally` bloco.

- Há um loop infinito, que é um loop que poderia executar um número grande ou uniforme de vezes infinito. Se você detectar tal condição, poderá usar `Exit For` para escapar o loop. Para obter mais informações, consulte [do... Instrução loop](do-loop-statement.md).

## <a name="iterators"></a>Iterators

Você usa um *iterador* para executar uma iteração personalizada em uma coleção. Um iterador pode ser uma função ou um `Get` acessador. Ele usa uma `Yield` instrução para retornar cada elemento da coleção um de cada vez.

Você chama um iterador usando uma `For Each...Next` instrução. Cada iteração do loop `For Each` chama o iterador. Quando uma `Yield` instrução é alcançada no iterador, a expressão na `Yield` instrução é retornada e o local atual no código é retido. A execução será reiniciada desse local na próxima vez que o iterador for chamado.

O exemplo a seguir usa uma função de iterador. A função Iterator tem uma `Yield` instrução que está dentro de um [para... Próximo](for-next-statement.md) loop. No `ListEvenNumbers` método, cada iteração do `For Each` corpo da instrução cria uma chamada para a função de iterador, que prossegue para a próxima `Yield` instrução.

[!code-vb[VbVbalrStatements#127](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class9.vb#127)]

Para obter mais informações, consulte [iteradores](../../programming-guide/concepts/iterators.md), [instrução yield](yield-statement.md)e [iterador](../modifiers/iterator.md).

## <a name="technical-implementation"></a>Implementação Técnica

Quando a `For Each` ...`Next` a instrução é executada, Visual Basic avalia a coleção somente uma vez, antes de o loop ser iniciado. Se sua instrução bloquear alterações `element` ou `group` , essas alterações não afetarão a iteração do loop.

Quando todos os elementos da coleção foram atribuídos sucessivamente `element` , o loop para `For Each` e o controle passa para a instrução após a `Next` instrução.

Se [Option Infer](option-infer-statement.md) estiver on (sua configuração padrão), o compilador de Visual Basic poderá inferir o tipo de dados de `element` . Se ele estiver desativado e não `element` tiver sido declarado fora do loop, você deverá declará-lo na `For Each` instrução. Para declarar explicitamente o tipo de dados `element` , use uma `As` cláusula. A menos que o tipo de dados do elemento seja definido fora da `For Each` construção... `Next` , seu escopo é o corpo do loop. Observe que você não pode declarar `element` tanto fora quanto dentro do loop.

Opcionalmente, você pode especificar `element` na `Next` instrução. Isso melhora a legibilidade do seu programa, especialmente se você tiver `For Each` loops aninhados. Você deve especificar a mesma variável como aquela que aparece na `For Each` instrução correspondente.

Talvez você queira evitar alterar o valor de `element` dentro de um loop. Fazer isso pode dificultar a leitura e a depuração do código. A alteração do valor de `group` não afeta a coleção ou seus elementos, que foram determinados quando o loop foi inserido pela primeira vez.

Quando você estiver aninhando loops, se uma `Next` instrução de um nível de aninhamento externo for encontrada antes do `Next` de um nível interno, o compilador sinalizará um erro. No entanto, o compilador pode detectar esse erro sobreposto somente se você especificar `element` em cada `Next` instrução.

Se o seu código depende de percorrer uma coleção em uma ordem específica, um `For Each` loop... `Next` não é a melhor opção, a menos que você saiba as características do objeto enumerador que a coleção expõe. A ordem de passagem não é determinada por Visual Basic, mas pelo <xref:System.Collections.IEnumerator.MoveNext%2A> método do objeto enumerador. Portanto, talvez você não consiga prever qual elemento da coleção é o primeiro a ser retornado em `element` , ou que é o próximo a ser retornado após um determinado elemento. Você pode obter resultados mais confiáveis usando uma estrutura de loops diferente, como `For` ... `Next` ou `Do` . `Loop` ..

O tempo de execução deve ser capaz de converter os elementos em `group` para `element` . A `Option Strict` instrução [] controla se as conversões de alargamento e de estreitamento são permitidas ( `Option Strict` está desativado, seu valor padrão) ou se apenas conversões ampliadas são permitidas ( `Option Strict` está ativada). Para obter mais informações, consulte [conversões redutoras](#narrowing-conversions).

O tipo de dados de `group` deve ser um tipo de referência que se refere a uma coleção ou uma matriz enumerável. Normalmente, isso significa que `group` se refere a um objeto que implementa a <xref:System.Collections.IEnumerable> interface do `System.Collections` namespace ou a <xref:System.Collections.Generic.IEnumerable%601> interface do `System.Collections.Generic` namespace. `System.Collections.IEnumerable`define o <xref:System.Collections.IEnumerable.GetEnumerator%2A> método, que retorna um objeto enumerador para a coleção. O objeto Enumerator implementa a `System.Collections.IEnumerator` interface do `System.Collections` namespace e expõe a <xref:System.Collections.IEnumerator.Current%2A> propriedade e os <xref:System.Collections.IEnumerator.Reset%2A> métodos e <xref:System.Collections.IEnumerator.MoveNext%2A> . Visual Basic os usa para atravessar a coleção.

### <a name="narrowing-conversions"></a>Conversões de redução

Quando `Option Strict` é definido como `On` , as conversões redutoras normalmente causam erros do compilador. Em uma `For Each` instrução, no entanto, as conversões dos elementos no `group` para `element` são avaliadas e executadas em tempo de execução, e os erros do compilador causados por conversões redutoras são suprimidos.

No exemplo a seguir, a atribuição de `m` como o valor inicial para `n` não compila quando `Option Strict` está on porque a conversão de um `Long` para um `Integer` é uma conversão de restrição. No `For Each` entanto, na instrução, nenhum erro do compilador é relatado, mesmo que a atribuição `number` exija a mesma conversão de `Long` para `Integer` . Na `For Each` instrução que contém um número grande, um erro de tempo de execução ocorre quando <xref:Microsoft.VisualBasic.CompilerServices.Conversions.ToInteger%2A> é aplicado ao número grande.

[!code-vb[VbVbalrStatements#89](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class3.vb#89)]

### <a name="ienumerator-calls"></a>Chamadas de IEnumerator

Quando a execução de um `For Each` loop... `Next` é iniciada, Visual Basic verifica se `group` refere-se a um objeto de coleção válido. Caso contrário, ele gera uma exceção. Caso contrário, ele chamará o <xref:System.Collections.IEnumerator.MoveNext%2A> método e a <xref:System.Collections.IEnumerator.Current%2A> Propriedade do objeto enumerador para retornar o primeiro elemento. Se `MoveNext` o indicar que não há nenhum elemento seguinte, ou seja, se a coleção estiver vazia, o `For Each` loop será interrompido e o controle passará para a instrução após a `Next` instrução. Caso contrário, Visual Basic define `element` para o primeiro elemento e executa o bloco de instrução.

Cada vez que Visual Basic encontra a `Next` instrução, ele retorna para a `For Each` instrução. Novamente, ele chama `MoveNext` e `Current` retorna o próximo elemento e, novamente, executa o bloco ou interrompe o loop dependendo do resultado. Esse processo continua até `MoveNext` indicar que não há nenhum elemento seguinte ou uma `Exit For` instrução é encontrada.

**Modificando a coleção.** O objeto enumerador retornado <xref:System.Collections.IEnumerable.GetEnumerator%2A> normalmente não permite que você altere a coleção adicionando, excluindo, substituindo ou reordenando quaisquer elementos. Se você alterar a coleção depois de iniciar um `For Each` loop... `Next` , o objeto enumerador se tornará inválido e a próxima tentativa de acessar um elemento causará uma <xref:System.InvalidOperationException> exceção.

No entanto, esse bloqueio de modificação não é determinado por Visual Basic, mas sim pela implementação da <xref:System.Collections.IEnumerable> interface. É possível implementar `IEnumerable` de forma que permita modificações durante a iteração. Se estiver pensando em fazer essa modificação dinâmica, certifique-se de entender as características da `IEnumerable` implementação na coleção que você está usando.

**Modificando elementos de coleção.** A <xref:System.Collections.IEnumerator.Current%2A> Propriedade do objeto Enumerator é [ReadOnly](../modifiers/readonly.md)e retorna uma cópia local de cada elemento da coleção. Isso significa que você não pode modificar os elementos em um `For Each` loop... `Next` . Qualquer modificação que você faça afeta apenas a cópia local de `Current` e não é refletida de volta na coleção subjacente. No entanto, se um elemento for um tipo de referência, você poderá modificar os membros da instância para a qual ele aponta. O exemplo a seguir modifica o `BackColor` membro de cada `thisControl` elemento. No entanto, você não pode modificar a `thisControl` si mesmo.

```vb
Sub LightBlueBackground(thisForm As System.Windows.Forms.Form)
    For Each thisControl In thisForm.Controls
        thisControl.BackColor = System.Drawing.Color.LightBlue
    Next thisControl
End Sub
```

O exemplo anterior pode modificar o `BackColor` membro de cada `thisControl` elemento, embora ele não possa `thisControl` se modificar.

**Atravessando matrizes.** Como a <xref:System.Array> classe implementa a <xref:System.Collections.IEnumerable> interface, todas as matrizes expõem o <xref:System.Array.GetEnumerator%2A> método. Isso significa que você pode iterar por meio de uma matriz com um `For Each` loop... `Next` . No entanto, você só pode ler os elementos da matriz. Você não pode alterá-los.

## <a name="example"></a>Exemplo

O exemplo a seguir lista todas as pastas em C:\ diretório usando a <xref:System.IO.DirectoryInfo> classe.

[!code-vb[VbVbalrStatements#124](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class9.vb#124)]

## <a name="example"></a>Exemplo

O exemplo a seguir ilustra um procedimento para a classificação de uma coleção. O exemplo classifica as instâncias de uma `Car` classe que são armazenadas em um <xref:System.Collections.Generic.List%601> . A classe `Car` implementa a interface <xref:System.IComparable%601>, que requer que o método <xref:System.IComparable%601.CompareTo%2A> seja implementado.

Cada chamada para o <xref:System.IComparable%601.CompareTo%2A> método faz uma única comparação que é usada para classificação. Os códigos escritos pelo usuário no método `CompareTo` retornam um valor para cada comparação do objeto atual com outro objeto. O valor retornado será menor que zero se o objeto atual for menor que o outro objeto, maior que zero se o objeto atual for maior que o outro objeto e zero, se eles forem iguais. Isso permite que você defina no código os critérios para maior que, menor que e igual.

No método `ListCars`, a instrução `cars.Sort()` classifica a lista. Essa chamada para o método <xref:System.Collections.Generic.List%601.Sort%2A> da <xref:System.Collections.Generic.List%601> faz com que o método `CompareTo` seja chamado automaticamente para os objetos `Car` na `List`.

[!code-vb[VbVbalrStatements#125](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class9.vb#125)]

## <a name="see-also"></a>Confira também

- [Coleções](../../../standard/collections/index.md)
- [Instrução For...Next](for-next-statement.md)
- [Estruturas de Loop](../../programming-guide/language-features/control-flow/loop-structures.md)
- [Instrução While...End While](while-end-while-statement.md)
- [Instrução Do...Loop](do-loop-statement.md)
- [Conversões de Widening e Narrowing](../../programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)
- [Inicializadores de objeto: tipos nomeados e anônimos](../../programming-guide/language-features/objects-and-classes/object-initializers-named-and-anonymous-types.md)
- [Inicializadores de coleção](../../programming-guide/language-features/collection-initializers/index.md)
- [Matrizes](../../programming-guide/language-features/arrays/index.md)
