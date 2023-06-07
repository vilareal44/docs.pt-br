---
title: Instrução For...Next
ms.date: 07/20/2015
f1_keywords:
- vb.Step
- vb.Next
- vb.To
- vb.for
helpviewer_keywords:
- infinite loops
- Next keyword [Visual Basic], For...Next statements
- For keyword [Visual Basic], For...Next statements
- Step keyword [Visual Basic], For...Next statements
- operator overloading, For...Next statement
- To keyword [Visual Basic], For...Next statements
- endless loops
- loops, endless
- instructions, repeating
- Next statement [Visual Basic], For...Next
- For...Next statements
- loop structures [Visual Basic], For...Next
- loops, infinite
- Exit statement [Visual Basic], For...Next statements
- For statement [Visual Basic]
ms.assetid: f5fc0d51-67ce-4c36-9f09-31c9a91c94e9
ms.openlocfilehash: 6896c6cfb4ec5d6207011e56b72639c459120e53
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404635"
---
# <a name="fornext-statement-visual-basic"></a>Instrução For...Next (Visual Basic)

Repete um grupo de instruções um número especificado de vezes.

## <a name="syntax"></a>Sintaxe

```vb
For counter [ As datatype ] = start To end [ Step step ]
    [ statements ]
    [ Continue For ]
    [ statements ]
    [ Exit For ]
    [ statements ]
Next [ counter ]
```

## <a name="parts"></a>Partes

