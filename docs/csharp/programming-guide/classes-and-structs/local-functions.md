---
title: Funções locais – Guia de Programação em C#
description: As funções locais em C# são métodos privados que são aninhados em outro membro e podem ser chamados de seus membros que os contêm.
ms.date: 06/14/2017
helpviewer_keywords:
- local functions [C#]
ms.openlocfilehash: 854ec7ab4a4cc637c0a5ad03e0344d2f1f7679d2
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88063296"
---
# <a name="local-functions-c-programming-guide"></a>Funções locais (Guia de Programação em C#)

Começando com o C# 7.0, o C# é compatível com *funções locais*. Funções locais são métodos privados de um tipo que estão aninhados em outro membro. Eles só podem ser chamados do membro que os contém. Funções locais podem ser declaradas em e chamadas de:

- Métodos, especialmente os métodos iteradores e os métodos assíncronos
- Construtores
- Acessadores de propriedades
- Acessadores de eventos
- Métodos anônimos
- Expressões lambda
- Finalizadores
- Outras funções locais

No entanto, as funções locais não podem ser declaradas dentro de um membro apto para expressão.

> [!NOTE]
> Em alguns casos, você pode usar uma expressão lambda para implementar uma funcionalidade que também tem suporte por uma função local. Para obter uma comparação, consulte [funções locais versus expressões lambda](#local-functions-vs-lambda-expressions).

Funções locais tornam a intenção do seu código clara. Qualquer pessoa que ler seu código poderá ver que o método não pode ser chamado, exceto pelo método que o contém. Para projetos de equipe, elas também impossibilitam que outro desenvolvedor chame o método por engano diretamente de qualquer outro lugar na classe ou no struct.

## <a name="local-function-syntax"></a>Sintaxe de função local

Uma função local é definida como um método aninhado dentro de um membro recipiente. Sua definição tem a seguinte sintaxe:

```csharp
<modifiers: async | unsafe> <return-type> <method-name> <parameter-list>
```

Funções locais podem usar os modificadores [async](../../language-reference/keywords/async.md) e [unsafe](../../language-reference/keywords/unsafe.md).

Observe que todas as variáveis locais que estão definidas no membro recipiente, incluindo seus parâmetros de método, são acessíveis na função local.

Ao contrário de uma definição de método, uma definição de função local não pode incluir o modificador de acesso de membro. Já que todas as funções locais são privadas, incluir um modificador de acesso como a palavra-chave `private` gera o erro do compilador CS0106, "O modificador 'private' não é válido para este item".

> [!NOTE]
> Antes do C# 8,0, as funções locais não podem incluir o `static` modificador. Incluir a palavra-chave `static` gera o erro do compilador CS0106, "O modificador 'static' não é válido para este item".

Além disso, os atributos não podem ser aplicados à função local ou aos respectivos parâmetros e parâmetros de tipo.

O exemplo a seguir define uma função local chamada `AppendPathSeparator` que é privada para um método chamado `GetText`:

[!code-csharp[LocalFunctionExample](~/samples/snippets/csharp/programming-guide/classes-and-structs/local-functions1.cs)]  

## <a name="local-functions-and-exceptions"></a>Funções locais e exceções

Um dos recursos úteis de funções locais é que elas podem permitir que exceções sejam apresentadas imediatamente. Para iteradores de método, as exceções são apresentadas somente quando a sequência retornada é enumerada e não quando o iterador é recuperado. Para métodos assíncronos, as exceções geradas em um método assíncrono são observadas quando a tarefa retornada é esperada.

O exemplo a seguir define um método `OddSequence` que enumera números ímpares entre um intervalo especificado. Já que ele passa um número maior que 100 para o método enumerador `OddSequence`, o método gera uma <xref:System.ArgumentOutOfRangeException>. Assim como demonstrado pela saída do exemplo, a exceção é apresentada somente quando você itera os números e não quando você recupera o enumerador.

[!code-csharp[LocalFunctionIterator1](~/samples/snippets/csharp/programming-guide/classes-and-structs/local-functions-iterator1.cs)]

Em vez disso, você pode lançar uma exceção ao executar a validação e antes de recuperar o iterador, retornando o iterador de uma função local, conforme mostrado no exemplo a seguir.

[!code-csharp[LocalFunctionIterator2](~/samples/snippets/csharp/programming-guide/classes-and-structs/local-functions-iterator2.cs)]

Funções locais podem ser usadas de maneira semelhante para lidar com exceções fora da operação assíncrona. Em geral, exceções geradas no método assíncrono exigem que você examine as exceções internas de um <xref:System.AggregateException>. Funções locais permitem que seu código falhe no modo rápido e permitem que a exceção seja gerada e observada de forma síncrona.

O exemplo a seguir usa um método assíncrono chamado `GetMultipleAsync` para pausar para um número especificado de segundos e retornar um valor que é um múltiplo aleatório desse número de segundos. O atraso máximo é de 5 segundos; um <xref:System.ArgumentOutOfRangeException> resulta em se o valor for maior que 5. Como mostra o exemplo a seguir, a exceção que é lançada quando um valor de 6 é passada para o método `GetMultipleAsync` é encapsulada em um <xref:System.AggregateException> depois que o método `GetMultipleAsync` inicia sua execução.

[!code-csharp[LocalFunctionAsync](~/samples/snippets/csharp/programming-guide/classes-and-structs/local-functions-async1.cs)]

Assim como foi feito com o método iterador, podemos refatorar o código deste exemplo para executar a validação antes de chamar o método assíncrono. Assim como demonstrado pela saída de exemplo a seguir, o <xref:System.ArgumentOutOfRangeException> não é empacotado em uma <xref:System.AggregateException>.

[!code-csharp[LocalFunctionAsync](~/samples/snippets/csharp/programming-guide/classes-and-structs/local-functions-async2.cs)]

## <a name="local-functions-vs-lambda-expressions"></a>Funções locais vs. expressões lambda

À primeira vista, funções locais e [expressões lambda](../../language-reference/operators/lambda-expressions.md) são muito semelhantes. Em muitos casos, a escolha entre usar expressões lambda e funções locais é uma questão de estilo e preferência pessoal. No entanto, há diferenças reais nos casos em que você pode usar uma ou outra, e é importante conhecer essas diferenças.

Examinaremos as diferenças entre a função local e as implementações de expressão lambda do algoritmo fatorial. Primeiro a versão usando uma função local:

[!code-csharp[LocalFunctionFactorial](../../../../samples/snippets/csharp/new-in-7/MathUtilities.cs#37_LocalFunctionFactorial "Recursive factorial using local function")]

Compare essa implementação com uma versão que usa expressões lambda:

[!code-csharp[26_LambdaFactorial](../../../../samples/snippets/csharp/new-in-7/MathUtilities.cs#38_LambdaFactorial "Recursive factorial using lambda expressions")]

As funções locais têm nomes. As expressões lambda são métodos anônimos que são atribuídos a variáveis dos tipos `Func` ou `Action`. Quando você declara uma função local, os tipos de argumento e o tipo de retorno fazem parte da declaração da função. Em vez de fazer parte do corpo da expressão lambda, os tipos de argumento e o tipo de retorno são parte da declaração de tipo de variável da expressão lambda. Essas duas diferenças podem resultar em um código mais claro.

As funções locais têm diferentes regras para atribuição definida em relação às expressões lambda. Uma declaração de função local pode ser referenciada em qualquer local do código em que ela esteja no escopo. Uma expressão lambda deve ser atribuída a uma variável de delegado antes de poder ser acessada (ou chamada por meio do delegado que referencia a expressão lambda). Observe que a versão que usa a expressão lambda deve declarar e inicializar a expressão lambda `nthFactorial` antes de defini-la. Não fazer isso resulta em um erro em tempo de compilação para referenciar `nthFactorial` antes de atribuí-lo. Essas diferenças significam que os algoritmos recursivos são mais fáceis de criar usando funções locais. Você pode declarar e definir uma função local que chame a si mesma. As expressões lambda devem ser declaradas e atribuídas a um valor padrão antes que possam ser reatribuídas a um corpo que referencie a mesma expressão lambda.

As regras de atribuição definidas também afetam as variáveis que são capturadas pela função local ou pela expressão lambda. As regras das funções locais e das expressões lambda exigem que as variáveis capturadas sejam definitivamente atribuídas no momento em que a expressão lambda ou a função local é convertida em um delegado. A diferença é que as expressões lambda são convertidas em delegados no momento em que são declaradas. As funções locais são convertidas em delegados somente quando usadas como um delegado. Se você declarar uma função local e só referenciá-la ao chamá-la como um método, ela não será convertida em um delegado. Essa regra permite que você declare uma função local em qualquer local conveniente no respectivo escopo delimitador. É comum declarar funções locais ao final do método pai, depois das instruções de retorno.

Em terceiro lugar, o compilador pode executar uma análise estática que permite que as funções locais atribuam definitivamente as variáveis capturadas no escopo delimitador. Considere este exemplo:

```csharp
int M()
{
    int y;
    LocalFunction();
    return y;

    void LocalFunction() => y = 0;
}
```

O compilador pode determinar que `LocalFunction` definitivamente atribua `y` quando chamada. Como a `LocalFunction` é chamada antes da instrução `return`, `y` é atribuído definitivamente na instrução `return`.

Essa análise de exemplo permite a quarta diferença. Dependendo do uso, as funções locais podem evitar as alocações de heap que são sempre necessárias nas expressões lambda. Se uma função local nunca é convertida em um delegado, e nenhuma das variáveis capturadas pela função local é capturada por outros lambdas ou funções locais que são convertidas em delegados, o compilador pode evitar alocações de heap.

Considere este exemplo assíncrono:

[!code-csharp[TaskLambdaExample](../../../../samples/snippets/csharp/new-in-7/AsyncWork.cs#36_TaskLambdaExample "Task returning method with lambda expression")]

O fechamento desta expressão lambda contém as variáveis `address`, `index` e `name`. No caso de funções locais, o objeto que implementa o encerramento pode ser um tipo `struct`. Esse tipo de struct seria passado por referência à função local. Essa diferença na implementação poderia economizar em uma alocação.

A instanciação necessária para expressões lambda ocasiona alocações adicionais de memória, tornando-se um fator de desempenho em caminhos de código com tempo crítico. As funções locais não incorrem nessa sobrecarga. No exemplo acima, a versão das funções locais tem 2 alocações a menos que a versão da expressão lambda.

> [!NOTE]
> A função local equivalente desse método também usa uma classe para o fechamento. O fechamento de uma função local ser implementado como um `class` ou como um `struct`, trata-se de um detalhe de implementação. Uma função local pode usar um `struct`, enquanto uma lambda sempre usará um `class`.

[!code-csharp[TaskLocalFunctionExample](../../../../samples/snippets/csharp/new-in-7/AsyncWork.cs#TaskExample "Task returning method with local function")]

Uma vantagem final não demonstrada neste exemplo é que as funções locais podem ser implementadas como iteradores, usando a sintaxe `yield return` para produzir uma sequência de valores. A instrução `yield return` não é permitida em expressões lambda.

Embora as funções locais possam parecer redundantes para expressões lambda, elas realmente têm finalidades e usos diferentes. As funções locais são mais eficientes para quando você deseja escrever uma função que é chamada apenas do contexto de outro método.

## <a name="see-also"></a>Consulte também

- [Métodos](methods.md)