|Parte|Descrição|
|----------|-----------------|
|`counter`|Necessário na `For` instrução. Variável numérica. A variável de controle para o loop. Para obter mais informações, consulte o [argumento Counter](#BKMK_Counter) mais adiante neste tópico.|
|`datatype`|Opcional. Tipo de dados de `counter` . Para obter mais informações, consulte o [argumento Counter](#BKMK_Counter) mais adiante neste tópico.|
|`start`|Obrigatórios. Expressão numérica. O valor inicial de `counter`.|
|`end`|Obrigatórios. Expressão numérica. O valor final de `counter` .|
|`step`|Opcional. Expressão numérica. A quantidade por que `counter` é incrementada a cada vez pelo loop.|
|`statements`|Opcional. Uma ou mais instruções entre `For` e `Next` que executam o número especificado de vezes.|
|`Continue For`|Opcional. Transfere o controle para a próxima iteração do loop.|
|`Exit For`|Opcional. Transfere o controle do `For` loop.|
|`Next`|Obrigatórios. Encerra a definição do `For` loop.|

> [!NOTE]
> A `To` palavra-chave é usada nesta instrução para especificar o intervalo para o contador. Você também pode usar essa palavra-chave na [marca de seleção... Instrução Case](select-case-statement.md) e em declarações de matriz. Para obter mais informações sobre declarações de matriz, consulte [instrução Dim](dim-statement.md).

## <a name="simple-examples"></a> Exemplos simples

Você usa uma `For` estrutura... `Next` quando quiser repetir um conjunto de instruções por um número definido de vezes.

No exemplo a seguir, a `index` variável começa com um valor de 1 e é incrementada com cada iteração do loop, terminando após o valor de `index` chegar a 5.

[!code-vb[VbVbalrStatements#111](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class7.vb#111)]

No exemplo a seguir, a `number` variável começa em 2 e é reduzida por 0,25 em cada iteração do loop, terminando após o valor de `number` chegar a 0. O `Step` argumento de `-.25` reduz o valor de 0,25 em cada iteração do loop.

[!code-vb[VbVbalrStatements#112](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class7.vb#112)]

> [!TIP]
> Um [tempo... Instrução End While](while-end-while-statement.md) ou [do... Instrução de loop](do-loop-statement.md) funciona bem quando você não sabe antecipadamente quantas vezes executar as instruções no loop. No entanto, quando você espera executar o loop um número específico de vezes, um `For` loop... `Next` é uma opção melhor. Você determina o número de iterações quando entra o loop pela primeira vez.

## <a name="nesting-loops"></a>Loops de aninhamento

Você pode aninhar `For` loops colocando um loop dentro de outro. O exemplo a seguir demonstra `For` estruturas aninhadas... `Next` que têm valores de etapa diferentes. O loop externo cria uma cadeia de caracteres para cada iteração do loop. O loop interno decrementa uma variável de contador de loop para cada iteração do loop.

[!code-vb[VbVbalrStatements#113](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class7.vb#113)]

Ao aninhar loops, cada loop deve ter uma `counter` variável exclusiva.

Você também pode aninhar estruturas de controle de tipos diferentes entre si. Para obter mais informações, consulte [estruturas de controle aninhado](../../programming-guide/language-features/control-flow/nested-control-structures.md).

## <a name="exit-for-and-continue-for"></a>Sair para e continuar para

A `Exit For` instrução sai imediatamente do `For` ...`Next` loop e transfere o controle para a instrução que segue a `Next` instrução.

A `Continue For` instrução transfere o controle imediatamente para a próxima iteração do loop. Para obter mais informações, consulte [instrução Continue](continue-statement.md).

O exemplo a seguir ilustra o uso das `Continue For` `Exit For` instruções e.

[!code-vb[VbVbalrStatements#115](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class7.vb#115)]

Você pode colocar qualquer número de `Exit For` instruções em um `For` ...`Next` While. Quando usado dentro de aninhado `For` ...`Next` loops, `Exit For` sai do loop mais interno e transfere o controle para o próximo nível mais alto de aninhamento.

`Exit For`geralmente é usado depois que você avalia alguma condição (por exemplo, em um `If` ... `Then` ...`Else` estrutura). Talvez você queira usar `Exit For` o para as seguintes condições:

- Continuar a iterar é desnecessário ou impossível. Um valor errado ou uma solicitação de encerramento pode criar essa condição.

- A `Try` ... `Catch` ...`Finally` a instrução captura uma exceção. Você pode usar `Exit For` no final do `Finally` bloco.

- Você tem um loop infinito, que é um loop que poderia executar um número grande ou mesmo infinito de vezes. Se você detectar tal condição, poderá usar `Exit For` para escapar o loop. Para obter mais informações, consulte [do... Instrução loop](do-loop-statement.md).

## <a name="technical-implementation"></a>Implementação Técnica

Quando um `For` loop... `Next` é iniciado, Visual Basic avalia `start` , `end` e `step` . Visual Basic avalia esses valores somente no momento e, em seguida, atribui `start` a `counter` . Antes da execução do bloco de instrução, Visual Basic `counter` se compara com `end` . Se `counter` já for maior que o `end` valor (ou menor se `step` for negativo), o `For` loop terminará e o controle passará para a instrução que segue a `Next` instrução. Caso contrário, o bloco de instrução será executado.

Cada vez que Visual Basic encontra a `Next` instrução, ele é incrementado `counter` por `step` e retorna à `For` instrução. Novamente, ele compara `counter` com `end` e, novamente, executa o bloco ou sai do loop, dependendo do resultado. Esse processo continua até que `counter` passes `end` ou uma `Exit For` instrução seja encontrada.

O loop não é interrompido até que `counter` tenha passado `end` . Se `counter` for igual a `end` , o loop continuará. A comparação que determina se o bloco será executado `counter`  <=  `end` se `step` for positivo e `counter`  >=  `end` se `step` for negativo.

Se você alterar o valor de `counter` enquanto estiver dentro de um loop, seu código poderá ser mais difícil de ler e depurar. Alterar o valor de `start` , `end` ou `step` não afeta os valores de iteração que foram determinados quando o loop foi inserido pela primeira vez.

Se você aninhar loops, o compilador sinalizará um erro se encontrar a `Next` instrução de um nível de aninhamento externo antes da `Next` instrução de um nível interno. No entanto, o compilador pode detectar esse erro sobreposto somente se você especificar `counter` em cada `Next` instrução.

### <a name="step-argument"></a>Argumento Step

O valor de `step` pode ser positivo ou negativo. Esse parâmetro determina o processamento de loop de acordo com a tabela a seguir:

|**Valor da etapa**|**O loop é executado se**|
|--------------------|--------------------------|
|Positivo ou zero|`counter` <= `end`|
|Negativo|`counter` >= `end`|

O valor padrão de `step` é 1.

### <a name="counter-argument"></a><a name="BKMK_Counter"></a>Argumento do contador

A tabela a seguir indica se o `counter` define uma nova variável local com escopo para todo o `For…Next` loop. Essa determinação depende de se o `datatype` está presente e se `counter` já está definido.

|Está `datatype` presente?|`counter`Já está definido?|Resultado ( `counter` define se uma nova variável local está no escopo de todo o `For...Next` loop)|
|----------------------------|-----------------------------------|-------------------------------------------------------------------------------------------------------------|
|Não|Sim|Não, porque `counter` já está definido. Se o escopo de `counter` não for local para o procedimento, ocorrerá um aviso de tempo de compilação.|
|Não|Não|Sim. O tipo de dados é inferido das `start` `end` expressões, e `step` . Para obter informações sobre a inferência de tipos, consulte [instrução Option Infer](option-infer-statement.md) e [inferência de tipo local](../../programming-guide/language-features/variables/local-type-inference.md).|
|Sim|Sim|Sim, mas somente se a `counter` variável existente estiver definida fora do procedimento. Essa variável permanece separada. Se o escopo da variável existente `counter` for local para o procedimento, ocorrerá um erro em tempo de compilação.|
|Sim|Não|Sim.|

O tipo de dados de `counter` determina o tipo da iteração, que deve ser um dos seguintes tipos:

- A `Byte` ,,,,,,, `SByte` `UShort` `Short` `UInteger` `Integer` `ULong` `Long` , `Decimal` , `Single` ou `Double` .

- Uma enumeração que você declara usando uma [instrução enum](enum-statement.md).

- Um `Object`.

- Um tipo `T` que tem os seguintes operadores, em que `B` é um tipo que pode ser usado em uma `Boolean` expressão.

  `Public Shared Operator >= (op1 As T, op2 As T) As B`

  `Public Shared Operator <= (op1 As T, op2 As T) As B`

  `Public Shared Operator - (op1 As T, op2 As T) As T`

  `Public Shared Operator + (op1 As T, op2 As T) As T`

Opcionalmente, você pode especificar a `counter` variável na `Next` instrução. Essa sintaxe melhora a legibilidade do seu programa, especialmente se você tiver `For` loops aninhados. Você deve especificar a variável que aparece na instrução correspondente `For` .

As `start` `end` expressões, e `step` podem ser avaliadas como qualquer tipo de dados que amplia o tipo de `counter` . Se você usar um tipo definido pelo usuário para `counter` , talvez seja necessário definir o `CType` operador de conversão para converter os tipos de `start` , `end` ou `step` para o tipo de `counter` .

## <a name="example"></a>Exemplo

O exemplo a seguir remove todos os elementos de uma lista genérica. Em vez de um [para cada... A próxima instrução](for-each-next-statement.md), o exemplo mostra uma `For` instrução... `Next` que itera em ordem decrescente. O exemplo usa essa técnica porque o `removeAt` método faz com que os elementos após o elemento removido tenham um valor de índice inferior.

[!code-vb[VbVbalrStatements#114](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class7.vb#114)]

## <a name="example"></a>Exemplo

O exemplo a seguir itera através de uma enumeração que é declarada usando uma [instrução enum](enum-statement.md).

[!code-vb[VbVbalrStatements#116](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class7.vb#116)]

## <a name="example"></a>Exemplo

No exemplo a seguir, os parâmetros de instrução usam uma classe que tem sobrecargas de operador para os `+` `-` operadores,, `>=` e `<=` .

[!code-vb[VbVbalrStatements#117](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class7.vb#117)]

## <a name="see-also"></a>Confira também

- <xref:System.Collections.Generic.List%601>
- [Estruturas de Loop](../../programming-guide/language-features/control-flow/loop-structures.md)
- [Instrução While...End While](while-end-while-statement.md)
- [Instrução Do...Loop](do-loop-statement.md)
- [Estruturas de Controle Aninhadas](../../programming-guide/language-features/control-flow/nested-control-structures.md)
- [Instrução Exit](exit-statement.md)
- [Coleções](../../programming-guide/concepts/collections.md)
